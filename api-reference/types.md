---
title: 'GraphQL: API Types Reference'
---

This section details the various types used throughout the API, providing insight into the structure of the data you'll be working with.

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
}
```

- `id`: Unique identifier for the source
- `name`: Human-readable name for the source
- `type`: The type of source (e.g., Shopify, Algolia, Feed)
- `url`: The URL where the source data can be accessed
- `sourceHeaders`: Any HTTP headers required for accessing the source
- `sourceBody`: Request body, if needed (e.g., for POST requests)
- `sourceMethod`: HTTP method for accessing the source (e.g., GET, POST)

### MappingSchema
Represents a saved mapping configuration.

```graphql
type MappingSchema {
  id: ID!
  mapping: JSON!
}
```

- `id`: Unique identifier for the mapping
- `mapping`: The actual mapping configuration, stored as a JSON object

### TargetFormatSchema
Represents a saved target format configuration.

```graphql
type TargetFormatSchema {
  id: ID!
  targetFormat: JSON!
}
```

- `id`: Unique identifier for the target format
- `targetFormat`: The target format configuration, stored as a JSON object

### JobSchema
Represents a job that combines a source, mapping, and target format.

```graphql
type JobSchema {
  id: ID!
  sourceID: ID!
  mappingID: ID!
  targetSchemaID: ID!
}
```

- `id`: Unique identifier for the job
- `sourceID`: ID of the associated source
- `mappingID`: ID of the associated mapping
- `targetSchemaID`: ID of the associated target format

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

Fields are similar to `SourceSchema`, but used for input in mutations.

## Enums

### SourceType
Defines the possible types of data sources.

```graphql
enum SourceType {
  Request
  Index
  Feed
  Shopify
  Algolia
  Custom
}
```

### QueryType
Defines the possible types of queries for product retrieval.

```graphql
enum QueryType {
  Link
  Text
  GTIN
  ImageLink
}
```

## Scalars

### JSON
Represents a JSON object. Used extensively throughout the API for flexible data structures.

```graphql
scalar JSON
```

This scalar allows for the representation of complex, nested data structures in a single field. It's used for mappings, target formats, and other scenarios where a rigid structure isn't suitable.

## Usage Notes

1. The `SourceSchema` and `SourceRequest` types are closely related. `SourceRequest` is used when creating or updating a source, while `SourceSchema` is returned when querying existing sources.

2. The `MappingSchema` and `TargetFormatSchema` types use the `JSON` scalar for their main content. This allows for flexible and complex configurations.

3. The `JobSchema` type ties together the other main types (source, mapping, and target format) using their IDs. This allows for modular construction of data processing pipelines.

4. When working with these types, pay attention to which fields are required (marked with `!`) and which are optional.

5. The `SourceType` enum allows for specifying different types of data sources, each of which may have different requirements or behaviors in the system.

6. The `QueryType` enum is used specifically for product retrieval operations, allowing for different search methods.

These types form the backbone of the API's data model, enabling the creation, management, and execution of complex data transformation workflows.