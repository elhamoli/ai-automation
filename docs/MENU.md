# The RED DISH demo menu

These items and prices are taken from the `Order Flow & Chatbot` prompt in `RED DISH last.json`. They describe a fictional restaurant; this is not a live price feed.

| Category | Item | Price (EGP) |
| --- | --- | ---: |
| Signature Dishes | Volcano Chicken Bowl | 120 |
| Signature Dishes | Inferno Beef Wrap | 135 |
| Signature Dishes | Red Fire Wings | 95 |
| Signature Dishes | Spicy Chicken Sandwich | 110 |
| Challenge Meals | Dragon Fire Burger | 145 |
| Challenge Meals | Hell Bowl | 140 |
| Challenge Meals | Red Dish Challenge Plate | 160 |
| Sides | Spicy Fries | 45 |
| Sides | Loaded Fire Fries | 65 |
| Sides | Jalapeño Cheese Bites | 70 |
| Sides | Onion Rings | 40 |
| Drinks | Cola | 25 |
| Drinks | Lemon Mint | 35 |
| Drinks | Ice Tea | 35 |
| Drinks | Milkshake | 45 |

The FAQ prompt places the fictional restaurant in Shebeen El-Kom, Menofia, with hours from 12:00 PM to 12:00 AM. Change these facts in the prompts when adapting the workflow.

## Original image dependency

The HTTP node points to an externally hosted `RED-DISH-MENU.png`. During this review that endpoint returned HTTP 503, so the original image could not be added to the repository. That dependency may also stop the menu branch during a demo. Replace it with a reachable image URL you control and configure binary output as described in [SETUP.md](SETUP.md).

This page preserves the menu information independently of that image host. It does not claim to reproduce the original menu artwork or LinkedIn post images.
