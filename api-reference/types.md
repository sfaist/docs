---
title: 'API Types Reference'
---

## Core Types

### SourceSchema
Represents a data source configuration.

```graphql
type SourceSchema {
  id: ID!
  name: String!
  type: SourceType!
  url: String!
  sourceHeaders: [String!]
  sourceBody: String
  sourceMethod: String
  jobs: [JobSchema!]
}
```

### MappingSchema
Represents a saved mapping configuration.

```graphql
type MappingSchema {
  id: ID!
  mapping: JSON!
  jobs: [JobSchema!]
}
```

### TargetFormatSchema
Represents a saved target format configuration.

```graphql
type TargetFormatSchema {
  id: ID!
  targetFormat: JSON!
  jobs: [JobSchema!]
}
```

### JobSchema
Represents a job that combines a source, mapping, and target format.

```graphql
type JobSchema {
  id: ID!
  status: String!
  sourceID: ID!
  mappingID: ID!
  targetSchemaID: ID!
  source: SourceSchema
  mapping: MappingSchema
  targetFormat: TargetFormatSchema
}
```

## Input Types

### SourceRequest
Used when creating or updating a source.

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

## Enums

### SourceType
Defines the possible types of data sources.

```graphql
enum SourceType {
  Request, Index, CJ, Awin, Impact.com, Partnerize, ShareASale, Rakuten, Shopify
}
```

### QueryType
Defines the possible types of queries for product retrieval.

```graphql
enum QueryType {
  Link, Text, GTIN, ImageLink
}
```

## Scalars

### JSON
Represents a JSON object. Used for flexible data structures.

```graphql
scalar JSON
```

## Additional Types

### ValidationResult
Represents the result of product validation.

```graphql
type ValidationResult {
  valid: Boolean!
  errors: [JSON!]
}
```

## Key Points

5. Use `SourceRequest` for creating/updating sources, and `SourceSchema` for querying existing sources.
6. `JSON` scalar is used for flexible configurations in mappings and target formats.
7. Required fields are marked with `!`. Optional fields don't have this marker.