Thank you for providing the correct structure for the targetFormat. I'll update the documentation with this information and reprint the updated version.

--- title: 'Product Extraction' ---

This API allows you to extract product information in a structured way from various sources. It provides flexible querying options to fetch single or multiple products and preview data from sources.

## Key Queries

### getProduct

Fetch a single product based on a specified query and type.

```graphql
getProduct(query: String!, type: QueryType!, targetFormat: TargetFormatInput): JSON
```

Parameters:
- `query`: The search term or identifier for the product.
- `type`: The type of query (Link, Text, GTIN, or ImageLink).
- `targetFormat`: Specifies the structure of the returned data, either as ID or object array, see [TargetFormatInput](./types#targetformatinput). If not specified, the default target format will be used.

#### Example:
```graphql
query {
  getProduct(
    query: "B08F7N4LF8",
    type: GTIN,
    targetFormat: {
      id: "4545", # use an ID from the saved target format
      data: {     # alternatively, submit the target format as a JSON object
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
        }
      }
    }
  )
}
```

This query allows you to search for a product using various identifiers. The `targetFormat` parameter lets you define the structure of the returned data, ensuring you get exactly the fields you need in the format you specify.

### findInCatalog

Search for products in the catalog based on a specified query and type.

```graphql
findInCatalog(query: String!, type: QueryType!): [JSON!]
```

Parameters:
- `query`: The search term or identifier for the products.
- `type`: The type of query (Link, Text, GTIN, or ImageLink).

#### Example:
```graphql
query {
  findInCatalog(
    query: "red shoes",
    type: Text
  )
}
```

This query searches the catalog for products matching the given query and returns an array of JSON objects representing the found products.

### getFromSource

Retrieve products from a specified source, applying a target format to the results.

```graphql
getFromSource(source: SourceInput!, targetFormat: TargetFormatInput): [JSON!]
```

Parameters:
- `source`: Defines the data source to fetch products from.
- `targetFormat`: Specifies the structure of the returned data, either as ID or object array, see [TargetFormatInput](./types#targetformatinput). If not specified, the default target format will be used.

For details on input types, see [SourceInput](./types#sourceinput) and [TargetFormatInput](./types#targetformatinput).

#### Example:
```graphql
query {
  getFromSource(
    source: {
      sourceRequest: {
        name: "My Shopify Store",
        type: Shopify,
        url: "https://mystore.myshopify.com"
      }
    },
    targetFormat: {
      id: "4545", # use an ID from the saved target format
      data: {     # alternatively, submit the target format as a JSON object
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
        }
      }
    }
  )
}
```

This query is powerful for bulk extraction of product data. You can specify various source types and define exactly how you want the product data structured in the response. This makes it easy to integrate with different e-commerce platforms or data sources.

### getProductPreview

Preview product data from a specified source without applying any transformations.

```graphql
getProductPreview(source: SourceInput!): [JSON!]
```

Parameters:
- `source`: Defines the data source to fetch the product preview from.

For details on the input type, see [SourceInput](./types#sourceinput).

Example:
```graphql
query {
  getProductPreview(
    source: {
      sourceRequest: {
        name: "My Custom Feed",
        type: Feed,
        url: "https://mycustomfeed.com/products"
      }
    }
  )
}
```

This query is useful for inspecting the raw data from a source before applying any mappings or transformations. It helps in understanding the structure of the source data, which can be valuable when setting up mappings or defining target formats.

