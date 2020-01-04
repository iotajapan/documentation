# Python でライブトランザクションをリッスンする
<!-- # Listen for live transactions in Python -->

**このガイドでは、[IRI ノードソフトウェア](root://node-software/0.1/iri/introduction/overview.md)を実行する[ノード](root://getting-started/0.1/network/nodes.md)の[ゼロメッセージキュー（ZMQ）](https://zeromq.org/)にサブスクライブして、タングル上の確定済みトランザクションをリッスンします。**
<!-- **In this guide, you listen to the Tangle for confirmed transactions by subscribing to the [zero message queue (ZMQ)](https://zeromq.org/) on [nodes](root://getting-started/0.1/network/nodes.md) that run the [IRI node software](root://node-software/0.1/iri/introduction/overview.md).** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには、次のパッケージをインストールする必要があります。
<!-- To complete this guide, you need to install the following package: -->

```bash
pip install zmq
```

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. パッケージをインポートします。
  <!-- 1. Import the packages -->

    ```python
    import zmq
    ```

2. ソケットをノードの ZMQ ポートに接続します。
  <!-- 2. Connect the socket to a node's ZMQ port -->

    ```python
    context = zmq.Context()
	socket = context.socket(zmq.SUB)
    socket.connect('tcp://zmq.devnet.iota.org:5556')
    ```

3. [`sn`]イベントをサブスクライブして、確認済みのトランザクションを確認します。
  <!-- 3. Subscribe to the [`sn`](root://node-software/0.1/iri/references/zmq-events.md) event to see confirmed transactions -->

    ```python
    socket.subscribe('sn')
    print ("Socket connected")
    ```

4. ノードが返すイベントデータを処理します。
  <!-- 4. Process the event data that the node returns -->

    ```python
    while True:
        print ("Waiting for events from the node")
        message = socket.recv()
        data = message.split()
        print ("Transaction confirmed by milestone index: ", data[1])
        print ("Transaction hash: ", data[2])
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

[REPL.it ツール](https://repl.it)を使用して、ブラウザーでサンプルコードを実行できます。
<!-- We use the [REPL.it tool](https://repl.it) to allow you to run sample code in the browser. -->

緑色のボタンをクリックして、このガイドのサンプルコードを実行し、ウィンドウで結果を確認できます。
<!-- Click the green button to run the sample code in this guide and see the results in the window. -->

:::info:
ノードからデータを受信するのに1〜2分かかる場合があります。
:::
<!-- :::info: -->
<!-- It may take a minute or two to receive data from the node. -->
<!-- ::: -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/ZMQ-example-Python?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## 次のステップ
<!-- ## Next steps -->

インスピレーションについては、[アプリ設計図](root://blueprints/0.1/introduction/overview.md)をご覧ください。
<!-- Take a look at our [app blueprints](root://blueprints/0.1/introduction/overview.md) for inspiration -->
