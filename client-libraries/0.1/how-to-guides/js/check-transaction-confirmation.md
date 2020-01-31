# トランザクションが確定したか確認する
<!-- # Check if a transaction is confirmed -->

**IOTA トークンを転送する前に、IOTA トークンを転送する[バンドル](root://getting-started/0.1/transactions/bundles.md)のトランザクションを確定する必要があります。バンドル内のトランザクションは、末尾トランザクションが[マイルストーン](root://getting-started/0.1/network/the-coordinator.md)によって参照および承認されるまでペンディング状態のままです。**
<!-- **Before IOTA tokens can be transferred, the transactions in the [bundle](root://getting-started/0.1/transactions/bundles.md) that transfers them must be confirmed. Transactions in a bundle remain in a pending state until the tail transaction is referenced and approved by a [milestone](root://getting-started/0.1/network/the-coordinator.md).** -->

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

## IOTAネットワーク
<!-- ## IOTA network -->

このガイドでは、次のネットワーク設定で[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)上の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet) with the following network settings: -->

- **[最小重量値 (MWM)](root://getting-started/0.1/network/minimum-weight-magnitude.md)**：9
<!-- - **[Minimum weight magnitude](root://getting-started/0.1/network/minimum-weight-magnitude.md)**: 9 -->

- **[深さ (depth)](root://getting-started/0.1/transactions/depth.md)**：3
<!-- - **[Depth](root://getting-started/0.1/transactions/depth.md)**: 3 -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. [devnet.thetangle.org](https://devnet.thetangle.org/) に行き、確定したトランザクションを見つけます。
  <!-- 1. Go to [devnet.thetangle.org](https://devnet.thetangle.org/) and find a confirmed transaction -->

    :::info:確定したトランザクションが見つかりませんか？
    `Latest milestones` 欄でトランザクションハッシュをクリックし、次にブランチトランザクションハッシュをクリックします。このトランザクションはマイルストーンによって参照および承認されているため、確定済みの状態です。
    :::
    <!-- :::info:Can't find a confirmed transaction? -->
    <!-- Click a transaction hash in the Latest milestones box, then click the branch transaction hash. This transaction is referenced and approved by the milestone, so it is in a confirmed state. -->
    <!-- ::: -->

2. トランザクションハッシュを[`getLatestInclusion()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getLatestInclusion) メソッドに渡して、ノードの最新のソリッドサブタングルマイルストーンが承認したかどうかを確認します。
  <!-- 2. Pass the transaction hash to the [`getLatestInclusion()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getLatestInclusion) method to check if the node's latest solid subtangle milestone approves it -->

    ```js
    iota.getLatestInclusion(['TRANSACTION HASH'])
    .then(states => console.log(states));
    ```

    ファイルを実行すると、`true` のブーリアンを含む配列が表示されます。これは、トランザクションが確定されていることを意味します。
    <!-- When you execute the file, you should see an array that contains the `true` boolean, meaning that the transaction is confirmed. -->

    :::info:
    また、`getInclusionStates()` メソッドを使用して、自分が指定する一連のトランザクションによってあるトランザクションが承認されているかどうかを確認することもできます。
    :::
    <!-- :::info: -->
    <!-- You could also use the `getInclusionStates()` method to check if a transaction is approved by an array of your own chosen transactions. -->
    <!-- ::: -->

3. [devnet.thetangle.org](https://devnet.thetangle.org) に行き、ペンディング中のトランザクションを見つけます。
  <!-- 3. Go to [devnet.thetangle.org](https://devnet.thetangle.org) and find a pending transaction -->

    :::info:ペンディング中のトランザクションが見つかりませんか？
    `Latest transactions` 欄でトランザクションハッシュをクリックします。このトランザクションはチップなので、ペンディング状態にあります。
    :::
    <!-- :::info:Can't find a pending transaction? -->
    <!-- Click a transaction hash in the Latest transactions box. This transaction is a tip, so it is in a pending state. -->
    <!-- ::: -->

4. トランザクションハッシュを `getLatestInclusion()` メソッドに渡して、IRI ノードの最新のソリッドサブタングルマイルストーンが承認したかどうかを確認します。
  <!-- 4. Pass the transaction hash to the `getLatestInclusion()` method to check if the IRI node's latest solid subtangle milestone approves it -->

    ```js
    iota.getLatestInclusion(['TRANSACTION HASH'])
    .then(states => console.log(states));
    ```

    ファイルを実行すると、`false` ブーリアンを含む配列が表示されます。これは、トランザクションがまだ確定されていないことを意味します。
    <!-- When you execute the file, you should see an array that contains the `false` boolean, meaning that the transaction is not yet confirmed. -->

## コードを実行する
<!-- ## Run the code -->

[REPL.it ツール](https://repl.it)を使用して、ブラウザーで JavaScript クライアントライブラリからサンプルコードを実行できます。
<!-- We use the [REPL.it tool](https://repl.it) to allow you to run sample code from the JavaScript client library in the browser. -->

このガイドのサンプルコードを実行してウィンドウに結果を表示するには、緑色のボタンをクリックします。
<!-- Click the green button to run the sample code in this guide and see the results in the window. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Check-transaction-confirmation?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## 次のステップ
<!-- ## Next steps -->

[ペンディング中のトランザクションが確定される可能性を高める](../js/confirm-pending-bundle.md)。
<!-- [Increase the likelihood of a pending transaction being confirmed](../js/confirm-pending-bundle.md) -->
