---
type: docs
title: "Error codes reference guide"
linkTitle: "Reference"
description: "List of gRPC and HTTP error codes in Dapr and their descriptions"
weight: 20
---

The following tables list the error codes returned by Dapr runtime:

### Actors API

| Error Code                       | Description                                |
| -------------------------------- | ------------------------------------------ |
| ERR_ACTOR_INSTANCE_MISSING       | Error when an actor instance is missing.   |
| ERR_ACTOR_RUNTIME_NOT_FOUND      | Error the actor instance.                  |
| ERR_ACTOR_REMINDER_CREATE        | Error creating a reminder for an actor.    |
| ERR_ACTOR_REMINDER_DELETE        | Error deleting a reminder for an actor.    |
| ERR_ACTOR_TIMER_CREATE           | Error creating a timer for an actor.       |
| ERR_ACTOR_TIMER_DELETE           | Error deleting a timer for an actor.       |
| ERR_ACTOR_REMINDER_GET           | Error getting a reminder for an actor.     |
| ERR_ACTOR_INVOKE_METHOD          | Error invoking a method on an actor.       |
| ERR_ACTOR_STATE_DELETE           | Error deleting the state for an actor.     |
| ERR_ACTOR_STATE_GET              | Error getting the state for an actor.      |
| ERR_ACTOR_STATE_TRANSACTION_SAVE | Error storing actor state transactionally. |
| ERR_ACTOR_REMINDER_NON_HOSTED    | Error setting reminder for an actor.       |

### Workflows API

| Error Code                       | Description                                                 |
| -------------------------------- | ----------------------------------------------------------- |
| ERR_GET_WORKFLOW                 | Error getting workflow.                                     |
| ERR_START_WORKFLOW               | Error starting the workflow.                                |
| ERR_PAUSE_WORKFLOW               | Error pausing the workflow.                                 |
| ERR_RESUME_WORKFLOW              | Error resuming the workflow.                                |
| ERR_TERMINATE_WORKFLOW           | Error terminating the workflow.                             |
| ERR_PURGE_WORKFLOW               | Error purging workflow.                                     |
| ERR_RAISE_EVENT_WORKFLOW         | Error raising an event within the workflow.                 |
| ERR_WORKFLOW_COMPONENT_MISSING   | Error when a workflow component is missing a configuration. |
| ERR_WORKFLOW_COMPONENT_NOT_FOUND | Error when a workflow component is not found.               |
| ERR_WORKFLOW_EVENT_NAME_MISSING  | Error when the event name for a workflow is missing.        |
| ERR_WORKFLOW_NAME_MISSING        | Error when the workflow name is missing.                    |
| ERR_INSTANCE_ID_INVALID          | Error invalid workflow instance ID provided.                |
| ERR_INSTANCE_ID_NOT_FOUND        | Error workflow instance ID not found.                       |
| ERR_INSTANCE_ID_PROVIDED_MISSING | Error workflow instance ID was provided but missing.        |
| ERR_INSTANCE_ID_TOO_LONG         | Error workflow instance ID exceeds allowable length.        |

### State Management API

| Error Code                            | Description                                                               |
| ------------------------------------- | ------------------------------------------------------------------------- |
| ERR_STATE_STORE_NOT_FOUND             | Error referencing a state store not found.                                |
| ERR_STATE_STORES_NOT_CONFIGURED       | Error no state stores configured.                                         |
| ERR_NOT_SUPPORTED_STATE_OPERATION     | Error transaction requested on a state store with no transaction support. |
| ERR_STATE_GET                         | Error getting a state for state store.                                    |
| ERR_STATE_DELETE                      | Error deleting a state from state store.                                  |
| ERR_STATE_SAVE                        | Error saving a state in state store.                                      |
| ERR_STATE_TRANSACTION                 | Error encountered during state transaction.                               |
| ERR_STATE_BULK_GET                    | Error performing bulk retrieval of state entries.                         |
| ERR_STATE_QUERY                       | Error querying the state store.                                           |
| ERR_STATE_STORE_NOT_CONFIGURED        | Error state store is not configured.                                      |
| ERR_STATE_STORE_NOT_SUPPORTED         | Error state store is not supported.                                       |
| ERR_STATE_STORE_TOO_MANY_TRANSACTIONS | Error exceeded maximum allowable transactions.                            |

### Configuration API

| Error Code                             | Description                                  |
| -------------------------------------- | -------------------------------------------- |
| ERR_CONFIGURATION_GET                  | Error retrieving configuration.              |
| ERR_CONFIGURATION_STORE_NOT_CONFIGURED | Error configuration store is not configured. |
| ERR_CONFIGURATION_STORE_NOT_FOUND      | Error configuration store not found.         |
| ERR_CONFIGURATION_SUBSCRIBE            | Error subscribing to a configuration.        |
| ERR_CONFIGURATION_UNSUBSCRIBE          | Error unsubscribing from a configuration.    |

