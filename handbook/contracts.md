---
title: Contracts
sidebar: Handbook
showTitle: true
---

index is a SaaS product, and as such, we have a number of contracts with customers. These contracts are used to manage the license and support arrangements for our customers. 

## Managing billing

All index instances talk to a common external **Billing Service**. The Billing Service is the source of truth for product information, what plans are offered on those products, and feature entitlements on those plans. Our payment provider Stripe is the source of truth for customer information, invoices, and payments. The billing service communicates with Stripe to pull all the relevant information together before responding to customer requests.

### Payment method

Our strong preference is for customers to pay by credit card, as this is easier to manage in Stripe and has a lower risk of the customer forgetting to make the payment (which means we have to spend more time chasing). 

If a customer wants to pay by ACH or bank transfer, we will usually only consider this if they are paying for 1 year or more up front. This is more likely to be the case for very large customers. 

## Annual plans and more

For customers who want to sign up for an annual (or longer) plan there is some additional paperwork needed to capture their contractual commitment to a minimum term, and likely custom pricing as well. At a minimum, they should sign an Order Form which references our standard [terms](/terms) and [privacy notice](/privacy).  In addition, they may want a custom Master Services Agreement (MSA) or Data Processing Agreement (DPA).

> If a customer wants to vary either our DPA, BAA or MSA terms it is a substantial effort for our legal counsel to review these changes.  At a minimum we should only do this for contracts above $20k a year, and even higher if they are asking for big changes (e.g. adding Service Level Agreements).

### Discounts

We offer a pretty generous 25% discount for annual committed use plans. Any usage above this commited use plan will be billed at the standard monthly rate.

If a customer has an annual plan but doesn't _pay_ the whole year up front, we will halve the discount. Our general principle is that a customer should get a discount because the cash up front is beneficial to index, as it allows us to invest more in building more products, faster, not because we want to lock them in to a particular plan.

### Order Form

An Order Form is a lightweight document that captures the customer details, credit amount, discount, term, and signatures from both 
index and the Customer.  They are either governed by our standard terms or a custom MSA (see below).

You will likely need to use the [Pricing Calculator](https://docs.google.com/spreadsheets/d/1QsDV2ECtMwM9IfC_D7Embmpu7K7q6qbq60t8ARglQaI/edit#gid=358353731) to get the correct credit amount to be included in the order form.
