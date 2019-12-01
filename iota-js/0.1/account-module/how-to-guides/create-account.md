# アカウントを作成する
<!-- # Create an account -->

**アカウントは、支払いの処理を容易にするオブジェクトです。任意のIOTAネットワークでアカウントを使用して、IOTAトークンを送受信できます。**
<!-- **An account is an object that makes it easier to handle payments. You can use your account on any IOTA network to send and receive IOTA tokens.** -->

アカウントを使用する多くの利点の1つは、アドレスがアクティブまたは期限切れになる条件を定義できることです。これらの条件は、送信者がIOTAトークンをアドレスに送信しても安全かどうかを判断するのに役立ちます。このため、アカウントのアドレスは_条件付き預け入れアドレス_（CDA）と呼ばれます。
<!-- One of the many benefits of using accounts is that you can define conditions in which your addresses are active or expired. These conditions help senders to decide whether it's safe to send tokens to an address. For this reason, addresses in accounts are called _conditional deposit addresses_ (CDA). -->

## 条件付き預け入れアドレス
<!-- ## Conditional deposit addresses -->

アカウントでは、アドレスには、支払いで使用される条件を指定できる追加機能があります。これらのアドレスは、条件付き預け入れアドレス（CDA）と呼ばれます。
<!-- In accounts, addresses come with extra features that allow you to specify the conditions in which they may be used in payments. These addresses are called conditional deposit addresses (CDA). -->

