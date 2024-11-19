---
type: docs
title: "Conversation API reference"
linkTitle: "Conversation API"
description: "Detailed documentation on the conversation API"
weight: 1400
---

Dapr provides users with an abstraction layer to reduce the complexity of interacting with LLMS using either a new runtime method or HTTP endpoint.

## Converse

This endpoint lets you converse with LLMs.

```
POST /v1.0/conversation/[component]/converse
```

### URL parameters

| Parameter | Description |
| --------- | ----------- |
| `component` | The name of the component you're using. [See a list of all available Conversation components]({{< ref supported-conversation >}})

### Request content

```json
REQUEST = {
  "conversationContext": "ID of an existing chat room",
  "inputs": ["what is Dapr", "Why use Dapr"],
  "parameters": {},
}
```

### HTTP response codes

Code | Description
---- | -----------
`202`  | Accepted
`400`  | Request was malformed
`500`  | Request formatted correctly, error in dapr code or underlying component

### Response content

```json
RESPONSE  = {
  "conversationContext": "ID of an existing chat room",
  "outputs": {
    {
       "result": "Dapr is distribution application runtime ...",
       "parameters": {},
    },
    {
       "result": "Dapr can help developers ...",
       "parameters": {},
    }
  },
}
```

## Next steps

[Conversation API overview]({{< ref conversation-overview.md >}})