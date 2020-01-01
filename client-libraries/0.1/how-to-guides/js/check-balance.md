# Node.js でアドレスの残高を確認する
<!-- # Check the balance of an address in Node.js -->

**このガイドでは、[ノード](root://getting-started/0.1/network/nodes.md)から[アドレス](root://getting-started/0.1/clients/addresses.md)の [IOTA トークン](root://getting-started/0.1/clients/token.md)の残高をリクエストします。**
<!-- **In this guide, you request the balance of [IOTA tokens](root://getting-started/0.1/clients/token.md) on [addresses](root://getting-started/0.1/clients/addresses.md) from a [node](root://getting-started/0.1/network/nodes.md).** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには、次のパッケージをインストールする必要があります。
<!-- To complete this guide, you need to install the following package: -->

--------------------
### npm
```bash
npm install @iota/core
```
---
### Yarn
```bash
yarn add @iota/core
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
    ```

2. ノードに接続します。
  <!-- 2. Connect to a node -->

    ```js
    const iota = Iota.composeAPI({
    provider: 'https://nodes.devnet.iota.org:443'
    });
    ```

3. 残高を確認するアドレスを定義します。
  <!-- 3. Define the address whose balance you want to check -->

    ```js
    const address =
    'NBZLOBCWG9BAQTODDKNF9LYYTBOUWSQSGCWFQVZZR9QXCOAIBRYDTZOEGBGMZKJYZOPPGRGFFWTXUKUKW';
    ```

4. [`getBalances()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getBalances) メソッドを使用して、ノードにアドレスの現在の残高を尋ねます。
  <!-- 4. Use the [`getBalances()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getBalances) method to ask the node for the current balance of the address -->

    ```js
    iota.getBalances([address], 100)
      .then(({ balances }) => {
      console.log(balances)
      })
      .catch(err => {
      console.error(err)
      });
    ```

    コンソールに、IOTA トークンの残高が表示されます。
    <!-- In the console, you should see a balance of IOTA tokens: -->

    ```bash
    [500]
    ```

:::success:おめでとうございます:tada:
アドレスの残高を確認しました。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just checked the balance of an address. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

[REPL.it ツール](https://repl.it)を使用して、ブラウザーでサンプルコードを実行できます。
<!-- We use the [REPL.it tool](https://repl.it) to allow you to run sample code in the browser. -->

緑色のボタンをクリックして、このガイドのサンプルコードを実行し、ウィンドウで結果を確認できます。
<!-- Click the green button to run the sample code in this guide and see the results in the window. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Check-the-balance-of-an-address?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## 次のステップ
<!-- ## Next steps -->

[タングル上のライブトランザクションをリッスンする](../js/listen-for-transactions.md)。
<!-- [Listen for live transactions on the Tangle](../js/listen-for-transactions.md). -->

[タングルエクスプローラー](https://utils.iota.org)などのユーティリティを使用して、アドレスの残高を確認することもできます。
<!-- You can also check the balance of an address, using a utility such as the [Tangle explorer](https://utils.iota.org). -->
