# マスクされた認証済みメッセージング入門
<!-- # Get started with Masked Authenticated Messaging -->

**[マスクされた認証済みメッセージング](root://getting-started/0.1/transactions/masked-authenticated-messaging.md)（Masked Authenticated Messaging, MAM）は、タングルでチャネルと呼ばれる暗号化されたデータストリームでメッセージを発行および取得できるデータ通信プロトコルです。MAMパッケージを使用して、暗号化および認証されたメッセージをチャネルに発行できます。**
**[Masked Authenticated Messaging](root://getting-started/0.1/transactions/masked-authenticated-messaging.md) (MAM) is a data communication protocol that allows you to publish and fetch messages in encrypted data streams, called channels, on the Tangle. You can use the MAM package to publish encrypted and authenticated messages to channels.**

:::warning:データソフトウェアです！
クライアントライブラリは2019年現在ベータ版であり、運用環境では使用しないでください。
:::
<!-- :::warning:Beta software -->
<!-- The client libraries are currently in beta and you should not use them in production environments. -->
<!-- ::: -->

## 対象読者
<!-- ## Audience -->

このドキュメントは、JavaScriptプログラミング言語とオブジェクト指向プログラミングの概念に精通している開発者向けです。また、[バンドル](root://getting-started/0.1/transactions/bundles.md)、[トランザクション](root://getting-started/0.1/transactions/transactions.md)、[シード](root://getting-started/0.1/clients/seeds.md)や[アドレス](root://getting-started/0.1/clients/addresses.md)などの[基本的なIOTAの概念](root://getting-started/0.1/introduction/overview.md)にも精通している必要があります。
<!-- This documentation is for developers who are familiar with the JavaScript programming language and object-oriented programming concepts. You should also be familiar with [basic IOTA concepts](root://getting-started/0.1/introduction/overview.md) such as [bundles](root://getting-started/0.1/transactions/bundles.md), [transactions](root://getting-started/0.1/transactions/transactions.md), [seeds](root://getting-started/0.1/clients/seeds.md), and [addresses](root://getting-started/0.1/clients/addresses.md). -->

## 前提条件
<!-- ## Prerequisites -->

次のいずれかが必要です。
<!-- You must have one of the following: -->

- Node.js 10以上。推奨バージョンは[最新LTS](https://nodejs.org/en/download/)です。
<!-- - Node.js 10 or higher. Recommended version is [latest LTS](https://nodejs.org/en/download/). -->
- Node.js 8

[ライブラリパッケージ](https://www.npmjs.com/org/iota)をインストールするには、次のいずれかのパッケージマネージャーが必要です。
<!-- To install [library packages](https://www.npmjs.com/org/iota), you must have one of the following package managers: -->

- [npm](https://www.npmjs.com/) (Node.js [ダウンロード](https://nodejs.org/en/download/)に含まれています)
<!-- - [npm](https://www.npmjs.com/) (Included in Node.js [downloads](https://nodejs.org/en/download/)) -->
- [Yarn](https://yarnpkg.com/)

`package.json`ファイルも必要です。コマンドラインインターフェイスで[`npm init`](https://docs.npmjs.com/cli/init)または[`yarn init`](https://yarnpkg.com/lang/en/docs/cli/init/)を実行して生成します。
<!-- You must also have a `package.json` file. Generate one by doing [`npm init`](https://docs.npmjs.com/cli/init) or [`yarn init`](https://yarnpkg.com/lang/en/docs/cli/init/) in the command-line interface. -->

## パッケージをインストールする
<!-- ## Install the package -->

`mam`パッケージとその依存関係をインストールするには、次のオプションのいずれかを使用できます。
<!-- To install the `mam` package and its dependencies, you can use one of the following options: -->

- npmでパッケージをインストールする
  <!-- - Install the package with npm -->
    ```bash
    npm install @iota/mam
    ```
- Yarnでパッケージをインストールする
  <!-- - Install the package with Yarn -->
    ```bash
    yarn add @iota/mam
    ```

## 始める
<!-- ## Get started -->

緑色のボタンをクリックして、パブリックMAMチャネルにメッセージを発行するサンプルコードを実行します。
<!-- Click the green button to run the sample code that publishes messages to a public MAM channel. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/MAM-public?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

[ガイド](../how-to-guides/create-public-channel.md)を参照して、独自のMAMメッセージを発行する方法を確認してください。
<!-- [Follow our guide](../how-to-guides/create-public-channel.md) to find out how to publish your own MAM messages. -->

## APIリファレンス
<!-- ## API reference -->

利用可能なすべてのAPIメソッドの詳細については、[リファレンスページ](../references/api-reference.md)を参照してください。
<!-- For details on all available API methods, see the [references page](../references/api-reference.md). -->

- [init](../references/api-reference.md#init)

- [changeMode](../references/api-reference.md#changeMode)

- [getRoot](../references/api-reference.md#getRoot)

- [decode](../references/api-reference.md#decode)

- [subscribe](../references/api-reference.md#subscribe)

- [listen](../references/api-reference.md#listen)

- [attach](../references/api-reference.md#attach)

- [fetch](../references/api-reference.md#fetch)

- [fetchSingle](../references/api-reference.md#fetchSingle)

## プロジェクトをサポートする
<!-- ## Support the project -->

IOTA JavaScriptクライアントライブラリが役に立ち、貢献したいと思う場合は、[バグレポート](https://github.com/iotaledger/mam.client.js/issues/new)、[機能リクエスト](https://github.com/iotaledger/mam.client.js/issues/new)または[プルリクエスト](https://github.com/iotaledger/mam.client.js/pulls/)の投稿を検討してください。
<!-- If the IOTA JavaScript client library has been useful to you and you feel like contributing, consider posting a [bug report](https://github.com/iotaledger/mam.client.js/issues/new), [feature request](https://github.com/iotaledger/mam.client.js/issues/new), or a [pull request](https://github.com/iotaledger/mam.client.js/pulls/). -->

### ソースからパッケージをビルドする
<!-- ### Build the package from source -->

パッケージのローカルコピーに変更を加える予定がある場合は、ソースからパッケージをビルドできます。
<!-- You may want to build the package from source if you plan on making changes to your local copy of it. -->

1. [iotaledgerのGitHubリポジトリ](https://github.com/iotaledger/mam.client.js)にアクセスします。
  <!-- 1. Go to the [iotaledger GitHub repository](https://github.com/iotaledger/mam.client.js) -->

2. 右上隅の**Fork**ボタンをクリックします。
  <!-- 2. Click the **Fork** button in the top-right corner -->

3. フォークされたリポジトリのクローンを作成し、そのディレクトリに移動します。
  <!-- 3. Clone your forked repository and change into its directory -->

    ```bash
    cd mam.client.js
    ```

4. 依存関係をインストールします。
  <!-- 4. Install the dependencies -->

    ```bash
    # npmパッケージマネージャーの場合
    npm install

    # Yarnパッケージマネージャーの場合
    yarn
    ```

5. ローカルコピーに必要な変更を加えます。
  <!-- 5. Make any changes you want to your local copy -->

6. 環境に応じてパッケージをビルドします。
  <!-- 6. Build the package, according to your environment -->

--------------------
### Node.js

開発バージョン：

```bash
# npmパッケージマネージャーの場合
npm run build-node-dev

# Yarnパッケージマネージャーの場合
yarn build-node-dev
```

運用バージョン：

```bash
# npmパッケージマネージャーの場合
npm run build-node-prod

# Yarnパッケージマネージャーの場合
yarn build-node-prod
```
---
### Webブラウザー

開発バージョン：

```bash
# npmパッケージマネージャーの場合
npm run build-web-dev

# Yarnパッケージマネージャーの場合
yarn build-web-dev
```

運用バージョン：

```bash
# npmパッケージマネージャーの場合
npm run build-web-prod

# Yarnパッケージマネージャーの場合
yarn build-web-prod
```
--------------------

### ドキュメントを更新する
<!-- ### Update documentation -->

変更内容がドキュメントに影響する場合は、更新してください。
<!-- If your changes affect the documentation, please update it. -->

## ディスカッションに参加する
<!-- ## Join the discussion -->

[Discord](https://discord.iota.org)に参加して、コミュニティに参加したり、助けを求めたり、技術について話し合ったりしてください。
<!-- Join our [Discord](https://discord.iota.org) to get involved in the community, ask for help, or to discuss the technology. -->
