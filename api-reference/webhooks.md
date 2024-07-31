---
title: 'Webhooks'
---

Webhooks are executed whenever specific events occur in the product data associated with a job run. Webhooks can be configured to monitor specific fields or events such as product additions, updates, or removals.

## Webhook Execution

The payload sent to the configured URL when a webhook is triggered contains a JSON object with the following structure:

### Payload Structure

- `timestamp`: The timestamp when the job execution triggered the webhook.
- `jobID`: The ID of the job associated with the webhook.
- `products`: An array of product objects that were affected by the event. Each product object includes:
  - `event`: The specific event type that occurred for the product (`ADDED`, `UPDATED`, `REMOVED`).
  - `productData`: The data fields for the product, as defined by the target format. For `UPDATED` events, this may only include the fields that changed, depending on the webhook configuration.

### Example Payload

```json
{
  "timestamp": "2024-07-31T12:34:56Z",
  "jobID": "644",
  "products": [
    {
      "event": "UPDATED",
      "productData": {
        "id": "product456",
        "name": "Ergonomic Chair",
        "price": 199.99,
        "stock": 15
      },
      "changedFields":  [ 
        { "name": "price", "from": 199.99, "to": 120.99 } 
    ]
    },
    {
      "event": "REMOVED",
      "productData": {
        "id": "product789",
        "name": "Standing Desk",
        "price": 299.99,
        "stock": 5
      }
    }
  ]
}
```

#### Product Object Fields
- **`event`**: Specifies the type of change that triggered the webhook for this product. Possible values include:
  - `ADDED`: The product was newly added to the data set.
  - `UPDATED`: The product's data was modified. The payload may include only the fields that were updated, if specific fields were monitored.
  - `REMOVED`: The product was removed from the data set.
- **`productData`**: Contains the relevant data fields for the product as defined by the target format. This can include full product details or just the fields that have changed, depending on the configuration.
- **`changedFields`**: An object containing the fields that changed for the product. This is only included for `UPDATED` events and only includes the fields that were monitored.
  - `name`: The name of the field that changed (refers to the field name in the target format).
  - `from`: The previous value of the field.
  - `to`: The new value of the field.

### Field-Specific Monitoring

`UPDATED` Webhooks can be set to trigger only on changes to specific fields. The `productData` will still include all fields, and the `changedFields` array will include the fields that changed.

### Webhook Delivery

Webhooks are sent as HTTP POST requests to the specified `url` in the webhook configuration. The payload is delivered as a JSON object in the request body. The receiving endpoint should respond with a 200 OK status to acknowledge receipt. If the delivery fails, the system may attempt retries.

## Webhook Schema

The `WebhookSchema` type includes the following fields:

- `id`: Unique identifier for the webhook
- `url`: The URL that will receive the webhook notifications
- `event`: The event type that triggers the webhook (`ADDED`, `UPDATED`, `REMOVED`)
- `fields`: A list of specific field names to monitor (optional). This refers to fields in the target format, not the source data.
- `jobID`: The ID of the associated job for which the webhook is set up

## Queries

### getWebhooks
Retrieves all configured webhooks.

```graphql
getWebhooks: [WebhookSchema!]
```

#### Example:
```graphql
query {
  getWebhooks {
    id
    url
    event
    fields
    jobID
  }
}
```

This query returns an array of all configured webhooks, including details about the URL, event type, monitored fields, and associated job.

## Mutations

### saveWebhook
Creates or updates a webhook for monitoring product data changes.

```graphql
saveWebhook(
  url: String!, 
  event: WebhookEvent!, 
  fields: [String], 
  jobID: ID!
): ID!
```

#### Example:
```graphql
mutation {
  saveWebhook(
    url: "https://example.com/webhook",
    event: UPDATED,
    fields: ["price", "stock"],
    jobID: "job123"
  )
}
```

This mutation creates or updates a webhook. Parameters include:
- `url`: The endpoint that will receive notifications when the specified event occurs.
- `event`: The type of event that triggers the webhook. Supported events are:
  - `ADDED`: Triggered when a new product is added.
  - `UPDATED`: Triggered when an existing product is updated.
  - `REMOVED`: Triggered when a product is deleted.
- `fields`: Specific product fields to monitor. If specified, the webhook will only trigger when these fields change.
- `jobID`: The ID of the associated job, linking the webhook to a specific data transformation job.

The mutation returns the ID of the newly created or updated webhook.

### deleteWebhook
Deletes a configured webhook.

```graphql
deleteWebhook(id: ID!): Boolean!
```

#### Example:
```graphql
mutation {
  deleteWebhook(id: "webhook789")
}
```

This mutation removes a webhook. It returns `true` if the deletion was successful, `false` otherwise.

## Webhook Events

### Supported Events
Webhooks can be configured to trigger on the following events:
- **`ADDED`**: Notifies when a new product is added to the system.
- **`UPDATED`**: Notifies when an existing product is updated. If specific fields are provided, the webhook will only trigger when those fields change.
- **`REMOVED`**: Notifies when a product is removed from the system.

### Monitoring Specific Fields
To receive notifications only when certain fields of a product change, specify those fields in the `fields` parameter when setting up the webhook. This can reduce the number of notifications by focusing only on changes relevant to your application.

#### Example:
To set up a webhook that only triggers when the `price` or `stock` fields of a product change:

```graphql
mutation {
  saveWebhook(
    url: "https://example.com/webhook",
    event: UPDATED,
    fields: ["price", "stock"],
    jobID: "4765"
  )
}
```

This configuration will notify the specified URL only when there are updates to the `price` or `stock` fields in the products processed by the job with ID `job123`.

## Managing Webhooks
To effectively manage webhooks:
1. **Create Webhooks**: Use the `saveWebhook` mutation to set up notifications for specific events or field changes.
2. **Update Webhooks**: Reuse the `saveWebhook` mutation to update existing webhook configurations.
3. **Delete Webhooks**: Use the `deleteWebhook` mutation to remove unnecessary webhooks.