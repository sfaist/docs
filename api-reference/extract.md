---
title: 'Product Fetching'
---



### getFromSource

Retrieve products from a specified source, applying a target format to the results.

```graphql
getFromSource(source: SourceInput!, targetFormat: TargetFormatInput): [JSON!]
```

Parameters:
- `source`: Defines the data source to fetch products from, see [`SourceInput`](./types#sourceinput).
- `targetFormat`: Specifies the structure of the returned data, either as ID or object array, see [`TargetFormatInput`](./types#targetformatinput). If not specified, the default target format will be used.

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

### getProductPreview

Preview product data from a specified source without applying any transformations.

```graphql
getProductPreview(source: SourceInput!): [JSON!]
```

Parameters:
- `source`: Defines the data source to fetch the product preview from.

For details on the input type, see [`SourceInput`](./types#sourceinput).

#### Example:

```graphql
query {
  getProductPreview(
    source: {
      sourceRequest: {
        name: "My Custom Feed",
        type: Request,
        url: "https://mycustomfeed.com/products"
      }
    }
  )
}
```