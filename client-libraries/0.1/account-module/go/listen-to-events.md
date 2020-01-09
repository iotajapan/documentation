# Go でアカウントイベントをリッスンする
<!-- # Listen to account events in Go -->

**アカウントオブジェクトは、イベントが発生するとイベントを発行します。イベントの例は、支払いを行うか支払いを受け取る場合です。このガイドでは、これらのイベントをリッスンし、コンソールに記録します。**
<!-- **An account object emits events when they happen. An example of an event is when you make or receive a payment. In this guide, you listen for these events and log them to the console.** -->

アカウントには、チャネルを使用するリスナーとコールバックを使用するリスナーの2種類のリスナーがあります。このガイドでは、コールバックリスナーを使用します。チャネルリスナーの使用に興味がある場合は、[イベントリスナープラグインの作成](../go/create-plugin.md)のガイドをご覧ください。
<!-- Accounts have two types of listeners: One that uses channels and one that uses callbacks. In this guide, we use callback listeners. If you're interested in using a channel listener, see our guide for [creating an event-listener plugin](../go/create-plugin.md). -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには、以下のパッケージをインストールする必要があります（Go モジュールを使用している場合は、以下のパッケージを参照するだけです）。
<!-- To complete this guide, you need to install the following packages (if you're using Go modules, you just need to reference them): -->

```bash
go get github.com/iotaledger/iota.go/api
go get github.com/iotaledger/iota.go/badger
go get github.com/iotaledger/iota.go/builder
go get github.com/iotaledger/iota.go/timesrc
go get github.com/iotaledger/iota.go/trinary
go get github.com/iotaledger/iota.go/account
```

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. `EventMachine` オブジェクトを持つアカウントをビルドして開始します。
  <!-- 1. Build and start an account that has an `EventMachine` object -->

    ```go
    account, err = builder.NewBuilder().
        // ノードに接続します
        WithAPI(iotaAPI).
        // データベースを作成します
        WithStore(store).
        WithSeed(seed).
        // デブネット用の最小重量値をセットします
        WithMWM(9).
        WithTimeSource(timesource).
        // イベントマシンをロードします
        WithEvents(em)
        // アカウントの機能を強化するデフォルトのプラグインをロードします
        WithDefaultPlugins().
        Build()
    handleErr(err)

    handleErr(account.Start())
    ```

2. 入金と出金を待機する新しい `CallbackEventListener` オブジェクトを作成します。
  <!-- 2. Create a new `CallbackEventListener` object that listens for incoming and outgoing payments -->

    ```go
    lis := listener.NewCallbackEventListener(em)
    lis.RegSentTransfers(func(bun bundle.Bundle) {
    	fmt.Println("Outgoing payment is pending")
    	fmt.Println("Bundle :", bun)
    })
    lis.RegPromotions(func(promoted *promoter.PromotionReattachmentEvent) {
    	fmt.Println("Promoting a pending bundle")
    	fmt.Printf("%+v\n", *promoted)
    })
    lis.RegReattachments(func(promoted *promoter.PromotionReattachmentEvent) {
    	fmt.Println("Reattaching a pending bundle")
    	fmt.Printf("%+v\n", *promoted)
    })
    lis.RegConfirmedTransfers(func(bun bundle.Bundle) {
    	fmt.Println("Outgoing payment confirmed")
    	fmt.Println("Bundle :", bun)
    })
    lis.RegReceivedMessages(func(bun bundle.Bundle) {
    	fmt.Println("Received a new message")
    	fmt.Println("Bundle :", bun)
    })
    lis.RegReceivingDeposits(func(bun bundle.Bundle) {
    	fmt.Println("Receiving a new payment")
    	fmt.Println("Bundle :", bun)
    })
    lis.RegReceivedDeposits(func(bun bundle.Bundle) {
    	fmt.Println("Received a new payment")
    	fmt.Println("Bundle :", bun)
    })
    ```

:::success:おめでとうございます:tada:
アカウントはリッスンして行動することができるイベントを発信しています。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You're account can now emit events that you can listen to and act on. -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

イベントリスナーを作ったので、これをテストするために[アカウントとの間で支払いを行う](../go/make-payment.md)。
<!-- Now that you have an event listener, start [making payments to/from your account](../go/make-payment.md) to test it. -->
