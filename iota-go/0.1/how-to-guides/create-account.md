# アカウントを作成する
<!-- # Create an account -->

**アカウントは、トランザクションの送受信を簡単にするためのオブジェクトです。アカウントは、アドレスやペンディング中のバンドルハッシュなどのデータをローカルデータベースに格納します。このデータにより、署名済みアドレスからの取り出しやペンディング中のトランザクションの促進や再添付を心配することなく、IOTAネットワークとやり取りすることができます。**
<!-- **An account is an object that makes it easier to send and receive transactions. Accounts store data such as addresses and pending bundle hashes in a local database. This data allows you to interact with an IOTA network without worrying about withdrawing from spent addresses or promoting and reattaching pending transactions.** -->

アカウントでは、すべてのアドレスは単純なIOTAアドレス以上のものです。これらのアドレスは、[条件付預け入れアドレス（CDA）](../how-to-guides/create-and-manage-cda.md)と呼ばれます。 CDAは81トライトのアドレスだけでなく、そのアドレスが[転送バンドル](root://getting-started/0.1/introduction/what-is-a-bundle.md)内で使用される可能性のある条件も定義できます。
<!-- In accounts, all addresses are more than simple IOTA addresses. These addresses are called [conditional deposit addresses (CDAs)](../how-to-guides/create-and-manage-cda.md). A CDA defines not only the 81-tryte address, but also the conditions in which that address may be used in a [transfer bundle](root://getting-started/0.1/introduction/what-is-a-bundle.md). -->

## シードステート
<!-- ## Seed state -->

アカウントがローカルデータベースに格納するデータは、シードステートと呼ばれます。アカウントはこのデータを使用して活動履歴を保持し、ノードへの不要なAPI呼び出しを回避します。
<!-- The data that accounts store is called the seed state. Accounts use this data to keep a history of activity and to avoid making unnecessary API calls to nodes. -->

|**データ**|**目的**|
|:---------|:-------|
|CDAの作成に使用された最後のキーインデックス|これまで使用されたことのない新しいCDAを作成する|
|すべてのアクティブなCDA|IOTAトークンの預け入れを受け取る可能性のあるCDAからのIOTAトークンの取り出しを停止する|
|ペンディング中の転送バンドル|ペンディング中のトランザクションを監視して再ブロードキャストするか、必要に応じて再添付を行う|

## 前提条件
<!-- ## Prerequisites -->

このガイドは[概要の「はじめに」](../README.md)で紹介されている、プロジェクトの依存関係を管理するための[Goモジュール](https://github.com/golang/go/wiki/Modules)を使っていると仮定します。
<!-- This guide assumes that you've followed our [Getting started guide](../README.md) and are using the [Go modules](https://github.com/golang/go/wiki/Modules) to manage dependencies in your project. -->

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
    <!-- :::danger:Protect your seed -->
    <!-- You should never hard code a seed as we do here. Instead, we recommend that you read the seed from a protected file. -->
    <!--  -->
    <!-- If you want to use a seed from a particular location, for example a hardware wallet, create a custom `SeedProvider` object, and pass it to the `WithSeedProvider()` method. -->
    <!-- ::: -->

    :::danger:初めてのアカウント作成ですか？
    これまでにアカウントを作成したことがなくても、トランザクションの送信にシードを使用したことがある場合、アカウントモジュールはシードステートを知りません。そのため、既存のシードを使用する代わりに、アカウントで使用する新しいシードを作成する必要があります。
    :::
    <!-- :::danger:Is this your first account? -->
    <!-- If you have never created an account before, but you have already used your seed to send transactions, the account module does not know the seed state. So, instead of using your existing seed, you must create a new seed to use with an account. -->
    <!-- ::: -->

3. ノードに接続するAPIオブジェクトを作成します。
  <!-- 3. Create an API object that connects to a node -->

    ```go
    // API object that connects to a node
    apiSettings := api.HTTPClientSettings{URI: node}
    iotaAPI, err := api.ComposeAPI(apiSettings)
    handleErr(err)
    ```

4. アカウントがシードステートを保存できるストレージオブジェクトを作成します。この例では、シードステートはBadgerDBデータベースに格納されています。データベースディレクトリを保存するパスを`db`に変更します。
  <!-- 4. Create a storage object to which the account can save the seed state. In this example, the seed state is stored in a BadgerDB database. Change `db` to the path where you want to save the database directory. -->

    ```go
    store, err := badger.NewBadgerStore("db")
    handleErr(err)
    ```

    :::danger:重要
    与えられた`Store`オブジェクトをclose可能であれば、closeする必要があります。そうしないと、データベースがロックされる可能性があります。

    たとえば、BadgerDBを使用している場合は、`defer store.Close()`の行を追加することをお勧めします。
    :::
    <!-- :::danger:Important -->
    <!-- If the given `Store` object is closeable, you must close it, otherwise the database may become locked. -->
    <!--  -->
    <!-- For example, if you use BadgerDB, you may want to add the following line `defer store.Close()`. -->
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

5. [`timesrc`パッケージ](https://github.com/iotaledger/iota.go/tree/master/account/timesrc)を使用して、正確な時刻を返すオブジェクトを作成します。この例では、タイムソースはGoogle NTP（ネットワークタイムプロトコル）サーバーです。
  <!-- 5. Use the [`timesrc` package](https://github.com/iotaledger/iota.go/tree/master/account/timesrc) to create an object that returns an accurate time. In this example, the time source is a Google NTP (network time protocol) server. -->

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

    :::danger:シードごとにアカウントを1つ作成してください
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
    ```

    30秒ごとに、`transfer-poller`プラグインは、取り出しトランザクションが確定されたかどうか、またはペンディング中の預け入れトランザクションがあるかどうかを確認します。それから、`promoter-reattacher`プラグインは、ペンディング中の取り出しトランザクションの促進または再添付を行います。
    <!-- Every 30 seconds, the `transfer-poller` plugin will check whether withdrawals have been confirmed or whether any deposits are pending. Then, the `promoter-reattacher` plugin will promote or reattach any pending withdrawal transactions. -->

    :::danger:重要
    アカウントが常にシャットダウンできることを確認してください。そうしないと、予期しない結果が生じる可能性があります。

    たとえば、`defer account.Shutdown()`の行を追加することをお勧めします。
    :::
    <!-- :::danger:Important -->
    <!-- Make sure that the account can always shut down, otherwise you may see unexpected results. -->
    <!--  -->
    <!-- For example, you may want to add the following line `defer account.Shutdown()`. -->
    <!-- ::: -->

    :::info:
    プラグインの動作をもっと制御したい場合は、`WithPlugin()`メソッドでプラグインをカスタマイズできます。
    :::
    <!-- :::info: -->
    <!-- If you want to have more control over the behavior of the plugins, you can customize them in the `WithPlugin()` method. -->
    <!-- ::: -->

8. [同期ノード](root://iri/0.1/how-to-guides/run-an-iri-node-on-linux.md#check-that-the-iri-is-synchronized)に接続していることを確認します。
  <!-- 8. Check that you're connected to a [synchronized node](root://iri/0.1/how-to-guides/run-an-iri-node-on-linux.md#check-that-the-iri-is-synchronized) -->

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

[トランザクションを送受信できるCDAを作成する](../how-to-guides/create-and-manage-cda.md)。
<!-- [Create a CDA so that you can send and receive transactions](../how-to-guides/create-and-manage-cda.md). -->
