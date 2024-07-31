---
title: 'Job Management'
---

This section of the API focuses on managing and executing jobs. Jobs are a way to combine a source, a mapping, and a target format into a reusable workflow for data transformation that is executed every 24 hours.

## Job Schema

The [`JobSchema`](./types#jobschema) type includes the following fields:

- `id`: Unique identifier for the job
- `status`: Current status of the job
- `sourceID`: ID of the associated source
- `mappingID`: ID of the associated mapping
- `targetSchemaID`: ID of the associated target format
- `lastRun`: Timestamp of the last execution
- `source`: Associated [`SourceSchema`](./types#sourceschema) object, this triggers an additional query
- `mapping`: Associated [`MappingSchema`](./types#mappingschema) object, this triggers an additional query to fetch
- `targetFormat`: Associated [`TargetFormatSchema`](./types#targetformatschema) object, this triggers an additional query to fetch

## Queries

### getJobs
Retrieves all saved jobs.

```graphql
getJobs: [JobSchema!]
```

#### Example:
```graphql
query {
  getJobs {
    id
    status
    sourceID
    mappingID
    targetSchemaID
    lastRun
    source {
      name
      type
    }
    mapping {
      name
    }
    targetFormat {
      name
    }
  }
}
```

This query returns an array of all saved jobs, including details about their associated [`source`](./types#sourceinput), [`mapping`](./types#mappinginput), and [`target format`](./types#targetformatinput).

## Mutations

### saveJob
Saves a new job, associating a source, mapping, and target format.

```graphql
saveJob(source: SourceInput!, mapping: MappingInput, targetFormat: TargetFormatInput): ID
```

#### Example:
```graphql
mutation {
  saveJob(
    source: {
      id: "source123",
      sourceRequest: {
        name: "My Shopify Store",
        type: Shopify,
        url: "https://mystore.myshopify.com/"
      }
    },
    mapping: {
      id: "mapping456"
    },
    targetFormat: {
      id: "format789"
    }
  )
}
```

This mutation creates a new job by combining existing or new components:
- [`source`](./types#sourceinput): The source to use (can be an existing source ID or a new source request).
- [`mapping`](./types#mappinginput): The mapping to apply. If no mapping is given, a mapping will be generated based on the source data.
- [`targetFormat`](./types#targetformatinput): The target format to transform the data into. If no target format is given, the default target format will be used.

After initialization, the job will be run once. After that, it will be automatically run every 24 hours. You can manually trigger a job execution using the [`runJob`](#runjob) mutation.

The mutation returns the ID of the newly created job.

### deleteJob
Deletes a saved job.

```graphql
deleteJob(id: ID!): Boolean!
```

#### Example:
```graphql
mutation {
  deleteJob(id: "587")
}
```

This mutation removes a job from the system. It returns `true` if the deletion was successful, `false` otherwise.

### runJob
Executes a saved job.

```graphql
runJob(id: ID!): Boolean!
```

#### Example:
```graphql
mutation {
  runJob(id: "job123")
}
```

This mutation triggers the manual execution of a saved job. It performs the following steps:
1. Fetches data from the associated source
2. Applies the specified mapping to the data
3. Transforms the result into the target format

It returns `true` if the job execution was successful, `false` otherwise.