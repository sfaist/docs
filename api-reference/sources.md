---
title: 'Data Sources'
---

Sources represent the origin of your product data. They can be various platforms or systems from which you retrieve product information, including affiliate networks.

## Types of Sources

Sources can include, but are not limited to:
- E-commerce platforms (Shopify)
- Affiliate networks (CJ, Awin, Impact.com, Partnerize, ShareASale, Rakuten)
- Custom data feeds or APIs

The full list of supported source types is defined in the [`SourceType`](types#sourcetype) enum.

## Source Operations

### Query: getSources

Retrieves all registered sources.

```graphql
getSources: [SourceSchema!]
```

This query returns an array of [`SourceSchema`](types#sourceschema) objects.

#### Example:
```graphql
query {
  getSources {
    id
    name
    type
    url
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
        "type": "Shopify",
        "url": "https://mystore.myshopify.com/"
      },
      {
        "id": "13",
        "name": "CJ Affiliate Feed",
        "type": "CJ",
        "url": "https://api.cj.com"
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

This mutation takes a [`SourceRequest`](types#sourcerequest) input and returns the ID of the newly created source.

#### Supported Affiliate Networks
| Affiliate Network | Required Credentials | Variable Names |
|-------------------|----------------------|----------------|
| CJ | Access Token, Company ID | `accessToken`, `companyId` |
| Awin | Publisher ID, API Key | `publisherId`, `apiKey` |
| Impact.com | Account SID, Auth Token | `accountSid`, `authToken` |
| Partnerize | API Key, Publisher ID | `apiKey`, `publisherId` |
| ShareASale | API Token, API Secret, Affiliate ID | `apiToken`, `apiSecret`, `affiliateId` |
| Rakuten | Security Key, Tracking ID | `securityKey`, `trackingId` |

#### Example for Shopify:
```graphql
mutation {
  saveSource(
    source: {
      name: "MrBeast Shopify Store",
      type: Shopify,
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
      url: "https://api.cj.com",
      credentials: { 
        accessToken: "YOUR_CJ_ACCESS_TOKEN", 
        companyId: "YOUR_CJ_COMPANY_ID" 
      } # Credentials will be stored securely and cannot be retrieved once save
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

## Note on Credentials

For security reasons, sensitive credentials (like API keys) should be passed in the `credentials` field of the `SourceRequest`. These will be stored securely and cannot be retrieved once saved. 