---
title: 'Product Validation'
---

The `validateProducts` function is used to validate an array of products against a specified target format. It returns an array of validation results for each product.

## Function Signature

```graphql
validateProducts(data: [JSON!]!, targetFormat: TargetFormatInput): [ValidationResult!]
```

### Parameters

- `data`: An array of JSON objects representing the products to be validated.
- `targetFormat`: Specifies the structure to validate against, either as ID or object array, see [`TargetFormatInput`](./types#targetformatinput). If not specified, the default target format will be used.

### Return Value

An array of `ValidationResult` objects, one for each product, each containing:

- `valid`: A boolean indicating whether the product passed all validations
- `errors`: An array of JSON objects containing any validation errors

```graphql
type ValidationResult {
  valid: Boolean!
  errors: [JSON!]
}
```

## Usage

```graphql
query {
  validateProducts(
    data: [
      { name: "Product 1", price: 10.99, category: "Electronics" },
      { name: "Product 2", price: -5, category: "" }
    ],
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
        "Category": {
          "type": "string",
          "description": "Product category",
          "attributes": ["required"]
        }
      } 
    }
  )
}
```

## Validation Rules

The `targetFormat` object specifies the validation rules for each field. The following attributes are supported:

- `required`: Field must not be empty
- `capitalized`: Each word in the field must start with an uppercase letter
- `positiveNumber`: Field must be a positive number
- `currency`: Field must be a valid currency amount

Additional attributes may be available depending on the specific implementation.

## Examples

### Example 1: Basic Validation

```graphql
query {
  validateProducts(
    data: [
      { name: "Laptop", price: 999.99, category: "Electronics" },
      { name: "smartphone", price: -100, category: "" }
    ],
    targetFormat: {
      id: "basic-format",
      data: {
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
        "Category": {
          "type": "string",
          "description": "Product category",
          "attributes": ["required"]
        }
      }
    }
  )
}
```

Expected output:

```javascript
[
  {
    valid: true,
    errors: []
  },
  {
    valid: false,
    errors: [
      { name: "Name must be capitalized (each word's first letter should be uppercase)" },
      { price: "Price must be a positive number" },
      { category: "Category is required" }
    ]
  }
]
```

### Example 2: Advanced Validation

```graphql
query {
  validateProducts(
    data: [
      {
        name: "Smart Watch",
        price: 199.99,
        email: "contact@smartwatch.com",
        releaseDate: "2023-05-15T10:00:00",
        discount: 10
      },
      {
        name: "Fitness tracker",
        price: 49.99,
        email: "invalid-email",
        releaseDate: "2023-05-15",
        discount: 5
      }
    ],
    targetFormat: {
      id: "advanced-format",
      data: {
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
        "Email": {
          "type": "string",
          "description": "Contact email",
          "attributes": ["required", "email"]
        },
        "ReleaseDate": {
          "type": "string",
          "description": "Product release date",
          "attributes": ["required", "datetime"]
        },
        "Discount": {
          "type": "number",
          "description": "Product discount percentage",
          "attributes": ["required", "percentage"]
        }
      }
    }
  )
}
```

Expected output:

```javascript
[
  {
    valid: true,
    errors: []
  },
  {
    valid: false,
    errors: [
      { name: "Name must be capitalized (each word's first letter should be uppercase)" },
      { email: "Email must be a valid email address" },
      { releaseDate: "ReleaseDate must be a valid datetime in the format YYYY-MM-DDTHH:mm:ss" },
      { discount: "Discount must be a valid percentage" }
    ]
  }
]
```

This updated documentation reflects the new schema structure and provides examples using the GraphQL query format. It also references the TargetFormatInput type, which should be detailed in the Types documentation.