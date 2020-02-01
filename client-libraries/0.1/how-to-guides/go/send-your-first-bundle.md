# Go で "hello world" トランザクションを送信する
<!-- # Send a "hello world" transaction in Go -->

**このガイドでは，ゼロトークントランザクションで "hello world" メッセージを送信します．ゼロトークントランザクションは，[IOTA トークン](root://getting-started/0.1/clients/token.md)を送信することなく[タングル](root://getting-started/0.1/network/the-tangle.md)にメッセージを保存するのに役立ちます．**
<!-- **In this guide, you send a "hello world" message in a zero-value transaction. These transactions are useful for storing messages on the [Tangle](root://getting-started/0.1/network/the-tangle.md) without having to send any [IOTA tokens](root://getting-started/0.1/clients/token.md).** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには，以下のパッケージをインストールする必要があります（Go モジュールを使用している場合は，以下のパッケージを参照するだけです）．
<!-- To complete this guide, you need to install the following packages (if you're using Go modules, you just need to reference them): -->

```bash
go get github.com/iotaledger/iota.go/api
go get github.com/iotaledger/iota.go/bundle
go get github.com/iotaledger/iota.go/converter
go get github.com/iotaledger/iota.go/trinary
```

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは，以下のネットワーク設定で[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します．
<!-- In this guide, we connect to a [node](root://getting-started/0.1/network/nodes.md) on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet) with the following network settings: -->

- **[最小重量値](root://getting-started/0.1/network/minimum-weight-magnitude.md)**：9
<!-- - **[Minimum weight magnitude](root://getting-started/0.1/network/minimum-weight-magnitude.md)**: 9 -->

- **[深度](root://getting-started/0.1/transactions/depth.md)**：3
<!-- - **[Depth](root://getting-started/0.1/transactions/depth.md)**: 3 -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. パッケージをインポートします．
  <!-- 1. Import the packages -->

    ```go
    package main

    import (
        . "github.com/iotaledger/iota.go/api"
        "github.com/iotaledger/iota.go/bundle"
        "github.com/iotaledger/iota.go/converter"
        "github.com/iotaledger/iota.go/trinary"
        "fmt"
    )
    ```

2. ノードに接続します．
  <!-- 2. Connect to a node -->

    ```go
    var node = "https://nodes.devnet.thetangle.org"
    api, err := ComposeAPI(HTTPClientSettings{URI: node})
    must(err)
    ```

3. 深度と最小重量値を定義します．
  <!-- 3. Define the depth and the minimum weight magnitude -->

    ```go
    const depth = 3;
    const minimumWeightMagnitude = 9;
    ```

4. メッセージの送信先[アドレス](root://getting-started/0.1/clients/addresses.md)を定義します．
  <!-- 4. Define an [address](root://getting-started/0.1/clients/addresses.md) to which you want to send a message -->

    ```go
    const address = trinary.Trytes("ZLGVEQ9JUZZWCZXLWVNTHBDX9G9KZTJP9VEERIIFHY9SIQKYBVAHIMLHXPQVE9IXFDDXNHQINXJDRPFDXNYVAPLZAW")
    ```

    :::info:
    このアドレスは誰のものである必要はありません．アドレスが有効であるには，81[トライト](root://getting-started/0.1/introduction/ternary.md)で構成される必要があります．
    :::
    <!-- :::info: -->
    <!-- This address does not have to belong to anyone. To be valid, the address just needs to consist of 81 [trytes](root://getting-started/0.1/introduction/ternary.md). -->
    <!-- ::: -->

5. シードを定義します．
  <!-- 5. Define a seed -->

    ```go
    const seed = trinary.Trytes("JBN9ZRCOH9YRUGSWIQNZWAIFEZUBDUGTFPVRKXWPAUCEQQFS9NHPQLXCKZKRHVCCUZNF9CZZWKXRZVCWQ")
    ```

    :::info:
    これはゼロトークントランザクションであるため，シードは使用されません．ただし，ライブラリは有効なシードを想定しているため，81文字のランダムな文字列を使用します．81文字未満で構成されるシードを入力すると，ライブラリはその末尾に9を追加して81文字のシードを作成します．
    :::
    <!-- :::info: -->
    <!-- Because this is a zero-value transaction, the seed is not used. However, the library expects a valid seed, so we use a random string of 81 characters. If you enter a seed that consists of less than 81 characters, the library will append 9s to the end of it to make 81 characters. -->
    <!-- ::: -->

6. アドレスに送信する JSON メッセージを作成し，トライトに変換します．
  <!-- 6. Create a JSON message that you want to send to the address and convert it to trytes -->

    ```go
    var data = "{'message' : 'Hello world'}"
    message, err := converter.ASCIIToTrytes(data)
    must(err)
    ```

    メッセージを JSON でエンコードして，次のガイドでタングルからトランザクションを取得するときにメッセージを読みやすくします．
    <!-- We encode the message in JSON to make it easier to read the message when we get the transaction from the Tangle in the next guide. -->

    :::info:
    `AsciiToTrytes()` メソッドは[基本的な ASCII 文字](https://en.wikipedia.org/wiki/ASCII#Printable_characters)のみをサポートします．その結果，アクセントやウムラウトなどの発音区別記号や日本語はサポートされず，`INVALID_ASCII_CHARS` エラーが発生します．
    :::
    <!-- :::info: -->
    <!-- The `AsciiToTrytes()` method supports only [basic ASCII characters](https://en.wikipedia.org/wiki/ASCII#Printable_characters). As a result, diacritical marks such as accents and umlauts aren't supported and result in an `INVALID_ASCII_CHARS` error. -->
    <!-- ::: -->

7. メッセージをアドレスに送信するゼロトークントランザクションを定義します．
  <!-- 7. Define a zero-value transaction that sends the message to the address -->

    ```go
    transfers := bundle.Transfers{
        {
            Address: address,
            Value: 0,
            Message: message,
        },
    }
    ```

8. `transfers` オブジェクトからバンドルを作成するには，`transfers` オブジェクトを [`PrepareTransfers()`](https://github.com/iotaledger/iota.go/blob/master/.docs/iota.go/reference/api_prepare_transfers.md) メソッドに渡します．次に，返されたバンドルトライトを [`SendTrytes()`](https://github.com/iotaledger/iota.go/blob/master/.docs/iota.go/reference/api_send_trytes.md) メソッドに渡します．[`SendTrytes()`](https://github.com/iotaledger/iota.go/blob/master/.docs/iota.go/reference/api_send_trytes.md) メソッドは[チップ選択](root://node-software/0.1/iri/concepts/tip-selection.md)，[リモートプルーフオブワーク](root://getting-started/0.1/transactions/proof-of-work.md)を処理し，バンドルをノードに送信します．
  <!-- 8. To create a bundle from your `transfers` object, pass it to the [`PrepareTransfers()`](https://github.com/iotaledger/iota.go/blob/master/.docs/iota.go/reference/api_prepare_transfers.md) method. Then, pass the returned bundle trytes to the [`SendTrytes()`](https://github.com/iotaledger/iota.go/blob/master/.docs/iota.go/reference/api_send_trytes.md) method, which handles [tip selection](root://node-software/0.1/iri/concepts/tip-selection.md), [remote proof of work](root://getting-started/0.1/transactions/proof-of-work.md), and sending the bundle to the node -->

    ```go
    trytes, err := api.PrepareTransfers(seed, transfers, PrepareTransfersOptions{})
    must(err)

    myBundle, err := api.SendTrytes(trytes, depth, minimumWeightMagnitude)
    must(err)

    fmt.Println(bundle.TailTransactionHash(myBundle))
    ```

    コンソールに，送信したばかりのトランザクションのバンドルハッシュが表示されます．
    <!-- In the console, you should see the bundle hash of the transaction you just sent. -->

:::success:おめでとうございます:tada:
最初のゼロトークントランザクションを送信しました．トランザクションはタングルにアタッチされ，ネットワークの残りの部分に転送されます．

このテールトランザクションハッシュを使用して，タングルからトランザクションを読み取ることができます．
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just sent your first zero-value transaction. Your transaction is attached to the Tangle, and will be forwarded to the rest of the network. -->

<!-- You can use this tail transaction hash to read the transaction from the Tangle. -->
<!-- ::: -->

:::warning:
ノードは，タングルのローカルコピーから古いトランザクションを削除できます．したがって，ノードからトランザクションをリクエストするときが来るかもしれませんが，ノードは古いトランザクションをもう持っていないかもしれません．

長期間タングルにデータを保存する場合は，[自分自身のノードの実行](root://node-software/0.1/iri/how-to-guides/quickstart.md)または[クロニクル](root://node-software/0.1/chronicle/introduction/overview.md)などのパーマノードの実行をお勧めします．
:::
<!-- :::warning: -->
<!-- Nodes can delete old transactions from their local copies of the Tangle. Therefore, a time may come where you request your transaction from a node, but the node doesn't have it anymore. -->

<!-- If you want to store data on the Tangle for extended periods of time, we recommend either [running your own node](root://node-software/0.1/iri/how-to-guides/quickstart.md) or running a permanode such as [Chronicle](root://node-software/0.1/chronicle/introduction/overview.md). -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

[REPL.it ツール](https://repl.it)を使用して，ブラウザーでサンプルコードを実行できます．
<!-- We use the [REPL.it tool](https://repl.it) to allow you to run sample code in the browser. -->

緑色のボタンをクリックして，このガイドのサンプルコードを実行し，ウィンドウで結果を確認できます．
<!-- Click the green button to run the sample code in this guide and see the results in the window. -->

<iframe height="400px" width="100%" src="https://repl.it/@jake91/Send-a-hello-world-transaction-Go?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## 次のステップ
<!-- ## Next steps -->

トランザクションのハッシュをメモして，[タングルからトランザクションを読み取る](../go/read-transactions.md)ことでメッセージを確認できるようにします．
<!-- Make a note of the bundle hash so you can [read the transaction from the Tangle](../go/read-transactions.md) to see your message. -->

[タングルエクスプローラー](https://utils.iota.org)などのユーティリティを使用して，トランザクションを読み取ることができます．
<!-- You can also read your transaction, using a utility such as the [Tangle explorer](https://utils.iota.org). -->
