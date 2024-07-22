---
title: 'Job Management'
---

This section of the API focuses on managing and executing jobs. Jobs are a way to combine a source, a mapping, and a target format into a reusable workflow for data transformation.

## Queries

### getJobs
Retrieves all saved jobs.

```graphql
getJobs: [JobSchema!]
```

Example:
```graphql
query {
  getJobs {
    id
    sourceID
    mappingID
    targetSchemaID
  }
}
```

This query returns an array of all saved jobs. Each job includes IDs for its associated source, mapping, and target format.

## Mutations

### saveJob
Saves a new job, associating a source, mapping, and target format.

```graphql
saveJob(sourceID: ID!, mappingID: ID!, targetFormatID: ID!): ID!
```

Example:
```graphql
mutation {
  saveJob(
    sourceID: "source123",
    mappingID: "mapping456",
    targetFormatID: "format789"
  )
}
```

This mutation creates a new job by combining existing components:
- `sourceID`: The ID of the data source to use
- `mappingID`: The ID of the mapping to apply
- `targetFormatID`: The ID of the target format to transform the data into

It returns the ID of the newly created job.

### deleteJob
Deletes a saved job.

```graphql
deleteJob(id: ID!): Boolean!
```

Example:
```graphql
mutation {
  deleteJob(id: "job123")
}
```

This mutation removes a job from the system. It returns `true` if the deletion was successful, `false` otherwise.

### runJob
Executes a saved job.

```graphql
runJob(id: ID!): Boolean!
```

Example:
```graphql
mutation {
  runJob(id: "job123")
}
```

This mutation triggers the execution of a saved job. It performs the following steps:
1. Fetches data from the associated source
2. Applies the specified mapping to the data
3. Transforms the result into the target format

It returns `true` if the job execution was successful, `false` otherwise.

## Job Workflow Example

Here's an example of how you might use these operations in a typical workflow:

1. First, create a source, mapping, and target format using the appropriate mutations.

2. Then, create a job that combines these components:

```graphql
mutation {
  saveJob(
    sourceID: "shopifySource123",
    mappingID: "shopifyToStandardMapping456",
    targetFormatID: "standardProductFormat789"
  )
}
```

3. Retrieve the list of jobs to confirm it was created:

```graphql
query {
  getJobs {
    id
    sourceID
    mappingID
    targetSchemaID
  }
}
```

4. Execute the job:

```graphql
mutation {
  runJob(id: "job123")
}
```

5. If the job is no longer needed, you can delete it:

```graphql
mutation {
  deleteJob(id: "job123")
}
```

These job-related operations allow you to create reusable data transformation workflows. By combining sources, mappings, and target formats into jobs, you can easily manage and execute complex data processing tasks with a single operation.