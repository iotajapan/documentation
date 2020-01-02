# Node.js で "hello world" トランザクションを送信する
<!-- # Send a "hello world" transaction in Node.js -->

**このガイドでは、ゼロトークントランザクションを用いて "hello world" メッセージを送信します。ゼロトークントランザクションは、[IOTA トークン](root://getting-started/0.1/clients/token.md)を送信することなく[タングル](root://getting-started/0.1/network/the-tangle.md)にメッセージを保存するのに役立ちます。**
<!-- **In this guide, you send a "hello world" message in a zero-value transaction. These transactions are useful for storing messages on the [Tangle](root://getting-started/0.1/network/the-tangle.md) without having to send any [IOTA tokens](root://getting-started/0.1/clients/token.md).** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには、次のパッケージをインストールする必要があります。
<!-- To complete this guide, you need to install the following packages: -->

--------------------
### npm
```bash
npm install @iota/core @iota/converter
```
---
### Yarn
```bash
yarn add @iota/core @iota/converter
```
--------------------

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、以下のネットワーク設定で[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a [node](root://getting-started/0.1/network/nodes.md) on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet) with the following network settings: -->

- **[最小重量値](root://getting-started/0.1/network/minimum-weight-magnitude.md)**: 9
<!-- - **[Minimum weight magnitude](root://getting-started/0.1/network/minimum-weight-magnitude.md)**: 9 -->

- **[深さ](root://getting-started/0.1/transactions/depth.md)**: 3
<!-- - **[Depth](root://getting-started/0.1/transactions/depth.md)**: 3 -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. パッケージをリクワイアします。
  <!-- 1. Require the packages -->

    ```js
    const Iota = require('@iota/core');
    const Converter = require('@iota/converter');
    ```

2. ノードに接続します。
  <!-- 2. Connect to a node -->

    ```js
    const iota = Iota.composeAPI({
    provider: 'https://nodes.devnet.iota.org:443'
    });
    ```

3. 深さと最小重量値を定義します。
  <!-- 3. Define the depth and the minimum weight magnitude -->

    ```js
    const depth = 3;
    const minimumWeightMagnitude = 9;
    ```

4. メッセージの送信先の[アドレス](root://getting-started/0.1/clients/addresses.md)を定義します。
  <!-- 4. Define an [address](root://getting-started/0.1/clients/addresses.md) to which you want to send a message -->

    ```js
    const address =
    'HEQLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWOR99D';
    ```

    :::info:
    このアドレスは誰のものである必要はありません。有効にするために、アドレスは81[トライト](root://getting-started/0.1/introduction/ternary.md)で構成される必要があるだけです。
    :::
    <!-- :::info: -->
    <!-- This address does not have to belong to anyone. To be valid, the address just needs to consist of 81 [trytes](root://getting-started/0.1/introduction/ternary.md). -->
    <!-- ::: -->

5. シードを定義します。
  <!-- 5. Define a seed -->

    ```js
    const seed =
    'PUEOTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX';
    ```

    :::info:
    これはゼロトークントランザクションであるため、シードとセキュリティレベルは使用されません。ただし、ライブラリは有効なシードを想定しているため、81文字のランダムな文字列を使用します。81文字未満で構成されるシードを入力すると、ライブラリはその末尾に9を追加して81文字を作成します。
    :::
    <!-- :::info: -->
    <!-- Because this is a zero-value transaction, the seed is not used. However, the library expects a valid seed, so we use a random string of 81 characters. If you enter a seed that consists of less than 81 characters, the library will append 9s to the end of it to make 81 characters. -->
    <!-- ::: -->

6. アドレスに送信する JSON メッセージを作成し、トライトに変換します。
  <!-- 6. Create a JSON message that you want to send to the address and convert it to trytes -->

    ```js
    const message = JSON.stringify({"message": "Hello world"});
    const messageInTrytes = Converter.asciiToTrytes(message);
    ```

    メッセージを JSON にエンコードして、次のガイドでタングルからトランザクションを取得するときにメッセージを読みやすくします。
    <!-- We encode the message in JSON to make it easier to read the message when we get the transaction from the Tangle in the next guide. -->

    :::info:
    `asciiToTrytes()` メソッドは[基本的な ASCII 文字](https://en.wikipedia.org/wiki/ASCII#Printable_characters)のみをサポートします。その結果、アクセントやウムラウトなどの発音区別記号や日本語はサポートされず、`INVALID_ASCII_CHARS` エラーが発生します。
    :::
    <!-- :::info: -->
    <!-- The `asciiToTrytes()` method supports only [basic ASCII characters](https://en.wikipedia.org/wiki/ASCII#Printable_characters). As a result, diacritical marks such as accents and umlauts aren't supported and result in an `INVALID_ASCII_CHARS` error. -->
    <!-- ::: -->

7. メッセージをアドレスに送信するゼロトークントランザクションを定義します。
  <!-- 7. Define a zero-value transaction that sends the message to the address -->

    ```js
    const transfers = [
    {
        value: 0,
        address: address,
        message: messageInTrytes
    }
    ];
    ```

8. `transfers` オブジェクトからバンドルを作成するには、`transfers` オブジェクトを [`prepareTransfers()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.prepareTransfers) メソッドに渡します。次に、返されたバンドルトライトを [`sendTrytes()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.sendTrytes) メソッドに渡します。このメソッドは[チップ選択](root://node-software/0.1/iri/concepts/tip-selection.md)、[リモートプルーフオブワーク](root://getting-started/0.1/transactions/proof-of-work.md)を処理し、バンドルをノードに送信します。
  <!-- 8. To create a bundle from your `transfers` object, pass it to the [`prepareTransfers()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.prepareTransfers) method. Then, pass the returned bundle trytes to the [`sendTrytes()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.sendTrytes) method, which handles [tip selection](root://node-software/0.1/iri/concepts/tip-selection.md), [remote proof of work](root://getting-started/0.1/transactions/proof-of-work.md), and sending the bundle to the node -->

    ```js
    iota.prepareTransfers(seed, transfers)
        .then(trytes => {
            return iota.sendTrytes(trytes, depth, minimumWeightMagnitude);
        })
        .then(bundle => {
            console.log(bundle)
        })
        .catch(err => {
            console.error(err)
        });
    ```

    コンソールに、送信したばかりのトランザクションのバンドルハッシュが表示されます。
    <!-- In the console, you should see the bundle hash of the transaction you just sent. -->

:::success:おめでとうございます:tada:
最初のゼロトークンランザクションを送信しました。トランザクションはタングルにアタッチされ、ネットワークの残りの部分に転送されます。このトランザクションはイミュータブルであり、バンドルハッシュがある限り、タングル上で読み取ることができます。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just sent your first zero-value transaction. Your transaction is attached to the Tangle, and will be forwarded to the rest of the network. This transaction is now immutable, and as long as you have its bundle hash, you can read it on the Tangle. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

[REPL.it ツール](https://repl.it)を使用して、ブラウザーでサンプルコードを実行できます。
<!-- We use the [REPL.it tool](https://repl.it) to allow you to run sample code in the browser. -->

このガイドのサンプルコードを実行してウィンドウに結果を表示するには、緑色のボタンをクリックします。
<!-- Click the green button to run the sample code in this guide and see the results in the window. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Send-a-hello-world-transaction?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## 次のステップ
<!-- ## Next steps -->

トランザクションのバンドルハッシュをメモして、[タングルからトランザクションを取得](../js/read-transactions.md)してメッセージを読むことができるようにします。
<!-- Make a note of your transaction's bundle hash so you can [Get the transaction from the Tangle](../js/read-transactions.md) and read your message. -->

[タングルエクスプローラー](https://utils.iota.org)などのユーティリティを使用して、トランザクションを読み取ることができます。
<!-- You can also read your transaction, using a utility such as the [Tangle explorer](https://utils.iota.org). -->
