# アカウントを作成する
<!-- # Create an account -->

**アカウントは、支払いの処理を容易にするオブジェクトです。任意のIOTAネットワークでアカウントを使用して、IOTAトークンを送受信できます。**
<!-- **An account is an object that makes it easier to handle payments. You can use your account on any IOTA network to send and receive IOTA tokens.** -->

アカウントを使用する多くの利点の1つは、アドレスがアクティブまたは期限切れになる条件を定義できることです。これらの条件は、送信者がIOTAトークンをアドレスに送信しても安全かどうかを判断するのに役立ちます。このため、アカウントのアドレスは_条件付き預け入れアドレス_（CDA）と呼ばれます。
<!-- One of the many benefits of using accounts is that you can define conditions in which your addresses are active or expired. These conditions help senders to decide whether it's safe to send tokens to an address. For this reason, addresses in accounts are called _conditional deposit addresses_ (CDA). -->

## 条件付き預け入れアドレス
<!-- ## Conditional deposit addresses -->

IOTAプロトコルでは、IOTAトークンは[シード](root://getting-started/0.1/introduction/what-is-a-seed.md)から派生した[アドレス](root://dev-essentials/0.1/concepts/addresses-and-signatures.md)に送信する必要があります。これらのアドレスは一度だけIOTAトークンを取り出すすることができます。そのため、IOTAトークンを一度取り出したアドレスにIOTAトークンを預けないことが重要です。しかし、あなたがアドレスから取り出すする前に、誰かがあなたのアドレスにIOTAトークンを預け入れようとしているのかどうかを知ることは困難です。
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
* Java 6（以上）
<!-- * Java 6 (or higher) -->
* [クライアントライブラリをインポートする](../introduction/overview.md#install-the-library)
<!-- * [Install the client library](../introduction/overview.md#install-the-library) -->

:::warning: 新しいシードを作成する
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
アカウントにはデフォルト設定がありますが、少なくともシードとMongoDBなどのストレージプロバイダを指定することをお勧めします。そうでなければ、コードの実行が停止した後にシードステートは保存されません。
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
    // デフォルト設定を使用してHTTPノードを作成します。
    Connection node = new HttpConnector(
                    "http",
                    "localhost",
                    1337,
                    // Optional connection timeout
                    500
                );

    // または、クラスによって定義されたカスタムノードを作成します。
    Connection customNode = new MyCustomNodeClass();

    // それをビルダーに渡します。
    IotaAPI api = new IotaAPI.Builder()
                    // ローカルプルーフオブワークを有効にします。
                    // And add the extra nodes
                    // 追加ノードを追加します。
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

アカウントで特定のイベントが発生すると、それらのイベントを発行し、イベントをリッスンすることができます。たとえば、新しい支払いについて自分のアカウントを監視したい場合があります。そのためには、[イベントリスナを作成する](../how-to-guides/listen-to-events.md)必要があります。
<!-- After certain events happen in your account, it emits them, and allows you to listen for them. For example, you may want to monitor your account for new payments. To do so, you need to [create an event listener](../how-to-guides/listen-to-events.md). -->

またはイベントを発行する[プラグインを作成する](../how-to-guides/create-plugin.md)こともできます。
<!-- Or, you can [create a plugin](../how-to-guides/create-plugin.md) that also emits events. -->
