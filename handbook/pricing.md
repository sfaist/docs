---
title: Pricing
sidebar: Handbook
showTitle: true
---

Our pricing is simple and transparent, based on the number of SKUs in your catalog on average each month. We offer a generous free tier to help you get started.

It can be daunting to figure out how much a usage-based platform like index will cost, but there are simple ways to estimate your usage and costs.

## Why do we price based on usage?

Really it comes down to this: The more you use index, the more value you get, and the more it costs us to process and store your data. Thus, we charge based on usage.

## Free Tier

* First 2,000 SKUs per month: free
* Includes both feed and other sources

## SKU Processing Rates (After Free Tier)

| Source | Price per 1,000 SKUs |
|--------|----------------------|
| Feeds  | $0.50                |
| Other  | $1.00                |

### Feed Sources

Process SKUs from feed and file sources at our most affordable rate:**$0.50 per 1,000 SKUs per month**

### Other Sources

For SKUs from other sources such as Shopify, APIs, and other sources: **$1.00 per 1,000 SKUs per month**

## How It Works

1. Your first 2,000 SKUs each month are always free.
2. We count the number of unique SKUs processed beyond the free tier.
3. SKUs are categorized by source (feeds or other).
4. Your monthly bill is calculated based on the volume in each category, after the free tier.

## How to estimate your usage

Let's say you have a catalog with 100,000 SKUs.
* 100,000 SKUs from feeds
* 50,000 SKUs from other sources

Your bill would be:
* Free tier: First 2,000 SKUs
* Feeds: (99,000 / 1,000) * \$0.50 \= \$49.50
* Other: (49,000 / 1,000) * \$1.00 \= \$49
* Total: \$99.50

For reference, most major retailers have a catalog with 1,000 to 10,000 SKUs.

## Enterprise

We offer yearly plans and support agreements for enterprise customers. You can find more details on the [Contracts](./contracts) page.

## Benefits

* Generous free tier for small businesses and startups
* No hidden fees
* Pay only for what you use beyond the free tier
* Scales with your business
* Transparent and predictable pricing

## Managing billing

All index instances talk to a common external **Billing Service**. The Billing Service is the source of truth for product information, what plans are offered on those products, and feature entitlements on those plans. Our payment provider Stripe is the source of truth for customer information, invoices, and payments. The billing service communicates with Stripe to pull all the relevant information together before responding to customer requests.
