# Python でアドレスを生成する
<!-- # Generate an address in Python -->

**このガイドでは、[セキュリティレベル](root://getting-started/0.1/clients/security-levels.md)を指定して[シード](root://getting-started/0.1/clients/seeds.md)の新しいアドレスを生成する方法を学習します。**
<!-- **In this guide, you learn how to generate a new address for a [seed](root://getting-started/0.1/clients/seeds.md) with a given [security level](root://getting-started/0.1/clients/security-levels.md).** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには、次のパッケージをインストールする必要があります。
<!-- To complete this guide, you need to install the following package: -->

```bash
pip install pyota
```

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a [node](root://getting-started/0.1/network/nodes.md) on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. パッケージをインポートします。
  <!-- 1. Import the packages -->

    ```python
    from iota import Iota
    ```

2. アドレスを生成するシードを定義します。
  <!-- 2. Define a seed for which to generate an address -->

    ```python
    seed = 'PUETPSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX'
    ```

3. ノードに接続します。
  <!-- 3. Connect to a node -->

    ```python
    api = Iota('https://nodes.devnet.iota.org:443', seed, testnet = True)
    ```

    :::info:
    `testnet` 引数は[最小重量値](root://getting-started/0.1/network/minimum-weight-magnitude.md)を9に設定します。
    :::
    <!-- :::info: -->
    <!-- The `testnet` argument sets the [minimum weight magnitude](root://getting-started/0.1/network/minimum-weight-magnitude.md) to 9. -->
    <!-- ::: -->

4. アドレスに使用するセキュリティレベルを定義します。
  <!-- 4. Define the security level that you want to use for your address -->

    ```python
    security_level = 2
    ```

5. [`get_new_addresses`](https://pyota.readthedocs.io/en/latest/api.html#get-new-addresses) メソッドを使用して、未使用のアドレスを生成します。接続されたノードに、指定されたインデックスのアドレスから IOTA トークンの取り出しを行う入力トランザクションがある場合、ノードはそのアドレスが使用済みアドレスであることを認識しているため、ライブラリは次の未使用アドレスを返します。
  <!-- 5. Use the [`get_new_addresses()`](https://pyota.readthedocs.io/en/latest/api.html#get-new-addresses) method to generate an unspent address. If the connected node has an input transaction that withdraws from the address with the given index, the node knows that the address is spent, so the library returns the next unspent address. -->

    ```python
    address = api.get_new_addresses(index=0, count=1, security_level = security_level)['addresses'][0]
    ```

    指定されたインデックスから開始して、接続されたノードは、アドレスから取り出しを行う入力トランザクション（ペンディング中または確定済み）のタングルの概観を確認します。
    <!-- Starting from the given index, the connected node checks its view of the Tangle for any input transactions (pending or confirmed) that withdraw from the address. -->

    指定されたインデックスのアドレスが使用済みになるか、タングル上で指定されたインデックスのアドレスに関連付けられた入力トランザクションがある場合、ノードが未使用アドレスを検出するまでインデックスがインクリメントされます。ローカルスナップショットのため、ノードは指定されたインデックスのアドレスに関する入力トランザクションを台帳からプルーニングした可能性があります。したがって、ノードの使用済みアドレスの記録も確認する必要があります。
    <!-- If an address with the given index is spent or has any input transactions associated with it on the Tangle, the index is incremented until the node finds an unspent. Because of local snapshots, a node may have pruned these input transactions from its ledger. Therefore, we should also check the node's record of spent addresses. -->

6. 接続されたノードに応じて、アドレスが使用されたことがあるかどうかを確認します。
  <!-- 6. Check if the address has ever been spent, according to the connected node -->

    ```py
    is_spent = api.were_addresses_spent_from([address])['states'][0]

    if is_spent:
        print('Address %s is spent!' % address )
    else:
        print('Your address is: %s' % address )
    ```

    :::warning:
    ノードでアドレス応答を生成するこの方法は、アドレスに関する有効なデータを返します。アドレスをより細かく制御するには、使用済みアドレスの記録をローカルデータベースに保存することをお勧めします。
    :::
    <!-- :::warning: -->
    <!-- This way of generating addresses replies on the node to return valid data about your addresses. To have more control over your addresses, we recommend keeping a record of spent ones in your own local database. -->
    <!-- ::: -->

コンソールにアドレスが表示されます。
<!-- In the console, you should see an address. -->

```bash
Your address is: WKJDF9LVQCVKEIVHFAOMHISHXJSGXWBJFYEQPOQKSVGZZFLTUUPBACNQZTAKXR9TFVKBGYSNSPHRNKKHA
```

:::success:おめでとうございます:tada:
新しい未使用のアドレスが生成されました。このアドレスは、トランザクションを送信したい人と共有できます。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just generated a new unspent address. You can share this address with anyone who wants to send you a transaction. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

[REPL.it ツール](https://repl.it)を使用して、ブラウザーでサンプルコードを実行できます。
<!-- We use the [REPL.it tool](https://repl.it) to allow you to run sample code in the browser. -->

緑色のボタンをクリックして、このガイドのサンプルコードを実行し、ウィンドウで結果を確認できます。
<!-- Click the green button to run the sample code in this guide and see the results in the window. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Generate-an-address-Python?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## 次のステップ
<!-- ## Next steps -->

[新しいアドレスにテスト IOTA トークンを送信する](../python/transfer-iota-tokens.md)。
<!-- [Send test IOTA tokens to your new address](../python/transfer-iota-tokens.md). -->