アカウントはCDAを使用して、[使用済みアドレス](root://getting-started/0.1/clients/addresses.md#spent-addresses)からの取り出しリスクを軽減します。誰かにIOTAトークンを要求すると、一定期間アクティブなCDAを作成できます。このようにして、その時間後にのみそのCDAからIOTAトークンを取り出すつもりであることを送信者に知らせます。その結果、送信者は、CDAに残っている時間に応じて、IOTAトークンの預け入れを行うかどうかを決定できます。
<!-- Accounts use CDAs to help reduce the risk of withdrawing from [spent addresses](root://getting-started/0.1/clients/addresses.md#spent-addresses). When you request IOTA tokens from someone, you can create a CDA that's active for a certain period of time. This way, you let the sender know that you intend to withdraw from that address only after that time. As a result, the sender can decide whether to make a deposit, depending on how much time is left on a CDA. -->

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

<!-- |:-----------------|:----------| -->
<!-- |**Data**| **Purpose**| -->
<!-- |:-----------------|:----------| -->
<!-- |The last key index that was used to create a CDA| Create a new CDA that has never been used before| -->
<!-- |All active CDAs|Stop withdrawals from CDAs that may receive deposits| -->
<!-- |Pending transfers| Monitor pending transactions and rebroadcast or reattach them if necessary| -->

## 前提条件
<!-- ## Prerequisites -->

このチュートリアルを完了するには、次のものが必要です。
<!-- To complete this tutorial, you need the following: -->

- コマンドラインインターフェイスへのアクセス
<!-- - Access to a command-line interface -->
- [Visual Studio Code](https://code.visualstudio.com/Download)などのコードエディタ
<!-- - A code editor such as [Visual Studio Code](https://code.visualstudio.com/Download) -->
- Node.js 8、またはNode.js 10以上。[最新のLTS](https://nodejs.org/en/download/)をお勧めします。
<!-- - Node.js 8, or Node.js 10 or higher. We recommend the [latest LTS](https://nodejs.org/en/download/). -->

:::warning: 新しいシードを作成する
これまでにアカウントを作成したことがない場合は、既存のシードステートが不明であるため、[新しいシードを作成する](root://getting-started/0.1/how-to-guides/get-started.md)必要があります。
:::
<!-- :::warning: Create a new seed -->
<!-- If you have never created an account before, you must [create a new seed](root://getting-started/0.1/how-to-guides/get-started.md) because existing seed states are unknown. -->
<!-- ::: -->

## 新しいアカウントを作成する
<!-- ## Create a new account -->
<!-- ## Create a new account -->

この例では、[Devnetノード](root://getting-started/0.1/network/iota-networks.md#devnet)に接続します。Devnetは、トークンが無料であること以外はMainnetとほぼ同じです。Devnetに送信したトランザクションは、Mainnetのような他のネットワークには存在しません。
<!-- In this example, we connect to a [Devnet node](root://getting-started/0.1/network/iota-networks.md#devnet). The Devnet is similar to the Mainnet, except the tokens are free. Any transactions that you send to the Devnet do not exist on other networks such as the Mainnet. -->

1. 新しいシードで`アカウント`オブジェクトを作成し、ノードに接続します。
  <!-- 1. Create an `account` object with a new seed and connect to a node -->

    ```js
    const { createAccount }  = require('@iota/account')

    const seed = 'PUEOTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX';

    // ノードに接続します。
    const provider = 'https://nodes.devnet.iota.org:443';

    const account = createAccount({
        seed,
        provider
    });
    ```

    デフォルトでは、ローカルデータベースはプロジェクトのルートに保存されます。
    <!-- By default, the local database is saved in the root of the project -->

    :::warning:シードを保護します
    ここで行っているようにシードをハードコーディングしないでください。代わりに、保護されたファイルからシードを読み込むことをお勧めします。
    :::
    <!-- :::warning:Protect your seed -->
    <!-- Never hard code a seed as we do here. Instead, we recommend that you read the seed from a protected file. -->
    <!-- ::: -->

    :::danger:シードごとにアカウントインスタンスを1つ作成します
    同じシードで複数のアカウントインスタンスを作成してはいけません。作成してしまうと、シードステートが上書きされるという競合状態が発生する可能性があります。
    :::
    <!-- :::danger:Create one account instance per seed -->
    <!-- You must not create multiple instances of an account with the same seed. Doing so could lead to a race condition where the seed state would be overwritten. -->
    <!-- ::: -->

:::success:おめでとうございます！:tada:
アドレスのステートを管理し、かつ自動的にトランザクションを促進、再添付するアカウントを作成しました。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You've created an account that will automatically promote and reattach transactions as well as manage the state of your addresses. -->
<!-- ::: -->

## アカウントをカスタマイズする
<!-- ## Customize your account -->

デフォルトのアカウント設定を使用する代わりに、アカウント設定をカスタマイズしてアカウントの動作を変更することができます。
<!-- Instead of using the default account settings, you can customize them to change how your account behaves. -->

1. アカウントが接続しているノードとどのように対話するかを変更するには、`depth`、`minWeightMagnitude`、`delay`、そして`maxDepth`フィールドの値をカスタマイズします。
  <!-- 1. To change how your account interacts with its connected nodes, customize the values of the `depth`, `minWeightMagnitude`, `delay`, and `maxDepth` fields -->

    ```js
    const account = createAccount({
      seed,
      provider,

      // タングル内でチップ選択を開始するために戻る深さ
      depth: 3,

      // Devnetにおける最小重量値9
      minWeightMagnitude: 9,

      // 次の添付ラウンドまでの待機時間
      delay: 1000 * 30,

      // トランザクションが促進できなくなる深さ
      // maxDepthまでのトランザクションは自動的に再添付されます。
      maxDepth: 6
    });
    ```

2. シードステートを保存するデータベース設定をカスタマイズするには、`persistenceAdapter`ファクトリーをアカウントに渡します。このアダプタは、アカウントがシードステートを保存できるローカルデータベースオブジェクトを作成します。デフォルトでは、ローカルデータベースはプロジェクトのルートに保存されます。`persistencePath`フィールドでローカルデータベースへのパスを変更できます。
  <!-- 2. To customize the database settings that store your seed state, pass a `persistenceAdapter` factory to your account. This adapter creates a local database object to which the account can save the seed state. By default, the local database is saved in the root of the project. You can change the path to the local database in the `persistencePath` field. -->

    ```js
    const { createPersistenceAdapter }  = require('@iota/persistence-adapter-level')

    const account = createAccount({
      seed,
      provider,
      persistencePath: './',
      persistenceAdapter: createPersistenceAdapter
    });
    ```

    :::info:
    [persistence-adapter-level](https://github.com/iotaledger/iota.js/tree/next/packages/persistence-adapter-level)パッケージがデフォルトです。このアダプターは、シードレベルを[LevelDBデータベース](https://github.com/google/leveldb)の`leveldown`フレーバーに保存します。別のデータベースを使用するようにこのアダプターをカスタマイズできます。

    1つのアダプターインスタンスを複数のアカウントに同時に使用することはできません。新しいアカウントごとにプライベートアダプターが作成されます。
    :::
    <!-- :::info: -->
    <!-- The [persistence-adapter-level](https://github.com/iotaledger/iota.js/tree/next/packages/persistence-adapter-level) package is the default. This adapter stores the seed state in the `leveldown` flavor of the [LevelDB database](https://github.com/google/leveldb). You can customize this adapter to use a different database. -->

    <!-- You can't use one adapter instance for multiple accounts at the same time. A private adapter is created for each new account. -->
    <!-- ::: -->

3. CDAは、定義した時間が経過すると失効します。アカウントにカスタムの時間ソースを使用させるには、現在の時間をミリ秒で出力する`timeSource`メソッドを作成し、`timeSource`メソッドを`account`オブジェクトに渡します
  <!-- 3. CDAs expire after a time that you define. To have your account use a custom source of time, create a `timeSource` method that outputs the current time in milliseconds, and pass it to your `account` object -->

    ```js
    const account = createAccount({
      seed,
      provider,
      timeSource: () => {
        // NTP時間を取得します。
        // const time = ...
        return time
      }
    })
    ```

## 次のステップ
<!-- ## Next steps -->

アカウントで特定のイベントが発生すると、アカウントがイベントを発行し、イベントをリッスンすることができます。たとえば、アカウントの新規支払いを監視することができます。そのためには、[イベントリスナーを作成します](../how-to-guides/listen-to-events.md)。
<!-- After certain events happen in your account, it emits them, and allows you to listen for them. For example, you may want to monitor your account for new payments. To do so, you need to [create an event listener](../how-to-guides/listen-to-events.md). -->
