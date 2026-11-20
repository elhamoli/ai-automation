# RED DISH: from conversation to operations

## Project context

RED DISH is a fictional restaurant brand used to explore how conversational AI can connect customer interactions with restaurant operations. The creator's LinkedIn Projects section lists it as **RED DISH: Autonomous AI Restaurant Ecosystem**, dated March 2026.

The project description emphasizes a five-stage n8n pipeline, Telegram customer interaction, Google Sheets records, Gemini-assisted support, and feedback routing to kitchen, delivery, or management. Human involvement is intended for operational work and escalations.

Source: [Eslam Elhamouly — LinkedIn Projects](https://www.linkedin.com/in/eslam-elhamouly/). The public profile description was available during review; the individual post and its attached media were not retrieved. Descriptive claims on the profile are project context, not independent runtime verification.

## The operational problem

A chat-only ordering assistant leaves work outside the conversation: someone still needs to copy customer details, notify the kitchen, follow up after delivery, and read feedback. The RED DISH design explores connecting these steps in one workflow.

| Customer moment | Intended operational handoff |
| --- | --- |
| Discover the restaurant | Show a menu and offer ordering/support choices. |
| Choose a meal | Guide selection and request confirmation. |
| Confirm an order | Record line items and notify the kitchen. |
| Finish delivery | Request feedback after a delay. |
| Report a problem | Route the feedback category to the responsible team. |

## Design decisions visible in the export

**Separate agents for ordering and support.** Ordering has menu prices and confirmation instructions; the FAQ agent has restaurant facts and a different conversational role. Each has a memory attachment.

**A tool for structured action.** A Google Sheets append tool attaches to the ordering agent. Its field mappings need completion to turn a confirmed conversation into a usable record.

**Explicit operational branches.** Email and sheet update nodes make handoffs visible in the automation canvas. They still need order-specific filtering and correctly mapped data.

**A separate feedback entry point.** The feedback sheet trigger starts classification independently from the Telegram conversation. Four category labels provide a simple interface between the LLM and routing logic.

## Walkthrough example

This is an illustrative scenario based on the menu prompt, not a captured conversation or executed order.

1. A customer chooses **ORDER NOW** and asks for two Volcano Chicken Bowls and one Cola.
2. The prompt's listed prices imply `2 × 120 + 25 = 265 EGP`, before any delivery charge or tax policy, neither of which is defined in the export.
3. The assistant presents the summary and asks for explicit confirmation.
4. In a completed configuration, a confirmed-order gate allows the record and kitchen handoff. Declining must leave the order unsubmitted.
5. After an actual delivery-completion event, the follow-up flow waits 60 minutes before asking for feedback.
6. A food score of 9 and delivery score of 4 should route to `NEGATIVE_DELIVERY`, subject to the configured rating interpretation and model-output validation.

## What a live evaluation should measure

No measured results are provided in the repository. Useful future measures include correct route selection, valid order totals, duplicate writes, cross-customer memory isolation, correct alert recipients, and agreement between expected and model-generated feedback categories.

See [setup gaps](SETUP.md#configuration-gaps) and the [manual acceptance checklist](DEMO.md) before treating this prototype as an operational deployment.
