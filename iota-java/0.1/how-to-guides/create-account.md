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
* Java 6以上
<!-- * Java 6 (or higher) -->
* [クライアントライブラリ](root://iota-java/0.1/README.md#download-the-library)のダウンロード
<!-- * [Download the client library](root://iota-java/0.1/README.md#download-the-library) -->

:::warning: 新しいシードを作成してください
これまでにアカウントを作成したことがない場合は、既存のシードステートが不明であるため、[新しいシードを作成する](root://getting-started/0.1/tutorials/get-started.md)必要があります。
:::
<!-- :::warning: Create a new seed -->
<!-- If you have never created an account before, you must [create a new seed](root://getting-started/0.1/tutorials/get-started.md) because existing seed states are unknown. -->
<!-- ::: -->

## 新しいアカウントを作成する
<!-- ## Create a new account -->

この例では、[Devnetノード](root://getting-started/0.1/references/iota-networks.md#devnet)に接続します。Devnetは、トークンが無料であること以外はMainnetとほぼ同じです。Devnetに送信したトランザクションは、Mainnetのような他のネットワークには存在しません。
<!-- In this example, we connect to a [Devnet node](root://getting-started/0.1/references/iota-networks.md#devnet). The Devnet is similar to the Mainnet, except the tokens are free. Any transactions that you send to the Devnet do not exist on other networks such as the Mainnet. -->

:::danger:重要
アカウントにはデフォルト設定がありますが、少なくともシードとMongoDBなどのストレージプロバイダーを指定することをお勧めします。そうでなければ、コードの実行が停止した後にシードステートは保存されません。
:::
<!-- :::danger:Important -->
<!-- Although the account has default settings, we recommend that you provide at least a seed and a storage provider such as MongoDB. Otherwise, the seed state will not be saved after the code stops running. -->
<!-- ::: -->

1. ノードに接続する`IotaAPI`オブジェクトを作成します。
  <!-- 1. Create an `IotaAPI` object that connects to a node -->

    ```java
    IotaAPI api = new IotaAPI.Builder()

                    .host("nodes.devnet.iota.org")

                    .port(443)

                    .protocol("https")

                    .timeout(500)
                    .build();
    ```

2. シードを保持するための変数を作成します。
  <!-- 2. Create a variable to hold your seed -->

    ```java
    String mySeed = "PUEOTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX";
    ```

    :::danger:シードを保護する
    ここで行っているようにシードをハードコーディングしないでください。代わりに、保護されたファイルからシードを読み込むことをお勧めします。

    ハードウェアウォレットなど、特定の場所のシードを使用する場合は、カスタム`SeedProvider`オブジェクトを作成して、それを`Builder()`コンストラクタに渡すことができます。
    :::
    <!-- ::: -->
    <!-- :::danger:Protect your seed -->
    <!-- You should never hard code a seed as we do here. Instead, we recommend that you read the seed from a protected file. -->
    <!--  -->
    <!-- If you want to use a seed from a particular location, for example a hardware wallet, you can make a custom `SeedProvider` object, and pass it to the `Builder()` constructor. -->
    <!-- ::: -->

3. アカウントがシードステートを保存できるストレージオブジェクトを作成します。この例では、シードステートはメモリストアデータベースに保存されます。
  <!-- 3. Create a storage object to which the account can save the seed state. In this example, the seed state is stored in a Memory Store database. -->

    ```Java
    AccountStore store = new AccountStoreImpl(new MemoryStore());
    ```

    :::info:
    ストレージ内で、各アカウントは一意のIDを持ちます。このIDは、インデックス0、セキュリティレベル2のアドレスのハッシュ値です。

    その結果、複数のアカウントに同じストレージオブジェクトを同時に使用することができます。
    :::
    <!-- :::info: -->
    <!-- In storage, each account has a unique ID, which is a hash of an address with index 0 and security level 2. -->
    <!--  -->
    <!-- As a result, you can use the same storage object for multiple accounts at the same time. -->
    <!-- ::: -->

4. カスタム設定を使用してアカウントを作成します。
  <!-- 4. Create the account using your custom settings -->

   ```java
   IotaAccount account = new IotaAccount.Builder(mySeed)

                    .store(store)
                    .api(api)
                    .build();
    ```

    :::info:デフォルト設定
    カスタム設定を指定しないとき、アカウントは[デフォルト設定](https://github.com/iotaledger/iota-java/blob/dev/jota/src/main/java/org/iota/jota/config/types/IotaDefaultConfig.java)を使用します。
    :::
    <!-- :::info:Default settings -->
    <!-- If you don't specify any custom settings, the account uses the [defaults](https://github.com/iotaledger/iota-java/blob/dev/jota/src/main/java/org/iota/jota/config/types/IotaDefaultConfig.java). -->
    <!-- ::: -->

:::success:おめでとうございます！:tada:
CDAの状態を管理するだけでなく、自動的にトランザクションを促進し再添付するアカウントを作成しました。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You've created an account that will automatically promote and reattach transactions as well as manage the state of your CDAs. -->
<!-- ::: -->

### 複数のノードに接続する
<!-- ### Connect to multiple nodes -->

1. 複数のノードに接続したい場合は、`HttpConnector`オブジェクトを作成するか、カスタムクラスを定義することができます。
  <!-- 1. If you want to connect to multiple nodes, you can either create a `HttpConnector` object, or define a custom class. -->

    ```java
    // Create an HTTP node using the default settings
    Connection node = new HttpConnector(
                    "http",
                    "localhost",
                    1337,
                    // Optional connection timeout
                    500
                );

    // Or create a custom node defined by a class
    Connection customNode = new MyCustomNodeClass();

    // Pass that to the builder
    IotaAPI api = new IotaAPI.Builder()
                    // Enable local proof of work
                    .localPoW(new PearlDiverLocalPoW())
                    // And add the extra nodes
                    .addNode(node)
                    .addNode(customNode)
                    .build();
    ```

## 既存のシードステートをインポートする
<!-- ## Import existing seed state -->

既存のシードステートをアカウントにインポートするには、ストレージオブジェクトを`store()`メソッドに渡します。シードステートは正しい形式でなければなりません。
<!-- To import an existing seed state into an account, pass the storage object to the `store()` method. The seed state must be in the correct format. -->

## 次のステップ
<!-- ## Next steps -->

アカウントで特定のイベントが発生すると、それらのイベントを発行し、イベントをリッスンすることができます。たとえば、新しい支払いについて自分のアカウントを監視したい場合があります。そのためには、[イベントリスナを作成する](root://iota-js/0.1/how-to-guides/listen-to-events.md)必要があります。
<!-- After certain events happen in your account, it emits them, and allows you to listen for them. For example, you may want to monitor your account for new payments. To do so, you need to [create an event listener](root://iota-js/0.1/how-to-guides/listen-to-events.md). -->

またはイベントを発行する[プラグインを作成する](../how-to-guides/create-plugin.md)こともできます。
<!-- Or, you can [create a plugin](../how-to-guides/create-plugin.md) that also emits events. -->
