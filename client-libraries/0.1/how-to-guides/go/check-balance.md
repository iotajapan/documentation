# Go でアドレスの残高を確認する
<!-- # Check the balance of an address in Java -->

**このガイドでは，[ノード](root://getting-started/0.1/network/nodes.md)から[アドレス](root://getting-started/0.1/clients/addresses.md)の [IOTA トークン](root://getting-started/0.1/clients/token.md)の残高をリクエストします．**
<!-- **In this guide, you request the balance of [IOTA tokens](root://getting-started/0.1/clients/token.md) on [addresses](root://getting-started/0.1/clients/addresses.md) from a [node](root://getting-started/0.1/network/nodes.md).** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには，以下のパッケージをインストールする必要があります（Go モジュールを使用している場合は，以下のパッケージを参照するだけです）．
<!-- To complete this guide, you need to install the following packages (if you're using Go modules, you just need to reference them): -->

```bash
go get github.com/iotaledger/iota.go/api
go get github.com/iotaledger/iota.go/trinary
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

3. 残高を確認するアドレスを定義します．
  <!-- 3. Define the address whose balance you want to check -->

    ```go
    const address = trinary.Trytes("MBYBBFONQZPYZYZHSEZJ9EBEBAFHAZKUFSPBM9YOXJUUAMBUCQQABOWFNPEAGXIGMAVWWFZWDCZJGUTBBZYDSALMPA")
    ```

4. [`GetBalances()`](https://github.com/iotaledger/iota.go/blob/master/.docs/iota.go/reference/api_get_balances.md) メソッドを使用して，ノードにアドレスの現在の残高を尋ねます．
  <!-- 4. Use the [`GetBalances()`](https://github.com/iotaledger/iota.go/blob/master/.docs/iota.go/reference/api_get_balances.md) method to ask the node for the current balance of the address -->

    ```go
    balances, err := api.GetBalances(trinary.Hashes{address}, 100)
    must(err)
    fmt.Println("\nBalance: ", balances.Balances[0])
    ```

    コンソールに，IOTAトークンの残高が表示されます．
    <!-- In the console, you should see a balance of IOTA tokens: -->

    ```bash
    Balance: 500
    ```

:::success:おめでとうございます:tada:
アドレスの残高を確認しました．
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just checked the balance of an address. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

[REPL.it ツール](https://repl.it)を使用して，ブラウザーでサンプルコードを実行できます．
<!-- We use the [REPL.it tool](https://repl.it) to allow you to run sample code in the browser. -->

緑色のボタンをクリックして，このガイドのサンプルコードを実行し，ウィンドウで結果を確認できます．
<!-- Click the green button to run the sample code in this guide and see the results in the window. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Check-the-balance-of-an-address-Go?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## 次のステップ
<!-- ## Next steps -->

[タングル上のライブトランザクションをリッスンする](../go/listen-for-transactions.md)．
<!-- [Listen for live transactions on the Tangle](../go/listen-for-transactions.md). -->

[タングルエクスプローラー](https://utils.iota.org)などのユーティリティを使用して，アドレスの残高を確認することもできます．
<!-- You can also check the balance of an address, using a utility such as the [Tangle explorer](https://utils.iota.org). -->
