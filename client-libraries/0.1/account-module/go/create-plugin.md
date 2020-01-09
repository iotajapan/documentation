# Go でアカウントプラグインを作成する
<!-- # Create an account plugin in Go -->

**プラグインはアカウントの機能を拡張します。このガイドでは、アカウントのイベントをコンソールに出力するプラグインを作成します。**
<!-- **Plugins extend the functionality of an account. In this guide, you create a plugin that prints your account's events to the console.** -->

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
go get github.com/iotaledger/iota.go/account/event
go get github.com/iotaledger/iota.go/account/event/listener
```

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## 手順1. エベントロガーを作成する
<!-- ## Step 1. Create the event logger -->

1. `eventLogger.go` という新しいファイルを作成します。
<!-- 1. Create a new file called `eventLogger.go` -->

2. `EventMachine` オブジェクトを引数として取り、`account.Plugin` オブジェクトを返す関数を作成します。
  <!-- 2. Create a function that takes an `EventMachine` object as an argument and returns an `account.Plugin` object -->

    ```go
    // NewLogPlugin ...
    func NewLogPlugin(em event.EventMachine) account.Plugin {
        return &logplugin{em: em, exit: make(chan struct{})}
    }

    type logplugin struct {
    	em   event.EventMachine
    	acc  account.Account
    	exit chan struct{}
    }
    ```

4. プラグインの名前を返す `Name()` 関数を作成します。
  <!-- 4. Create a `Name()` function that returns the name of the plugin -->

    ```go
    func (l *logplugin) Name() string {
    	return "eventLogger"
    }
    ```

    :::info:
    `account` オブジェクトは、デバッグに役立つようにエラーメッセージでこの名前を使用します。
    :::
    <!-- :::info: -->
    <!-- The `account` object uses this name in error messages to help with debugging. -->
    <!-- ::: -->

5. アカウントの開始時に呼び出される `Start()` 関数を作成します。
  <!-- 5. Create a `Start()` function that will be called when the account starts -->

    ```go
    func (l *logplugin) Start(acc account.Account) error {
    	l.acc = acc
    	l.log()
    	return nil
    }
    ```

    :::info:
    すべてのプラグインは独自の Go ルーチンで実行され、アカウントと共に起動およびシャットダウンします。
    :::
    <!-- :::info: -->
    <!-- All plugins run in their own goroutine and start and shut down together with an account. -->
    <!-- ::: -->

6. アカウントと同時にプラグインをシャットダウンする`Shutdown()` 関数を作成します。
  <!-- 6. Create a `Shutdown()` function that shuts down the plugin at the same time as the account -->

    ```go
    func (l *logplugin) Shutdown() error {
    	l.exit <- struct{}{}
    	return nil
    }
    ```

7. イベント発生時にすべてのイベントをスクリーンに出力する `log()` 関数を作成します。
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

## 手順2. イベントロガーとともにアカウントを起動する
<!-- ## Step 2. Start your account with the event logger -->

1. `account.go` という新しいファイルを作成します。
  <!-- 1. Create a new file called `account.go` -->

2. イベントマシンを初期化します。
  <!-- 2. Initialize an event machine -->

    ```go
    em := event.NewEventMachine()
    ```

3. `NewEventLoggerPlugin()` 関数を使用してアカウントをビルドします。
  <!-- 3. Build your account with the `NewEventLoggerPlugin()` function -->

    ```go
    account, err = builder.NewBuilder().
        // IOTA API をロードします
        WithAPI(iotaAPI).
        // データベースオブジェクトをロードします
        WithStore(store).
        // アカウントのシードをロードします
        WithSeed(seed).
        // デブネット用の最小重量値を使用します
        WithMWM(9).
        // 入力選択中に使用するタイムソースをロードします
        WithTimeSource(timesource).
        // イベントマシンをロードします
        WithEvents(em)
        // アカウントの機能を強化するデフォルトのプラグインをロードします
        WithDefaultPlugins().
        // カスタムプラグインをロードします
		Build( NewLogPlugin(em) )
    handleErr(err)
    ```

:::success:おめでとうございます:tada:
最初のプラグインを作成しました。

今、アカウントが開始されると、イベントをリッスンするために何もする必要はありません。プラグインはイベントが起こるたびにコンソールにすべてのイベントを出力します。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You've just created your first plugin. -->

<!-- Now, when your account starts, you don't have to do anything to listen to events. Your plugin will print all events to the console as they happen. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

これらのコードサンプルは [GitHub](https://github.com/iota-community/account-module) でホストされています。
<!-- These code samples are hosted on [GitHub](https://github.com/iota-community/account-module). -->

開始するには、デバイスに [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) がインストールされている必要があります。
<!-- To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device. -->

Go 開発環境がない場合、または Go クライアントライブラリを初めて使用する場合は、[スタートガイド](../../getting-started/go-quickstart.md)を完了してください。
<!-- If you don't have a Go development environment, or if this is your first time using the Go client library, complete our [getting started guide](../../getting-started/go-quickstart.md). -->

コマンドラインで、次を実行します。
<!-- In the command-line, do the following: -->

```bash
git clone https://github.com/iota-community/account-module.git
cd account-module/go/account-module
go mod download
go run create-plugin/account.go create-plugin/eventLogger.go
```
アカウントが開始されると、イベントロガーが開始されることがわかります。
<!-- You should see that the event logger starts when your account does. -->

## 次のステップ
<!-- ## Next steps -->

[条件付きデポジットアドレスを生成する](../go/generate-cda.md)。
<!-- [Generate a conditional deposit address](../go/generate-cda.md). -->
