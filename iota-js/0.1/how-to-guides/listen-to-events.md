# アカウント内のイベントをリッスンする
<!-- # Listen to events in an account -->

**アカウントオブジェクトは、イベントが発生したときにイベントを発行します。イベントの例としては、支払いをするときや受け取るときがあります。あなたはこれらのイベントをリッスンして行動することができます。**
<!-- **An account object emits events when they happen. An example of an event is when you make or receive a payment. You can listen for these events and act on them.** -->

## 前提条件
<!-- ## Prerequisites -->

[アカウントを作成します](../how-to-guides/create-account.md)。
<!-- [Create an account](../how-to-guides/create-account.md). -->

このガイドは、[Node.jsのイベント](https://nodejs.org/api/events.html)の概念を理解していることを前提としています。イベントリスナは、コールバックを特定のイベントタイプに割り当てるために使用されます。[イベントリスナを使い終えたら](https://nodejs.org/api/events.html#events_emitter_removelistener_eventname_listener)、必ずイベントリスナを削除してください。
<!-- This guide assumes that you understand the concept of [events in Node.js](https://nodejs.org/api/events.html). Event listeners are used to assign callbacks to specific event types. You should always [remove event listeners](https://nodejs.org/api/events.html#events_emitter_removelistener_eventname_listener) when you're finished with them. -->

## 入金と出金についてアカウントを監視する
<!-- ## Monitor your account for incoming and outgoing payments -->

アカウントの接続ノードが残高に影響するバンドルを受け取ると、アカウントは2種類のイベントをトリガできます。1つはバンドルが**ペンディング**状態にあるとき、もう1つは**含まれた**（確定済み）状態にあるときです。
<!-- When your account's connected nodes receive a bundle that affects your balance, your account can trigger two types of event: One when the bundle is in a **pending** state, and one when it's in an **included** (confirmed) state. -->

アカウントへのすべての入金は預け入れと呼ばれ、出金は取り出しと呼ばれます。
<!-- Any incoming payments to your account are called deposits, and outgoing payments are called withdrawals. -->

1. 預け入れイベントと取り出しイベントのリスナをアカウントに添付します。
  <!-- 1. Attach listeners to your account for deposit and withdrawal events -->

    ```js
    account.on('pendingDeposit', ({ address, bundle }) => {
        console.log('Receiving a new payment')
        console.log('Address:', address, 'Tail transaction hash:', bundle[0].hash);
    });

    account.on('includedDeposit', ({ address, bundle }) => {
        console.log('Received a new payment')
        console.log('Address:', address, 'Tail transaction hash:', bundle[0].hash);
    });

    account.on('pendingWithdrawal', ({ address, bundle }) => {
        console.log('Outgoing payment is pending')
        console.log('Address:', address, 'Tail transaction hash:', bundle[0].hash);
    });

    account.on('includedWithdrawal', ({ address, bundle }) => {
        console.log('Outgoing payment confirmed')
        console.log('Address:', address, 'Tail transaction hash:', bundle[0].hash);
    });
    ```

2. `error`イベントを購読します。`error`イベントは、アプリケーションをデバッグしたり、バックグラウンドでスローされる可能性がある例外に対処したりするのに役立ちます。
  <!-- 2. Subscribe to `error` events. These events are useful for debugging your application and reacting to exceptions that may be thrown in the background. -->

    ```js
    account.on('error', (error) => {
        console.log(`Something went wrong: ${error}`);
    });
    ```

    :::info:
    アカウントのアクティブなCDAに影響する、接続しているノードの台帳の各トランザクションに対してイベントが発生します。
    :::
    <!-- :::info: -->
    <!-- An event is triggered for each transaction in the connected node's ledger that affects any active CDAs in your account. -->
    <!-- ::: -->

:::success:おめでとうございます！:tada:
アカウントはリッスンして行動することができるイベントを発信しています。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You're account can now emit events that you can listen to and act on. -->
<!-- ::: -->

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
<!-- |**Event name**|**Callback arguments**| -->
<!-- | :----------| :----------| -->
<!-- |`pendingDeposit`|`{ address, bundle }`| -->
<!-- |`includedDeposit`|`{ address, bundle }`| -->
<!-- |`pendingWithdrawal`|`{ address, bundle }`| -->
<!-- |`includedWithdrawal`|`{ address, bundle }`| -->
<!-- |`selectInput`|`{ transfer, input }`| -->
<!-- |`prepareTransfer`|`{ transfer, trytes }`| -->
<!-- |`getTransactionsToApprove`|`{ trytes, { trunkTransaction, branchTransaction } }`| -->
<!-- |`attachToTangle`|`transactionObjects`| -->
<!-- |`broadcast`|`transactionObjects`| -->
<!-- |`error`|`Error`| -->

## 次のステップ
<!-- ## Next steps -->

イベントリスナを作ったので、これをテストするために[アカウントとの間で支払いを行います](../how-to-guides/create-and-manage-cda.md)。
<!-- Now that you have an event listener, start [making payments to/from your account](../how-to-guides/create-and-manage-cda.md) to test it. -->
