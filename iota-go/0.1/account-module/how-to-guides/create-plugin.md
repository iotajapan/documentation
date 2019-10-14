# プラグインを作成する
<!-- # Create a plugin -->

**プラグインはアカウントの機能を拡張します。すべてのプラグインは独自のゴルーチンで実行され、アカウントとともに起動およびシャットダウンされます。**
<!-- **Plugins extend the functionality of an account. All plugins run in their own goroutine and start and shut down together with an account.** -->

## 前提条件
<!-- ## Prerequisites -->

[アカウントを作成します](../how-to-guides/create-account.md)。
<!-- [Create an account](../how-to-guides/create-account.md). -->

このガイドは[概要の「はじめに」](../introduction/overview.md)で紹介されている、プロジェクトの依存関係を管理するための[Goモジュール](https://github.com/golang/go/wiki/Modules)を使っていると仮定します。
<!-- This guide assumes that you've followed our [Getting started guide](../introduction/overview.md) and are using the [Go modules](https://github.com/golang/go/wiki/Modules) to manage dependencies in your project. -->

## スクリーンにイベントを出力するプラグインを作成する
<!-- ## Create a plugin that prints events to the screen -->

このガイドでは、プラグインの作成方法を説明するために、イベントが発生したときにイベントをスクリーンに出力するプラグインを作成します。
<!-- To explain how to create a plugin, this guide helps you to create one that prints events to the screen as they happen. -->

:::info:
利用可能性なすべてのチャネルイベントの一覧は[こちら](https://github.com/iotaledger/iota.go/blob/master/account/event/listener/channel_listener.go)をご参照ください。
:::
<!-- :::info: -->
<!-- See the list of all possible [channel events](https://github.com/iotaledger/iota.go/blob/master/account/event/listener/channel_listener.go). -->
<!-- ::: -->

1. `event_logger.go`という新しいファイルを作成します。
  <!-- 1. Create a new file called `event_logger.go` -->

2. 必要なパッケージをインポートします。
  <!-- 2. Import the required packages -->

    ```go
    package main

    import (
    	"fmt"

    	"github.com/iotaledger/iota.go/account"
    	"github.com/iotaledger/iota.go/account/event"
    	"github.com/iotaledger/iota.go/account/event/listener"
    )
    ```

3. `EventMachine`オブジェクトを引数として受け取り、`account.Plugin`オブジェクトを返す関数を作成します。
  <!-- 3. Create a function that takes an `EventMachine` object as an argument and returns an `account.Plugin` object -->

    ```go
    // NewEventLoggerPlugin ...
    func NewEventLoggerPlugin(em event.EventMachine) account.Plugin {
    	return &logplugin{em: em, exit: make(chan struct{})}
    }

    type logplugin struct {
    	em   event.EventMachine
    	acc  account.Account
    	exit chan struct{}
    }
    ```

4. プラグインの名前を返す`Name()`関数を作成します。
  <!-- 4. Create a `Name()` function that returns the name of the plugin -->

    ```go
    func (l *logplugin) Name() string {
    	return "eventLogger"
    }
    ```

    :::info:
    `account`オブジェクトは、デバッグに役立つようにエラーメッセージでこの名前を使用します。
    :::
    <!-- :::info: -->
    <!-- The `account` object uses this name in error messages to help you debug. -->
    <!-- ::: -->

5. アカウントの開始時に呼び出される`Start()`関数を作成します。アカウントが開始されると、すべてのプラグインは`account`オブジェクトを引数として取ります。
  <!-- 5. Create a `Start()` function that will be called when the account starts. When an account is started, all plugins take the `account` object as an argument. -->

    ```go
    func (l *logplugin) Start(acc account.Account) error {
    	l.acc = acc
    	l.log()
    	return nil
    }
    ```

6. アカウントと同時にプラグインをシャットダウンする`Shutdown()`関数を作成します。
  <!-- 6. Create a `Shutdown()` function that shuts down the plugin at the same time as the account -->

    ```go
    func (l *logplugin) Shutdown() error {
    	l.exit <- struct{}{}
    	return nil
    }
    ```

7. イベント発生時にすべてのイベントをスクリーンに出力する`log()`関数を作成します。
  <!-- 7. Create the `log()` function that will print all events to the screen when they happen -->

    ```go
    func (l *logplugin) log() {
    	lis := listener.NewChannelEventListener(l.em).All()

    	go func() {
    		defer lis.Close()
    		exit:
    		for {
    			select {
    			case ev := <-lis.Promoted:
    				fmt.Printf("Promoted %s with %s\n", ev.BundleHash[:10], ev.PromotionTailTxHash)
    			case ev := <-lis.Reattached:
    				fmt.Printf("Reattached %s with %s\n", ev.BundleHash[:10], ev.ReattachmentTailTxHash)
    			case ev := <-lis.SentTransfer:
    				tail := ev[0]
    				fmt.Printf("Sent %s with tail %s\n", tail.Bundle[:10], tail.Hash)
    			case ev := <-lis.TransferConfirmed:
    				tail := ev[0]
    				fmt.Printf("Transfer confirmed %s with tail %s\n", tail.Bundle[:10], tail.Hash)
    			case ev := <-lis.ReceivingDeposit:
    				tail := ev[0]
    				fmt.Printf("Receiving deposit %s with tail %s\n", tail.Bundle[:10], tail.Hash)
    			case ev := <-lis.ReceivedDeposit:
    				tail := ev[0]
    				fmt.Printf("Received deposit %s with tail %s\n", tail.Bundle[:10], tail.Hash)
    			case ev := <-lis.ReceivedMessage:
    				tail := ev[0]
    				fmt.Printf("Received msg %s with tail %s\n", tail.Bundle[:10], tail.Hash)
    			case balanceCheck := <-lis.ExecutingInputSelection:
    				fmt.Printf("Doing input selection (balance check: %v) \n", balanceCheck)
    			case <-lis.PreparingTransfers:
    				fmt.Printf("Preparing transfers\n")
    			case <-lis.GettingTransactionsToApprove:
    				fmt.Printf("Getting transactions to approve\n")
    			case <-lis.AttachingToTangle:
    				fmt.Printf("Doing proof of work\n")
    			case err := <-lis.InternalError:
    				fmt.Printf("Received internal error: %s\n", err.Error())
    			case <-l.exit:
    				break exit
    			}
    		}
    	}()
    }
    ```

8. ファイルを保存します。
  <!-- 8. Save the file -->

9. `myAccount.go`という新しいファイルを作成します。
  <!-- 9. Create a new file called `myAccount.go` -->

10. `NewEventLoggerPlugin()`関数を使用してアカウントを作成します。
  <!-- 10. Build your account with the `NewEventLoggerPlugin()` function -->

    ```go
    account, err = builder.NewBuilder().
    	// 使用するIOTA APIをロードします。
    	WithAPI(iotaAPI).
    	// 使用するデータベースオブジェクトをロードします。
    	WithStore(store).
    	// アカウントのシードをロードします。
    	WithSeed(seed).
    	// Devnetの最小重量値を使用します。
    	WithMWM(9).
    	//入力選択中に使用するタイムソースをロードします。
    	WithTimeSource(timesource).
    	// EventMachineをロードします。
    	.WithEvents(em)
    	// アカウントの機能を強化するデフォルトのプラグインをロードします。
    	WithDefaultPlugins().
    	// カスタムプラグインをロードします。
    	Build( NewEventLoggerPlugin(em) )
    handleErr(err)
    ```

:::success:おめでとうございます！:tada:
最初のプラグインを作成しました。

今、アカウントが開始されると、イベントをリッスンするために何もする必要はありません。プラグインはイベントが起こるたびにコンソールにすべてのイベントを出力します。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You've just created your first plugin. -->
<!--  -->
<!-- Now, when your account starts, you don't have to do anything to listen to events. Your plugin will print all events to the console as they happen. -->
<!-- ::: -->
