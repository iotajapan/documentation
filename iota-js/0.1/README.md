# IOTA JAクライアントライブラリ
<!-- # IOTA JS Library -->

これは**公式**のJavaScriptクライアントライブラリで、次のことができます。
<!-- This is the **official** JavaScript client library, which allows you to do the following: -->
- アカウントを作成、インポート、エクスポート、および管理する
<!-- - Create, import, export, and manage accounts -->
- トランザクションを送信する
<!-- - Send transactions -->
- ペンディングトランザクションの促進と再添付を行う
<!-- - Promote and reattach pending transactions -->
- 条件付き預け入れアドレス（CDA）への預け入れをリクエストする
<!-- - Request deposits into conditional deposit addresses (CDA) -->
- IRIノードと対話する
<!-- - Interact with an IRI node -->

ライブラリの機能の詳細については、以下をご覧ください。
<!-- To learn more about how the library works: -->

- [アカウントを作成する方法](root://iota-js/0.1/how-to-guides/create-account.md)
<!-- - See how you can [work with accounts](root://iota-js/0.1/how-to-guides/create-account.md). -->
- [トランザクションの送受信方法](root://iota-js/0.1/how-to-guides/create-and-manage-cda.md)
<!-- - See how you can [send and receive](root://iota-js/0.1/how-to-guides/create-and-manage-cda.md) transactions. -->

[IOTA JS GitHubレポジトリ](https://github.com/iotaledger/iota.js)でライブラリを入手してください。
<!-- Get the library at the [IOTA JS  GitHub repository](https://github.com/iotaledger/iota.js). -->

:::warning:ベータソフトウェア
クライアントライブラリは現在ベータ版です。本番環境での使用はサポートされていません。
:::
<!-- :::warning:Beta software -->
<!-- The client libraries are currently in beta. Their use in production is not supported. -->
<!-- ::: -->

問題があれば[issueトラッカー](https://github.com/iotaledger/iota.js/issues/new)で報告してください。
<!-- Please report any issues in our [issue tracker](https://github.com/iotaledger/iota.js/issues/new). -->

## 対象読者
<!-- ## Audience -->

このドキュメントは、JavaScriptプログラミング言語とオブジェクト指向プログラミングの概念に精通している人を対象にしています。[アドレスの再利用](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse)や[バンドルとトランザクション](root://iota-basics/0.1/concepts/bundles-and-transactions.md)などのIOTAの基本概念にも精通している必要があります。
<!-- This documentation is designed for people who are familiar with the JavaScript programming language and object-oriented programming concepts. You should also be familiar with basic IOTA concepts such as [address reuse](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse),  [bundles, and transactions](root://iota-basics/0.1/concepts/bundles-and-transactions.md). -->

このガイドは、IOTAを使用したアプリケーションの探求と開発を迅速に開始できるようにすることを目的としています。
<!-- This guide is designed to let you quickly start exploring and developing applications with IOTA. -->

## 前提条件
<!-- ## Prerequisites -->

このライブラリを使用するには、コンピュータに[Node.js](https://nodejs.org)の次の[サポート対象](https://github.com/iotaledger/iota.js/blob/next/.travis.yml#L5)のバージョンのいずれかがインストールされている必要があります。
<!-- To use the library, your computer must have one of the following [supported](https://github.com/iotaledger/iota.js/blob/next/.travis.yml#L5) versions of [Node.js](https://nodejs.org): -->
- Node.js 10以上。[最新のLTS](https://nodejs.org/en/download/)をお勧めします。
<!-- - Node.js 10 or higher. Recommended version is [latest LTS](https://nodejs.org/en/download/). -->
- Node.js 8

[ライブラリパッケージ](https://www.npmjs.com/org/iota)をインストールするには、コンピュータに次のいずれかのパッケージマネージャが必要です。
<!-- To install [library packages](https://www.npmjs.com/org/iota), your computer must have one of the following package managers: -->

- [npm](https://www.npmjs.com/)（Node.jsの[ダウンロード](https://nodejs.org/en/download/)に含まれています）
<!-- - [npm](https://www.npmjs.com/) (Included in Node.js [downloads](https://nodejs.org/en/download/)) -->
- [Yarn](https://yarnpkg.com/)

`package.json`ファイルも必要です。 [`npm init`](https://docs.npmjs.com/cli/init)または[`yarn init`](https://yarnpkg.com/lang/en/docs/cli/init/)を実行して生成してください。
<!-- You must also have a `package.json` file. Generate one by doing [`npm init`](https://docs.npmjs.com/cli/init) or [`yarn init`](https://yarnpkg.com/lang/en/docs/cli/init/). -->

## ライブラリをインストールする
<!-- ## Install the library -->

IOTA JavaScriptクライアントライブラリとその依存関係をインストールするには、以下のいずれかのオプションを使用します。
<!-- To install the IOTA JavaScript client library and its dependencies, you can use one of the following options: -->

- npmでライブラリをインストールする
  <!-- - Install the library with npm -->
    ```bash
    npm install @iota/core
    ```
- Yarnを使ってライブラリをインストールする
  <!-- - Install the library with Yarn -->
    ```bash
    yarn add @iota/core
    ```

## はじめに
<!-- ## Get started -->

[ライブラリをインストール](#ライブラリをインストールする)した後は、IRIノードに接続して、IRIノードとやり取りすることができます。
<!-- After you've [installed the library](#install-the-library), you can connect to an IRI and interface with it. -->

ローカルIRIノードに接続するには、以下の手順に従います。
<!-- To connect to a local IRI node, do the following: -->

```js
import { composeAPI } from '@iota/core'

const iota = composeAPI({
    // replace with your IRI node address
    // or connect to a Devnet node for testing: 'https://nodes.devnet.iota.org:443'
    provider: 'http://localhost:14265'
})

iota.getNodeInfo()
    .then(info => console.log(info))
    .catch(error => {
        console.log(`Request error: ${error.message}`)
    })
```

## APIリファレンス
<!-- ## API reference -->

利用可能なすべてのAPIメソッドの詳細については、[リファレンスページ](https://github.com/iotaledger/iota.js/blob/next/api_reference.md)を参照してください。
<!-- For details on all available API methods, see the [reference page](https://github.com/iotaledger/iota.js/blob/next/api_reference.md). -->


- [.composeApi([settings])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.composeApi)
- [.addNeighbors(uris, [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.addNeighbors)
- [.attachToTangle(trunkTransaction, branchTransaction, minWeightMagnitude, trytes, [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.attachToTangle)
- [.broadcastBundle(tailTransactionHash, [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.broadcastBundle)
- [.broadcastTransactions(trytes, [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.broadcastTransactions)
- [.checkConsistency(transactions, [options], [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.checkConsistency)
- [.findTransactionObjects(query, [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.findTransactionObjects)
- [.findTransactions(query, [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.findTransactions)
- [.getAccountData(seed, options, [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getAccountData)
- [.getBalances(addresses, threshold, [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getBalances)
- [.getBundle(tailTransactionHash, [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getBundle)
- [.getInclusionStates(transactions, tips, [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getInclusionStates)
- [.getInputs(seed, [options], [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getInputs)
- [.getLatestInclusion(transactions, tips, [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getLatestInclusion)
- [.getNeighbors([callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getNeighbors)
- [.getNewAddress(seed, [options], [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getNewAddress)
- [.getNodeInfo([callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getNodeInfo)
- [getTips([callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getTips)
- [getTransactionObjects(hashes, [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getTransactionObjects)
- [.getTransactionsToApprove(depth, [reference], [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getTransactionsToApprove)
- [.getTrytes(hashes, [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getTrytes)
- [.isPromotable(tail, [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.isPromotable)
- [.prepareTransfers(seed, transfers, [options], [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.prepareTransfers)
- [.promoteTransaction(tail, depth, minWeightMagnitude, transfer, [options], [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.promoteTransaction)
- [.removeNeighbors(uris, [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.removeNeighbors)
- [.replayBundle(tail, depth, minWeightMagnitude, [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.replayBundle)
- [.sendTrytes(trytes, depth, minWeightMagnitude, [reference], [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.sendTrytes)
- [.storeAndBroadcast(trytes, [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.storeAndBroadcast)
- [.storeTransactions(trytes, [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.storeTransactions)
- [.traverseBundle(trunkTransaction, [bundle], [callback])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.traverseBundle)
- [.generateAddress(seed, index, [security], [checksum])](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.generateAddress)

## 例
<!-- ## Examples -->

以下に示す例と同様に、IOTA財団の[例ディレクトリ](https://github.com/iotaledger/iota.js/tree/next/examples)もご覧ください。
<!-- As well as the following examples, you can take a look at our [examples directory](https://github.com/iotaledger/iota.js/tree/next/examples) for more. -->

### トランザクションを作成してブロードキャストする
<!-- ### Create and broadcast transactions -->

この例では、[`prepareTransfers`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.prepareTransfers)メソッドを呼び出して、準備済みバンドルを[`sendTrytes`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.sendTrytes)メソッドにパイプ処理することによって、トランザクションを作成してIRIノードに送信する方法を示します。
<!-- This example shows you how to create and send a transaction to an IRI node by calling the [`prepareTransfers`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.prepareTransfers) method and piping the prepared bundle to the [`sendTrytes`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.sendTrytes) method. -->

```js
import { composeAPI } from '@iota/core'

const iota = composeAPI({
    // replace with your IRI node address
    // or connect to a Devnet node for testing: 'https://nodes.devnet.iota.org:443'
    provider: 'http://localhost:14265'
})

// Must be truly random & 81-trytes long.
const seed = ' your seed here '

// Array of transfers which defines transfer recipients and value transferred in IOTAs.
const transfers = [{
    address: ' recipient address here ',
    value: 1000, // 1Ki
    tag: '', // optional tag of `0-27` trytes
    message: '' // optional message in trytes
}]

// Depth or how far to go for tip selection entry point.
const depth = 3

// Difficulty of Proof-of-Work required to attach transaction to tangle.
// Minimum value on mainnet is `14`, `7` on spamnet and `9` on devnet and other testnets.
const minWeightMagnitude = 14

// Prepare a bundle and signs it.
iota.prepareTransfers(seed, transfers)
    .then(trytes => {
        // Persist trytes locally before sending to network.
        // This allows for reattachments and prevents key reuse if trytes can't
        // be recovered by querying the network after broadcasting.

        // Does tip selection, attaches to tangle by doing PoW and broadcasts.
        return iota.sendTrytes(trytes, depth, minWeightMagnitude)
    })
    .then(bundle => {
        console.log(`Published transaction with tail hash: ${bundle[0].hash}`)
        console.log(`Bundle: ${JSON.stringify(bundle, null, 1)}`)
    })
    .catch(err => {
        // handle errors here
    })
```

### カスタムAPIメソッドを作成する
<!-- ### Create custom API methods -->

1. IRI HTTPクライアントをインストールする
  <!-- 1. Install an IRI HTTP client -->

    ```bash
    npm install @iota/http-client
    ```

2. APIメソッドを作成する
  <!-- 2. Create an API method -->

    ```js
    import { createHttpClient } from '@iota/http-client'
    import { createGetNodeInfo } from '@iota/core'

    const client = createHttpClient({
        // replace with your IRI node address
        // or connect to a Devnet node for testing: 'https://nodes.devnet.iota.org:443'
        provider: 'http://localhost:14265'
    })

    const getNodeInfo = createGetNodeInfo(client)
    ```

## プロジェクトを支援する
<!-- ## Support the project -->

IOTA JavaScriptクライアントライブラリがあなたにとって役に立ち、プロジェクトに貢献したいとお考えでしたら、[バグレポート](https://github.com/iotaledger/iota.js/issues/new)、[機能リクエスト](https://github.com/iotaledger/iota.js/issues/new)または[プルリクエスト](https://github.com/iotaledger/iota.js/pulls/)を投稿することをご検討ください。
<!-- If the IOTA JavaScript client library has been useful to you and you feel like contributing, consider posting a [bug report](https://github.com/iotaledger/iota.js/issues/new), [feature request](https://github.com/iotaledger/iota.js/issues/new) or a [pull request](https://github.com/iotaledger/iota.js/pulls/). -->

### GitHubでリポジトリのクローンを作成してブートストラップする
<!-- ### Clone and bootstrap the repository on GitHub -->

1. 右上隅にある<kbd>Fork</kbd>ボタンをクリックする
  <!-- 1. Click the <kbd>Fork</kbd> button in the top-right corner -->
2. フォークをクローンしてディレクトリに移動する
  <!-- 2. Clone your fork and change directory into it -->
3. 環境をブートストラップする
  <!-- 3. Bootstrap your environment -->

    ```bash
    npm run init
    ```

このステップでは、すべての依存関係をダウンロードし、パッケージをビルドしてリンクします。 iota.jsは[Lerna](https://lerna.js.org)を使って複数のパッケージを管理します。`lerna bootstrap`コマンドを使用して、いつでもセットアップを再ブートストラップできます。
<!-- This step will download all dependencies, build and link the packages together. iota.js uses [Lerna](https://lernajs.io/) to manage multiple packages. You can re-bootstrap your setup at any point with `lerna bootstrap` command. -->

### テストを実行する
<!-- ### Run tests -->

単一のパッケージまたは複数のパッケージに変更を加え、ルートディレクトリから次のコマンドを実行してシステムをテストします。
<!-- Make your changes on a single package or across multiple packages and test the system by running the following from the root directory: -->

```bash
npm test
```

特定のパッケージのテストを実行するには、ディレクトリをパッケージのディレクトリに変更し、そこから`npm test`を実行します。
<!-- To run tests of specific package, change directory into the package's directory and run `npm test` from there. -->

### ドキュメントを更新する
<!-- ### Update documentation -->

必要に応じて[`JSDoc`](https://jsdoc.app)アノテーションを編集し、ルートディレクトリから`npm run docs`を実行してドキュメントを更新してください。
<!-- Please update the documention when needed by editing [`JSDoc`](http://usejsdoc.org) annotations and running `npm run docs` from the root directory. -->

## ディスカッションに参加する
<!-- ## Join the discussion -->

コミュニティに参加したい、セットアップの手助けを必要とする、ライブラリに関する問題がある、または単にIOTA、分散レジストリ技術（DRT）、およびIoTについて他の人々と議論したい場合は、気軽に[Discord](https://discord.iota.org)に参加してください。
<!-- If you want to get involved in the community, need help with getting setup, have any issues related with the library or just want to discuss IOTA, Distributed Registry Technology (DRT) and IoT with other people, feel free to join our [Discord](https://discord.iota.org). -->