### Crypto API

| Error Code                          | Description                                |
| ----------------------------------- | ------------------------------------------ |
| ERR_CRYPTO                          | General crypto building block error.       |
| ERR_CRYPTO_KEY                      | Error related to a crypto key.             |
| ERR_CRYPTO_PROVIDER_NOT_FOUND       | Error specified crypto provider not found. |
| ERR_CRYPTO_PROVIDERS_NOT_CONFIGURED | Error no crypto providers configured.      |

### Secrets API

| Error Code                       | Description                                          |
| -------------------------------- | ---------------------------------------------------- |
| ERR_SECRET_STORES_NOT_CONFIGURED | Error that no secret store is configured.            |
| ERR_SECRET_STORE_NOT_FOUND       | Error that specified secret store is not found.      |
| ERR_SECRET_GET                   | Error retrieving the specified secret.               |
| ERR_PERMISSION_DENIED            | Error access denied due to insufficient permissions. |

### Pub/Sub API

| Error Code                  | Description                                              |
| --------------------------- | -------------------------------------------------------- |
| ERR_PUBSUB_NOT_FOUND        | Error referencing the Pub/Sub component in Dapr runtime. |
| ERR_PUBSUB_PUBLISH_MESSAGE  | Error publishing a message.                              |
| ERR_PUBSUB_FORBIDDEN        | Error message forbidden by access controls.              |
| ERR_PUBSUB_CLOUD_EVENTS_SER | Error serializing Pub/Sub event envelope.                |
| ERR_PUBSUB_EMPTY            | Error empty Pub/Sub.                                     |
| ERR_PUBSUB_NOT_CONFIGURED   | Error Pub/Sub component is not configured.               |
| ERR_PUBSUB_REQUEST_METADATA | Error with metadata in Pub/Sub request.                  |
| ERR_PUBSUB_EVENTS_SER       | Error serializing Pub/Sub events.                        |
| ERR_PUBLISH_OUTBOX          | Error publishing message to the outbox.                  |
| ERR_TOPIC_NAME_EMPTY        | Error topic name for Pub/Sub message is empty.           |

### Conversation API

| Error Code                      | Description                                     |
| ------------------------------- | ----------------------------------------------- |
| ERR_INVOKE_OUTPUT_BINDING       | Error invoking an output binding.               |
| ERR_DIRECT_INVOKE               | Error in direct invocation.                     |
| ERR_CONVERSATION_INVALID_PARMS  | Error invalid parameters for conversation.      |
| ERR_CONVERSATION_INVOKE         | Error invoking the conversation.                |
| ERR_CONVERSATION_MISSING_INPUTS | Error missing required inputs for conversation. |
| ERR_CONVERSATION_NOT_FOUND      | Error conversation not found.                   |

### Distributed Lock API

| Error Code                    | Description                         |
| ----------------------------- | ----------------------------------- |
| ERR_TRY_LOCK                  | Error attempting to acquire a lock. |
| ERR_UNLOCK                    | Error attempting to release a lock. |
| ERR_LOCK_STORE_NOT_CONFIGURED | Error lock store is not configured. |
| ERR_LOCK_STORE_NOT_FOUND      | Error lock store not found.         |

### Healthz

| Error Code                    | Description                                                     |
| ----------------------------- | --------------------------------------------------------------- |
| ERR_HEALTH_NOT_READY          | Error that Dapr is not ready.                                   |
| ERR_HEALTH_APPID_NOT_MATCH    | Error the app-id does not match expected value in health check. |
| ERR_OUTBOUND_HEALTH_NOT_READY | Error outbound connection health is not ready.                  |

### Common

| Error Code                 | Description                                      |
| -------------------------- | ------------------------------------------------ |
| ERR_API_UNIMPLEMENTED      | Error API is not implemented.                    |
| ERR_APP_CHANNEL_NIL        | Error application channel is nil.                |
| ERR_BAD_REQUEST            | Error client request is badly formed or invalid. |
| ERR_BODY_READ              | Error reading body.                              |
| ERR_INTERNAL               | Internal server error encountered.               |
| ERR_MALFORMED_REQUEST      | Error with a malformed request.                  |
| ERR_MALFORMED_REQUEST_DATA | Error request data is malformed.                 |
| ERR_MALFORMED_RESPONSE     | Error response data is malformed.                |

## Next steps

- [Handling HTTP error codes]({{< ref http-error-codes.md >}})
- [Handling gRPC error codes]({{< ref grpc-error-codes.md >}})