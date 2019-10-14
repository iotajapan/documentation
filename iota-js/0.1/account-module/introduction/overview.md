# アカウントモジュール入門
<!-- # Get started with the account module -->

**アカウントは、トランザクションの送受信を容易にするオブジェクトです。アカウントは、アドレスやペンディング中のバンドルハッシュなどのデータをローカルデータベースに保存します。このデータにより、使用済みアドレスからの取り出しやペンディング中のトランザクションの促進と再添付を心配することなく、IOTAネットワークと対話できます。**
<!-- **An account is an object that makes it easier to send and receive transactions. Accounts store data such as addresses and pending bundle hashes in a local database. This data allows you to interact with an IOTA network without worrying about withdrawing from spent addresses or promoting and reattaching pending transactions.** -->

アカウントはIOTAプロトコルの複雑さを抽象化し、支払いに集中できるようにします。アカウントでは、支払いは次の2つのタイプのいずれかになります。
<!-- Accounts abstract the complexity of the IOTA protocol and allow you to focus on making payments. In accounts, a payment can be one of two types: -->

* **入金：**アカウントにIOTAトークンを預け入れるバンドル
* **支払：**アカウントからIOTAトークンを取り出すバンドル
<!-- * **Incoming payment:** A bundle that deposits IOTA tokens into an account -->
<!-- * **Outgoing payment:** A bundle that withdraws IOTA tokens from an account -->

:::warning:ベータソフトウェアです！
クライアントライブラリは2019年現在ベータ版であり、運用環境では使用しないでください。
:::
<!-- :::warning:Beta software -->
<!-- The client libraries are currently in beta and you should not use them in production environments. -->
<!-- ::: -->

## 対象読者
<!-- ## Audience -->

