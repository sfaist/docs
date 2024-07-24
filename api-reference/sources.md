---
title: 'Data Sources'
---

Sources represent the origin of your product data. They can be various platforms or systems from which you retrieve product information, including affiliate networks.

## Types of Sources

Sources can include, but are not limited to:
- E-commerce platforms (e.g., Shopify, WooCommerce, Magento)
- Inventory management systems
- Spreadsheets or CSV files
- External APIs
- Databases
- Affiliate networks (e.g., Commission Junction, ShareASale, Awin)

## Source Operations

### Query: getSources

Retrieves all registered sources.

```graphql
getSources: [SourceSchema!]
```

#### Example:
```graphql
query {
  getSources {
    id
    name
    type
    url
    lastSyncDate
    status
  }
}
```

#### Response:
```json
{
  "data": {
    "getSources": [
      {
        "id": "12",
        "name": "My Shopify Store",
        "type": "SHOPIFY",
        "url": "https://mystore.myshopify.com/",
      },
      {
        "id": "13",
        "name": "CJ Affiliate Feed",
        "type": "CJ",
      }
    ]
  }
}
```

### Mutation: saveSource

Saves a new data source, including affiliate networks.

```graphql
saveSource(source: SourceRequest!, targetFormat: JSON!): ID!
```

#### Example for Shopify:
```graphql
mutation {
  saveSource(
    source: {
      name: "MrBeast Shopify Store",
      type: SHOPIFY,
      url: "https://mrbeast.shop/",
    }
  )
}
```

#### Example for CJ Affiliate:
```graphql
mutation {
  saveSource(
    source: {
      name: "CJ Affiliate Feed",
      type: CJ,
      credentials: { // Credentials will be stored securely and cannot be retrieved once saved
        developerKey: "YOUR_CJ_DEVELOPER_KEY", 
        websiteId: "YOUR_CJ_WEBSITE_ID"
      }
    }
  )
}
```

#### Example for ShareASale:
```graphql
mutation {
  saveSource(
    source: {
      name: "ShareASale Feed",
      type: SHAREASALE,
      url: "https://api.shareasale.com/x.cfm",
      credentials: {
        affiliateId: "YOUR_SHAREASALE_AFFILIATE_ID",
        token: "YOUR_SHAREASALE_API_TOKEN",
        secretKey: "YOUR_SHAREASALE_API_SECRET"
      },
    }
  )
}
```

#### Response:
```json
{
  "data": {
    "saveSource": "121" // ID of the newly created source
  }
}
```

### Mutation: deleteSource

Deletes a saved data source.

```graphql
deleteSource(id: ID!): Boolean!
```

#### Example:
```graphql
mutation {
  deleteSource(id: "121")
}
```

#### Response:
```json
{
  "data": {
    "deleteSource": true // whether the source was deleted successfully
  }
}
```