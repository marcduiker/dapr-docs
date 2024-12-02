---
type: docs
title: "Conversation overview"
linkTitle: "Overview"
weight: 1000
description: "Overview of the conversation API building block"
---

Using the Dapr conversation API, you can reduce the complexity of interacting with Large Language Models (LLMs) and enable critical performance and security functionality with features like prompt caching and personally identifiable information (PII) data obfuscation.

## Features

### Prompt caching

todo

### Personally identifiable information (PII) obfuscation

todo

## Supported SDKs

You can use the following SDKs to author a workflow.

| Language stack | Package |
| - | - |
| .NET | [Dapr.AI.Conversation](todo) |
| Rust | [converse](https://github.com/dapr/rust-sdk/blob/main/dapr/src/client.rs#L542) |
| Go | [conversation](https://github.com/dapr/go-sdk/tree/main/client/conversation.go) |

## Try out conversation

### Quickstarts and tutorials

Want to put the Dapr conversation API to the test? Walk through the following quickstart and tutorials to see it in action:

| Quickstart/tutorial | Description |
| ------------------- | ----------- |
| [Conversation quickstart](todo) | . |

### Start using pub/sub directly in your app

Want to skip the quickstarts? Not a problem. You can try out the conversation building block directly in your application. After [Dapr is installed]({{< ref "getting-started/_index.md" >}}), you can begin using the conversation API starting with [the how-to guide](todo).

## Next steps

- [How-To: Converse with an LLM using the conversation API]({{< ref howto-conversation-layer.md >}})
- [Conversation API components]({{< ref supported-conversation >}})