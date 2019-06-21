# トランザクションが確定したか確認する
<!-- # Check if a transaction is confirmed -->

**IOTAトークンを転送する前に、IOTAトークンを転送するバンドルを確認する必要があります。バンドル内のトランザクションは、最後のトランザクションがマイルストーンによって参照および承認されるまでペンディング状態のままになります。**
<!-- **Before IOTA tokens can be transferred, the bundle that transfers them must be confirmed. Transactions in a bundle remain in a pending state until the tail transaction is referenced and approved by a milestone.** -->

:::info:クライアントライブラリを初めて使用されますか？
公式クライアントライブラリを使い始めるための[クイックスタートガイド](root://getting-started/0.1/tutorials/get-started.md)をお試してください。
:::
<!-- :::info:First time using a client library? -->
<!-- [Try our quickstart guide](root://getting-started/0.1/tutorials/get-started.md) for getting started with the official client libraries. -->
<!-- ::: -->

:::info:
コーディネーター、マイルストーン、または確定という用語に慣れていない場合は、[タングルについて](root://the-tangle/0.1/introduction/overview.md)を読むことをお勧めします。
:::
<!-- :::info: -->
<!-- If you're unfamiliar with the terms Coordinator, milestone, or confirmation, we recommend that you [read about the Tangle](root://the-tangle/0.1/introduction/overview.md). -->
<!-- ::: -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完成するには、次のものが必要です。
<!-- To complete this guide, you need the following: -->

* Node.js 8、またはNode.js 10以上。[最新のLTS](https://nodejs.org/en/download/)をお勧めします。
<!-- * Node.js 8, or Node.js 10 or higher. We recommend the [latest LTS](https://nodejs.org/en/download/). -->
* [Visual Studio Code](https://code.visualstudio.com/Download)のようなコードエディタ
<!-- * A code editor such as [Visual Studio Code](https://code.visualstudio.com/Download) -->
* コマンドプロンプトへのアクセス
<!-- * Access to a command prompt -->
* インターネット接続
<!-- * An Internet connection -->

## トランザクションが確定したか確認する
<!-- ## Check if a transaction is confirmed -->

この例では、[Devnet](root://getting-started/0.1/references/iota-networks.md#devnet)でトランザクションが確定されたかどうかを確認します。Devnetは、トークンが無料であること以外はMainnetとほぼ同じです。Devnetに送信したトランザクションは、Mainnetのような他のネットワークには存在しません。
<!-- In this example, we check if a transaction is confirmed on the [Devnet](root://getting-started/0.1/references/iota-networks.md#devnet). The Devnet is similar to the Mainnet, except the tokens are free. Any transactions that you send to the Devnet do not exist on other networks such as the Mainnet. -->

1. IOTAクライアントライブラリが必要です。
  <!-- 1. Require the IOTA client library -->

    ```js
    const Iota = require('@iota/core');
    ```

2. IOTAオブジェクトのインスタンスを作成し、`provider`フィールドを使用してIRIノードに接続します。
  <!-- 2. Create an instance of the IOTA object and use the `provider` field to connect to a node -->

    ```js
    const iota = Iota.composeAPI({
        provider: 'https://nodes.devnet.iota.org:443'
    });
    ```

3. [devnet.thetangle.org](https://devnet.thetangle.org/)に行き、確定したトランザクションを見つけます。
  <!-- 3. Go to [devnet.thetangle.org](https://devnet.thetangle.org/) and find a confirmed transaction -->

    :::info:確定したトランザクションが見つかりませんか？
    `Latest milestones`欄でトランザクションハッシュ値をクリックし、次にブランチトランザクションハッシュ値をクリックします。このトランザクションはマイルストーンによって参照および承認されているため、確定済みの状態です。
    :::
    <!-- :::info:Can't find a confirmed transaction? -->
    <!-- Click a transaction hash in the Latest milestones box, then click the branch transaction hash. This transaction is referenced and approved by the milstone, so it is in a confirmed state. -->
    <!-- ::: -->

4. トランザクションハッシュを[`getLatestInclusion()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getLatestInclusion)メソッドに渡して、IRIノードの最新のソリッドサブタングルマイルストーンが承認したかどうかを確認します。
  <!-- 4. Pass the transaction hash to the [`getLatestInclusion()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getLatestInclusion) method to check if the IRI node's latest solid subtangle milestone approves it -->

    ```js
    iota.getLatestInclusion(['TRANSACTION HASH'])
    .then(states => console.log(states));
    ```

    ファイルを実行すると、`true`のブーリアンを含む配列が表示されます。これは、トランザクションが確定されたことを意味します。
    <!-- When you execute the file, you should see an array that contains the `true` boolean, meaning that the transaction is confirmed. -->

    :::info:
    また、`getInclusionStates()`メソッドを使用して、自分が指定する一連のトランザクションによってあるトランザクションが承認されているかどうかを確認することもできます。
    :::
    <!-- :::info: -->
    <!-- You could also use the `getInclusionStates()` method to check if a transaction is approved by an array of your own chosen transactions. -->
    <!-- ::: -->

5. [devnet.thetangle.org](https://devnet.thetangle.org)に行き、ペンディング中のトランザクションを見つけます。
  <!-- 5. Go to [devnet.thetangle.org](https://devnet.thetangle.org) and find a pending transaction -->

    :::info:ペンディング中のトランザクションが見つかりませんか？
    `Latest transactions`欄でトランザクションハッシュ値をクリックします。このトランザクションはチップなので、ペンディング状態にあります。
    :::
    <!-- :::info:Can't find a pending transaction? -->
    <!-- Click a transaction hash in the Latest transactions box. This transaction is a tip, so it is in a pending state. -->
    <!-- ::: -->

6. トランザクションハッシュ値を`getLatestInclusion()`メソッドに渡して、IRIノードの最新のソリッドサブタングルマイルストーンが承認したかどうかを確認します。
  <!-- 6. Pass the transaction hash to the `getLatestInclusion()` method to check if the IRI node's latest solid subtangle milestone approves it -->

    ```js
    iota.getLatestInclusion(['TRANSACTION HASH'])
    .then(states => console.log(states));
    ```

    ファイルを実行すると、`false`ブーリアンを含む配列が表示されます。これは、トランザクションがまだ確定されていないことを意味します。
    <!-- When you execute the file, you should see an array that contains the `false` boolean, meaning that the transaction is not yet confirmed. -->

## コードを走らせる
<!-- ## Run the code -->

このガイドのサンプルコードを実行してWebブラウザに結果を表示するには、緑色のボタンをクリックします。
<!-- Click the green button to run the sample code in this guide and see the results in the web browser. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Check-transaction-confirmation?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## 次のステップ
<!-- ## Next steps -->

[ペンディング中のトランザクションが確定される可能性を高める](../how-to-guides/confirm-pending-bundle.md)。
<!-- [Increase the likelihood of a pending transaction being confirmed](../how-to-guides/confirm-pending-bundle.md) -->
