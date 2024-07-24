---
title: 'Mappings and Transformations'
---

This document outlines the API operations available for managing and applying mappings and transformations to product data.

## Queries

### applyMapping
Applies a saved mapping to the provided data.

```graphql
applyMapping(data: JSON!, mappingId: ID): JSON!
```

Example:
```graphql
query {
  applyMapping(
    data: {
      "product_name": "Wireless Mouse",
      "product_price": "29.99",
      "brand": "TechGear",
      "in_stock": true
    },
    mappingId: "mapping123"
  )
}
```

### applyMappingWithDefinition
Applies a custom mapping to transform product data.

```graphql
applyMappingWithDefinition(data: JSON!, mapping: JSON!): JSON!
```

Example:
```graphql
query {
  applyMappingWithDefinition(
    data: {
      "product_name": "Wireless Mouse",
      "product_price": "29.99",
      "brand": "TechGear",
      "in_stock": true
    },
    mapping: [
      {
        "sourceField": "product_name",
        "targetField": "name"
        "transform": "value.capitalize()"
      },
      {
        "sourceField": "product_price",
        "targetField": "price",
      },
      {
        "sourceField": "brand",
        "targetField": "manufacturer"
      },
      {
        "sourceField": "in_stock",
        "targetField": "availability"
      }
    ]
  )
}
```

### generateMapping
Generates a mapping based on sample data and a desired target format.

```graphql
generateMapping(sampleData: [JSON!]!, targetFormat: JSON!): JSON!
```

Example:
```graphql
query {
  generateMapping(
    sampleData: [
      {
        "product_name": "Ergonomic Chair",
        "product_price": 199.99,
        "brand": "ComfortSeating",
        "in_stock": true
      }
    ],
    targetFormat: {
      "name": { "type": "string" },
      "price": { "type": "number" },
      "manufacturer": { "type": "string" },
      "availability": { "type": "boolean" }
    }
  )
}
```

### transform
Applies a transformation to the provided data using a saved target format.

```graphql
transform(data: JSON!, targetFormatID: ID!): JSON!
```

Example:
```graphql
query {
  transform(
    data: [
      {
        "product_name": "Wireless Mouse",
        "product_price": "29.99",
        "brand": "TechGear",
        "in_stock": true
      }
    ],
    targetFormatID: "format123"
  )
}
```

### transformWithDefinition
Applies a transformation to the provided data using a custom target format.

```graphql
transformWithDefinition(data: JSON!, targetFormat: JSON!): JSON!
```

Example:
```graphql
query {
  transformWithDefinition(
    data: [
      {
        "product_name": "Wireless Mouse",
        "product_price": "29.99",
        "brand": "TechGear",
        "in_stock": true
      }
    ],
    targetFormat: {
      "name": { "type": "string" },
      "price": { "type": "number" },
      "manufacturer": { "type": "string" },
      "availability": { "type": "boolean" }
    }
  )
}
```

### getMappings
Retrieves all saved mappings.

```graphql
getMappings: [MappingSchema!]
```

Example:
```graphql
query {
  getMappings {
    id
    mapping
  }
}
```

## Mutations

### saveMapping
Saves a new mapping.

```graphql
saveMapping(name: String!, mapping: JSON!): ID!
```

Example:
```graphql
mutation {
  saveMapping(
    name: "Shopify to Standard Format",
    mapping: [
      {
        "sourceField": "title",
        "targetField": "name",
        "transform": "value.capitalize()"
      },
      {
        "sourceField": "price",
        "targetField": "salePrice",
      },
      {
        "sourceField": "brand",
        "targetField": "manufacturer"
      }
    ]
  )
}
```

### deleteMapping
Deletes a saved mapping.

```graphql
deleteMapping(id: ID!): Boolean!
```

Example:
```graphql
mutation {
  deleteMapping(id: "mapping123")
}
```

## Advanced Features

### Complex Field Mapping 
   - Array selectors: e.g., `images[0]` to select the first image
   - Nested field access: e.g., `variants[0].price`
   - Constant values: e.g., `"'TRUE'"` to set a constant value

