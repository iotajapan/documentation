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
* Node.js 8、またはNode.js 10以上。[最新のLTS](https://nodejs.org/en/download/)をお勧めします。
<!-- * Node.js 8, or Node.js 10 or higher. We recommend the [latest LTS](https://nodejs.org/en/download/). -->

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

1. パッケージをインストールします。
  <!-- 1. Install the package -->

    ```bash
    npm install @iota/account
    ```

2. 新しいシードで`account`オブジェクトを作成してノードに接続します。
  <!-- 2. Create an `account` object with a new seed and connect to a node -->

      ```js
      const { createAccount }  = require('@iota/account')

      const seed = 'PUEOTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX';

      // Connect to a node;
      const provider = 'https://nodes.devnet.iota.org:443';

      const account = createAccount({
            seed,
            provider
      });
      ```

      :::warning:シードを保護しくてください
      ここで行っているようにシードをハードコーディングしないでください。代わりに、保護されたファイルからシードを読み込むことをお勧めします。
      :::
      <!-- :::warning:Protect your seed -->
      <!-- Never hard code a seed as we do here. Instead, we recommend that you read the seed from a protected file. -->
      <!-- ::: -->

      :::danger:シードごとにアカウントを1つ作成してください
      同じシードで複数のアカウントを作成してはいけません。作成してしまうと、シードステートが上書きされるという競合状態が発生する可能性があります。
      :::
      <!-- :::danger:Create one account per seed -->
      <!-- You must not create multiple accounts with the same seed. Doing so could lead to a race condition where the seed state would be overwritten. -->
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

            // How far back in the Tangle to start the tip selection
            depth: 3,

            // The minimum weight magnitude is 9 on the Devnet
            minWeightMagnitude: 9,

            // How long to wait before the next attachment round
            delay: 1000 * 30,

            // The depth at which transactions are no longer promotable
            // Those transactions are automatically re-attached
            maxDepth: 6
      });
      ```

2. シードステートを保存するデータベース設定をカスタマイズするには、`persistenceAdapter`ファクトリをアカウントに渡します。このアダプタは、アカウントがシードステートを保存できるローカルデータベースオブジェクトを作成します。デフォルトでは、ローカルデータベースはプロジェクトのルートに保存されます。`persistencePath`フィールドでローカルデータベースへのパスを変更できます。
  <!-- 2. To customize the database settings, which stores your seed state, pass a `persistenceAdapter` factory to your account. This adapter creates a local database object to which the account can save the seed state. By default, the local database is saved in the root of the project. You can change the path to the local database in the `persistencePath` field. -->

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
      [@iota/persistence-adapter-level](https://github.com/iotaledger/iota.js/tree/next/packages/persistence-adapter-level)アダプタがデフォルトです。このアダプタは、シードステートを[LevelDBデータベース](https://github.com/google/leveldb)の`leveldown`フレーバーに保存します。別のデータベースを使用するようにこのアダプタをカスタマイズすることもできます。

      同時に複数のアカウントに1つのアダプタインスタンスを使用することはできません。新しいアカウントごとにプライベートアダプタが作成されます。
      :::
      <!-- :::info: -->
      <!-- The [@iota/persistence-adapter-level](https://github.com/iotaledger/iota.js/tree/next/packages/persistence-adapter-level) adapter is the default. This adapter stores the seed state in the `leveldown` flavor of the [LevelDB database](http://leveldb.org/). You can customize this adapter to use a different database. -->
      <!--  -->
      <!-- You can't use one adapter instance for multiple accounts at the same time. A private adapter is created for each new account. -->
      <!-- ::: -->

3. カスタムの時間源を使用するには、現在の時刻をミリ秒単位で出力する`timeSource`メソッドを作成し、それを`account`オブジェクトに渡します。
  <!-- 3. To use a custom source of time, create a `timeSource` method that outputs the current time in milliseconds, and pass it to your `account` object -->

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

## 次のステップ
<!-- ## Next steps -->

アカウントで特定のイベントが発生すると、それらのイベントを発行し、イベントをリッスンすることができます。たとえば、新しい支払いについて自分のアカウントを監視したい場合があります。そのためには、[イベントリスナを作成する](root://iota-js/0.1/how-to-guides/listen-to-events.md)必要があります。
<!-- After certain events happen in your account, it emits them, and allows you to listen for them. For example, you may want to monitor your account for new payments. To do so, you need to [create an event listener](root://iota-js/0.1/how-to-guides/listen-to-events.md). -->
