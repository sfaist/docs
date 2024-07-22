---
title: 'GraphQL: Product Extraction API'
---

This API allows you to extract product information in a structured way from various sources. It provides flexible querying options to fetch single or multiple products and preview data from sources.

## Key Queries

### getProduct

Fetch a single product based on a specified query and type.

```graphql
getProduct(query: String!, targetFormat: JSON!, type: QueryType): JSON
```

Parameters:
- `query`: The search term or identifier for the product.
- `targetFormat`: Specifies the structure of the returned data.
- `type`: The type of query (Link, Text, GTIN, or ImageLink).

Example:

```graphql
query {
  getProduct(
    query: "B08F7N4LF8",
    targetFormat: {
      "name": "string",
      "price": "number",
      "description": "string",
      "brand": "string"
    },
    type: GTIN
  )
}
```

This query allows you to search for a product using various identifiers like GTIN, product URL, or even image links. The `targetFormat` parameter lets you define the structure of the returned data, ensuring you get exactly the fields you need in the format you specify.

### getAllProducts

Retrieve multiple products from a specified source, applying a target format to the results.

```graphql
getAllProducts(source: SourceRequest!, targetFormat: JSON!): [JSON!]
```

Parameters:
- `source`: Defines the data source to fetch products from.
- `targetFormat`: Specifies the structure of the returned data for each product.

Example:

```graphql
query {
  getAllProducts(
    source: {
      name: "My Shopify Store",
      type: Shopify,
      url: "https://mystore.myshopify.com"
    },
    targetFormat: {
      "id": "string",
      "name": "string",
      "price": "number",
      "category": "string",
      "inStock": "boolean"
    }
  )
}
```

This query is powerful for bulk extraction of product data. You can specify various source types (like Shopify, Algolia, or custom feeds) and define exactly how you want the product data structured in the response. This makes it easy to integrate with different e-commerce platforms or data sources.

### getProductPreview

Preview product data from a specified source without applying any transformations.

```graphql
getProductPreview(source: SourceRequest!): [JSON!]
```

Parameters:
- `source`: Defines the data source to fetch the product preview from.

Example:

```graphql
query {
  getProductPreview(
    source: {
      name: "My Custom Feed",
      type: Feed,
      url: "https://mycustomfeed.com/products"
    }
  )
}
```

This query is useful for inspecting the raw data from a source before applying any mappings or transformations. It helps in understanding the structure of the source data, which can be valuable when setting up mappings or defining target formats.

## Common Parameters

### SourceRequest

Used in `getAllProducts` and `getProductPreview` to specify the data source:

```graphql
input SourceRequest {
  name: String!
  type: SourceType!
  url: String!
  sourceHeaders: [String!]
  sourceBody: String
  sourceMethod: String
}
```

- `name`: A name for the source (e.g., "My Shopify Store").
- `type`: The type of source (e.g., Shopify, Algolia, Feed).
- `url`: The URL of the data source.
- `sourceHeaders`: Any headers required for the request.
- `sourceBody`: Request body, if needed.
- `sourceMethod`: HTTP method for the request.

### QueryType

Used in `getProduct` to specify the type of query:

- `Link`: Search by product URL.
- `Text`: Search by text (e.g., product name).
- `GTIN`: Search by Global Trade Item Number.
- `ImageLink`: Search by image URL.

This API provides a flexible and powerful way to extract product information from various sources, allowing you to specify exactly what data you need and how you want it structured. It's designed to integrate easily with different e-commerce platforms and data sources, making it a versatile tool for product data extraction and transformation.