Example:
```graphql
query {
  applyMappingWithDefinition(
    data: {
      "product": {
        "title": "Gaming Laptop",
        "variants": [
          {
            "price": "999.99",
            "inventory_quantity": 5
          }
        ],
        "images": [
          "https://example.com/image1.jpg",
          "https://example.com/image2.jpg"
        ]
      }
    },
    mapping: [
      {
        "sourceField": "product.title",
        "targetField": "name"
      },
      {
        "sourceField": "product.variants[0].price",
        "targetField": "price",
      },
      {
        "sourceField": "product.images[0]",
        "targetField": "mainImage"
      },
      {
        "sourceField": "product.variants[0].inventory_quantity",
        "targetField": "inStock",
        "transform": "value > 0"
      }
    ]
  )
}
```

### Transformation Functions

Transformation functions allow you to modify or convert data as part of the mapping process. These functions can be applied to source fields before they are mapped to target fields. Here's a list of available transformation functions with examples:

- `capitalize()`: Capitalizes the first letter of each word.
  Example: `"product name" -> "Product Name"`

- `trim()`: Removes whitespace from both ends of a string.
  Example: `"  product  " -> "product"`

- `toNumber()`: Converts a string to a number.
  Example: `"123.45" -> 123.45`

- `toBoolean()`: Converts a value to a boolean.
  Example: `"true" -> true, "1" -> true, "false" -> false, "0" -> false`

- `toString()`: Converts a value to a string.
  Example: `123 -> "123"`

- `removeHTMLTags()`: Strips HTML tags from a string.
  Example: `"<p>Product description</p>" -> "Product description"`

- `convert(fromUnit, toUnit)`: Converts between units.
  Example: `convert("cm", "in")` applied to `"100" -> "39.37"`

- `split(separator, index)`: Splits a string and returns the specified index.
  Example: `split(",", "1")` applied to `"red,green,blue" -> "green"`

- `slice(start, end?)`: Returns a portion of an array or string.
  Example: `slice("1", "3")` applied to `"abcdef" -> "bcd"`

- `match(regex)`: Returns the first match of a regular expression.
  Example: `match("\\d+")` applied to `"Product123" -> "123"`

- `replace(search, replacement)`: Replaces all occurrences of a substring.
  Example: `replace("old", "new")` applied to `"old product" -> "new product"`

- `join(separator)`: Joins array elements into a string.
  Example: `join(", ")` applied to `["red", "green", "blue"] -> "red, green, blue"`

These functions can be used in the `transform` field of a mapping definition. For example:

```graphql
{
  "sourceField": "product.description",
  "targetField": "cleanDescription",
  "transform": "removeHTMLTags().trim().capitalize()"
}
```

This would remove HTML tags, trim whitespace, and capitalize the first letter of each word in the product description.

### Choice Mappings

Choice mappings allow you to map specific source values to predefined target values. This is particularly useful for standardizing categorical data or handling enumerations. Here's an expanded explanation with examples:

```graphql
{
  "sourceField": "product.status",
  "targetField": "productStatus",
  "choices": {
    "active": ["in_stock", "available", "1"],
    "inactive": ["out_of_stock", "unavailable", "0"],
    "discontinued": ["retired", "end_of_life"]
  }
}
```

In this example:
- If the source field contains "in_stock", "available", or "1", it will be mapped to "active" in the target field.
- If the source field contains "out_of_stock", "unavailable", or "0", it will be mapped to "inactive".
- If the source field contains "retired" or "end_of_life", it will be mapped to "discontinued".

You can also combine choice mappings with transformation functions:

```graphql
{
  "sourceField": "product.category",
  "targetField": "mainCategory",
  "transform": "trim().toLowerCase()",
  "choices": {
    "electronics": ["tech", "gadgets", "devices"],
    "clothing": ["apparel", "fashion", "wear"],
    "home": ["household", "interior", "domestic"]
  }
}
```

In this case, the source field is first trimmed and converted to lowercase before being matched against the choices.

Choice mappings can also handle more complex scenarios:

```graphql
{
  "sourceField": "product.attributes",
  "targetField": "productType",
  "transform": "join(', ').toLowerCase()",
  "choices": {
    "smartphone": ["mobile", "phone", "cellular"],
    "laptop": ["notebook", "computer", "pc"],
    "tablet": ["ipad", "slate", "pad"],
    "other": [""]
  }
}
```

This example assumes `product.attributes` is an array. It joins the array into a string, converts it to lowercase, and then matches against the choices to determine the `productType`.

By using these advanced features, you can create powerful and flexible mappings that can handle a wide variety of data transformation scenarios.