このドキュメントは、JavaScriptプログラミング言語とオブジェクト指向プログラミングの概念に精通している開発者向けです。また、[バンドル、トランザクション](root://dev-essentials/0.1/concepts/bundles-and-transactions.md)、[アドレスから一度だけIOTAトークンを取り出す理由](root://dev-essentials/0.1/concepts/addresses-and-signatures.md#address-reuse)などのIOTAの基本概念にも精通している必要があります。
<!-- This documentation is for developers who are familiar with the JavaScript programming language and object-oriented programming concepts. You should also be familiar with basic IOTA concepts such as [bundles, transactions](root://dev-essentials/0.1/concepts/bundles-and-transactions.md), and [why you should withdraw from addresses only once](root://dev-essentials/0.1/concepts/addresses-and-signatures.md#address-reuse). -->

## 前提条件
<!-- ## Prerequisites -->

このライブラリを使用するには、[サポートされている](https://github.com/iotaledger/iota.js/blob/next/.travis.yml#L5)バージョンの[Node.js](https://nodejs.org)のいずれかが必要です。
<!-- To use this library, you must have one of the following [supported](https://github.com/iotaledger/iota.js/blob/next/.travis.yml#L5) versions of [Node.js](https://nodejs.org): -->

- Node.js 10以上。推奨バージョンは[最新LTS](https://nodejs.org/en/download/)です。
<!-- - Node.js 10 or higher. Recommended version is [latest LTS](https://nodejs.org/en/download/). -->
- Node.js 8

[ライブラリパッケージ](https://www.npmjs.com/org/iota)をインストールするには、次のいずれかのパッケージマネージャーが必要です。
<!-- To install [library packages](https://www.npmjs.com/org/iota), you must have one of the following package managers: -->

- [npm](https://www.npmjs.com/) (Node.js [ダウンロード](https://nodejs.org/en/download/)に含まれています)
<!-- - [npm](https://www.npmjs.com/) (Included in Node.js [downloads](https://nodejs.org/en/download/)) -->
- [Yarn](https://yarnpkg.com/)

## パッケージをインストールする
<!-- ## Install the packages -->

JavaScriptアカウントモジュールパッケージとその依存関係をインストールするには、次のオプションのいずれかを使用できます。
<!-- To install the JavaScript account module packages and their dependencies, you can use one of the following options: -->

- npmでパッケージをインストールする
  <!-- - Install the packages with npm -->
    ```bash
    npm install @iota/account @iota/cda @iota/persistence-adapter-level
    ```
- Yarnでパッケージをインストールする
  <!-- - Install the packages with Yarn -->
    ```bash
    yarn add @iota/account @iota/cda @iota/persistence-adapter-level
    ```

## 始める
<!-- ## Get started -->

緑色のボタンをクリックして、新しいアカウントを作成し、利用可能な残高を確認するサンプルコードを実行します。
<!-- Click the green button to run the sample code that creates a new account and checks its available balance. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Create-account?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

[ガイド](../how-to-guides/create-account.md)を参照して、独自のアカウントを作成する方法を確認してください。
<!-- [Follow our guide](../how-to-guides/create-account.md) to find out how to create your own account. -->

## APIリファレンス
<!-- ## API reference -->

利用可能なすべてのAPIメソッドの詳細については、[ソースコード](https://github.com/iotaledger/iota.js/tree/next/packages/account)を参照してください。
<!-- For details on all available API methods, see the [source code](https://github.com/iotaledger/iota.js/tree/next/packages/account). -->

## プロジェクトをサポートする
<!-- ## Support the project -->

JavaScriptライブラリが役に立ち、貢献したいと思う場合は、[バグレポート](https://github.com/iotaledger/iota.js/issues/new)、機能要求、または[プルリクエスト](https://github.com/iotaledger/iota.js/pulls/)の投稿を検討してください。
<!-- If the IOTA JavaScript client library has been useful to you and you feel like contributing, consider posting a [bug report](https://github.com/iotaledger/iota.js/issues/new), [feature request](https://github.com/iotaledger/iota.js/issues/new), or a [pull request](https://github.com/iotaledger/iota.js/pulls/). -->

### GitHubのリポジトリを複製してブートストラップする
<!-- ### Clone and bootstrap the repository on GitHub -->

パッケージのローカルコピーに変更を加える予定がある場合は、ソースからパッケージをビルドできます。
<!-- You may want to build the package from source if you plan on making changes to your local copy of it. -->

1. [iotaledgerのGitHubリポジトリ](https://github.com/iotaledger/iota.js)にアクセスします。
  <!-- 1. Go to the [iotaledger GitHub repository](https://github.com/iotaledger/iota.js) -->

2. 右上隅の[**Fork**]ボタンをクリックします。
  <!-- 2. Click the **Fork** button in the top-right corner -->

3. フォークされたリポジトリのクローンを作成し、そのディレクトリに移動します。
  <!-- 3. Clone your forked repository and change into its directory -->

    ```bash
    cd iota.js
    ```

4. 環境を初期化します。
  <!-- 4. Initialize your environment -->

    ```bash
    npm run init
    ```

このステップでは、すべての依存関係をダウンロードしてビルドし、パッケージをリンクします。
<!-- This step will download all dependencies, build them, then link the packages together. -->

JavaScriptクライアントライブラリは、[Lerna](https://lerna.js.org/)を使用して複数のパッケージを管理します。したがって、`lerna bootstrap`コマンドを使用して、いつでも環境を初期化できます。
<!-- The JavaScript client libraries use [Lerna](https://lerna.js.org/) to manage multiple packages. So, you can initialize your environment again at any point with the `lerna bootstrap` command. -->

### テストを実行する
<!-- ### Run tests -->

単一のパッケージまたは複数のパッケージに変更を加え、ルートディレクトリから次のコマンドを実行してテストします。
<!-- Make your changes on a single package or across multiple packages and test the them by running the following command from the root directory: -->

```bash
npm run test
```

特定のパッケージのテストを実行するには、パッケージのディレクトリに移動して、`npm run test`コマンドを実行します。
<!-- To run tests for a specific package, change into the package's directory and run the `npm run test` command. -->

### ドキュメントを更新する
<!-- ### Update documentation -->

変更がドキュメントに影響する場合は、[`JSDoc`](http://usejsdoc.org)アノテーションを編集し、ルートディレクトリから`npm run docs`を実行してドキュメントを更新してください。
<!-- If your changes affect the documentation, please update it by editing the [`JSDoc`](http://usejsdoc.org) annotations and running `npm run docs` from the root directory. -->

## ディスカッションに参加する
<!-- ## Join the discussion -->

[Discord](https://discord.iota.org)に参加して、コミュニティに参加したり、助けを求めたり、技術について話し合ったりしてください。
<!-- Join our [Discord](https://discord.iota.org) to get involved in the community, ask for help, or to discuss the technology. -->
