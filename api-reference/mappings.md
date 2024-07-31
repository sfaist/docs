---
title: 'Mappings and Transformations'
---

This document outlines the API operations available for managing and applying mappings and transformations to product data.

## Queries

### applyMapping
Applies a saved mapping to transform data from a source into a specified target format. Specify both the source data and the mapping configuration to be used.

```graphql
applyMapping(
  source: SourceInput!, 
  mapping: MappingInput!, 
  targetFormat: TargetFormatInput
): [JSON!]
```

If no target format is provided, the default target format will be used.

#### Example:
```graphql
query {
  applyMapping(
    source: {
      id: "source123",
      data: [
        {
          "product_name": "Wireless Mouse",
          "product_price": "29.99",
          "brand": "TechGear",
          "in_stock": true
        }
      ]
    },
    mapping: {
      id: "mapping123"
    },
    targetFormat: {
      id: "format123"
    }
  )
}
```

### generateMapping
Generates a mapping based on provided sample data and the desired target format. Use this to create a new mapping configuration by analyzing the structure and fields in the sample data.

```graphql
generateMapping(
  sampleData: SourceInput!, 
  targetFormat: TargetFormatInput
): [JSON!]
```

If no target format is provided, the default target format will be used.

#### Example:
```graphql
query {
  generateMapping(
    sampleData: {
      data: [
        {
          "product_name": "Ergonomic Chair",
          "product_price": 199.99,
          "brand": "ComfortSeating",
          "in_stock": true
        }
      ]
    },
    targetFormat: {
      data: {
        "name": { "type": "string" },
        "price": { "type": "number" },
        "manufacturer": { "type": "string" },
        "availability": { "type": "boolean" }
      }
    }
  )
}
```

## Additional Mapping Functions

### Complex Field Mapping
Use advanced features like array selectors, nested field access, and constant values to customize data mapping.

```graphql
query {
  applyMapping(
    source: {
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
      }
    },
    mapping: {
      data: [
        {
          "sourceField": "product.title",
          "targetField": "name"
        },
        {
          "sourceField": "product.variants[0].price",
          "targetField": "price"
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
    },
    targetFormat: {
      data: {
        "name": { "type": "string" },
        "price": { "type": "number" },
        "mainImage": { "type": "string" },
        "inStock": { "type": "boolean" }
      }
    }
  )
}
```

### Transformation Functions
Use transformation functions to modify data as part of the mapping process. These functions manipulate and convert field values, providing flexibility in how data is processed.

| Function         | Description                                               | Example                                      |
|------------------|-----------------------------------------------------------|----------------------------------------------|
| `capitalize()`   | Capitalizes the first letter of each word.                | `"product name" -> "Product Name"`           |
| `trim()`         | Removes whitespace from both ends of a string.            | `"  product  " -> "product"`                 |
| `toNumber()`     | Converts a string to a number.                            | `"123.45" -> 123.45`                         |
| `toBoolean()`    | Converts a value to a boolean.                            | `"true" -> true, "1" -> true, "false" -> false, "0" -> false` |
| `toString()`     | Converts a value to a string.                             | `123 -> "123"`                               |
| `removeHTMLTags()` | Strips HTML tags from a string.                         | `"<p>Product description</p>" -> "Product description"` |
| `convert(fromUnit, toUnit)` | Converts between units.                        | `convert("cm", "in")` applied to `"100" -> "39.37"` |
| `split(separator, index)` | Splits a string and returns the specified index. | `split(",", "1")` applied to `"red,green,blue" -> "green"` |
| `slice(start, end?)` | Returns a portion of an array or string.              | `slice("1", "3")` applied to `"abcdef" -> "bcd"` |
| `match(regex)`   | Returns the first match of a regular expression.          | `match("\\d+")` applied to `"Product123" -> "123"` |
| `replace(search, replacement)` | Replaces all occurrences of a substring.    | `replace("old", "new")` applied to `"old product" -> "new product"` |
| `join(separator)`| Joins array elements into a string.                       | `join(", ")` applied to `["red", "green", "blue"] -> "red, green, blue"` |

#### Example usage of transformation functions in a mapping:

```graphql
{
  "sourceField": "product.description",
  "targetField": "cleanDescription",
  "transform": "removeHTMLTags().trim().capitalize()"
}
```

### Choice Mappings
Use choice mappings to standardize and map specific source values to predefined target values, making it easier to handle categorical data or enumerations.

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

Combining choice mappings with transformation functions:

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

More complex example:

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