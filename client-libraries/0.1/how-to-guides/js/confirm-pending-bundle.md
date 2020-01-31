# バンドルが確定するのを助ける
<!-- # Help a bundle to become confirmed -->

**あるトランザクションが[マイルストーン](root://getting-started/0.1/network/the-coordinator.md)によって参照されるためには、チップ選択時にそのトランザクションが選択される必要があります。これにより、古いトランザクションよりも新しいトランザクションが優先されます。[バンドル](root://getting-started/0.1/transactions/bundles.md)が[ペンディング状態](root://getting-started/0.1/network/the-tangle.md#transaction-states)に留まる時間が長いほど、確定される可能性は低くなります。バンドルが確定される可能性を高めるために、末尾トランザクションの再添付や促進を行うことができます。**
<!-- **To be referenced by a [milestone](root://getting-started/0.1/network/the-coordinator.md), a transaction must be selected during tip selection, which favors new transactions over old ones. Therefore, the longer a [bundle](root://getting-started/0.1/transactions/bundles.md) is stuck in a [pending state](root://getting-started/0.1/network/the-tangle.md#transaction-states), the less likely it is to be confirmed. To increase the chances of a bundle being confirmed, you can reattach and promote its tail transaction.** -->

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

- **[最小重量値](root://getting-started/0.1/network/minimum-weight-magnitude.md)**：9
<!-- - **[Minimum weight magnitude](root://getting-started/0.1/network/minimum-weight-magnitude.md)**: 9 -->

- **[深さ (depth)](root://getting-started/0.1/transactions/depth.md)**：3
<!-- - **[Depth](root://getting-started/0.1/transactions/depth.md)**: 3 -->

## 手順 1. タイマー関数を作成する
<!-- ## Step 1. Create a timer function -->

バンドルの確定に費やす時間を長く保つには、タイマー機能を作成します。
<!-- To keep a counter of long it took for a bundle to be confirmed, create a timer function. -->

1. 確定するペンディング中のバンドルの末尾トランザクションハッシュを定義します。
  <!-- 1. Define the tail transaction hash of the pending bundle that you want to confirm -->

    ```js
    const tails = ["tail transaction hash"];
    ```

    :::info:
    再添付バンドルの末尾トランザクションも、チェックのためにこの配列に追加されます。
    :::
    <!-- :::info: -->
    <!-- The tail transactions of any reattachment bundles will also be appended to this array for checking. -->
    <!-- ::: -->

2. タイマーの秒数を格納するための変数を作成します。
  <!-- 2. Create a variable to store the number of seconds for the timer -->

    ```js
    var seconds = 0;
    ```

3. バンドルが確定されたかを測定するためのタイマーを設定します。毎秒、タイマーは `seconds` 変数を1ずつ増やします。
  <!-- 3. Set the timer to measure how long it takes for the bundle to be confirmed. Every second, the timer will increment the `seconds` variable by one. -->

    ```js
    var timer = setInterval(stopWatch, 1000);
    function stopWatch (){
        seconds++
    }
    ```

## 手順 2. バンドルを自動促進および自動再添付する関数を作成する
<!-- ## Step 2. Create a function to auto-promote and auto-reattach bundles -->

バンドルを促進や再添付するには、末尾トランザクションハッシュをクライアントライブラリの関連する関数に渡す必要があります。
<!-- To promote and reattach a bundle, you need to pass its tail transaction hash to the relevant function in the client library. -->

[`isPromotable()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.isPromotable) メソッドは、末尾トランザクションが矛盾していないことと、最新の6マイルストーンより前に末尾トランザクションがタングルに添付されていないことを確認します。
<!-- The [`isPromotable()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.isPromotable) method checks if the tail transaction is consistent and was not attached to the Tangle before the most recent 6 milestones. -->

末尾トランザクションが促進可能であれば、[`promoteTransaction()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.promoteTransaction) メソッドは末尾トランザクションを促進します。
<!-- If the tail transaction is promotable, the [`promoteTransaction()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.promoteTransaction) method promotes it. -->

末尾トランザクションが促進できない場合は、[`replayBundle()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.replayBundle) メソッドがバンドルを再添付し、後で確定されたかどうかを確認するために、新しく再添付されたバンドルの末尾トランザクションハッシュを `tails` 配列に追加します。
<!-- If the tail transaction isn't promotable, the [`replayBundle()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.replayBundle) method reattaches the bundle, then the new reattached bundle's tail transaction hash is added to the `tails` array so that it can be checked for confirmation later on. -->

```js
function autoPromoteReattach (tail) {
  iota.isPromotable(tail)
    .then(promote => promote
    ? iota.promoteTransaction(tail, 3, 14)
        .then(()=> {
            console.log(`Promoted transaction hash: ${tail}`);
        })
    : iota.replayBundle(tail, 3, 14)
        .then(([reattachedTail]) => {
            const newTailHash = reattachedTail.hash;

            console.log(`Reattached transaction hash: ${tail}`);

            // 再アタッチされたすべての末尾トランザクションハッシュを追跡して確定を確認します
            tails.push(newTailHash);
        })
    )
    .catch((error)=>{
         console.log(error);
    });
}
```

## 手順 3. 定期的に確定したかを確認する関数を作成する
<!-- ## Step 3. Create a function to check for confirmation at regular intervals -->

末尾トランザクション配列が確定しかたどうかを定期的にチェックできるようにするには、`setInterval()` 関数に渡すことができる関数が必要です。
<!-- To be able to check the array of tail transactions for confirmation at regular intervals, you need a function that can be passed to a `setInterval()` function. -->

[`getLatestInclusion()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getLatestInclusion) メソッドは、配列内の末尾トランザクションのいずれかが確定しているかどうかを確認します。いずれかの末尾トランザクションが確定している場合、このメソッドは`true`を返します。
<!-- The [`getLatestInclusion()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getLatestInclusion) method checks if any of the tail transactions in the array have been confirmed. If any of the transactions have been confirmed this method returns `true`. -->

`tail` 変数は、最新の再添付したバンドルをさらに促進または再添付できるように、最新の末尾トランザクションを配列に格納します。
<!-- The `tail` variable stores the last tail transaction in the array so that the latest reattachment can be promoted or reattached. -->

末尾トランザクションがまだ確定していない場合は、`tail` 変数が上記で作成した [`autoPromoteReattach()`](#create-a-function-to-auto-promote-and-auto-reattach-bundles) 関数に渡されます。
<!-- If none of the tail transactions have been confirmed yet, the `tail` variable is passed to the [`autoPromoteReattach()`](#create-a-function-to-auto-promote-and-auto-reattach-bundles) function. -->

末尾トランザクションが確定した場合、確定に要した分数とともにコンソールに記録されます。
<!-- If a tail transaction has been confirmed, it's logged to the console along with the number of minutes it took to confirm. -->

```js
function autoConfirm(tails){
console.log(tails);
    iota.getLatestInclusion(tails)
        .then(states => {
            // トランザクションが確定されていないことを確認します
            if (states.indexOf(true) === -1) {
                // Get latest tail hash
                const tail = tails[tails.length - 1]
                autoPromoteReattach(tail);
            } else {
                console.log(JSON.stringify(states, null, 1));
                clearInterval(interval);
                clearInterval(timer);
                var minutes = (seconds / 60).toFixed(2);
                var confirmedTail = tails[states.indexOf(true)];
                console.log(`Confirmed transaction hash in ${minutes} minutes: ${confirmedTail}`);
                return;
            }
        }).catch(error => {
            console.log(error);
        }
    );
}
```

## コードを実行する
<!-- ## Run the code -->

[REPL.it ツール](https://repl.it)を使用して、ブラウザーで JavaScript クライアントライブラリからサンプルコードを実行できます。
<!-- We use the [REPL.it tool](https://repl.it) to allow you to run sample code from the JavaScript client library in the browser. -->

このガイドのサンプルコードを実行してウィンドウに結果を表示するには、緑色のボタンをクリックします。
<!-- Click the green button to run the sample code in this guide and see the results in the window. -->

このサンプルコードを実行する前に、ペンディング中の末尾トランザクションハッシュを見つけ、それを `tails` 配列に格納します。
<!-- Before you run this sample code, find a pending tail transaction hash and store it in the `tails` array. -->

:::info:ペンディング中のトランザクションが見つかりませんか?
[devnet.thetangle.org](https://devnet.thetangle.org) に行き、`Latest transactions` 欄でトランザクションハッシュをクリックします。このトランザクションはチップなので、ペンディング状態にあります。
:::
<!-- :::info:Can't find a pending transaction? -->
<!-- Go to [devnet.thetangle.org](https://devnet.thetangle.org) and click a transaction hash in the Latest transactions box. This transaction is a tip, so it is in a pending state. -->
<!-- ::: -->

<iframe height="500px" width="100%" src="https://repl.it/@jake91/Confirm-pending-bundle?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

:::info:
このサンプルコードは、完了までに数分かかることがあります。`Started autoConfirm() function => undefined` と表示された場合、コードはバックグラウンドで実行されています。コードが終了するまで待ちます。コンソールにメッセージが表示されるはずです。
:::
<!-- :::info: -->
<!-- This sample code may take a few minutes to complete. If you see `Started autoConfirm() function => undefined`, the code is running in the background. Wait until the code finishes. You should see messages appear in the console. -->
<!-- ::: -->
