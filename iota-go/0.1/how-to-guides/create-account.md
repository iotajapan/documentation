# アカウントを作成する
<!-- # Create an account -->

**アカウントは、支払いの処理を簡単にし、ペンディング中および確定済みの履歴を保持するためのオブジェクトです。アカウントはどのIOTAネットワークでも使用できます。**
<!-- **An account is an object that makes it easier to handle payments and keep a history of pending and confirmed ones. You can use your account on any IOTA network.** -->

## 前提条件
<!-- ## Prerequisites -->

このチュートリアルを完了するには、次のものが必要です。
<!-- To complete this tutorial, you need the following: -->

* コマンドプロンプトへのアクセス
<!-- * Access to a command prompt -->
* [Visual Studio Code](https://code.visualstudio.com/Download)などのコードエディタ
<!-- * A code editor such as [Visual Studio Code](https://code.visualstudio.com/Download) -->
* インターネット接続
<!-- * An Internet connection -->
* [入門ガイド](../README.md)に従っている
<!-- * Follow our [Getting started guide](../README.md) -->

:::warning: 新しいシードを作成する
これまでにアカウントを作成したことがない場合は、既存のシードステートが不明であるため、[新しいシードを作成する](root://getting-started/0.1/tutorials/get-started.md)必要があります。
:::
<!-- :::warning: Create a new seed -->
<!-- If you have never created an account before, you must [create a new seed](root://getting-started/0.1/tutorials/get-started.md) because existing seed states are unknown. -->
<!-- ::: -->

## アカウントを作成する
<!-- ## Create a new account -->

この例では、[Devnetノード](root://getting-started/0.1/references/iota-networks.md#devnet)に接続します。Devnetは、トークンが無料であること以外はMainnetとほぼ同じです。Devnetに送信したトランザクションは、Mainnetのような他のネットワークには存在しません。
<!-- In this example, we connect to a [Devnet node](root://getting-started/0.1/references/iota-networks.md#devnet). The Devnet is similar to the Mainnet, except the tokens are free. Any transactions that you send to the Devnet do not exist on other networks such as the Mainnet. -->

1. 必要なパッケージをインポートします。
  <!-- 1. Import the required packages -->

    ```go
    package main

    import (
    "github.com/iotaledger/iota.go/account/builder"
    "github.com/iotaledger/iota.go/account/store/badger"
    "github.com/iotaledger/iota.go/account/timesrc"
    "github.com/iotaledger/iota.go/api"
    )
    ```

2. 2つの変数を作成します。1つはシード用、もう1つはアカウントが接続するノード用です。
  <!-- 2. Create two variables: One for your seed and another for the node that the account connects to -->

    ```go
    seed := "PUEOTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX"
    node := "https://nodes.devnet.iota.org:443"
    ```

    :::danger:シードを保護する
    ここで行うように、シードを絶対にハードコーディングしないでください。代わりに、保護されたファイルからシードを読み込むことをお勧めします。

    ハードウェアウォレットなど、特定の場所のシードを使用する場合は、カスタム`SeedProvider`オブジェクトを作成して`WithSeedProvider()`メソッドに渡します。
    :::

3. ノードに接続するAPIオブジェクトを作成します。
  <!-- 3. Create an API object that connects to a node -->

    ```go
    // API object that connects to a node
    apiSettings := api.HTTPClientSettings{URI: node}
    iotaAPI, err := api.ComposeAPI(apiSettings)
    handleErr(err)
    ```

4. アカウントがシードステートを保存できるストレージオブジェクトを作成します。この例では、シードステートはBadgerDBデータベースに保存されます。データベースディレクトリを保存するパスを`db`に変更します。
  <!-- 4. Create a storage object to which the account can save the seed state. In this example, the seed state is stored in a BadgerDB database. Change `db` to the path where you want to save the database directory. -->

    ```go
    store, err := badger.NewBadgerStore("db")
    handleErr(err)

    // Make sure the database closes
    defer store.Close()
    ```

    :::danger:重要
    与えられた`Store`オブジェクトをclose可能であれば、closeする必要があります。
    :::
    <!-- :::danger:Important -->
    <!-- If the given `Store` object is closeable, you must close it, otherwise the database may become locked. -->
    <!-- ::: -->

    :::info:
    ストレージ内で、各アカウントは一意のIDを持ちます。このIDは、インデックス0およびセキュリティレベル2のアカウントの最初のアドレスのハッシュ値です。

    その結果、複数のアカウントに同じストレージオブジェクトを同時に使用することができます。
    :::
    <!-- :::info: -->
    <!-- In storage, each account has a unique ID, which is the hash of the first address of the account at index 0 and security level 2. -->
    <!--  -->
    <!-- As a result, you can use the same storage object for multiple accounts at the same time. -->
    <!-- ::: -->

5. 正確な時刻を返すオブジェクトを作成します。この例の時刻源はGoogle NTP（ネットワークタイムプロトコル）サーバーです。
  <!-- 5. Create an object that returns an accurate time. In this example, the time source is a Google NTP (network time protocol) server. -->

     ```go
    // create an accurate time source (in this case Google's NTP server).
    timesource := timesrc.NewNTPTimeSource("time.google.com")
    ```

6. カスタム設定と`WithDefaultPlugins()`メソッドの両方を使用してアカウントを作成すると、デフォルトの`transfer poller`プラグインと`promoter-reattacher`プラグインがアカウントに追加されます。
  <!-- 6. Build the account using both your custom settings and the `WithDefaultPlugins()` method adds the default `transfer poller` and `promoter-reattacher` plugins to the account. -->

    ```go
    account, err := builder.NewBuilder().
    	// the IOTA API to use
    	WithAPI(iotaAPI).
    	// the database onject to use
    	WithStore(store).
    	// the seed of the account
    	WithSeed(seed).
    	// the minimum weight magnitude for the Devnet
    	WithMWM(9).
    	// the time source to use during input selection
    	WithTimeSource(timesource).
    	// load the default plugins that enhance the functionality of the account
    	WithDefaultPlugins().
    	Build()
    handleErr(err)
    ```

    :::danger:シードごとにアカウントを1つ作成します
    同じシードで複数のアカウントを作成してはいけません。そうすると、シードステートが上書きされるという競合状態が発生する可能性があります。
    :::
    <!-- :::danger:Create one account per seed -->
    <!-- You must not create multiple accounts with the same seed. Doing so could lead to a race condition where the seed state would be overwritten. -->
    <!-- ::: -->

    :::info:デフォルト設定
    カスタム設定を指定しないとき、アカウントは[デフォルト設定](https://github.com/iotaledger/iota.go/blob/master/account/settings.go)を使用します。
    :::
    <!-- :::info:Default settings -->
    <!-- If you don't specify any custom settings, the account uses the [defaults](https://github.com/iotaledger/iota.go/blob/master/account/settings.go). -->
    <!-- ::: -->

7. アカウントとプラグインを起動します。
  <!-- 7. Start the account and the plugins -->

    ```go
    handleErr(account.Start())

    // Make sure the account shuts down
    defer account.Shutdown()
    ```

    30秒ごとに、`transfer-poller`プラグインは、取り出しトランザクションが確定されたかどうか、またはペンディング中の預け入れトランザクションがあるかどうかを確認します。それから、`promoter-reattacher`プラグインは、ペンディング中の取り出しトランザクションの促進または再添付を行います。
    <!-- Every 30 seconds, the `transfer-poller` plugin will check whether withdrawals have been confirmed or whether any deposits are pending. Then, the `promoter-reattacher` plugin will promote or reattach any pending withdrawal transactions. -->

    :::info:
    プラグインの動作をもっと制御したい場合は、`WithPlugin()`メソッドでプラグインをカスタマイズできます。
    :::
    <!-- :::info: -->
    <!-- If you want to have more control over the behavior of the plugins, you can customize them in the `WithPlugin()` method. -->
    <!-- ::: -->

8. [同期しているノード](root://node-software/0.1/iri/how-to-guides/run-an-iri-node-on-linux.md#check-that-the-iri-is-synchronized)に接続していることを確認します。
  <!-- 8. Check that you're connected to a [synchronized node](root://node-software/0.1/iri/how-to-guides/run-an-iri-node-on-linux.md#check-that-the-iri-is-synchronized) -->

    ```go
    nodeInfo, err := iotaAPI.GetNodeInfo()
    handleErr(err)

    fmt.Println("latest milestone index:", nodeInfo.LatestMilestoneIndex)
    fmt.Println("latest milestone index:", nodeInfo.LatestSolidSubtangleMilestone)
    ```

:::success:おめでとうございます！:tada:
シードステートを管理し、かつ自動的にトランザクションを促進、再添付するアカウントを作成しました。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You've created an account that will automatically promote and reattach transactions as well as manage the state of your seed. -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

アカウントで特定のイベントが発生すると、それらのイベントを発行し、イベントをリッスンすることができます。たとえば、新しい支払いについて自分のアカウントを監視したい場合があります。そのためには、[イベントリスナを作成する](root://iota-go/0.1/how-to-guides/listen-to-events.md)必要があります。
<!-- After certain events happen in your account, it emits them, and allows you to listen for them. For example, you may want to monitor your account for new payments. To do so, you need to [create an event listener](root://iota-js/0.1/how-to-guides/listen-to-events.md). -->

またはイベントを発行する[プラグインを作成する](../how-to-guides/create-plugin.md)こともできます。
<!-- Or, you can [create a plugin](../how-to-guides/create-plugin.md) that also emits events. -->
