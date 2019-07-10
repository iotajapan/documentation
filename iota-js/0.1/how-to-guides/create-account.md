<<<<<<< HEAD
# アカウントを作成する
<!-- # Create an account -->

**アカウントは、トランザクションの送受信を簡単にするためのオブジェクトです。アカウントは、アドレスやペンディング中のバンドルハッシュなどのデータをローカルデータベースに格納します。このデータにより、署名済みアドレスの再利用やペンディング中のトランザクションの促進や再添付を心配することなく、IOTAネットワークとやり取りすることができます。**
<!-- **An account is an object that makes it easier to send and receive transactions. Accounts store data such as addresses and pending bundle hashes in a local database. This data allows you to interact with an IOTA network without worrying about reusing spent addresses or promoting and reattaching pending transactions.** -->

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

この例では、[Devnetノード](root://getting-started/0.1/references/iota-networks.md#devnet)に接続します。Devnetは、トークンが無料であること以外はMainnetとほぼ同じです。Devnetに送信したトランザクションは、Mainnetのような他のネットワークには存在しません。
<!-- In this example, we connect to a [Devnet node](root://getting-started/0.1/references/iota-networks.md#devnet). The Devnet is similar to the Mainnet, except the tokens are free. Any transactions that you send to the Devnet do not exist on other networks such as the Mainnet. -->

1. ライブラリをインストールします。
  <!-- 1. Install the library -->

    ```bash
    npm install @iota/account
    ```

2. 新しいシードで`account`オブジェクトを作成してノードに接続します。
  <!-- 2. Create an `account` object with a new seed and connect to a node -->

    ```js
    const { createAccount }  = require('@iota/account')

    const seed = 'ASFITGPSD9ASDFKRWE...';

    // Local node to connect to;
    const provider = 'http://<node-url>:14265';

    const account = createAccount({
          seed,
          provider,
    });
    ```
    :::danger:シードを保護する
    ここで行うように、シードを絶対にハードコーディングしないでください。代わりに、保護されたファイルからシードを読み込むことをお勧めします。
    :::
    <!-- :::danger:Protect your seed -->
    <!-- You should never hard code a seed as we do here. Instead, we recommend that you read the seed from a protected file. -->
    <!-- ::: -->

    :::danger:新しいシードを使用する
    これまでにアカウントを作成したことがない場合は、既存のシードの状態が不明であるため、新しいシードを作成する必要があります。
    :::
    <!-- :::danger:Use a new seed -->
    <!-- If you have never created an account before, you must create a new seed because existing seed states are unknown. -->
    <!-- ::: -->

    :::danger:シードごとにアカウントを1つ作成する
    同じシードで複数のアカウントを作成してはいけません。作成してしまうと、シードステートが上書きされるという競合状態が発生する可能性があります。
    :::
    <!-- :::danger:Create one account per seed -->
    <!-- You must not create multiple accounts with the same seed. Doing so could lead to a race condition where the seed state would be overwritten. -->
    <!-- ::: -->

3. **オプション：** トランザクション添付に関連するデフォルトパラメータを変更する
  <!-- 3. **Optional:** Modify default parameters related to transaction attachment -->

      ```js
      const { createAccount }  = require('@iota/account')

      const seed = 'ASFITGPSD9ASDFKRWE...';

      // Local node to connect to;
      const provider = 'http://<node-url>:14265';

      const account = createAccount({
            seed,
            provider,

            // How far to go for the tip selection.
            // Defaults to 3.
            depth: 3,

            // Default is 9 on devnet.
            minWeightMagnitude: 9,

            // How long to wait before the next attachment round.
            delay: 1000 * 30,

            // Specifies at which depth attached transactions are
            // no longer promotable.
            // Those transactions are automatically re-attached.
            // Defaults to 6.
            maxDepth: 6,
      });
      ```

4. **オプション：** **`persistenceAdapter`**ファクトリをアカウントに渡す。このアダプタは、アカウントがシードステートを保存できるローカルデータベースオブジェクトを作成します。デフォルトでは、ローカルデータベースはプロジェクトのルートに保存されます。 `persistencePath`フィールドでローカルデータベースへのパスを変更できます。
  <!-- 4. **Optional:** Pass a **`persistenceAdapter`** factory to your account. This adapter creates a local database object to which the account can save the seed state. By default, the local database is saved in the root of the project. You can change the path to the local database in the `persistencePath` field. -->

    ```js
    const { createPersistenceAdapter }  = require('@iota/persistence-adapter-level')

    const account = createAccount({
          seed,
          provider,
          persistencePath: './',
          stateAdapter: createPersistenceAdapter,
    });
    ```

    :::info:
    [@iota/persistence-adapter-level](https://github.com/iotaledger/iota.js/tree/next/packages/persistence-adapter-level)アダプタがデフォルトです。このアダプタは、シードステートを[LevelDBデータベース](https://github.com/google/leveldb)の`leveldown`フレーバーに保管します。別のデータベースを使用するようにこのアダプタをカスタマイズすることもできます。

    同時に複数のアカウントに1つのアダプタインスタンスを使用することはできません。新しいアカウントごとにプライベートアダプタが作成されます。
    :::
    <!-- :::info: -->
    <!-- The [@iota/persistence-adapter-level](https://github.com/iotaledger/iota.js/tree/next/packages/persistence-adapter-level) adapter is the default. This adapter stores the seed state in the `leveldown` flavor of the [LevelDB database](http://leveldb.org/). You can customize this adapter to use a different database. -->
    <!--  -->
    <!-- You can't use one adapter instance for multiple accounts at the same time. A private adapter is created for each new account. -->
    <!-- ::: -->

5. **オプション：** 現在の時刻をミリ秒単位で出力する`timeSource`メソッドを作成し、それを`account`オブジェクトに渡す。
  <!-- 4. **Optional** Create a `timeSource` method that outputs the current time in milliseconds, and pass it to your `account` object -->

    ```js
    const account = createAccount({
          seed,
          provider,
          timeSource: () => {
                // Get time with NTP
                // const time = ...
                return time
          }
    })
    ```

:::success:おめでとうございます！:tada:
CDAのステートを管理し、かつ自動的にトランザクションを促進、再添付するアカウントを作成しました。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You've created an account that will automatically promote and reattach transactions as well as manage the state of your CDAs. -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

[トランザクションを送受信できるCDAを作成する](../how-to-guides/create-and-manage-cda.md)。
<!-- [Create a CDA so that you can send and receive transactions](../how-to-guides/create-and-manage-cda.md). -->
