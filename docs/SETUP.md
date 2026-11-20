# Set up a RED DISH demo

## Prerequisites

Use an n8n instance supporting the exported node types, a Telegram bot, Gemini API access, Google Sheets, and Gmail. The export does not identify a tested n8n release. Keep the imported workflow inactive while configuring it.

Import `RED DISH last.json` through **Import from File** in the workflow menu. [Official instructions](https://docs.n8n.io/workflows/export-import/).

## Connect accounts

| Service | Exported credential types | Configure |
| --- | --- | --- |
| Telegram | `telegramApi` | Your test bot in the trigger and action nodes. |
| Gemini | `googlePalmApi` | Your credentials and an available model in the Gemini node. |
| Sheets | `googleSheetsOAuth2Api`, `googleSheetsTriggerOAuth2Api` | Access to your own sheets, for both action and trigger nodes. |
| Gmail | `gmailOAuth2` | A test sender and recipients you control. |

Re-select credentials after import. References in the JSON do not provide access to the author's accounts. Configure spreadsheet IDs, sheet selections, recipients, menu URL, and feedback form URL directly in the nodes. No environment-variable loader is included.

## Prepare sheets

The CSVs under `examples/` provide headers and fictional records. Import them into separate demo tabs, then select each tab in the appropriate node. They are setup aids; node mappings still need configuration.

| Data | Fields |
| --- | --- |
| Customer info | `Order ID`, `Customer Name`, `Email Address`, `Phone`, `Address` |
| Customer requests | `Timestamp`, `Order ID`, `Customer Name`, `Phone`, `Address`, `Order Type`, `Spice Level`, `Contact Method`, `Notes`, `Status`, `Email Sent` |
| Order lines | `Order ID`, `Item`, `Quantity`, `Price`, `Total` |
| Feedback | `Order ID`, `Customer Name`, and the four exact question titles in `examples/feedback.csv`. |

Preserve header spelling used in expressions. Connect a feedback form to the selected sheet and insert its actual link into the follow-up message.

## Configuration gaps

Findings below come from static inspection of the latest export. Original workflow JSON files are preserved unchanged.

| Area | Finding | Required setup or repair |
| --- | --- | --- |
| Replies | Several Telegram nodes use a fixed chat ID. | Bind replies to the incoming message or callback chat. |
| Memory | Both memory nodes use the same fixed session key. | Derive a separate key from each customer's chat ID. |
| Routing | The first Switch matches only `ORDER NOW`, `MENU`, and `Q/As`, without a fallback. | Route subsequent free text and food category callbacks to the correct agent. |
| Callback data | Expressions often read only `message.text` or `message.chat.id`. | Handle callback data and callback message chat IDs too. |
| Menu image | Telegram expects binary data; the HTTP node has no explicit file response option. | Configure file response and matching binary property. |
| Order writes | The append tool has an empty `columns.value` mapping. | Map confirmed items and totals; verify the tool name exposed to the agent. |
| Order confirmation | Operations follow the ordering agent's reply, without a deterministic confirmation gate. | Gate writes/notifications on a confirmed, validated order. |
| Lookups | Customer/order sheet reads have no order-specific filters. | Filter to the confirmed order and customer. |
| Update expression | `Update row in sheet` refers to missing `Google Sheets Trigger1`. | Bind the correct order source and check item linking. |
| Email copy | One email uses generic `{{order_id}}` and similar placeholders. | Replace with valid expressions from the correct record. |
| Completion | The delivery + `DONE` filter sits in the immediate ordering path. | Add a completion event/polling flow; a later status edit alone will not restart this branch. Prevent duplicate sends. |
| Feedback emails | All three reuse the new-order kitchen template. | Include real feedback, category, order context, and distinct recipients. |
| Classification | Single-negative instructions appear before the both-negative rule. | Prioritize the both-negative case; validate allowed outputs and add fallback routing. |
| Score meaning | The food question measures spiciness. | Use a satisfaction question or define the interpretation explicitly. |

Follow [DEMO.md](DEMO.md) after configuration. Use test accounts: executing nodes can send real messages and write real rows.
