# アカウントのイベントをリッスンする
<!-- # Listen to events in an account -->

**アカウントオブジェクトは、イベントが発生したときにイベントを発行します。イベントの例としては、支払いをするときや受け取るときがあります。これらのイベントをリッスンして行動することができます。**
<!-- **An account object emits events when they happen. An example of an event is when you make or receive a payment. You can listen for these events and act on them.** -->

アカウントには2種類のリスナがあります。1つはチャネルを使用するもので、もう1つはコールバックを使用するものです。このガイドでは、コールバックリスナを使います。チャネルの使い方は、[イベントリスナプラグインを作成する](../how-to-guides/create-plugin.md)をご覧ください。
<!-- Accounts have two types of listeners: One that uses channels and one that uses callbacks. In this guide, we use callback listeners. If you're interested in using a channel listener, see our guide for [creating an event-listener plugin](../how-to-guides/create-plugin.md). -->

:::info:
利用可能性なすべてのコールバックイベントの一覧は[こちら](https://github.com/iotaledger/iota.go/blob/master/account/event/listener/callback_listener.go)をご参照ください。
:::
<!-- :::info: -->
<!-- See the list of all possible [callback events](https://github.com/iotaledger/iota.go/blob/master/account/event/listener/callback_listener.go). -->
<!-- ::: -->

## 前提条件
<!-- ## Prerequisites -->

[アカウントを作成します](../how-to-guides/create-account.md)。
<!-- [Create an account](../how-to-guides/create-account.md). -->

## 入金と出金についてアカウントを監視する
<!-- ## Monitor your account for incoming and outgoing payments -->

アカウントの接続ノードが残高に影響するバンドルを受け取ると、アカウントは2種類のイベントをトリガできます。1つはバンドルが**ペンディング**状態にあるとき、もう1つは**含まれた**（確定済み）状態にあるときです。
<!-- When your account's connected nodes receive a bundle that affects your balance, your account can trigger two types of event: One when the bundle is in a **pending** state, and one when it's in an **included** (confirmed) state. -->

アカウントへのすべての入金は預け入れと呼ばれ、出金は取り出しと呼ばれます。
<!-- Any incoming payments to your account are called deposits, and outgoing payments are called withdrawals. -->

1. `EventMachine`オブジェクトを持つアカウントを作成して開始します。
  <!-- 1. Build and start an account that has an `EventMachine` object -->

    ```go
    node := "https://nodes.devnet.iota.org:443"
    seed := "PUETTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX"

    // ノードに接続するAPIオブジェクト
    apiSettings := api.HTTPClientSettings{URI: node}
    iotaAPI, err := api.ComposeAPI(apiSettings)
    handleErr(err)

    store, err := badger.NewBadgerStore("db")
    handleErr(err)

    em := event.NewEventMachine()

    // 正確なタイムソースを作成します（この場合、GoogleのNTPサーバー）。
    timesource := timesrc.NewNTPTimeSource("time.google.com")

    account, err = builder.NewBuilder().
    	// 使用するIOTA APIをロードします。
    	WithAPI(iotaAPI).
    	// 使用するデータベースオブジェクトをロードします。
    	WithStore(store).
    	// アカウントのシードをロードします。
    	WithSeed(seed).
    	// Devnetの最小重量値を使用します。
    	WithMWM(9).
    	// 入力選択中に使用するタイムソースをロードします。
    	WithTimeSource(timesource).
    	// EventMachineをロードします。
    	.WithEvents(em)
    	// アカウントの機能を強化するデフォルトのプラグインをロードします。
    	WithDefaultPlugins().
    	Build()
    handleErr(err)

    handleErr(account.Start())
    ```

2. 入金と出金を待機する新しい`CallbackEventListener`オブジェクトを作成します。
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

:::success:おめでとうございます！:tada:
アカウントはリッスンして行動することができるイベントを発信しています。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You're account can now emit events that you can listen to and act on. -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

イベントリスナを作ったので、これをテストするために[アカウントとの間で支払いを行います](../how-to-guides/create-and-manage-cda.md)。
<!-- Now that you have an event listener, start [making payments to/from your account](../how-to-guides/create-and-manage-cda.md) to test it. -->
