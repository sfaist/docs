---
title: Pricing
sidebar: Handbook
showTitle: true
---

Our pricing is simple and transparent, based on the volume of data processed each month. Processing means that data is fetched from a source, transformed, and stored. We do not charge for queries on your catalog or affiliate purchases, and we offer a generous free tier to help you get started.

## Why do we price based on usage?

Really it comes down to this: The more you use the platform, the more value you get, and the more it costs us to process and store your data. Thus, we charge based on usage.

## Processing Rates (After Free Tier)

| Tier | Monthly Processed Volume (M) | Price per Million Records |
|------|------------------------------|---------------------------|
| Free | 0 - 1 M                       | $0                        |
| S    | 1 - 50 M                      | $10                       |
| M    | 50 - 500 M                    | $5                        |
| L    | 500 - 5000 M                  | $3                        |
| XL   | Above 5000 M                  | Contact us                |

## How to estimate your usage

Let's say you have a catalog with 10 Feeds and 10,000 products with 5 variants each, processed every day.
For reference, most retailers have a catalog with 5,000 to 20,000 SKUs and 1-5 variants per product.

<CardGroup cols={3}>
  <Card title="Starting out" icon="square-1">
    * Feeds: 10
    * Products per feed: 10,000
    * Variants: 3
    * Runs per month: 30
    * Processed per month: 9 M

    **Monthly Cost: $80.00**
  </Card>
  <Card title="Large catalog" icon="square-2">
    * Feeds: 100
    * Products per feed: 30,000
    * Variants: 5
    * Runs per month: 30
    * Processed per month: 450 M

    **Monthly Cost: $2,500**
  </Card>
  <Card title="Very large catalog" icon="square-3">
    * Feeds: 500
    * Products per feed: 50,000
    * Variants: 5
    * Runs per month: 30
    * Processed per month: 3,750 M

    **Cost: $12,750.00**
  </Card>
</CardGroup>

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

All instances communicate with a central **Billing Service**. The Billing Service manages product information, plans, and feature entitlements. Our payment provider Stripe handles customer information, invoices, and payments. The billing service synchronizes with Stripe to ensure all information is accurate before responding to customer requests.