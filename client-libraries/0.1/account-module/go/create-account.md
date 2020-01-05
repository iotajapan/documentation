# Go でアカウントを作成する
<!-- # Create an account in Go -->

**このガイドでは、ローカルデータベースでシードステートを追跡するためのアカウントを作成し、アカウントの残高をコンソールに出力します。**
<!-- **In this guide, you create an account to keep track of your seed state in a local database and print your account's balance to the console.** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには、次のパッケージをインストールする必要があります（Go モジュールを使用している場合は、これらのパッケージを参照するだけです）。
<!-- To complete this guide, you need to install the following packages (if you're using Go modules, you just need to reference these packages): -->

```bash
go get github.com/iotaledger/iota.go/api
go get github.com/iotaledger/iota.go/badger
go get github.com/iotaledger/iota.go/builder
go get github.com/iotaledger/iota.go/timesrc
go get github.com/iotaledger/iota.go/trinary
```

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、以下のネットワーク設定で[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet) with the following network settings: -->

- **[最小重量値(MWM)](root://getting-started/0.1/network/minimum-weight-magnitude.md)**: 9
<!-- - **[Minimum weight magnitude](root://getting-started/0.1/network/minimum-weight-magnitude.md)**: 9 -->

- **[深度](root://getting-started/0.1/transactions/depth.md)**: 3
<!-- - **[Depth](root://getting-started/0.1/transactions/depth.md)**: 3 -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1\. 新しいシードを作成し、シードをバックアップします。
<!-- 1\. Create a new seed and back it up -->

:::info:
既存のシードは、シードステートが不明であるため、使用しても安全ではありません。そのため、これらのシードは、アカウントが認識していない使用済みアドレスを持っている場合が有ります。
:::
<!-- :::info: -->
<!-- Existing seeds are not safe to use because their state is unknown. As such, these seeds may have spent addresses that the account is not aware of. -->
<!-- ::: -->

--------------------
### Linux
```bash
cat /dev/urandom |tr -dc A-Z9|head -c${1:-81}
```
---
### macOS
```bash
cat /dev/urandom |LC_ALL=C tr -dc 'A-Z9' | fold -w 81 | head -n 1
```
---
### Windows PowerShell
```bash
$b=[byte[]] (1..81);(new-object Security.Cryptography.RNGCryptoServiceProvider).GetBytes($b);-join($b|%{[char[]] (65..90+57..57)[$_%27]})
```
--------------------

2\. アカウントを使用するシードを定義します。
<!-- 2\. Define the seed that your account will use -->

```go
var seed = trinary.Trytes("PUEOTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX")
```

3\. ノードに接続します。
<!-- 3\. Connect to a node -->

```go
apiSettings := api.HTTPClientSettings{URI: "https://nodes.devnet.iota.org:443"}
iotaAPI, err := api.ComposeAPI(apiSettings)
handleErr(err)
```

4\. アカウントがシードステートを保存できるストレージオブジェクトを作成します。この例では、シードステートは BadgerDB データベースに保存されます。`db` をデータベースディレクトリを保存するパスに変更します。
<!-- 4\. Create a storage object to which the account can save the seed state. In this example, the seed state is stored in a BadgerDB database. Change `db` to the path where you want to save the database directory. -->

```go
store, err := badger.NewBadgerStore("seed-state-database")
handleErr(err)

// コードが停止したときにデータベースが閉じるようにします
defer store.Close()
```

5\. 正確な時刻を返す `timesource` オブジェクトを作成します。これは、アカウントが CDA がまだアクティブかどうかを判断するために使用します。この例では、タイムソースは [Google NTP（ネットワークタイムプロトコル）サーバー](https://developers.google.com/time/faq)です。
<!-- 5\. Create a `timesource` object that returns an accurate time, which the account will use to decide if your CDAs are still active. In this example, the time source is a [Google NTP (network time protocol) server](https://developers.google.com/time/faq). -->

```go
timesource := timesrc.NewNTPTimeSource("time.google.com")
```

6\. アカウントを作成します。カスタム設定を指定しない場合、アカウントは[デフォルトの設定](https://github.com/iotaledger/iota.go/blob/master/.docs/iota.go/reference/account_default_settings.md)を使用します。
<!-- 6\. Build your account. If you don't specify any custom settings, the account uses the [defaults](https://github.com/iotaledger/iota.go/blob/master/.docs/iota.go/reference/account_default_settings.md). -->

```go
account, err := builder.NewBuilder().
    // ノードに接続します
    WithAPI(iotaAPI).
    // データベースを作成します
    WithStore(store).
    WithSeed(seed).
    // デブネット用の最小重量値をセットします
    WithMWM(9).
    WithTimeSource(timesource).
    // アカウントの機能を強化するデフォルトのプラグインをロードします
    WithDefaultPlugins().
    Build()
handleErr(err)
```

デフォルトのプラグインには、`transfer-poller` と `promoter-reattacher` プラグインが含まれます。
<!-- The default plugins include the `transfer-poller` and the `promoter-reattacher` plugins. -->

30秒ごとに、`transfer-poller` プラグインは、取り出しが確定されたかどうか、またはペンディング中のデポジットがあるかどうかをチェックします。
<!-- Every 30 seconds, the `transfer-poller` plugin checks whether withdrawals have been confirmed or whether any deposits are pending. -->

`promoter-reattacher` プラグインは、`transfer-poller` が検出したすべてのペンディング中の取り出しトランザクションを[プロモートまたは再アタッチ](root://getting-started/0.1/transactions/reattach-rebroadcast-promote.md)します。
<!-- The `promoter-reattacher` plugin [promotes or reattaches](root://getting-started/0.1/transactions/reattach-rebroadcast-promote.md) any pending withdrawal transactions that the `transfer-poller` finds. -->

:::info:
CDA のデフォルトのセキュリティレベルは2です。この設定は、新しいセキュリティレベルを `WithSecurityLevel()` メソッドに渡すことで変更できます。
:::
<!-- :::info: -->
<!-- The default security level for CDAs is 2. You can change this setting by passing a new security level to the `WithSecurityLevel()` method. -->
<!-- ::: -->

7\. アカウントとプラグインを起動します。
<!-- 7\. Start the account and the plugins -->

```go
handleErr(account.Start())

defer account.Shutdown()
```

8\. アカウントの残高を確認します。
<!-- 8\. Check your account's balance -->

```go
balance, err := account.AvailableBalance()
handleErr(err)
fmt.Println("Total available balance: ")
fmt.Println(balance)
```

:::success:おめでとうございます:tada:
トランザクションを自動的にプロモートおよび再アタッチし、シードステートを管理するアカウントを作成しました。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You've created an account that will automatically promote and reattach transactions as well as manage the state of your seed. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

開始するには、デバイスに [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) がインストールされている必要があります。
<!-- To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device. -->

Go 開発環境がない場合、または Go クライアントライブラリを初めて使用する場合は、[スタートガイド](../../getting-started/go-quickstart.md)を完了してください。
<!-- If you don't have a Go development environment, or if this is your first time using the Go client library, complete our [getting started guide](../../getting-started/go-quickstart.md). -->

コマンドラインで、次を実行します。
<!-- In the command-line, do the following: -->

```bash
git clone https://github.com/JakeSCahill/iota-samples.git
cd iota-samples/go/account-module
go mod download
go run create-account/create-account.go
```
新しいアカウントの残高が表示されます。
<!-- You should see the balance of your new account. -->

シードステートを追跡するデータベースファイルもあります。
<!-- You'll also have a database file that keeps track of your seed state. -->

## 次のステップ
<!-- ## Next steps -->

アカウントで特定のイベントが発生すると、アカウントはそのイベントを発行するので、発行されたイベントをリッスンすることができます。たとえば、アカウントの新規支払いをモニタリングすることができます。そのためには、[イベントリスナーを作成する](../go/listen-to-events.md)必要があります。
<!-- After certain events happen in your account, it emits them, and allows you to listen for them. For example, you may want to monitor your account for new payments. To do so, you need to [create an event listener](../go/listen-to-events.md). -->

または、イベントを発行する[プラグインを作成](../go/create-plugin.md)することもできます。
<!-- Or, you can [create a plugin](../go/create-plugin.md) that also emits events. -->
