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

Let's take a look at how the [Dapr conversation building block]({{< ref conversation-overview.md >}})  

You can try out this conversation quickstart by either:

- [Running all applications in this sample simultaneously with the Multi-App Run template file]({{< ref "#run-using-multi-app-run" >}}), or
- [Running one application at a time]({{< ref "#run-one-job-application-at-a-time" >}})

{{< tabs ".NET" >}}

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

Install the dependencies:

```bash
dotnet restore
dotnet build
```

### Step 3: Launch the conversation service

Start the conversation service with the following command:

```bash
dapr run --app-id conversation --resources-path "../../../components/" -- dotnet run
```

**Expected output**

```dotnetcli
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

Running `dapr run -f .` in this Quickstart... 

#### `dapr.yaml` Multi-App Run template file

Running the [Multi-App Run template file]({{< ref multi-app-dapr-run >}}) with `dapr run -f .` starts all applications in your project. In this Quickstart, the `dapr.yaml` file contains the following:

```yml

```

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

// Set prompt to send to the Anthropic LLM
const string prompt = "Please write a witty sonnet about the Dapr distributed programming framework at dapr.io";
var response = await conversationClient.ConverseAsync("conversation",
        [new(prompt, DaprConversationRole.Generic)]);
Log.LogRequest(logger, prompt);

// Send prompt to LLM
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

{{< /tabs >}}

## Watch the demo

See the conversation API in action using a .NET example.

## Tell us what you think!

We're continuously working to improve our Quickstart examples and value your feedback. Did you find this Quickstart helpful? Do you have suggestions for improvement?

Join the discussion in our [discord channel](https://discord.com/channels/778680217417809931/953427615916638238).

## Next steps

- SDK samples of this quickstart:
  - [.NET](https://github.com/dapr/quickstarts/tree/master/conversation/go/http)
- Learn more about [the conversation building block]({{< ref conversation-overview.md >}})

{{< button text="Explore Dapr tutorials  >>" page="getting-started/tutorials/_index.md" >}}
