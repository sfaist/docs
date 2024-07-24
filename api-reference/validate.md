---
title: 'Product Validation'
---

The `validateProducts` function is used to validate an array of products against a specified target format. It returns an array of validation results for each product.

## Function Signature

```typescript
validateProducts(data: [JSON!]!, targetFormat: JSON!): [ValidationResult!]
```

### Parameters

- `data`: An array of JSON objects representing the products to be validated.
- `targetFormat`: A JSON object specifying the validation rules for each field.

### Return Value

An array of `ValidationResult` objects, each containing:

- `name`: The name of the product (String)
- `valid`: A boolean indicating whether the product passed all validations
- `errors`: An array of JSON objects containing any validation errors

## Usage

```typescript
const products = [
  { name: "Product 1", price: "10.99", category: "Electronics" },
  { name: "Product 2", price: "invalid", category: "" }
];

const targetFormat = {
  name: { attributes: ["required", "capitalized"] },
  price: { attributes: ["required", "positiveNumber"] },
  category: { attributes: ["required"] }
};

const results = await validateProducts(products, targetFormat);
```

## Validation Rules

The `targetFormat` object specifies the validation rules for each field. The following attributes are supported:

- `required`: Field must not be empty
- `capitalized`: Each word in the field must start with an uppercase letter
- `wholeNumber`: Field must be a whole number
- `positiveNumber`: Field must be a positive number
- `decimalNumber`: Field must be a decimal number
- `email`: Field must be a valid email address
- `url`: Field must be a valid URL
- `phoneNumber`: Field must be a valid phone number
- `datetime`: Field must be a valid datetime in the format "YYYY-MM-DDTHH:mm:ss"
- `currency`: Field must be a valid currency amount
- `percentage`: Field must be a valid percentage

## Examples

### Example 1: Basic Validation

```typescript
const products = [
  { name: "Laptop", price: "999.99", category: "Electronics" },
  { name: "smartphone", price: "-100", category: "" }
];

const targetFormat = {
  name: { attributes: ["required", "capitalized"] },
  price: { attributes: ["required", "positiveNumber"] },
  category: { attributes: ["required"] }
};

const results = await validateProducts(products, targetFormat);

console.log(results);
```

Expected output:

```javascript
[
  {
    name: "Laptop",
    valid: true,
    errors: []
  },
  {
    name: "smartphone",
    valid: false,
    errors: [
      { name: "name must be capitalized (each word's first letter should be uppercase)" },
      { price: "price must be a positive number" },
      { category: "category is required" }
    ]
  }
]
```

### Example 2: Advanced Validation

```typescript
const products = [
  {
    name: "Smart Watch",
    price: "199.99",
    email: "contact@smartwatch.com",
    releaseDate: "2023-05-15T10:00:00",
    discount: "10%"
  },
  {
    name: "Fitness tracker",
    price: "49.99",
    email: "invalid-email",
    releaseDate: "2023-05-15",
    discount: "5"
  }
];

const targetFormat = {
  name: { attributes: ["required", "capitalized"] },
  price: { attributes: ["required", "positiveNumber"] },
  email: { attributes: ["required", "email"] },
  releaseDate: { attributes: ["required", "datetime"] },
  discount: { attributes: ["required", "percentage"] }
};

const results = await validateProducts(products, targetFormat);

console.log(results);
```

Expected output:

```javascript
[
  {
    name: "Smart Watch",
    valid: true,
    errors: []
  },
  {
    name: "Fitness tracker",
    valid: false,
    errors: [
      { name: "name must be capitalized (each word's first letter should be uppercase)" },
      { email: "email must be a valid email" },
      { releaseDate: "releaseDate must be a valid datetime" },
      { discount: "discount must be a valid percentage" }
    ]
  }
]
```