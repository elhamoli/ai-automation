# Demo walkthrough

Complete [SETUP.md](SETUP.md) first. This is a manual acceptance checklist, not a record of passed live tests.

| Scenario | Try | Expected after configuration |
| --- | --- | --- |
| Welcome | Send `/start`. | Greeting and menu/order/support choices. |
| Menu | Select `MENU`. | Menu photo in the same chat. |
| Support | Select `Q/As`, ask about hours. | Agent answers from configured restaurant facts. |
| Ordering | Select `ORDER NOW`, give a name, request one Volcano Chicken Bowl. | Guided conversation and 120 EGP total, matching the supplied prompt. |
| Confirmation | Decline, then try a separately confirmed order. | No write/notification on decline; one coherent order on confirmation. |
| Inline buttons | Select a food category. | Callback reaches the ordering agent; reply returns to the same chat. |
| Isolation | Use two chats with different customer names. | Separate memory and reply destinations. |
| Completion | Mark a demo delivery `DONE` in the configured completion flow. | One follow-up after the delay, without touching unrelated orders. |
| Positive | Food 9, delivery 9. | `POSITIVE`; no negative alert. |
| Kitchen | Food 4, delivery 9. | `NEGATIVE_KITCHEN`; kitchen receives feedback context. |
| Delivery | Food 9, delivery 4. | `NEGATIVE_DELIVERY`; delivery receives feedback context. |
| Combined | Food 4, delivery 4. | `NEGATIVE_BOTH`; manager receives feedback context. |
| Incomplete | Omit a rating. | Review/fallback handling, without fabricating a score. |

Inspect execution data at each boundary: trigger, route, agent, confirmation gate, sheet row, email. Verify recipient, order ID, item association, binary photo property, and duplicate prevention. Use fictional customers and hide account identifiers when recording a demo.
