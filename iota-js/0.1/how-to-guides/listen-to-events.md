# アカウント内のイベントをリッスンする
<!-- # Listen to events in an account -->

**アカウントオブジェクトは、発生したときにイベントを発行します。イベントの例としては、他のアドレスに資金を送ったり、預け入れを受け取ったりしたときです。これらのイベントをリッスンして行動することができます。**
<!-- **An account object emits events when they happen. An example of an event is when you send funds to other accounts or receive a deposit. You can listen for these events and act on them.** -->

## 前提条件
<!-- ## Prerequisites -->

このガイドは、読者が[Node.jsのイベント](https://nodejs.org/api/events.html)の概念を理解していることを前提としています。イベントリスナーは、コールバックを特定のイベントタイプに割り当てるために使用されます。[イベントリスナを使い終えたら](https://nodejs.org/api/events.html#events_emitter_removelistener_eventname_listener)、必ずイベントリスナを削除してください。
<!-- This guide assumes that you understand the concept of events in Node.js (https://nodejs.org/api/events.html). Event listeners are used to assign callbacks to specific event types. You should always [remove event listeners](https://nodejs.org/api/events.html#events_emitter_removelistener_eventname_listener) when you're finished with them. -->

## 預け入れイベントと取り出しイベントをリッスンする
<!-- ## Listening to deposit and withdrawal events -->

預け入れバンドルと取り出しバンドルが検出されるとすぐに預け入れイベントと取り出しイベントが発生します。これらの各バンドルは2つのステップでイベントを発生させることができます。1つは**ペンディング**状態用、もう1つは**含まれる**（確定済み）状態用です。
<!-- Deposit and withdrawal events are emitted as soon as a deposit or withdrawal bundle is detected. Each of those bundles may trigger events in two steps: One for its **pending** state, and one for its **included** (confirmed) state. -->

コールバックは引数としてオブジェクトを与えられます。このオブジェクトには関連アドレスと完全な預け入れバンドルか取り出しバンドルが含まれます。
<!-- Callbacks are given an object as an argument, which contains the relevant address and the complete deposit or withdrawal bundle. -->

1. 預け入れバンドルと取り出しバンドルのリスナーを添付します。
  <!-- 1. Attach listeners for deposit and withdrawal events -->

    ```js
    account.on('pendingDeposit', ({ address, bundle }) => {
        console.log('Address:', address, 'Tail transaction hash:', bundle[0].hash);
        // ...
    });

    account.on('includedDeposit', ({ addresses, bundle }) => {
        console.log('Address:', address, 'Tail transaction hash:', bundle[0].hash);
        // ...
    });

    account.on('pendingWithdrawal', ({ address, bundle }) => {
        // ...
    });

    account.on('includedWithdrawal', ({ addresses, bundle }) => {
        // ...
    });
    ```

2. アプリケーションのデバッグやバックグラウンドでスローされる可能性のある例外への対応に役立つ `error`イベントをサブスクライブします。
  <!-- 2. Subscribe to `error` events, which are useful for debugging your application and reacting to exceptions that may be thrown in the background -->

    ```js
    account.on('error', (error) => {
        console.log(`Something went wrong: ${error}`);
    });
    ```

3. CDAを生成し、明日に期限切れになるように設定します。
  <!-- 3. Generate a CDA and set it to expire tomorrow -->

    ```js
    const cda = account
        .generateCDA({
            timeoutAt: Date.now() + 24 * 60 * 60 * 1000,
            expectedAmount: 100
        });
    ```

4. CDAに預け入れを送信します。
  <!-- 4. Send a deposit to the CDA -->

    ```js
    cda
        .tap(cda => console.log('Sending to:', cda.address))
        .then(cda =>
            account.sendToCDA({
                ...cda,
                value: 100
            })
        )
        .catch(error => console.log(error));
    ```

    :::info:
    接続されたノードの台帳のいずれかのCDAから取り出しまたはCDAへの預け入れが行われるトランザクションごとにイベントがトリガーされます。
    :::
    <!-- :::info: -->
    <!-- An event is triggered for each transaction in the connected node's ledger that withdraws from or deposits into one of your CDAs. -->
    <!-- ::: -->

出力には、トランザクションがペンディング中の場合はアドレスと末尾トランザクションハッシュが表示され、トランザクションが確定されると同じアドレスと末尾トランザクションハッシュが表示されます。
<!-- In the output, you should see an address and a tail transaction hash when the transaction is pending, and the same address and tail transaction hash when the transaction is confirmed. -->

## アカウントイベント
<!-- ## Account events -->

| **イベント名** | **コールバック引数** |
| :------------- | :------------------- |
| `pendingDeposit` | `{ address, bundle }` |
| `includedDeposit` | `{ address, bundle }` |
| `pendingWithdrawal` | `{ address, bundle }` |
| `includedWithdrawal` | `{ address, bundle }` |
| `selectInput` | `{ transfer, input }` |
| `prepareTransfer` | `{ transfer, trytes }` |
| `getTransactionsToApprove` | `{ trytes, { trunkTransaction, branchTransaction } }` |
| `attachToTangle` | `transactionObjects` |
| `broadcast` | `transactionObjects` |
| `error` | `Error` |

:::success:おめでとうございます！:tada:
アカウントはリッスンして行動することができるイベントを発信しています。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You're account is now emitting events that you can listen to and act on. -->
<!-- ::: -->
