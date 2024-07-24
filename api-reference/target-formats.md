---
title: 'Target Formats'
---

Target Formats define the structure and validation rules for your product data. They ensure consistency and data quality across different sources and can be used for data validation and transformation.

## Target Format Schema

A Target Format is composed of fields, each with its own set of properties:

```typescript
interface TargetFormatField {
  type: 'string' | 'number' | 'boolean' | 'object';
  description?: string;
  choices?: string[];
  attributes?: TargetFieldAttribute[];
  fields?: TargetFormat;
}

enum TargetFieldAttribute {
  required,
  capitalized,
  wholeNumber,
  positiveNumber,
  decimalNumber,
  email,
  url,
  phoneNumber,
  date,
  time,
  datetime,
  currency,
  percentage,
}

interface TargetFormat {
  [key: string]: TargetFormatField;
}
```

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
  }
}
```

#### Response:
```json
{
  "data": {
    "getTargetFormats": [
      {
        "id": "format1",
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
        }
      }
    ]
  }
}
```

### Mutation: saveTargetFormat

Saves a new target format.

```graphql
saveTargetFormat(name: String!, targetFormat: JSON!): ID!
```

#### Example:
```graphql
mutation {
  saveTargetFormat(
    name: "Extended Product Format",
    targetFormat: {
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

## Validation Using Target Formats

Target Formats can be used to validate product data, ensuring it meets the specified structure and rules. For detailed information on product validation, please refer to the [validate.md](validate.md) document.