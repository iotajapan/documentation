# Go でアドレスを生成する
<!-- # Generate an address in Go -->

**このガイドでは，[セキュリティレベル](root://getting-started/0.1/clients/security-levels.md)を指定して[シード](root://getting-started/0.1/clients/seeds.md)の新しいアドレスを生成する方法を学習します．**
<!-- **In this guide, you learn how to generate a new address for a [seed](root://getting-started/0.1/clients/seeds.md) with a given [security level](root://getting-started/0.1/clients/security-levels.md).** -->

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
<!-- In this guide, we connect to a [node](root://getting-started/0.1/network/nodes.md) on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

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

3. アドレスに使用するセキュリティレベルを定義します．
  <!-- 3. Define the security level that you want to use for your address -->

    ```go
    const securityLevel = 2;
    ```

4. 生成するアドレスのシードを定義します．
  <!-- 4. Define a seed for which to generate an address -->

    ```go
    const seed = trinary.Trytes("PUETPSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX")
    ```

5. [`GetNewAddress()`](https://github.com/iotaledger/iota.go/blob/master/.docs/iota.go/reference/api_get_new_address.md) メソッドを使用して，未使用のアドレスを生成します．
  <!-- 5. Use the [`GetNewAddress()`](https://github.com/iotaledger/iota.go/blob/master/.docs/iota.go/reference/api_get_new_address.md) method to generate an unspent address -->

    ```go
    addresses, err := api.GetNewAddress(seed, GetNewAddressOptions{Security:securityLevel})
    must(err)

    fmt.Println("\nYour address is: ", addresses[0])
    ```

接続されたノードは，指定されたインデックスから開始して，次の操作を実行してアドレスが使用されているかどうかを確認します．
<!-- Starting from the given index, the connected node checks if the address is spent by doing the following: -->

- アドレスから IOTA トークンを取り出す入力トランザクションがタングルの概観内にあるかどうかを検索する．
<!-- - Search its view of the Tangle for input transactions that withdraw from the address -->
- 使用済みアドレスのリスト内にアドレスがあるかどうかを検索する．
<!-- - Search for the address in the list of spent addresses -->

指定されたインデックスのアドレスが使用されていると，ノードは使用されていないアドレスを見つけるまでインデックスをインクリメントします．
<!-- If an address with the given index is spent, the index is incremented until the node finds one that isn't spent. -->

:::warning:
ノードでアドレス応答を生成するこの方法は，アドレスに関する有効なデータを返します．アドレスをより細かく制御するには，[アカウントモジュール](../../account-module/introduction/overview.md)を使用して，ローカルデータベース内に保存した使用済みアドレスを追跡することをお勧めします．
:::
<!-- :::warning: -->
<!-- This way of generating addresses replies on the node to return valid data about your addresses. To have more control over your addresses, we recommend using the [account module](../../account-module/introduction/overview.md) to keep track of spent addresses in your own local database. -->
<!-- ::: -->

コンソールにアドレスが表示されます．
<!-- In the console, you should see an address. -->

```bash
Your address is: WKJDF9LVQCVKEIVHFAOMHISHXJSGXWBJFYEQPOQKSVGZZFLTUUPBACNQZTAKXR9TFVKBGYSNSPHRNKKHA
```

:::success:おめでとうございます:tada:
新しい未使用のアドレスが生成されました．このアドレスは，トランザクションを送信したい人と共有可能です．
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just generated a new unspent address. You can share this address with anyone who wants to send you a transaction. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

[REPL.it ツール](https://repl.it)を使用して，ブラウザーでサンプルコードを実行できます．
<!-- We use the [REPL.it tool](https://repl.it) to allow you to run sample code in the browser. -->

緑色のボタンをクリックして，このガイドのサンプルコードを実行し，ウィンドウで結果を確認できます．
<!-- Click the green button to run the sample code in this guide and see the results in the window. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Generate-an-address-Go?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## 次のステップ
<!-- ## Next steps -->

[新しいアドレスにテスト IOTA トークンを送信する](../go/transfer-iota-tokens.md)．
<!-- [Send test IOTA tokens to your new address](../go/transfer-iota-tokens.md). -->
