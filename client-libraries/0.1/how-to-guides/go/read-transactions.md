# Go でタングルからトランザクションを読み取る
<!-- # Read transactions from the Tangle in Go -->

**このガイドでは，[ノード](root://getting-started/0.1/network/nodes.md)にテールトランザクションハッシュを与えることで，タングルから "hello world" [トランザクション](root://getting-started/0.1/transactions/transactions.md)を読み取ります．**
<!-- **In this guide, you read your "hello world" [transaction](root://getting-started/0.1/transactions/transactions.md) from the Tangle by giving a [node](root://getting-started/0.1/network/nodes.md) your tail transaction hash.** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには，以下のパッケージをインストールする必要があります（Go モジュールを使用している場合は，以下のパッケージを参照するだけです）．
<!-- To complete this guide, you need to install the following packages (if you're using Go modules, you just need to reference them): -->

```bash
go get github.com/iotaledger/iota.go/api
go get github.com/iotaledger/iota.go/trinary
go get github.com/iotaledger/iota.go/transaction
```

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは，[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します．
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. パッケージをインポートします．
  <!-- 1. Import the packages -->

    ```go
    package main

    import (
        . "github.com/iotaledger/iota.go/api"
        "github.com/iotaledger/iota.go/trinary"
        "github.com/iotaledger/iota.go/transaction"
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

3. トランザクションのフィルタリングに使用するテールトランザクションハッシュを定義します．
  <!-- 3. Define the tail transaction hash that you want to use to filter transactions -->

    ```go
    const tailTransactionHash = trinary.Trytes("RXPDFDAUJHMSYBSWUHHNJM9YTOACXYYIRSIEIVUOGQIRUUAHQFNXQBURQJHLXWYLZLWNRMVIABKC9C999")
    ```

    :::info:
    `signatureMessageFragment` フィールドはハッシュの一部であるため，テールトランザクションハッシュを使用します．したがって，トランザクション内のメッセージはイミュータブルです．

    バンドルハッシュを使用する場合，誰でもテールトランザクション内のメッセージを変更し，バンドルのコピーをタングルにアタッチできるため，別のメッセージが表示される場合があります．
    :::

    <!-- :::info: -->
    <!-- We use the tail transaction hash because the `signatureMessageFragment` field is part of the hash. Therefore, the message in the transaction is immutable. -->

    <!-- If you were to use the bundle hash, you may see a different message because anyone can change the message in the tail transaction and attach a copy of the bundle to the Tangle. -->
    <!-- ::: -->

4. [`GetBundle()`](https://github.com/iotaledger/iota.go/blob/master/.docs/iota.go/reference/api_get_bundle.md) メソッドを使用して，テールトランザクションのバンドル内のすべてのトランザクションを取得します．次に，[`ExtractJSON()`](https://github.com/iotaledger/iota.go/blob/master/.docs/iota.go/reference/transaction_extract_j_s_o_n.md) メソッドを使用して，トランザクションの `signatureMessageFragment` フィールドの JSON メッセージをデコードし，コンソールに出力します．
  <!-- 4. Use the [`GetBundle()`](https://github.com/iotaledger/iota.go/blob/master/.docs/iota.go/reference/api_get_bundle.md) method to get all transactions in the tail transaction's bundle. Then, use the [`ExtractJSON()`](https://github.com/iotaledger/iota.go/blob/master/.docs/iota.go/reference/transaction_extract_j_s_o_n.md) method to decode the JSON messages in the `signatureMessageFragment` fields of the transactions and print them to the console -->

    ```go
    bundle, err := api.GetBundle(tailTransactionHash)
    must(err)

    jsonMsg, err := transaction.ExtractJSON(bundle)
    must(err)
    fmt.Println(jsonMsg)
    ```

    コンソールに，JSON メッセージが表示されます．
    <!-- In the console, you should see your JSON message: -->

    ```json
    {"message": "Hello world"}
    ```

:::success:おめでとうございます:tada:
タングルからトランザクションを見つけて読み取りました．
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just found and read a transaction from the Tangle. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

[REPL.it ツール](https://repl.it)を使用して，ブラウザーでサンプルコードを実行できます．
<!-- We use the [REPL.it tool](https://repl.it) to allow you to run sample code in the browser. -->

緑色のボタンをクリックして，このガイドのサンプルコードを実行し，ウィンドウで結果を確認できます．
<!-- Click the green button to run the sample code in this guide and see the results in the window. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Read-a-transaction-from-the-Tangle-Go?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## 次のステップ
<!-- ## Next steps -->

[アドレスを生成する](../go/generate-an-address.md)．
<!-- [Generate a new address](../go/generate-an-address.md). -->
