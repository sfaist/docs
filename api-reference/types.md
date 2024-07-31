---
title: 'API Types Reference'
---

## Core Types

### SourceSchema
Represents a data source configuration.

```graphql
type SourceSchema {
  id: ID!
  name: String
  type: SourceType!
  url: String
  description: String
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
  name: String
  mapping: JSON!
  jobs: [JobSchema!]
}
```

### TargetFormatSchema
Represents a saved target format configuration.

```graphql
type TargetFormatSchema {
  id: ID!
  name: String
  targetFormat: JSON!
  jobs: [JobSchema!]
  default: Boolean!
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
  lastRun: String
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
  name: String
  type: SourceType!
  url: String
  credentials: JSON
  description: String
  sourceHeaders: [String!]
  sourceBody: String
  sourceMethod: String
}
```

### SourceInput
Specifies data for interacting with sources. Users can either provide an `id` to reference an existing source or provide a `sourceRequest` object to define a new source.

```graphql
input SourceInput {
  id: ID
  sourceRequest: SourceRequest
  data: [JSON!]
}
```

### MappingInput
Used for specifying mappings. Users can provide an `id` to reference an existing mapping or provide data as a list of JSON objects.

```graphql
input MappingInput {
  id: ID
  data: [JSON!]
}
```

### TargetFormatInput
Defines the target format for data operations. Users can either reference an existing format using an `id` or provide the format as a JSON object.

```graphql
input TargetFormatInput {
  id: ID
  data: JSON
}
```

## Enums

### SourceType
Defines the possible types of data sources.

```graphql
enum SourceType {
  Request, # a url request
  Index, # an index commerce source specified by name
  Shopify, # a shopify source specified by shop url
  CJ,
  Awin, 
  Impact_com, 
  ShareASale, 
  Rakuten, 
  Shopify,
  ClickBank,
  FlexOffers
}
```

Refer to the [Data Sources](./sources) documentation for more information on using each data source type.

### QueryType

Used in various queries to specify the type of query:

- `Link`: Search by product URL.
- `Text`: Search by text (e.g., product name).
- `GTIN`: Search by Global Trade Item Number.
- `ImageLink`: Search by image URL.

Refer to the [Product Extraction](./extract) documentation for more information on using each query type.