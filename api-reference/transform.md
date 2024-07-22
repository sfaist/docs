Certainly! Here's a more elaborate version with examples:

---
title: 'GraphQL: Transformations and Data Management'
---

This API provides comprehensive functionality for managing product data, including data transformation, source management, and format control. It allows you to create, apply, and manage mappings, sources, and target formats for flexible data processing.

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
      "title": "Ergonomic Office Chair",
      "price": 199.99,
      "brand": "ComfortSeating"
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
      "title": "Ergonomic Office Chair",
      "price": 199.99,
      "brand": "ComfortSeating"
    },
    mapping: [
      {
        "sourceField": "title",
        "targetField": "name",
        "transform": "value.trim()"
      },
      {
        "sourceField": "price",
        "targetField": "salePrice",
        "transform": "value.toNumber()"
      },
      {
        "sourceField": "brand",
        "targetField": "manufacturer"
      },
      {
        "sourceField": "category",
        "targetField": "productType",
        "choices": {
          "Electronics": ["laptop", "smartphone", "tablet"],
          "Home & Kitchen": ["appliance", "kitchenware", "furniture"],
          "Clothing": ["shirt", "pants", "dress"]
        }
      },
      {
        "sourceField": "description",
        "targetField": "shortDescription",
        "transform": "value.substring(0, 100) + '...'"
      },
      {
        "sourceField": "variants",
        "targetField": "inStock",
        "transform": "value.some(v => v.inventory_quantity > 0)"
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
        "productName": "Wireless Mouse",
        "cost": "29.99",
        "manufacturer": "TechGear"
      }
    ],
    targetFormat: {
      "name": { "type": "string" },
      "price": { "type": "number" },
      "brand": { "type": "string" }
    }
  )
}
```

### transform
Applies a transformation to the provided data using a target format.

```graphql
transform(data: JSON!, targetFormat: JSON!): JSON!
```

Example:
```graphql
query {
  transform(
    data: {
      "productName": "Wireless Mouse",
      "cost": "29.99",
      "manufacturer": "TechGear"
    },
    targetFormat: {
      "name": { "type": "string" },
      "price": { "type": "number" },
      "brand": { "type": "string" }
    }
  )
}
```

### getSources
Retrieves all registered sources.

```graphql
getSources: [SourceSchema!]
```

Example:
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

### getTargetFormats
Retrieves all saved target formats.

```graphql
getTargetFormats: [TargetFormatSchema!]
```

Example:
```graphql
query {
  getTargetFormats {
    id
    targetFormat
  }
}
```

## Mutations

### saveSource
Saves a new data source.

```graphql
saveSource(source: SourceRequest!, targetFormat: JSON!): ID!
```

Example:
```graphql
mutation {
  saveSource(
    source: {
      name: "My Shopify Store",
      type: Shopify,
      url: "https://mystore.myshopify.com/",
    },
    targetFormat: {
      "name": { "type": "string" },
      "price": { "type": "number" },
      "brand": { "type": "string" }
    }
  )
}
```

### deleteSource
Deletes a saved data source.

```graphql
deleteSource(id: ID!): Boolean!
```

Example:
```graphql
mutation {
  deleteSource(id: "source123")
}
```

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
        "transform": "value.trim()"
      },
      {
        "sourceField": "price",
        "targetField": "salePrice",
        "transform": "value.toNumber()"
      },
      {
        "sourceField": "brand",
        "targetField": "manufacturer"
      },
      {
        "sourceField": "category",
        "targetField": "productType",
        "choices": {
          "Electronics": ["laptop", "smartphone", "tablet"],
          "Home & Kitchen": ["appliance", "kitchenware", "furniture"],
          "Clothing": ["shirt", "pants", "dress"]
        }
      },
      {
        "sourceField": "description",
        "targetField": "shortDescription",
        "transform": "value.substring(0, 100) + '...'"
      },
      {
        "sourceField": "variants",
        "targetField": "inStock",
        "transform": "value.some(v => v.inventory_quantity > 0)"
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

### saveTargetFormat
Saves a new target format.

```graphql
saveTargetFormat(name: String!, targetFormat: JSON!): ID!
```

Example:
```graphql
mutation {
  saveTargetFormat(
    name: "Standard Product Format",
    targetFormat: {
      "name": { "type": "string" },
      "price": { "type": "number" },
      "brand": { "type": "string" },
      "category": { 
        "type": "choice",
        "choices": ["Electronics", "Furniture", "Clothing"]
      },
      "inStock": { "type": "boolean" }
    }
  )
}
```

### deleteTargetFormat
Deletes a saved target format.

```graphql
deleteTargetFormat(id: ID!): Boolean!
```

Example:
```graphql
mutation {
  deleteTargetFormat(id: "format123")
}
```

This API provides a robust set of tools for managing product data workflows. You can create and manage data sources, define mappings and target formats, and perform data transformations. The query operations allow you to retrieve saved entities and apply transformations, while the mutations enable you to create, update, and delete various components of your data pipeline.

Use these operations to build flexible and powerful data processing workflows, allowing you to standardize product data from various sources into consistent formats for your specific needs.