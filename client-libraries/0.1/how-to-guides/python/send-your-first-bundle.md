# Python で "hello world" トランザクションを送信する
<!-- # Send a "hello world" transaction in Python -->

**このガイドでは、ゼロトークントランザクションを用いて "hello world" メッセージを送信します。ゼロトークントランザクションは、[IOTA トークン](root://getting-started/0.1/clients/token.md)を送信することなく[タングル](root://getting-started/0.1/network/the-tangle.md)にメッセージを保存するのに役立ちます。**
<!-- **In this guide, you send a "hello world" message in a zero-value transaction. These transactions are useful for storing messages on the [Tangle](root://getting-started/0.1/network/the-tangle.md) without having to send any [IOTA tokens](root://getting-started/0.1/clients/token.md).** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには、次のパッケージをインストールする必要があります。
<!-- To complete this guide, you need to install the following package: -->

```bash
pip install pyota
```

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、以下のネットワーク設定で[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a [node](root://getting-started/0.1/network/nodes.md) on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet) with the following network settings: -->

- **[最小重量値](root://getting-started/0.1/network/minimum-weight-magnitude.md)**: 9
<!-- - **[Minimum weight magnitude](root://getting-started/0.1/network/minimum-weight-magnitude.md)**: 9 -->

- **[深度](root://getting-started/0.1/transactions/depth.md)**: 3
<!-- - **[Depth](root://getting-started/0.1/transactions/depth.md)**: 3 -->

1. パッケージをインポートします。
  <!-- 1. Import the packages -->

    ```python
    from iota import Iota
    from iota import ProposedTransaction
    from iota import Address
    from iota import Tag
    from iota import TryteString
    ```

2. ノードに接続します。
  <!-- 2. Connect to a node -->

    ```python
    api = Iota('https://nodes.devnet.iota.org:443', testnet = True)
    ```

    :::info:
    `testnet` 引数は[最小重量値](root://getting-started/0.1/network/minimum-weight-magnitude.md)を9に設定します。
    :::
    <!-- :::info: -->
    <!-- The `testnet` argument sets the [minimum weight magnitude](root://getting-started/0.1/network/minimum-weight-magnitude.md) to 9. -->
    <!-- ::: -->

3. メッセージを送信する[アドレス](root://getting-started/0.1/clients/addresses.md)を定義します。
  <!-- 3. Define an [address](root://getting-started/0.1/clients/addresses.md) to which you want to send a message -->

    ```python
    address = 'ZLGVEQ9JUZZWCZXLWVNTHBDX9G9KZTJP9VEERIIFHY9SIQKYBVAHIMLHXPQVE9IXFDDXNHQINXJDRPFDXNYVAPLZAW'
    ```

    :::info:
    このアドレスは誰のものである必要はありません。有効にするために、アドレスは81[トライト](root://getting-started/0.1/introduction/ternary.md)で構成される必要があるだけです。
    :::
    <!-- :::info: -->
    <!-- This address does not have to belong to anyone. To be valid, the address just needs to consist of 81 [trytes](root://getting-started/0.1/introduction/ternary.md). -->
    <!-- ::: -->

4. アドレスに送信するメッセージを定義し、トライトに変換します。
  <!-- 4. Define a message that you want to send to the address and convert it to trytes -->

    ```python
    message = TryteString.from_unicode('Hello world')
    ```

    :::info:
    `from_unicode()` メソッドは UTF-8 文字をサポートしています。他の公式ライブラリで標準化されるまで[基本的な ASCII 文字](https://en.wikipedia.org/wiki/ASCII#Printable_characters)のみを使用することをお勧めします。
    :::
    <!-- :::info: -->
    <!-- Although the `from_unicode()` method supports UTF-8 characters, we recommended using only [basic ASCII characters](https://en.wikipedia.org/wiki/ASCII#Printable_characters) until it has been standardized across the other official libraries. -->
    <!-- ::: -->

5. メッセージをアドレスに送信するゼロトークントランザクションを定義します。
  <!-- 5. Define a zero-value transaction that sends the message to the address -->

    ```python
    tx = ProposedTransaction(
      address = Address(address),
      message = message,
      value = 0
    )
    ```

    :::info:
    Python ライブラリは、提案されたトランザクションオブジェクトと通常のトランザクションオブジェクトを区別します。提案されたトランザクションオブジェクトは、まだタングルにアタッチされていないため、編集できるオブジェクトです。対照的に、通常のトランザクションオブジェクトは既にタングルにアタッチされているためイミュータブルです。
    :::
    <!-- :::info: -->
    <!-- The Python library makes a disctinction between proposed and regular transaction objects. Proposed transaction objects are those that you can edit because they are not yet attached to the Tangle. In contrast, regular transaction objects are immutable because they are already attached to the Tangle. -->
    <!-- ::: -->

6. `ProposedTransaction` オブジェクトを [`send_transfer()`](https://pyota.readthedocs.io/en/latest/api.html#send-transfer) メソッドに渡して[チップ選択](root://node-software/0.1/iri/concepts/tip-selection.md)、[リモートプルーフオブワーク](root://getting-started/0.1/transactions/proof-of-work.md)を行い、バンドルをノードに送信します。
  <!-- 6. Pass your `ProposedTransaction` object to the [`send_transfer()`](https://pyota.readthedocs.io/en/latest/api.html#send-transfer) method to do [tip selection](root://node-software/0.1/iri/concepts/tip-selection.md), [remote proof of work](root://getting-started/0.1/transactions/proof-of-work.md), and to send the bundle to the node -->

    ```python
    result = api.send_transfer(transfers = [tx])

    print(result['bundle'].tail_transaction.hash)
    ```


    コンソールに、送信したばかりのバンドルの末尾トランザクションが表示されます。
    <!-- In the console, you should see the tail transaction hash of the bundle you just sent. -->

:::success:おめでとうございます:tada:
最初のゼロトークントランザクションを送信しました。トランザクションはタングルにアタッチされ、ネットワークの残りの部分に転送されます。

この末尾トランザクションハッシュを使用して、タングルからトランザクションを読み取ることができます。
:::

<!-- :::success:Congratulations :tada: -->
<!-- You've just sent your first zero-value transaction. Your transaction is attached to the Tangle, and will be forwarded to the rest of the network. -->

<!-- You can use this tail transaction hash to read the transaction from the Tangle. -->
<!-- ::: -->

:::warning:
ノードは、タングルのローカルコピーから古いトランザクションを削除できます。したがって、ノードからトランザクションをリクエストするときが来るかもしれませんが、ノードは古いトランザクションをもう持っていないかもしれません。

長期間タングルにデータを保存する場合は、[自分自身のノードの実行](root://node-software/0.1/iri/how-to-guides/quickstart.md)または[クロニクル](root://node-software/0.1/chronicle/introduction/overview.md)などのパーマノードの実行をお勧めします。
:::
<!-- :::warning: -->
<!-- Nodes can delete old transactions from their local copies of the Tangle. Therefore, a time may come where you request your transaction from a node, but the node doesn't have it anymore. -->

<!-- If you want to store data on the Tangle for extended periods of time, we recommend either [running your own node](root://node-software/0.1/iri/how-to-guides/quickstart.md) or running a permanode such as [Chronicle](root://node-software/0.1/chronicle/introduction/overview.md). -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

[REPL.it ツール](https://repl.it)を使用して、ブラウザーでサンプルコードを実行できます。
<!-- We use the [REPL.it tool](https://repl.it) to allow you to run sample code in the browser. -->

緑色のボタンをクリックして、このガイドのサンプルコードを実行し、ウィンドウで結果を確認できます。
<!-- Click the green button to run the sample code in this guide and see the results in the window. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Send-a-hello-world-transaction-Python?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## 次のステップ
<!-- ## Next steps -->

末尾トランザクションハッシュを書き留めて、[タングルからトランザクションを読み取り](../python/read-transactions.md)、メッセージを表示する。
<!-- Make a note of the tail transaction hash so you can [read the transaction from the Tangle](../python/read-transactions.md) to see your message. -->

[タングルエクスプローラー](https://utils.iota.org)などのユーティリティを使用して、トランザクションを読み取ることができます。
<!-- You can also read your transaction, using a utility such as the [Tangle explorer](https://utils.iota.org). -->
