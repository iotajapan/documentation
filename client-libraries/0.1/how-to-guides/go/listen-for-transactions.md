# Listen for live transactions in Go

**[Nodes](root://getting-started/0.1/network/nodes.md) that run the [IRI node software](root://node-software/0.1/iri/introduction/overview.md) can enable their [zero message queue (ZMQ)](https://zeromq.org/), which allows you to subscribe to events such as transaction confirmations. In this guide, you subscribe to all transactions as well as confirmed transactions.**

## Packages

To complete this guide, you need to install the following package (if you're using Go modules, you just need to reference this package):

```bash
go get github.com/pebbe/zmq4
```

## IOTA network

In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet).

## Code walkthrough

1. Import the packages

    ```go
    package main

    import (
        zmq "github.com/pebbe/zmq4"
        "fmt"
        "strings"
    )
    ```

2. Connect the socket to a node's ZMQ port

    ```go
	client, _ := zmq.NewSocket(zmq.SUB)

    defer client.Close()

	client.Connect("tcp://zmq.devnet.iota.org:5556")
    ```

3. Subscribe to the [`tx` and `sn`](root://node-software/0.1/iri/references/zmq-events.md) events to see all transactions and confirmed transactions

    ```go
    client.SetSubscribe("tx")
    client.SetSubscribe("sn")
    ```

4. Process the event data that the node returns

    ```go
    for {
		msg, _ := client.RecvMessage(0)
		for _, str := range msg {
        
            words := strings.Fields(str)

            if(words[0] == "tx") {
                fmt.Println("New transaction: ", words[1])
            }
            if(words[0] == "sn") {
                fmt.Println("Confirmed transaction: ", words[2], "for milestone", words[1])
            }
		}

    }
    ```

    In the console, you should see transaction data.

:::success:Congratulations :tada:
You're listening to transactions
:::

## Run the code

Use this sample code to get started with the ZMQ.

```go
package main

import (
    zmq "github.com/pebbe/zmq4"
    "fmt"
    "strings"
)

func main() {
	client, _ := zmq.NewSocket(zmq.SUB)

    // Make sure the connection is closed after stopping the program
    defer client.Close()

    // Connect to a Devnet node's ZMQ address
	client.Connect("tcp://zmq.devnet.iota.org:5556")

    // Subscribe to both tx and sn (confirmed tx) topics
    client.SetSubscribe("tx")
    client.SetSubscribe("sn")

    // Keep looping for messages
    for {
		msg, _ := client.RecvMessage(0)
		for _, str := range msg {

            // Split the fields by the space character
            words := strings.Fields(str)

            if(words[0] == "tx") {
                fmt.Println("New transaction: ", words[1])
            }
            if(words[0] == "sn") {
                fmt.Println("Confirmed transaction: ", words[2], "for milestone", words[1])
            }
		}

	}
}
```

## Next steps

Take a look at our [app blueprints](root://blueprints/0.1/introduction/overview.md) for inspiration
