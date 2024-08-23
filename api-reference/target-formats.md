---
title: 'Target Formats'
---


## Target Format Operations

### Query: getTargetFormats

Retrieves all saved target formats.

```graphql
getTargetFormats: [TargetFormatSchema!]
```

#### Example:
```graphql
query {
  getTargetFormats {
    id
    name
    targetFormat
    default
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
    "getTargetFormats": [
      {
        "id": "453",
        "name": "Standard Product Format",
        "targetFormat": {
          "Name": {
            "type": "string",
            "attributes": ["required", "capitalized", "primary"]
          },
          "Brand": {
            "type": "string",
            "attributes": ["required", "capitalized", "primary"]
          },
          "Link": {
            "type": "string",
            "attributes": ["required", "url", "primary"]
          },
          "Status": {
            "type": "string",
            "description": "Current status of the product (e.g., active, draft, archived)",
            "attributes": ["required"],
            "choices": ["active", "draft", "archived"]
          },
          "Gift Card": {
            "type": "boolean",
            "description": "Indicates if the product is a gift card (TRUE/FALSE)",
            "attributes": ["required"]
          },
          "Image Src": {
            "type": "array",
            "description": "URLs of the product images",
            "attributes": ["url"]
          },
        },
        "default": true,
        "jobs": [
          {
            "id": "job1",
            "status": "Live"
          }
        ]
      }
    ]
  }
}
```

### Mutation: saveTargetFormat

Saves a new target format.

```graphql
saveTargetFormat(name: String, targetFormat: [JSON!]!, makeDefault: Boolean): ID
```

#### Example:
```graphql
mutation {
  saveTargetFormat(
    name: "Extended Product Format",
    targetFormat: [
      {
        "Name": {
          "type": "string",
          "description": "Product name",
          "attributes": ["required", "capitalized"]
        },
        "Price": {
          "type": "number",
          "description": "Product price",
          "attributes": ["required", "positiveNumber", "currency"]
        },
        "Description": {
          "type": "string",
          "description": "Product description"
        },
        "Category": {
          "type": "string",
          "description": "Product category",
          "attributes": ["required"]
        },
        "Stock": {
          "type": "number",
          "description": "Current stock quantity",
          "attributes": ["required", "wholeNumber"]
        },
        "Rating": {
          "type": "number",
          "description": "Product rating (0-5)",
          "attributes": ["decimalNumber"]
        },
        "Launch Date": {
          "type": "string",
          "description": "Product launch date",
          "attributes": ["date"]
        }
      }
    ],
    makeDefault: false
  )
}
```

#### Response:
```json
{
  "data": {
    "saveTargetFormat": "54" // ID of the newly created target format
  }
}
```

### Mutation: deleteTargetFormat

Deletes a saved target format.

```graphql
deleteTargetFormat(id: ID!): Boolean!
```

#### Example:
```graphql
mutation {
  deleteTargetFormat(id: "54")
}
```

#### Response:
```json
{
  "data": {
    "deleteTargetFormat": true
  }
}
```

### Mutation: setDefaultTargetFormat

Sets a target format as the default.

```graphql
setDefaultTargetFormat(id: ID!): Boolean!
```

#### Example:
```graphql
mutation {
  setDefaultTargetFormat(id: "54")
}
```

#### Response:
```json
{
  "data": {
    "setDefaultTargetFormat": true // whether the target format was set as default successfully
  }
}
```

## Validation Using Target Formats

Target Formats can be used to validate product data, ensuring it meets the specified structure and rules. For detailed information on product validation, please refer to [Product Validation](./validate).