# Listen for live transactions in Java

**In this guide, you listen to the Tangle for recent transactions by subscribing to the [zero message queue (ZMQ)](https://zeromq.org/) on [nodes](root://getting-started/0.1/network/nodes.md) that run the [IRI node software](root://node-software/0.1/iri/introduction/overview.md).**

## IOTA network

In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet).

## Code walkthrough

1. Import the class

    ```java
    package com.iota;

    import org.zeromq.ZMQ;
    ```

2. Connect the socket to a node's ZMQ port

    ```java
	ZMQ.Context context = ZMQ.context(1);
    ZMQ.Socket socket = context.socket(ZMQ.SUB);

    socket.connect("tcp://zmq.devnet.iota.org:5556");
    ```

3. Subscribe to the [`tx` and `sn`](root://node-software/0.1/iri/references/zmq-events.md) events to see all transactions and confirmed transactions

    ```java
    socket.subscribe("tx");
    socket.subscribe("sn");
    ```

4. Process the event data that the node returns

    ```java
    while(true) {
        byte[] reply = socket.recv(0);
        String[] data = (new String(reply).split(" "));

        if(data[0].equals("tx")) System.out.println("NEW TRANSACTION" + "\n" + "Transaction hash: " + data[1] + "\n" + "Address: " + data[2] + "\n" + "Value: " + data[3] + "\n" + "Tag: " + data[4] + "\n");
        if(data[0].equals("sn")) System.out.println("CONFIRMED" + "\n" + "Transaction hash: " + data[2] + "\n" + "Address: " + data[3] + "\n");
    }
    ```

    In the console, you should see transaction data.

:::success:Congratulations :tada:
You're listening to transactions
:::

## Run the code

To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device.

You also need a Java development environment that uses the [Maven](https://maven.apache.org/download.cgi) build tool. If this is your first time using the Java client library, complete our [getting started guide](../../getting-started/java-quickstart.md), and follow the instructions for installing the library with Maven.

In the command-line, do the following:

--------------------
### Linux and macOS
```bash
git clone https://github.com/JakeSCahill/java-iota-workshop.git
cd java-iota-workshop
mvn clean install
mvn exec:java -Dexec.mainClass="com.iota.ListenToZMQ"
```
---
### Windows
```bash
git clone https://github.com/JakeSCahill/java-iota-workshop.git
cd java-iota-workshop
mvn clean install
mvn exec:java -D"exec.mainClass"="com.iota.ListenToZMQ"
```
--------------------

In the console, you should see transaction data.

## Next steps

Take a look at our [app blueprints](root://blueprints/0.1/introduction/overview.md) for inspiration
