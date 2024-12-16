---
type: docs
title: "How-To: Converse with an LLM using the conversation API"
linkTitle: "How-To: Converse"
weight: 2000
description: "Learn how to abstract the complexities of interacting with large language models"
---

{{% alert title="Alpha" color="primary" %}}
The conversation API is currently in [alpha]({{< ref "certification-lifecycle.md#certification-levels" >}}).
{{% /alert %}}

Let's get started using the [conversation API]({{< ref conversation-overview.md >}}). In this guide, you'll learn how to:

- Set up one of the available Dapr components (echo) that work with the conversation API.   
- Add the conversation client to your application.

## Set up the conversation component

Create a new configuration file called `conversation.yaml` and save to a components or config sub-folder in your application directory. 

Select your [preferred conversation component spec]({{< ref supported-conversation >}}) for your `conversation.yaml` file.

For this scenario, we use a simple echo component.

```yml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: echo
spec:
  type: conversation.echo
  version: v1
```

## Connect the conversation client


{{< tabs ".NET" "Go" "Rust" >}}


 <!-- .NET -->
{{% codetab %}}

```dotnet
todo
```

{{% /codetab %}}

 <!-- Go -->
{{% codetab %}}

```go
package main

import (
	"context"
	"fmt"
	dapr "github.com/dapr/go-sdk/client"
	"log"
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

 <!-- Rust -->
{{% codetab %}}

```rust
use dapr::client::{ConversationInputBuilder, ConversationRequestBuilder};
use std::thread;
use std::time::Duration;

type DaprClient = dapr::Client<dapr::client::TonicClient>;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    // Sleep to allow for the server to become available
    thread::sleep(Duration::from_secs(5));

    // Set the Dapr address
    let address = "https://127.0.0.1".to_string();

    let mut client = DaprClient::connect(address).await?;

    let input = ConversationInputBuilder::new("hello world").build();

    let conversation_component = "echo";

    let request =
        ConversationRequestBuilder::new(conversation_component, vec![input.clone()]).build();

    println!("conversation input: {:?}", input.message);

    let response = client.converse_alpha1(request).await?;

    println!("conversation output: {:?}", response.outputs[0].result);
    Ok(())
}
```

{{% /codetab %}}

{{< /tabs >}}


## Next steps

- [Conversation API reference guide]({{< ref conversation_api.md >}})
- [Available conversation components]({{< ref supported-conversation >}})