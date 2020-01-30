# JavaScript を用いてアカウント内のイベントをリッスンする
<!-- # Listen to events in an account in JavaScript -->

**アカウントオブジェクトは，イベントが発生するとイベントを発行します．イベントの例は，支払いを行うか受け取る場合です．このガイドでは，支払いを行ったり，受け取ったりするイベントをリッスンし，コンソールにログします．**
<!-- **An account object emits events when they happen. An example of an event is when you make or receive a payment. In this guide, you listen for these events and log them to the console.** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには，次のパッケージをインストールする必要があります．
<!-- To complete this guide, you need to install the following packages: -->

--------------------
### npm
```bash
npm install @iota/account ntp-client
```
---
### Yarn
```bash
yarn add @iota/account ntp-client
```
--------------------

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは，[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します．
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. デポジットイベントと取り出しイベントのリスナーをアカウントにアタッチします．
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

2. `error` イベントをサブスクライブします．`error` イベントは，アプリケーションをデバッグしたり，バックグラウンドでスローされる可能性がある例外に対処したりするのに役立ちます．
  <!-- 2. Subscribe to `error` events. These events are useful for debugging your application and reacting to exceptions that may be thrown in the background. -->

    ```js
    account.on('error', (error) => {
      console.log(`Something went wrong: ${error}`);
    });
    ```

    :::info:
    アカウントのアクティブな CDA に影響する，接続しているノードの台帳の各トランザクションに対してイベントが発生します
    :::
    <!-- :::info: -->
    <!-- An event is triggered for each transaction in the connected node's ledger that affects any active CDAs in your account. -->
    <!-- ::: -->

:::success:おめでとうございます:tada:
アカウントはリッスンして行動することができるイベントを発信しています．
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You're account can now emit events that you can listen to and act on. -->
<!-- ::: -->

## アカウントイベント
<!-- ## Account events -->

| **イベント名**             | **コールバック引数**                                  |
| :----------                | :----------                                           |
| `pendingDeposit`           | `{ address, bundle }`                                 |
| `includedDeposit`          | `{ address, bundle }`                                 |
| `pendingWithdrawal`        | `{ address, bundle }`                                 |
| `includedWithdrawal`       | `{ address, bundle }`                                 |
| `selectInput`              | `{ transfer, input }`                                 |
| `prepareTransfer`          | `{ transfer, trytes }`                                |
| `getTransactionsToApprove` | `{ trytes, { trunkTransaction, branchTransaction } }` |
| `attachToTangle`           | `transactionObjects`                                  |
| `broadcast`                | `transactionObjects`                                  |
| `error`                    | `Error`                                               |

## 次のステップ
<!-- ## Next steps -->

イベントリスナーができたので，[アカウント間で支払いを行う](../js/make-payment.md)を開始してテストする．
<!-- Now that you have an event listener, start [making payments to/from your account](../js/make-payment.md) to test it. -->
