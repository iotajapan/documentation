# Node.js でタングル上のトランザクションを読む
<!-- # Read transactions on the Tangle in Node.js -->

**このガイドでは、[ノード](root://getting-started/0.1/network/nodes.md)に接続し、特定のバンドルハッシュを持つトランザクションのみを返すように要求することで、タングル上の "hello world" [トランザクション](root://getting-started/0.1/transactions/transactions.md)を読みます。**
<!-- **In this guide, you read your "hello world" [transaction](root://getting-started/0.1/transactions/transactions.md) on the Tangle by connecting to a [node](root://getting-started/0.1/network/nodes.md) and asking it to return only transactions with a given bundle hash.** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには、次のパッケージをインストールする必要があります。
<!-- To complete this guide, you need to install the following packages: -->

--------------------
### npm
```bash
npm install @iota/core @iota/extract-json
```
---
### Yarn
```bash
yarn add @iota/core @iota/extract-json
```
--------------------

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. パッケージをリクワイアします。
  <!-- 1. Require the packages -->

    ```js
    const Iota = require('@iota/core');
    const Extract = require('@iota/extract-json');
    ```

2. ノードに接続します。
  <!-- 2. Connect to a node -->

    ```js
    const iota = Iota.composeAPI({
    provider: 'https://nodes.devnet.iota.org:443'
    });
    ```

3. トランザクションのフィルタリングに使用するバンドルハッシュを定義します。
  <!-- 3. Define the bundle hash that you want to use to filter transactions -->

    ```js
    const bundle =
    'SIHQISXRUHFGZBCHOQLRYFXYTQBIERIJZHCHUUJZPAZC9YEQQVXAJFZNZKEBKPILI9GHYX9QCPAYGFWDD';
    ```

4. [`findTransactionObjects()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.findTransactionObjects) メソッドを使用して、`bundle` フィールドの値でトランザクションを取得します。次に、[`extractJSON()`](https://github.com/iotaledger/iota.js/tree/next/packages/extract-json) メソッドを使用して、トランザクションの `signatureMessageFragment` フィールドの JSON メッセージをデコードし、コンソールに出力します。
  <!-- 4. Use the [`findTransactionObjects()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.findTransactionObjects) method to get transactions by the value of their `bundle` field. Then, use the [`extractJSON()`](https://github.com/iotaledger/iota.js/tree/next/packages/extract-json) method to try to decode the JSON message in the `signatureMessageFragment` fields of the transactions and print it to the console -->

    ```js
    iota.findTransactionObjects({ bundles: [bundle] })
    .then(bundle => {
        console.log(JSON.parse(Extract.extractJson(bundle)));
    })
    .catch(err => {
        console.error(err);
    });
    ```

    コンソールに、JSON メッセージが表示されます。
    In the console, you should see your JSON message:

    ```json
    {"message": "Hello world"}
    ```

:::success:おめでとうございます:tada:
タングル上のトランザクションを見つけて読み取りました。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just found and read a transaction on the Tangle. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

[REPL.it ツール](https://repl.it)を使用して、ブラウザーでサンプルコードを実行できます。
<!-- We use the [REPL.it tool](https://repl.it) to allow you to run sample code in the browser. -->

このガイドのサンプルコードを実行してウィンドウに結果を表示するには、緑色のボタンをクリックします。
<!-- Click the green button to run the sample code in this guide and see the results in the window. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Read-a-transaction-on-the-Tangle?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## 次のステップ
<!-- ## Next steps -->

[新しいアドレスを生成する](../js/generate-an-address.md)。
<!-- [Generate a new address](../js/generate-an-address.md). -->
