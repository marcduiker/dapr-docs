---
type: docs
title: "Quickstart: Conversation"
linkTitle: Conversation
weight: 90
description: Get started with the Dapr conversation building block
---

{{% alert title="Alpha" color="warning" %}}
The conversation building block is currently in **alpha**. 
{{% /alert %}}

Let's take a look at how the [Dapr conversation building block]({{< ref conversation-overview.md >}}) makes interacting with the Anthropic LLM easier. In this quickstart, you use the Anthropic component to communicate with the LLM and ask it for a poem about Dapr. 

{{< tabs ".NET" Go >}}

 <!-- .NET -->
{{% codetab %}}


### Step 1: Pre-requisites

For this example, you will need:

- [Dapr CLI and initialized environment](https://docs.dapr.io/getting-started).
- [.NET SDK or .NET 6 SDK installed](https://dotnet.microsoft.com/download).
<!-- IGNORE_LINKS -->
- [Docker Desktop](https://www.docker.com/products/docker-desktop)
<!-- END_IGNORE -->

### Step 2: Set up the environment

Clone the [sample provided in the Quickstarts repo](https://github.com/dapr/quickstarts/tree/master/conversation).

```bash
git clone https://github.com/dapr/quickstarts.git
```

From the root of the Quickstarts directory, navigate into the conversation directory:

```bash
cd conversation/csharp/sdk/conversation
```

### Step 3: Launch the conversation service

Start the conversation service with the following command:

```bash
dapr run --app-id conversation --resources-path "../../../components/" -- dotnet run
```

**Expected output**

```
== APP == info: System.Net.Http.HttpClient.Default.LogicalHandler[100]
== APP ==       Start processing HTTP request POST http://localhost:50115/dapr.proto.runtime.v1.Dapr/ConverseAlpha1
== APP == info: System.Net.Http.HttpClient.Default.ClientHandler[100]
== APP ==       Sending HTTP request POST http://localhost:50115/dapr.proto.runtime.v1.Dapr/ConverseAlpha1
== APP == info: System.Net.Http.HttpClient.Default.ClientHandler[101]
== APP ==       Received HTTP response headers after 3241.804ms - 200
== APP == info: System.Net.Http.HttpClient.Default.LogicalHandler[101]
== APP ==       End processing HTTP request after 3260.3206ms - 200
== APP == info: Program[1330097018]
== APP ==       Sent prompt to conversation API: 'Please write a witty sonnet about the Dapr distributed programming framework at dapr.io'
== APP == info: Program[1283986522]
== APP ==       Received message from the conversation API: 'Here's a witty sonnet about the Dapr distributed programming framework:
== APP == 
== APP == O Dapr, thou art a framework most divine,
== APP == Distributed apps, thy specialty so fine.
== APP == With microservices, thou dost unite,
== APP == And make cloud-native dreams take flight.
== APP == 
== APP == Thy building blocks, like stars in cosmic dance,
== APP == State management, pub/sub, and more enhance.
== APP == Polyglot and platform-agnostic too,
== APP == Developers swoon, their hearts you woo.
== APP == 
== APP == From Kubernetes to edge, you scale with ease,
== APP == Your sidecar pattern doth the experts please.
== APP == Open-source and loved by geeks galore,
== APP == At dapr.io, they come to explore.
== APP == 
== APP == Though some may mock my verse as quite absurd,
== APP == I say Dapr's praises must be heard!
== APP ==       '
Exited App successfully
```

### What happened?

#### `conversation.yaml` LLM component

In [`conversation/components`](https://github.com/dapr/quickstarts/tree/master/conversation/components) directly of the quickstart, the [`conversation.yaml` file](https://github.com/dapr/quickstarts/tree/master/conversation/components/conversation.yml) configures the Anthropic LLM component. 

```yml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: conversation
spec:
  type: conversation.anthropic
  version: v1
  metadata:
    - name: key
      secretKeyRef:
        name: anthropic
        key: anthropic
auth:
  secretStore: envvar-secrets
```

For authentication, the component also uses a secret store called [`envvar-secrets`](https://github.com/dapr/quickstarts/tree/master/conversation/components/envvar.yml). 

#### `Program.cs` conversation app

```csharp
using System.Text;
using Dapr.AI.Conversation;
using Dapr.AI.Conversation.Extensions;

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddDaprConversationClient();
var app = builder.Build();

var conversationClient = app.Services.GetRequiredService<DaprConversationClient>();
var logger = app.Services.GetRequiredService<ILogger<Program>>();

// Send prompt to send to the Anthropic LLM
const string prompt = "Please write a witty sonnet about the Dapr distributed programming framework at dapr.io";
var response = await conversationClient.ConverseAsync("conversation",
        [new(prompt, DaprConversationRole.Generic)]);
Log.LogRequest(logger, prompt);

var stringBuilder = new StringBuilder();
foreach (var resp in response.Outputs)
{
        stringBuilder.AppendLine(resp.Result);
}
Log.LogResponse(logger, stringBuilder.ToString());

// Output log with response from LLM
static partial class Log
{
        [LoggerMessage(LogLevel.Information, "Sent prompt to conversation API: '{message}'")]
        internal static partial void LogRequest(ILogger logger, string message);

        [LoggerMessage(LogLevel.Information, "Received message from the conversation API: '{message}'")]
        internal static partial void LogResponse(ILogger logger, string message);
}
```

{{% /codetab %}}

 <!-- Go -->
{{% codetab %}}


### Step 1: Pre-requisites

For this example, you will need:

- [Dapr CLI and initialized environment](https://docs.dapr.io/getting-started).
- [Latest version of Go](https://go.dev/dl/).
<!-- IGNORE_LINKS -->
- [Docker Desktop](https://www.docker.com/products/docker-desktop)
<!-- END_IGNORE -->

### Step 2: Set up the environment

Clone the [sample provided in the Quickstarts repo](https://github.com/dapr/quickstarts/tree/master/conversation).

```bash
git clone https://github.com/dapr/quickstarts.git
```

From the root of the Quickstarts directory, navigate into the conversation directory:

```bash
cd conversation/go/sdk/conversation
```

### Step 3: Launch the conversation service

Start the conversation service with the following command:

```bash

```

**Expected output**

```

```

### What happened?

#### `conversation.yaml` LLM component

In [`conversation/components`](https://github.com/dapr/quickstarts/tree/master/conversation/components) directly of the quickstart, the [`conversation.yaml` file](https://github.com/dapr/quickstarts/tree/master/conversation/components/conversation.yml) configures the Anthropic LLM component. 

```yml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: echo
spec:
  type: conversation.echo
  version: v1
```

For authentication, the component also uses a secret store called [`envvar-secrets`](https://github.com/dapr/quickstarts/tree/master/conversation/components/envvar.yml). 

#### `conversation.go` conversation app

```go
package main

import (
	"context"
	"fmt"
	"log"

	dapr "github.com/dapr/go-sdk/client"
)

func main() {
	client, err := dapr.NewClient()
	if err != nil {
		panic(err)
	}

	input := dapr.ConversationInput{
		Message: "hello world",
		// Role:     nil, // Optional
		// ScrubPII: nil, // Optional
	}

	fmt.Printf("conversation input: %s\n", input.Message)

	var conversationComponent = "echo"

	request := dapr.NewConversationRequest(conversationComponent, []dapr.ConversationInput{input})

	resp, err := client.ConverseAlpha1(context.Background(), request)
	if err != nil {
		log.Fatalf("err: %v", err)
	}

	fmt.Printf("conversation output: %s\n", resp.Outputs[0].Result)
}
```

{{% /codetab %}}

{{< /tabs >}}

## Demo

Watch the demo presented during [Diagrid's Dapr v1.15 celebration](https://www.diagrid.io/videos/dapr-1-15-deep-dive) to see how the conversation API works using the .NET SDK.

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/NTnwoDhHIcQ?si=37SDcOHtEpgCIwkG&amp;start=5444" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Tell us what you think!

We're continuously working to improve our Quickstart examples and value your feedback. Did you find this Quickstart helpful? Do you have suggestions for improvement?

Join the discussion in our [discord channel](https://discord.com/channels/778680217417809931/953427615916638238).

## Next steps

- SDK samples of this quickstart:
  - [.NET](https://github.com/dapr/quickstarts/tree/master/conversation/go/http)
  - [Go](todo)
- Learn more about [the conversation building block]({{< ref conversation-overview.md >}})

{{< button text="Explore Dapr tutorials  >>" page="getting-started/tutorials/_index.md" >}}
