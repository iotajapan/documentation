# マスクされた認証済みメッセージングライブラリ
<!-- # Masked Authenticated Messaging Library -->

これは**公式** JavaScript MAM ライブラリであり、次のことができます。
<!-- This is the **official** JavaScript MAM library, which allows you to do the following: -->
- さまざまな暗号化スキームで MAM メッセージを作成する
<!-- - Create MAM messages with varying encryption schemes -->
- MAM メッセージを公開する（IOTA.JS クライアントライブラリを使用）
<!-- - Publish MAM messages (using the IOTA.JS Client library) -->
- MAM ストリームのフェッチとデコード
<!-- - Fetch & decode MAM streams -->
- 単一のメッセージのフェッチとデコード
<!-- - Fetch & Decode singular messages -->

ライブラリの機能の詳細については、次をご覧ください。
<!-- To learn more about how the library works: -->

- ["公開" MAM メッセージを送信する方法](root://mam/0.1/how-to/publishAndFetchPublic.md)をご覧ください。
<!-- - See how you can [send "Public" MAM message](root://mam/0.1/how-to/publishAndFetchPublic.md). -->
- [トランザクションの送受信方法](root://iota-js/0.1/how-to-guides/create-and-manage-cda.md)をご覧ください。
<!-- - See how you can [send and receive](root://iota-js/0.1/how-to-guides/create-and-manage-cda.md) transactions. -->

[IOTA JS GitHub リポジトリ](https://github.com/iotaledger/iota.js)でライブラリを取得します。
<!-- Get the library at the [IOTA JS  GitHub repository](https://github.com/iotaledger/iota.js). -->

:::warning:ベータソフトウェア
このライブラリは現在ベータ版です。本番環境での使用はサポートされていません。
:::
<!-- :::warning:Beta software -->
<!-- This library is currently in beta. Their use in production is not supported. -->
<!-- ::: -->

[issue tracker](https://github.com/iotaledger/mam.client.js/issues/new)で問題をご報告ください。
<!-- Please report any issues in our [issue tracker](https://github.com/iotaledger/mam.client.js/issues/new). -->

## 対象読者
<!-- ## Audience -->

このドキュメントは、JavaScriptプログラミング言語とオブジェクト指向プログラミングの概念に精通している人向けに設計されています。また、[バンドル、トランザクション](root://dev-essentials/0.1/concepts/bundles-and-transactions.md)などのIOTAの基本概念に精通している必要があります。
<!-- This documentation is designed for people who are familiar with the JavaScript programming language and object-oriented programming concepts. You should also be familiar with basic IOTA concepts such as  [bundles, and transactions](root://dev-essentials/0.1/concepts/bundles-and-transactions.md). -->

このガイドは、MAM機能を備えたアプリケーションの調査と開発をすばやく開始できるように設計されています。
<!-- This guide is designed to let you quickly start exploring and developing applications with MAM capabilities. -->

## 前提条件
<!-- ## Prerequisites -->

ライブラリを使用するには、コンピューターに次のいずれかが必要です。
<!-- To use the library, your computer must have one of the following: -->
- Node.js 10以降。推奨バージョンは[最新LTS](https://nodejs.org/en/download/)です。
<!-- - Node.js 10 or higher. Recommended version is [latest LTS](https://nodejs.org/en/download/). -->
- Node.js 8

[ライブラリパッケージ](https://www.npmjs.com/org/iota)をインストールするには、コンピューターに次のいずれかのパッケージマネージャーが必要です。
<!-- To install [library packages](https://www.npmjs.com/org/iota), your computer must have one of the following package managers: -->

- [npm](https://www.npmjs.com/)（Node.js [ダウンロード](https://nodejs.org/en/download/)に含まれています）
<!-- - [npm](https://www.npmjs.com/) (Included in Node.js [downloads](https://nodejs.org/en/download/)) -->
- [Yarn](https://yarnpkg.com/)

`package.json`ファイルも必要です。[`npm init`](https://docs.npmjs.com/cli/init)または[`yarn init`](https://yarnpkg.com/lang/en/docs/cli/init/)を実行して生成します。
<!-- You must also have a `package.json` file. Generate one by doing [`npm init`](https://docs.npmjs.com/cli/init) or [`yarn init`](https://yarnpkg.com/lang/en/docs/cli/init/). -->

## ライブラリをインストールする
<!-- ## Install the library -->

IOTA JavaScriptクライアントライブラリとその依存関係をインストールするには、次のオプションのいずれかを使用できます。
<!-- To install the IOTA JavaScript client library and its dependencies, you can use one of the following options: -->

- npmでライブラリをインストールする
  <!-- - Install the library with npm -->
    ```bash
    npm install @iota/mam
    ```
- Yarnを使用してライブラリをインストールする
  <!-- - Install the library with Yarn -->
    ```bash
    yarn add @iota/mam
    ```

## APIリファレンス
<!-- ## API references -->

利用可能なすべてのAPIメソッドの詳細については、[APIリファレンスページ](https://github.com/iotaledger/mam.client.js/blob/master/README.md)を参照してください。
<!-- For details on all available API methods, see the [references page](https://github.com/iotaledger/mam.client.js/blob/master/README.md). -->

<!-- ## API -->

<!-- For details on all available API methods, see the [references page](root://mam/0.1/references/api.md). -->

- [init](root://mam/0.1/references/api.md#init)

- [changeMode](root://mam/0.1/references/api.md#changeMode)

- [getRoot](root://mam/0.1/references/api.md#getRoot)

- [decode](root://mam/0.1/references/api.md#decode)

- [subscribe](root://mam/0.1/references/api.md#subscribe)

- [listen](root://mam/0.1/references/api.md#listen)

- [attach](root://mam/0.1/references/api.md#attach)

- [fetch](root://mam/0.1/references/api.md#fetch)

- [fetchSingle](root://mam/0.1/references/api.md#fetchSingle)


### ドキュメントを更新する
<!-- ### Update documentation -->

[`JSDoc`](http://usejsdoc.org)アノテーションを編集し、ルートディレクトリから`npm run docs`を実行して、必要に応じてドキュメントを更新してください。
<!-- Please update the documention when needed by editing [`JSDoc`](http://usejsdoc.org) annotations and running `npm run docs` from the root directory. -->

## ディスカッションに参加する
<!-- ## Join the discussion -->

コミュニティに参加したい場合、セットアップの支援が必要な場合、ライブラリに関連する問題がある場合、またはIOTA、分散レジストリ技術（DRT）、IoTについて他の人と話し合う場合は、お気軽に[Discord](https://discord.iota.org)にご参加ください。
<!-- If you want to get involved in the community, need help with getting setup, have any issues related with the library or just want to discuss IOTA, Distributed Registry Technology (DRT) and IoT with other people, feel free to join our [Discord](https://discord.iota.org). -->
