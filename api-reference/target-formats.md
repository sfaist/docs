---
title: 'Target Formats'
---

Target Formats define the structure and validation rules for your product data. They ensure consistency and data quality across different sources and can be used for data validation and transformation.

## Target Format Schema

A Target Format is composed of fields, each with its own set of properties:

Certainly! I'll update the table with the information you provided. Here's the revised table for the Target Format Schema:

| Name | Type | Properties | Description |
|------|------|------------|-------------|
| `TargetFormatSchema` | Interface | `id`: string<br>`targetFormat`: TargetFormat<br>`name?`: string<br>`jobs?`: JobSchema[]<br>`default?`: boolean | Defines the overall structure of a target format |
| `TargetFormat` | Interface | `[key: string]`: TargetFormatField | Defines the structure of the target format, with keys as field names and values as TargetFormatField objects |
| `TargetFormatField` | Interface | `type`: 'string' \| 'number' \| 'boolean' \| 'object'<br>`description?`: string<br>`choices?`: string[]<br>`attributes?`: TargetFieldAttribute[]<br>`fields?`: TargetFormat | Defines the structure of a field in the target format |
| `TargetFieldAttribute` | Enum | `required`<br>`capitalized`<br>`wholeNumber`<br>`positiveNumber`<br>`decimalNumber`<br>`email`<br>`url`<br>`phoneNumber`<br>`date`<br>`time`<br>`datetime`<br>`currency`<br>`percentage` | Enum of possible attributes for a target field |

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
          "Status": {
            "type": "string",
            "description": "Current status of the product (e.g., active, draft, archived)",
            "attributes": ["required"],
            "choices": ["active", "draft", "archived"]
          },
          "Vendor": {
            "type": "string",
            "description": "Name of the product supplier or manufacturer",
            "attributes": ["capitalized"]
          },
          "Gift Card": {
            "type": "boolean",
            "description": "Indicates if the product is a gift card (TRUE/FALSE)",
            "attributes": ["required"]
          },
          "Image Src": {
            "type": "string",
            "description": "URL of the product image",
            "attributes": ["url"]
          },
          "Published": {
            "type": "boolean",
            "description": "Indicates if the product is visible on the store (TRUE/FALSE)",
            "attributes": ["required"]
          },
          "SEO Title": {
            "type": "string",
            "description": "SEO-optimized title for the product page",
            "attributes": ["capitalized"]
          }
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