# Python でアドレスの残高を確認する
<!-- # Check the balance of an address in Python -->

**このガイドでは、シードの[アドレス](root://getting-started/0.1/clients/addresses.md)の[IOTA トークン](root://getting-started/0.1/clients/token.md)の合計残高を[ノード](root://getting-started/0.1/network/nodes.md)から要求します。**
<!-- **In this guide, you request the total balance of [IOTA tokens](root://getting-started/0.1/clients/token.md) on a seed's [addresses](root://getting-started/0.1/clients/addresses.md) from a [node](root://getting-started/0.1/network/nodes.md).** -->

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
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. パッケージをインストールします。
  <!-- 1. Import the packages -->

    ```python
    from iota import Iota
    ```

2. シードを定義します。このシードを、テスト IOTA トークンを持つアドレスを所有するシードに置き換えます。
  <!-- 2. Define your seed. Replace this seed with one that owns an address with test IOTA tokens -->

    ```python
    seed = 'JBN9ZRCOH9YRUGSWIQNZWAIFEZUBDUGTFPVRKXWPAUCEQQFS9NHPQLXCKZKRHVCCUZNF9CZZWKXRZVCWQ'
    ```

3. ノードに接続します。
  <!-- 3. Connect to a node -->

    ```python
    api = Iota('https://nodes.devnet.iota.org:443', seed)
    ```

4. [`get_account_data()`](https://pyota.readthedocs.io/en/latest/api.html#get-account-data)メソッドを使用して、シードのアドレスの現在の残高をノードに要求します。
  <!-- 4. Use the [`get_account_data()`](https://pyota.readthedocs.io/en/latest/api.html#get-account-data) method to ask the node for the current balance of the seed's addresses -->

    ```python
    balance = api.get_account_data()
    print('The balance for your seed is: ', balance['balance'])
    ```

    :::info:
    [`get_account_data()`](https://pyota.readthedocs.io/en/latest/api.html#get-account-data) メソッドはインデックス0から開始し、アドレスから IOTA トークンの取り出しを行う1つ以上の入力トランザクションがあるかどうかをノードに尋ねます。ノードに取り出しを行う入力トランザクションがある場合、インデックスが増分され、このプロセスは未使用のアドレスが見つかるまで継続されます。
    :::
    <!-- :::info: -->
    <!-- This method starts from index 0 and asks the node if it has one or more input transactions that withdraw from the address. If the node has input transactions that withdraw from the address, the index is incremented and this process continues until an unspent address is found. -->
    <!-- ::: -->

    コンソールに、IOTA トークンのバランスが表示されます。
    <!-- In the console, you should see a balance of IOTA tokens: -->

    ```bash
    The balance for your seed is: 500
    ```

:::success:おめでとうございます:tada:
シードの残高を確認しました。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just checked the seed's balance. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

[REPL.it ツール](https://repl.it)を使用して、ブラウザーでサンプルコードを実行できます。
<!-- We use the [REPL.it tool](https://repl.it) to allow you to run sample code in the browser. -->

緑色のボタンをクリックして、このガイドのサンプルコードを実行し、ウィンドウで結果を確認できます。
<!-- Click the green button to run the sample code in this guide and see the results in the window. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Check-the-balance-of-an-address-Python?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## 次のステップ
<!-- ## Next steps -->

[タングル上のライブトランザクションをリッスンする](../python/listen-for-transactions.md)。
<!-- [Listen for live transactions on the Tangle](../python/listen-for-transactions.md). -->

[タングルエクスプローラー](https://utils.iota.org)などのユーティリティを使用して、アドレスの残高を確認することもできます。
<!-- You can also check the balance of an address, using a utility such as the [Tangle explorer](https://utils.iota.org). -->
