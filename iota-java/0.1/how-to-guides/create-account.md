# アカウントを作成する
<!-- # Create an account -->

**アカウントは、トランザクションの送受信を簡単にするためのオブジェクトです。アカウントは、アドレスやペンディング中のバンドルハッシュなどのデータをローカルデータベースに格納します。このデータにより、署名済みアドレスからの取り出しやペンディング中のトランザクションの促進や再添付を心配することなく、IOTAネットワークとやり取りすることができます。**
<!-- **An account is an object that makes it easier to send and receive transactions. Accounts store data such as addresses and pending bundle hashes in a local database. This data allows you to interact with an IOTA network without worrying about withdrawing from spent addresses or promoting and reattaching pending transactions.** -->

アカウントでは、すべてのアドレスは単純なIOTAアドレス以上のものです。これらのアドレスは、[条件付預け入れアドレス（CDA）](../how-to-guides/create-and-manage-cda.md)と呼ばれます。 CDAは81トライトのアドレスだけでなく、そのアドレスが[転送バンドル](root://getting-started/0.1/introduction/what-is-a-bundle.md)内で使用される可能性のある条件も定義できます。
<!-- In accounts, all addresses are more than simple IOTA addresses. These addresses are called [conditional deposit addresses (CDAs)](../how-to-guides/create-and-manage-cda.md). A CDA defines not only the 81-tryte address, but also the conditions in which that address may be used in a [transfer bundle](root://getting-started/0.1/introduction/what-is-a-bundle.md). -->

## シードステート
<!-- ## Seed state -->

アカウントがローカルデータベースに格納するデータは、シードステートと呼ばれます。アカウントはこのデータを使用して活動履歴を保持し、ノードへの不要なAPI呼び出しを回避します。
<!-- The data that accounts store in a local database is called the seed state. Accounts use this data to keep a history of activity and to avoid making unnecessary API calls to nodes. -->

|**データ**|**目的**|
|:---------|:-------|
|CDAの作成に使用された最後のキーインデックス|これまで使用されたことのない新しいCDAを作成する|
|すべてのアクティブなCDA|IOTAトークンの預け入れを受け取る可能性のあるCDAからのIOTAトークンの取り出しを停止する|
|ペンディング中の転送バンドル|ペンディング中のトランザクションを監視して再ブロードキャストするか、必要に応じて再添付を行う|

## 新しいアカウントを作成する
<!-- ## Create a new account -->

アカウントを作成するには、IOTAネットワークに接続するための`IotaAPI`オブジェクトと、シードを管理するための`IotaAccount`オブジェクトを作成する必要があります。
<!-- To create an account, you need to create an `IotaAPI` object to connect to an IOTA network and an `IotaAccount` object to manage a seed. -->

この例では、[Devnetノード](root://getting-started/0.1/references/iota-networks.md#devnet)に接続します。Devnetは、トークンが無料であること以外はMainnetとほぼ同じです。Devnetに送信したトランザクションは、Mainnetのような他のネットワークには存在しません。
<!-- In this example, we connect to a [Devnet node](root://getting-started/0.1/references/iota-networks.md#devnet). The Devnet is similar to the Mainnet, except the tokens are free. Any transactions that you send to the Devnet do not exist on other networks such as the Mainnet. -->

:::danger:重要:
`IotaAccount`オブジェクトにはデフォルトの設定がありますが、少なくともシードとMongoDBなどのストレージプロバイダーを指定することをお勧めします。そうでなければ、コードの実行が停止した後にシード状態は保存されません。
:::
<!-- :::danger:Important: -->
<!-- Although the `IotaAccount` object has default settings, we recommend that you provide at least a seed and a storage provider such as MongoDB. Otherwise, the seed state will not be saved after the code stops running. -->
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
  <!-- 2. Create a variable to hold a seed -->

    ```java
    String mySeed = "ASFITGPSD9ASDFKRWE...";
    ```

    :::danger:シードを保護する
    ここで行うように、シードを絶対にハードコーディングしないでください。代わりに、保護されたファイルからシードを読み込むことをお勧めします。

    ハードウェアウォレットなど、特定の場所のシードを使用する場合は、カスタム`SeedProvider`オブジェクトを作成して、それを`Builder()`コンストラクタに渡すことができます。
    :::
    <!-- ::: -->
    <!-- :::danger:Protect your seed -->
    <!-- You should never hard code a seed as we do here. Instead, we recommend that you read the seed from a protected file. -->
    <!--  -->
    <!-- If you want to use a seed from a particular location, for example a hardware wallet, you can make a custom `SeedProvider` object, and pass it to the `Builder()` constructor. -->
    <!-- ::: -->

    :::danger:初めてのアカウント作成ですか？
    これまでにアカウントを作成したことがなくても、トランザクションの送信にシードを使用したことがある場合、アカウントモジュールはシードステートを知りません。そのため、既存のシードを使用する代わりに、アカウントで使用する新しいシードを作成する必要があります。
    :::
    <!-- :::danger:Is this your first account? -->
    <!-- If you have never created an account before, but you have already used your seed to send transactions, the account module does not know the seed state. So, instead of using your existing seed, you must create a new seed to use with an account. -->
    <!-- ::: -->

3. アカウントがシードステートを保存できるストレージオブジェクトを作成します。この例では、シードステートはメモリストアデータベースに格納されています。
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

### 複数のIRIノードに接続する
<!-- ### Connect to multiple IRI nodes -->

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

[トランザクションを送受信できるCDAを作成する](../how-to-guides/create-and-manage-cda.md)。
<!-- [Create a CDA so that you can send and receive transactions](../how-to-guides/create-and-manage-cda.md). -->
