---
title: 'Mappings and Transformations'
---

Certainly. Here's a comprehensive markdown document that covers mappings and transformations:

# Mappings and Transformations

This document outlines the API operations available for managing and applying mappings and transformations to product data.

## Table of Contents
1. [Queries](#queries)
2. [Mutations](#mutations)
3. [Advanced Features](#advanced-features)
4. [Best Practices](#best-practices)

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

1. **Complex Field Mapping**: 
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

2. **Transformation Functions**: A set of predefined transformation functions are available for use in mappings, such as `toNumber()`, `trim()`, `replace()`, etc.

3. **Choice Mappings**: For fields with predefined choices, mappings can specify how source values should be mapped to target choices.

4. **Dynamic Mapping Generation**: The `generateMapping` query uses AI to automatically create mappings based on sample data and a target format.

5. **User-Specific Mappings**: Mappings are associated with specific users, allowing for personalized data transformation workflows.