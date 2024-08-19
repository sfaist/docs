---
title: 'Getting Started'
---

<Note>
  index is currently in closed beta. Please [reach out](https://calendar.app.google/jQVSUAfNheaqS9JE6
  ) if you want to get early access.
</Note>

index has a powerful API that enables you to build a product catalog with any product on the internet. Once you've signed up, you can start extracting and transforming your products. You can also use our [dashboard](https://dashboard.index-commerce.com) to manage your sources and data.

The API is designed as a private API that you call through your servers.
We do not provide a public API for clients to query directly at this time.

We provide a GraphQL endpoint that can be called with Postman, curl, and common GraphQL APIs. 
**Requests are handled via https://graphql.index-commerce.com.**

<CardGroup cols={2}>
<Card
  title="Extract"
  icon="ufo"
  href="/api-reference/extract"
>
  Learn how to extract product information from your catalog and the web
</Card>
<Card
  title="Sources"
  icon="cauldron"
  href="/api-reference/sources"
>
  Learn how to add and manage sources
</Card>
<Card
  title="Target Formats"
  icon="bullseye"
  href="/api-reference/mappings"
>
  Learn how to use target formats for mappings and validation
</Card>
<Card
  title="Mappings & Transformations"
  icon="map"
  href="/api-reference/mappings"
>
  Learn how to manage mappings and transformations
</Card>
<Card
  title="Jobs"
  icon="user-doctor"
  href="/api-reference/jobs"
>
  Learn how to run jobs to store products and keep them up to date
</Card>
<Card
  title="Search"
  icon="searchengin"
  href="/api-reference/search"
>
  Learn how to search for products within your sources
</Card>
</CardGroup>


## Authentication

All API endpoints are authenticated using Bearer tokens. This is specified as follows:

```shell
curl -H "Authorization: Bearer ${INDEXCOMMERCE_API_KEY}"
```

To get started with the index API, you'll need to obtain your authentication credentials. Please contact our support team to set up your account and receive your Bearer token.

## Next Steps

Explore our API reference to learn how to:

- Extract structured product information
- Create and apply data mappings
- Manage product sources
- Access and update your product database

By leveraging index, you can transform any platform into a fully-fledged commerce business in a matter of minutes.