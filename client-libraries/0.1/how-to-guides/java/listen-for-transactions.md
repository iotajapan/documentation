# Java でライブトランザクションをリッスンする
<!-- # Listen for live transactions in Java -->

**このガイドでは、[IRI ノードソフトウェア](root://node-software/0.1/iri/introduction/overview.md)を実行する[ノード](root://getting-started/0.1/network/nodes.md)の[ゼロメッセージキュー（ZMQ）](https://zeromq.org/)にサブスクライブして、タングル上の最近のトランザクションをリッスンします。**
<!-- **In this guide, you listen to the Tangle for recent transactions by subscribing to the [zero message queue (ZMQ)](https://zeromq.org/) on [nodes](root://getting-started/0.1/network/nodes.md) that run the [IRI node software](root://node-software/0.1/iri/introduction/overview.md).** -->

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. クラスをインポートします。
  <!-- 1. Import the class -->

    ```java
    package com.iota;

    import org.zeromq.ZMQ;
    ```

2. ソケットをノードの ZMQ ポートに接続します。
  <!-- 2. Connect the socket to a node's ZMQ port -->

    ```java
    ZMQ.Context context = ZMQ.context(1);
    ZMQ.Socket socket = context.socket(ZMQ.SUB);

    socket.connect("tcp://zmq.devnet.iota.org:5556");
    ```

3. [`tx` および `sn`](root://node-software/0.1/iri/references/zmq-events.md) イベントをサブスクライブして、すべてのトランザクションと確定済みのトランザクションを表示します。
  <!-- 3. Subscribe to the [`tx` and `sn`](root://node-software/0.1/iri/references/zmq-events.md) events to see all transactions and confirmed transactions -->

    ```java
    socket.subscribe("tx");
    socket.subscribe("sn");
    ```

4. ノードが返すイベントデータを処理します。
  <!-- 4. Process the event data that the node returns -->

    ```java
    while(true) {
        byte[] reply = socket.recv(0);
        String[] data = (new String(reply).split(" "));

        if(data[0].equals("tx")) System.out.println("NEW TRANSACTION" + "\n" + "Transaction hash: " + data[1] + "\n" + "Address: " + data[2] + "\n" + "Value: " + data[3] + "\n" + "Tag: " + data[4] + "\n");
        if(data[0].equals("sn")) System.out.println("CONFIRMED" + "\n" + "Transaction hash: " + data[2] + "\n" + "Address: " + data[3] + "\n");
    }
    ```

    コンソールに、トランザクションデータが表示されます。
    <!-- In the console, you should see transaction data. -->

:::success:おめでとうございます:tada:
トランザクションをリッスンしています。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You're listening to transactions -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

これらのコードサンプルは [GitHub](https://github.com/JakeSCahill/java-iota-workshop) でホストされています。
<!-- These code samples are hosted on [GitHub](https://github.com/JakeSCahill/java-iota-workshop). -->

開始するには、デバイスに [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) がインストールされている必要があります。
<!-- To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device. -->

また、[Maven](https://maven.apache.org/download.cgi) ビルドツールを使用する Java 開発環境も必要です。Java クライアントライブラリを初めて使用する場合は、[スタートガイド](../../getting-started/java-quickstart.md)を完了し、Maven でライブラリをインストールするための指示に従ってください。
<!-- You also need a Java development environment that uses the [Maven](https://maven.apache.org/download.cgi) build tool. If this is your first time using the Java client library, complete our [getting started guide](../../getting-started/java-quickstart.md), and follow the instructions for installing the library with Maven. -->

コマンドラインで、次を実行します。
<!-- In the command-line, do the following: -->

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

コンソールに、トランザクションデータが表示されます。
<!-- In the console, you should see transaction data. -->

## 次のステップ
<!-- ## Next steps -->

インスピレーションについては、[アプリ設計図](root://blueprints/0.1/introduction/overview.md)をご覧ください。
<!-- Take a look at our [app blueprints](root://blueprints/0.1/introduction/overview.md) for inspiration. -->
