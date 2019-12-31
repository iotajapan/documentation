# Go でライブトランザクションをリッスンする
<!-- # Listen for live transactions in Go -->

**このガイドでは、[IRI ノードソフトウェア](root://node-software/0.1/iri/introduction/overview.md)を実行する[ノード](root://getting-started/0.1/network/nodes.md)の[ゼロメッセージキュー（ZMQ）](https://zeromq.org/)にサブスクライブして、タングル上の最近のトランザクションをリッスンします。**
<!-- **In this guide, you listen to the Tangle for recent transactions by subscribing to the [zero message queue (ZMQ)](https://zeromq.org/) on [nodes](root://getting-started/0.1/network/nodes.md) that run the [IRI node software](root://node-software/0.1/iri/introduction/overview.md).** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには、次のパッケージをインストールする必要があります（Go モジュールを使用している場合は、このパッケージを参照する必要があります）。
<!-- To complete this guide, you need to install the following package (if you're using Go modules, you just need to reference this package): -->

```bash
go get github.com/pebbe/zmq4
```

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)上のノードに接続します。
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. パッケージをインポートします。
  <!-- 1. Import the packages -->

    ```go
    package main

    import (
    	zmq "github.com/pebbe/zmq4"
    	"fmt"
    	"strings"
    )
    ```

2. ソケットをノードの ZMQ ポートに接続します。
  <!-- 2. Connect the socket to a node's ZMQ port -->

    ```go
    client, _ := zmq.NewSocket(zmq.SUB)

    defer client.Close()

    client.Connect("tcp://zmq.devnet.iota.org:5556")
    ```

3. [`tx` および `sn`](root://node-software/0.1/iri/references/zmq-events.md) イベントをサブスクライブして、すべてのトランザクションと確定済みのトランザクションを表示します。
  <!-- 3. Subscribe to the [`tx` and `sn`](root://node-software/0.1/iri/references/zmq-events.md) events to see all transactions and confirmed transactions -->

    ```go
    client.SetSubscribe("tx")
    client.SetSubscribe("sn")
    ```

4. ノードが返すイベントデータを処理します。
  <!-- 4. Process the event data that the node returns -->

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

以下のサンプルコードを使用して、ZMQを開始します。
<!-- Use this sample code to get started with the ZMQ. -->

```go
package main

import (
	zmq "github.com/pebbe/zmq4"
	"fmt"
	"strings"
)

func main() {
	client, _ := zmq.NewSocket(zmq.SUB)

	// プログラムを停止した後、接続が閉じられていることを確認してください
	defer client.Close()

	// デブネットノードの ZMQ アドレスに接続します
	client.Connect("tcp://zmq.devnet.iota.org:5556")

	// tx とsn（確定済み tx）の両方のトピックを購読します
	client.SetSubscribe("tx")
	client.SetSubscribe("sn")

	// メッセージのループを続けます
	for {
		msg, _ := client.RecvMessage(0)
		for _, str := range msg {

			// スペース文字でフィールドを分割します
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

## 次のステップ
<!-- ## Next steps -->

インスピレーションについては、[アプリ設計図](root://blueprints/0.1/introduction/overview.md)をご覧ください。
<!-- Take a look at our [app blueprints](root://blueprints/0.1/introduction/overview.md) for inspiration -->
