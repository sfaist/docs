---
title: 'Target Formats'
---

Target Formats define the structure and validation rules for your product data. They ensure consistency and data quality across different sources and can be used for data validation and transformation.

## Target Format Schema

A Target Format is composed of fields, each with its own set of properties:

| Name | Type | Properties | Description |
|------|------|------------|-------------|
| `TargetFormatField` | Interface | `type`: 'string' \| 'number' \| 'boolean' \| 'array'<br/>`description?`: string<br/>`choices?`: string[]<br/>`attributes?`: TargetFieldAttribute[]<br/>`fields?`: TargetFormat | Defines the structure of a field in the target format |
| `TargetFieldAttribute` | Enum | `required`<br/>`capitalized`<br/>`wholeNumber`<br/>`positiveNumber`<br/>`decimalNumber`<br/>`email`<br/>`url`<br/>`phoneNumber`<br/>`date`<br/>`time`<br/>`datetime`<br/>`currency`<br/>`percentage`<br/>`primary`<br/>`indexProductID`<br/>`embedText`<br/>`embedImage` | Enum of possible attributes for a target field |

For each field, you can specify the type of data it will contain, along with any additional attributes or constraints.

| Type | Description | Default Value |
|------|-------------|---------------|
| `string` | Represents textual data. Can be any sequence of characters. | `""` (empty string) |
| `number` | Represents numeric data. Includes integers and floating-point numbers. | `0` |
| `boolean` | Represents a logical value. Can be either true or false. | `false` |
| `array` | Represents a list or collection of values. Can contain elements of any type. | `[]` (empty array) |

index provides a list of attributes that can be used to further customize the target field and specify formatting and validation rules.

| Attribute | Compatible Types | Transformation Explanation |
|-----------|------------------|----------------------------|
| `capitalized` | string, array | Transforms the first letter of each word to uppercase. E.g., "hello world" becomes "Hello World". |
| `wholeNumber` | number | Rounds down to the nearest integer. E.g., 3.7 becomes 3. |
| `positiveNumber` | number | Ensures the number is greater than 0. |
| `decimalNumber` | number | Ensures the value is parsed as a float with 2 decimal places. |
| `email` | string, array | Validates the string as an email address and normalizes format (lowercase, trim whitespace). |
| `url` | string, array | Validates and normalizes URL format, adds "https://" if missing. |
| `phoneNumber` | string, array | Formats phone number to a standard format, e.g., (xxx) xxx-xxxx in the US. |
| `date` | string, array | Parses various date formats into a standard date format: YYYY-MM-DD. |
| `time` | string, array | Parses various time formats into a standard time format: HH:MM:SS. |
| `datetime` | string, array | Combines date and time parsing, outputting a standard format: YYYY-MM-DDTHH:MM:SS. |
| `currency` | string, array | Converts string to 3 letter currency code. E.g. dollar, usd $ => USD. |
| `percentage` | string, array | Converts decimal to percentage or ensures correct format, e.g., 0.5 to 50% or 50 to 50%. Must be used with types string or array. |

index also specifies the following special attributes that are used to handle the indexing process:

| Attribute | Compatible Types | Transformation Explanation |
|-----------|------------------|----------------------------|
| `required` | string, number, boolean, array | Ensures a non-null, non-empty value is provided. If no value is provided, the field will be skipped during indexing. |
| `primary` | string, number, boolean, array | Defines the field as the primary index that is used to match during updates. This will be used to create the row id. You can define multiple fields as primary. During updates, if all of a new row's primary fields are equal to an existing row, it will update the existing row. It is best practice to apply primary to the indexProductId and the product link fields to get the best results. |
| `indexProductID` | string | Special flag that leverages the index product grouper. Fills the field with a unique and product id provided by index that is persistent across merchants and sources. |
| `embedText` | string, array | Uses the specified field to create an embedding from the field content. At least one `embedText` or `embedText` is required if you want to use semantic search. |
| `embedImage` | string, array | Uses the specified field to create an image embedding from the field content. Field content needs to be a URL that points to an image. At least one of the flags is required if you want to use semantic search. |

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