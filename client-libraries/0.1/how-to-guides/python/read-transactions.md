# Python でタングルからトランザクションを読み取る
<!-- # Read transactions from the Tangle in Python -->

**このガイドでは，[ノード](root://getting-started/0.1/network/nodes.md)に末尾トランザクションハッシュを与えることで，タングルから "hello world" [トランザクション](root://getting-started/0.1/transactions/transactions.md)を読み取ります．**
<!-- **In this guide, you read your "hello world" [transaction](root://getting-started/0.1/transactions/transactions.md) from the Tangle by giving a [node](root://getting-started/0.1/network/nodes.md) your tail transaction hash.** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには，次のパッケージをインストールする必要があります．
<!-- To complete this guide, you need to install the following package: -->

```bash
pip install pyota
```

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは，[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します．
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. パッケージをインポートします．
  <!-- 1. Import the packages -->

    ```python
    from iota import Iota
    ```

2. ノードに接続します．
  <!-- 2. Connect to a node -->

    ```python
    api = Iota('https://nodes.devnet.iota.org:443', testnet = True)
    ```

3. バンドルの末尾トランザクションハッシュを定義します．
  <!-- 3. Define the tail transaction hash of the bundle -->

    ```python
    tail_transaction_hash = 'ZFICKFQXASUESAWLSFFIWHVOAJCSJHJNXMRC9AJSIOTNGNKEWOFLECHPULLJSNRCNJPYNZEC9VGOSV999'
    ```

    :::info:
    `signatureMessageFragment` フィールドはハッシュの一部であるため，テールトランザクションハッシュを使用します．したがって，トランザクション内のメッセージはイミュータブルです．

    バンドルハッシュを使用する場合，誰でもテールトランザクションのメッセージを変更し，バンドルのコピーをタングルにアタッチできるため，別のメッセージが表示される場合があります．
    :::

    <!-- :::info: -->
    <!-- We use the tail transaction hash because the `signatureMessageFragment` field is part of the hash. Therefore, the message in the transaction is immutable. -->

    <!-- If you were to use the bundle hash, you may see a different message because anyone can change the message in the tail transaction and attach a copy of the bundle to the Tangle. -->
    <!-- ::: -->

4. [`get_bundles()`](https://pyota.readthedocs.io/en/latest/extended_api.html?highlight=getbundles#get-bundles) メソッドを使用して，末尾トランザクションのバンドル内のすべてのトランザクションを取得します．
  <!-- 4. Use the [`get_bundles()`](https://pyota.readthedocs.io/en/latest/extended_api.html?highlight=getbundles#get-bundles) method to get all transactions in the tail transaction's bundle -->

    ```python
    bundle = api.get_bundles(tail_transaction_hash)
    ```

5. バンドルの最初のトランザクションのメッセージを ASCII 文字に変換し，コンソールに出力します．
  <!-- 5. Convert the message in the bundle's first transaction to ASCII characters and print it to the console -->

    ```python
    message = bundle['bundles'][0].tail_transaction.signature_message_fragment
    print(message.decode())
    ```

    コンソールにメッセージが表示されます．
    <!-- In the console, you should see your message: -->

    ```bash
    Hello world
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

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Read-a-transaction-from-the-Tangle-Python?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## 次のステップ
<!-- ## Next steps -->

[新しいアドレスを生成する](../python/generate-an-address.md)．
<!-- [Generate a new address](../python/generate-an-address.md). -->
