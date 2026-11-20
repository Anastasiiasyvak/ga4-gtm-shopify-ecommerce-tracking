# Docs

## Measurement plan

- **Live version:** [Google Sheet (view only)](https://docs.google.com/spreadsheets/d/118zeVlSlUoL5wLcUmnNEzaojKMSldGNu5-D8vvUjH-g/edit?usp=sharing)
- **Offline copy:** [Reel-Cozy-Measurement-Plan.xlsx](Reel-Cozy-Measurement-Plan.xlsx)

The measurement plan follows a simple logic: **business goal → KPI → event**. Every tracked event either feeds a KPI or is explained in the Notes column.

| Tab | What it holds |
| --- | --- |
| How to use | Tab overview, legend and naming conventions |
| Project Setup | Store URL, GA4 and GTM IDs and other project settings |
| Business Goals | 4 business goals with owner and priority |
| KPIs | 8 KPIs: definition, formula, data source, GA4 report and target |
| Event Spec | 14 events: trigger, parameters, example values, implementation, linked KPI |
| QA Log | Test cases and results (filled during QA) |

### Key decisions

- **Storefront events** are tracked with GTM; **checkout events** with a Shopify custom pixel, because Shopify checkout does not load theme code.
- **`value`** in `purchase` = items after discounts, excluding tax and shipping (sent separately as `tax` and `shipping`).
- **Funnel rates are measured per session**, not per event, because one visitor can add to cart several times.
- **Site search** uses GA4 enhanced measurement (`view_search_results`) instead of a custom `search` event to avoid duplicates.
- **Scroll tracking** uses a custom `scroll_depth` event (25/50/75/90%) instead of the enhanced measurement `scroll` event (90% only).
- **No personal data** (e.g. email addresses) is sent to GA4.
