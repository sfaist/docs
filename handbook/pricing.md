---
title: Pricing
sidebar: Handbook
showTitle: true
---

Our pricing is simple and transparent, based on the number of SKUs processed each month. Processed means that an SKU is fetched from a source and transformed. We do not charge for queries on your catalog or affiliate purchases and offer a generous free tier to help you get started. 

## Why do we price based on usage?

Really it comes down to this: The more you use index, the more value you get, and the more it costs us to process and store your data. Thus, we charge based on usage.

## Free Tier

* First 50,000 processed SKUs from Product Feeds \& Files per month: free
* First 50,000 processed SKUs from Shopify, Index, Requests, APIs: free
* First 100 processed SKUs from Websites: free

## SKU Processing Rates (After Free Tier)

| Source | Price per 100,000 processed SKUs |
|--------|----------------------|
| Product Feeds \& Files  | $1.00        |
| Shopify, Index, Requests, APIs    | $2.00              |
| Websites    | $200.00              |

## How to estimate your usage

Let's say you have a catalog with 150,000 SKUs, processed every day for 30 days.
* 100,000 SKUs from feeds (processed once every day for 30 days)
* 50,000 SKUs from other sources (processed once every day for 30 days)

Your bill would be:
* Feeds: (100,000 * 30 - 50,000) / 100,000 * \$1,00 \= \$29.50
* Shopify: (50,000 * 30 - 50,000) / 100,000 * \$1,00 \= \$14.50 
* Total: \$44.00

For reference, most major retailers have a catalog with 1,000 to 10,000 SKUs.

## Enterprise

We offer yearly plans with committed use discounts and support agreements for enterprise customers. You can find more details on the [Contracts](./contracts) page.

## Benefits

* No cost or quota for queries, searches, and affiliate purchases
* Generous free tier for small businesses and startups
* No hidden fees
* Pay only for what you use beyond the free tier
* Scales with your business
* Transparent and predictable pricing

## Managing billing

All index instances talk to a common external **Billing Service**. The Billing Service is the source of truth for product information, what plans are offered on those products, and feature entitlements on those plans. Our payment provider Stripe is the source of truth for customer information, invoices, and payments. The billing service communicates with Stripe to pull all the relevant information together before responding to customer requests.
