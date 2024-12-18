---
type: docs
title: "Conversation overview"
linkTitle: "Overview"
weight: 1000
description: "Overview of the conversation API building block"
---

{{% alert title="Alpha" color="primary" %}}
The conversation API is currently in [alpha]({{< ref "certification-lifecycle.md#certification-levels" >}}).
{{% /alert %}}


Using the Dapr conversation API, you can reduce the complexity of interacting with Large Language Models (LLMs) and enable critical performance and security functionality with features like prompt caching and personally identifiable information (PII) data obfuscation.

## Features

### Prompt caching

To significantly reduce latency and cost, frequent prompts are stored in a cache to be reused, instead of reprocessing the information for every new request. Prompt caching optimizes performance by storing and reusing prompts that are often repeated across multiple API calls.

### Personally identifiable information (PII) obfuscation

The PII obfuscation feature identifies and removes any PII from a conversation response. This feature protects your privacy by eliminating sensitive details like names, addresses, phone numbers, or other details that could be used to identify an individual.

## Try out conversation

### Quickstarts and tutorials

Want to put the Dapr conversation API to the test? Walk through the following quickstart and tutorials to see it in action:

| Quickstart/tutorial | Description |
| ------------------- | ----------- |
| [Conversation quickstart](todo) | . |

### Start using the conversation API directly in your app

Want to skip the quickstarts? Not a problem. You can try out the conversation building block directly in your application. After [Dapr is installed]({{< ref "getting-started/_index.md" >}}), you can begin using the conversation API starting with [the how-to guide]({{< ref howto-conversation-layer.md >}}).

## Next steps

- [How-To: Converse with an LLM using the conversation API]({{< ref howto-conversation-layer.md >}})
- [Conversation API components]({{< ref supported-conversation >}})