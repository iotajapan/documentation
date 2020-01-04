# Python でタングル上のトランザクションを読む
<!-- # Read transactions on the Tangle in Python -->

**このガイドでは、[ノード](root://getting-started/0.1/network/nodes.md)に接続し、バンドルハッシュでフィルタリングするように要求することで、[トランザクション](root://getting-started/0.1/transactions/transactions.md)をタングルから取得します。次に、トランザクション内のメッセージをデコードして、コンソールに出力します。**
<!-- **In this guide, you get [transactions](root://getting-started/0.1/transactions/transactions.md) from the Tangle by connecting to a [node](root://getting-started/0.1/network/nodes.md) and asking it to filter them by their bundle hash. Then, you decode the message in the transaction and print it to the console.** -->

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

1. パッケージをインポートします。
  <!-- 1. Import the packages -->

    ```python
    from iota import Iota
    from iota import Transaction
    ```

2. ノードに接続します。
  <!-- 2. Connect to a node -->

    ```python
    api = Iota('https://nodes.devnet.iota.org:443', testnet = True)
    ```

3. トランザクションのフィルタリングに使用するバンドルハッシュを定義します。
  <!-- 3. Define the bundle hash that you want to use to filter transactions -->

    ```python
    bundle = 'IYPHGPIAO99XFAIBRXB9BEQLTZBCXTAGHUXL9UUXGGHHNKEBVEANQIBOALKSO9KLHTEEZXXPB9IOBK9RB'
    ```

4. [`find_transaction_objects()`](https://pyota.readthedocs.io/en/latest/api.html#find-transaction-objects) メソッドを使用して、 `bundle` フィールドの値によってトランザクションを取得します。
  <!-- 4. Use the [`find_transaction_objects()`](https://pyota.readthedocs.io/en/latest/api.html#find-transaction-objects) method to get transactions by the value of their `bundle` field -->

    ```python
    transactions = api.find_transaction_objects(bundles=[bundle])
    ```

5. バンドルの最初のトランザクションのメッセージを ASCII 文字に変換し、コンソールに出力します。
  <!-- 5. Convert the message in the bundle's first transaction to ASCII characters and print it to the console -->

    ```python
    # python api は 'transactions' を含む python 辞書を返すことに注意してください
    # キーとその値としてのトランザクションオブジェクトのリストです
    message = transactions['transactions'][0].signature_message_fragment
    print(message.decode())
    ```

    コンソールにメッセージが表示されます。
    <!-- In the console, you should see your message: -->

    ```bash
    Hello world
    ```

:::success:おめでとうございます:tada:
タングルでトランザクションを見つけて読み取りました。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just found and read a transaction on the Tangle. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

[REPL.it ツール](https://repl.it)を使用して、ブラウザーでサンプルコードを実行できます。
<!-- We use the [REPL.it tool](https://repl.it) to allow you to run sample code in the browser. -->

緑色のボタンをクリックして、このガイドのサンプルコードを実行し、ウィンドウで結果を確認できます。
<!-- Click the green button to run the sample code in this guide and see the results in the window. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Read-a-transaction-on-the-Tangle-Python?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## 次のステップ
<!-- ## Next steps -->

[新しいアドレスを生成する](../python/generate-an-address.md)。
<!-- [Generate a new address](../python/generate-an-address.md). -->
