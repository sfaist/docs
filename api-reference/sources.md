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
    description
    sourceHeaders
    sourceBody
    sourceMethod
    jobs {
      id
      status
    }
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
        "url": "https://mystore.myshopify.com/",
        "jobs": [
          {
            "id": "543",
            "status": "Live"
          }
        ]
      },
      {
        "id": "13",
        "name": "Vestiare Collective",
        "type": "CJ",
        "description": "US Feed, 4564341 products",
        "jobs": []
      },
      {
        "id": "14",
        "name": "Ruggable Google Feed",
        "type": "Request",
        "url": "https://ruggable.com/feed/google",
        "sourceHeaders": ["Authorization: Bearer ${RUGGABLE_API_KEY}"], // the RUGGABLE_API_KEY credential was submitted in the credentials field on saving and cannot be retrieved 
        "jobs": []
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
| CJ | [Access Token](https://developers.cj.com/account/personal-access-tokens), [Company ID (CID)](https://profile.cj.com/manage-accounts) | `accessToken`, `companyId` |
| Awin | [API Key](https://ui.awin.com/user/settings/accounts) | `apiKey` |
| ShareASale | API Token, API Secret, Affiliate ID | `apiToken`, `apiSecret`, `affiliateId` |
| ClickBank | API Key | `apiKey` |
| FlexOffers | API Token | `apiToken` |
| Rakuten | Security Key, Tracking ID | `securityKey`, `trackingId` |
| Impact.com | Account SID, Auth Token | `accountSid`, `authToken` |

#### Example for Shopify:

Check [built-with](https://builtwith.com/) to validate whether a shop is using Shopify.

```graphql
mutation {
  saveSource(
    source: {
      name: "MrBeast Shopify Store",
      type: Shopify,
      url: "https://mrbeast.shop/", # use the shop url
    }
  )
}
```

#### Example for CJ Affiliate:
```graphql
mutation {
  saveSource(
    source: {
      name: "Vestiare Collective on CJ",
      type: CJ,
      credentials: { 
        accessToken: "YOUR_CJ_ACCESS_TOKEN", 
        companyId: "YOUR_CJ_COMPANY_ID" 
      } # Credentials will be stored securely and cannot be retrieved once saved.
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
Credentials can be referenced in the 'url', 'sourceHeaders', and 'sourceBody' fields of the `SourceRequest` object with the \`${CREDENTIAL_NAME}\` syntax.

#### Example:

```graphql
url: "https://example.com/api/v1/${CREDENTIAL_NAME}"
sourceHeaders: ["Authorization: Bearer ${CREDENTIAL_NAME}"]
sourceBody: { "apiKey": "${CREDENTIAL_NAME}" }
```

The `CREDENTIAL_NAME` will be replaced with the decrypted value of the credential when the source is executed.