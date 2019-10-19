# アカウントを作成する
<!-- # Create an account -->

**アカウントは、支払いの処理を容易にするオブジェクトです。任意のIOTAネットワークでアカウントを使用して、IOTAトークンを送受信できます。**
<!-- **An account is an object that makes it easier to handle payments. You can use your account on any IOTA network to send and receive IOTA tokens.** -->

アカウントを使用する多くの利点の1つは、アドレスがアクティブまたは期限切れになる条件を定義できることです。これらの条件は、送信者がIOTAトークンをアドレスに送信しても安全かどうかを判断するのに役立ちます。このため、アカウントのアドレスは_条件付き預け入れアドレス_（CDA）と呼ばれます。
<!-- One of the many benefits of using accounts is that you can define conditions in which your addresses are active or expired. These conditions help senders to decide whether it's safe to send tokens to an address. For this reason, addresses in accounts are called _conditional deposit addresses_ (CDA). -->

## 条件付き預け入れアドレス
<!-- ## Conditional deposit addresses -->

IOTAプロトコルでは、IOTAトークンは[シード](root://getting-started/0.1/introduction/what-is-a-seed.md)から派生した[アドレス](root://dev-essentials/0.1/concepts/addresses-and-signatures.md)に送信する必要があります。これらのアドレスは一度だけIOTAトークンを取り出すことができます。そのため、IOTAトークンを一度取り出したアドレスにIOTAトークンを再度預け入れないことが重要です。しかし、あなたがアドレスから取り出すする前に、誰かがあなたのアドレスにIOTAトークンを預け入れようとしているのかどうかを知ることは困難です。
<!-- In the IOTA protocol, IOTA tokens must be sent to [addresses](root://dev-essentials/0.1/concepts/addresses-and-signatures.md), which are derived from your [seed](root://getting-started/0.1/introduction/what-is-a-seed.md). These addresses may be withdrawn from only once. As a result, it's important that no one deposits IOTA tokens into a withdrawn address. But, it's difficult to know when or if someone is going to deposit IOTA tokens into your address before you withdraw from it. -->

アカウントでは、アドレスには、支払いで使用される条件を指定できる追加機能があります。これらのアドレスは、条件付き預け入れアドレス（CDA）と呼ばれます。
<!-- In accounts, addresses come with extra features that allow you to specify the conditions in which they may be used in payments. These addresses are called conditional deposit addresses (CDA). -->

アカウントはCDAを使用して、[使用済みアドレスからの取り出しリスク](root://dev-essentials/0.1/concepts/addresses-and-signatures.md#address-reuse)を軽減します。誰かにIOTAトークンを要求すると、一定期間アクティブなCDAを作成できます。このようにして、その時間後にのみそのCDAからIOTAトークンを取り出すつもりであることを送信者に知らせます。その結果、送信者は、CDAに残っている時間に応じて、IOTAトークンの預け入れを行うかどうかを決定できます。
<!-- Accounts use CDAs to help reduce the [risk of withdrawing from spent addresses](root://dev-essentials/0.1/concepts/addresses-and-signatures.md#address-reuse). When you request IOTA tokens from someone, you can create a CDA that's active for a certain period of time. This way, you let the sender know that you intend to withdraw from that address only after that time. As a result, the sender can decide whether to make a deposit, depending on how much time is left on a CDA. -->

## シードステート
<!-- ## Seed state -->

アカウントは、デバイス上のローカルデータベースにステートが保存されている1つのシードにリンクされています。
<!-- An account is linked to one seed whose state is stored in a local database on your device. -->

支払いを追跡するために、このデータベースにはすべてのCDAのステートが保存されます。このステートはシードステートと呼ばれます。
<!-- To keep track of payments, this database stores the state of all CDAs. This state is called the seed state. -->

アカウントはこのデータを使用して、アクティビティの履歴を保持し、ノードへの不要なAPI呼び出しを回避します。
<!-- Accounts use this data to keep a history of activity and to avoid making unnecessary API calls to nodes. -->

| **データ** | **目的** |
| :----- | :--- |
| CDAの作成に使用された最後のキーインデックス | これまでに使用したことのない新しいCDAを作成するため。 |
| すべてのアクティブなCDA | 預け入れを受け取る可能性のあるCDAからの取り出しを停止するため。 |
| ペンディング中の転送 | ペンディング中のトランザクションを監視し、必要に応じて再ブロードキャストまたは再添付するため。 |

<!-- |**Data**| **Purpose**| -->
<!-- |:-----------------|:----------| -->
<!-- |:-----------------|:----------| -->
<!-- |The last key index that was used to create a CDA| Create a new CDA that has never been used before| -->
<!-- |All active CDAs|Stop withdrawals from CDAs that may receive deposits| -->
<!-- |Pending transfers| Monitor pending transactions and rebroadcast or reattach them if necessary| -->

## 前提条件
<!-- ## Prerequisites -->

このチュートリアルを完了するには、次のものが必要です。
<!-- To complete this tutorial, you need the following: -->

* コマンドプロンプトへのアクセス
<!-- * Access to a command prompt -->
* [Visual Studio Code](https://code.visualstudio.com/Download)などのコードエディタ
<!-- * A code editor such as [Visual Studio Code](https://code.visualstudio.com/Download) -->
* [入門ガイド](../introduction/overview.md)に従っている
<!-- * Follow our [Getting started guide](../introduction/overview.md) -->

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
    // ノードに接続するAPIオブジェクト
    apiSettings := api.HTTPClientSettings{URI: node}
    iotaAPI, err := api.ComposeAPI(apiSettings)
    handleErr(err)
    ```

4. アカウントがシードステートを保存できるストレージオブジェクトを作成します。この例では、シードステートはBadgerDBデータベースに保存されます。データベースディレクトリを保存するパスを`db`に変更します。
  <!-- 4. Create a storage object to which the account can save the seed state. In this example, the seed state is stored in a BadgerDB database. Change `db` to the path where you want to save the database directory. -->

    ```go
    store, err := badger.NewBadgerStore("db")
    handleErr(err)

    // データベースが閉じていることを確認します。
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
    // 正確なタイムソースを作成します（この場合、GoogleのNTPサーバー）。
    timesource := timesrc.NewNTPTimeSource("time.google.com")
    ```

6. カスタム設定と`WithDefaultPlugins()`メソッドの両方を使用してアカウントを作成すると、デフォルトの`transfer poller`プラグインと`promoter-reattacher`プラグインがアカウントに追加されます。
  <!-- 6. Build the account using both your custom settings and the `WithDefaultPlugins()` method adds the default `transfer poller` and `promoter-reattacher` plugins to the account. -->

    ```go
    account, err := builder.NewBuilder().
    	// 使用するIOTA API
    	WithAPI(iotaAPI).
    	// 使用するデータベースオブジェクト
    	WithStore(store).
    	// アカウントのシード
    	WithSeed(seed).
    	// Devnetの最小重量値
    	WithMWM(9).
    	// インプット選択中に使用するタイムソース
    	WithTimeSource(timesource).
    	// アカウントの機能を強化するデフォルトのプラグインをロードします。
    	WithDefaultPlugins().
    	Build()
    handleErr(err)
    ```

    :::danger:シードごとにアカウントインスタンスを1つ作成します
    同じシードで複数のアカウントを作成してはいけません。そうすると、シードステートが上書きされるという競合状態が発生する可能性があります。
    :::
    <!-- :::danger:Create one account instance per seed -->
    <!-- You must not create multiple instances of an account with the same seed. Doing so could lead to a race condition where the seed state would be overwritten. -->
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

    // アカウントがシャットダウンすることを確認します。
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

アカウントで特定のイベントが発生すると、それらのイベントを発行し、イベントをリッスンすることができます。たとえば、新しい支払いについて自分のアカウントを監視したい場合があります。そのためには、[イベントリスナを作成する](../how-to-guides/listen-to-events.md)必要があります。
<!-- After certain events happen in your account, it emits them, and allows you to listen for them. For example, you may want to monitor your account for new payments. To do so, you need to [create an event listener](../how-to-guides/listen-to-events.md). -->

またはイベントを発行する[プラグインを作成する](../how-to-guides/create-plugin.md)こともできます。
<!-- Or, you can [create a plugin](../how-to-guides/create-plugin.md) that also emits events. -->
