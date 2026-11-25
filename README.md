# GA4 + GTM E-commerce Tracking for a Shopify Store

> **Status:** In progress. This repository is updated as the project moves forward.

End-to-end implementation of Google Analytics 4 e-commerce tracking via Google Tag Manager for **Reel Cozy**, a demo Shopify store built for this case study.

**Reel Cozy** is a fictional US brand selling themed movie night boxes, costume party accessories and sleepover essentials. The store, products and brand were created specifically to showcase a production-grade tracking setup. No real customers or client data are involved.

---

## The problem this project solves

Shopify store owners often face the same issues:

- GA4 purchase data doesn't match Shopify orders
- Key funnel steps (add to cart, checkout, payment) are missing or duplicated
- Nobody knows which channels and products actually drive revenue

This project shows how to plan, implement and QA a full e-commerce tracking setup that fixes all three.

## Progress

- [x] Demo Shopify store: products, variants, SKUs, collections, shipping rules, discount code, test payments
- [x] GA4 property and GTM container created and configured
- [x] Measurement plan: business goals, KPIs and event specification (14 events)
- [x] GTM installed on Shopify, GA4 base tag published
- [x] Storefront e-commerce events (6) via dataLayer and GTM, tested and published
- [ ] Checkout e-commerce events (4) via Shopify custom pixel
- [ ] Custom events
- [ ] QA and Shopify vs GA4 reconciliation
- [ ] Case study and video walkthrough

## Scope

| Area | Details |
| --- | --- |
| Measurement plan | Business goals → KPIs → event specification |
| E-commerce events | `view_item_list`, `select_item`, `view_item`, `add_to_cart`, `remove_from_cart`, `view_cart`, `begin_checkout`, `add_shipping_info`, `add_payment_info`, `purchase` |
| Custom events | `newsletter_signup`, `filter_used`, `scroll_depth` |
| Enhanced measurement | `view_search_results` (site search, used instead of a custom `search` event to avoid duplicates) |
| Shopify checkout | Tracked via Shopify Customer Events (custom pixel) |
| Data quality | Duplicate purchase protection, internal traffic filter, QA log |
| Reconciliation | Shopify orders vs GA4 purchases |

## How storefront tracking works

```
Shopify theme snippet → dataLayer → GTM (1 Custom Event trigger + 1 GA4 Event tag) → GA4
```

- A Liquid snippet in the theme (`gtm/theme-datalayer-snippets.liquid`) pushes GA4-formatted e-commerce events to the dataLayer.
- Cart events fire only after Shopify confirms the change, so failed requests (e.g. out of stock) are not tracked.
- In GTM, one Custom Event trigger (regex) and one GA4 Event tag with "Send Ecommerce data" handle all six storefront events.
- Every event was checked in the browser console, GTM Preview and GA4 DebugView.

## Issues found during QA

| Issue | Cause | Fix |
| --- | --- | --- |
| Two `page_view` hits per page | The theme updates the URL through the History API, and GA4 enhanced measurement counted it as a new page view | Turned off "Page changes based on browser history events" |
| `item_category` = "Home page" for one product | Shopify's hidden "Home page" collection was first in the product's collection list | Skip that collection and use the first real one |
| Empty `item_category` in cart events | The category lookup depended on pages the visitor had already seen | Build the lookup on the server for all products on every page |
| Two `view_cart` events per cart opening | The theme marks the cart dialog as open more than once | Fire only when the dialog changes from closed to open |

Full test cases are in the QA Log tab of the measurement plan.

## Tech stack

Shopify · Liquid · JavaScript · Google Tag Manager · Google Analytics 4 · Shopify Customer Events · Tag Assistant · GA4 DebugView · Google Sheets

## Repository structure

```
├── docs/           Measurement plan and QA log
├── gtm/            Theme dataLayer code, exported GTM container (JSON) and custom pixel code
├── screenshots/    Store, GTM Preview, DebugView and QA screenshots
└── case-study/     Full case study write-up
```

## Demo

- **Live store:** [reel-cozy.myshopify.com](https://reel-cozy.myshopify.com) (password: `reelcozy`; Shopify development stores are always password protected)
- **Measurement plan:** [Google Sheet (view only)](https://docs.google.com/spreadsheets/d/118zeVlSlUoL5wLcUmnNEzaojKMSldGNu5-D8vvUjH-g/edit?usp=sharing)
- **Video walkthrough:** coming soon

## Credits

- Product photos: [Unsplash](https://unsplash.com) and [Pexels](https://www.pexels.com) (free license)
- Movie Night Box images: AI-generated with Google Gemini

## Author

**Anastasiia Syvak** · Web & E-commerce Analytics (GA4, GTM, Looker Studio, BigQuery)
