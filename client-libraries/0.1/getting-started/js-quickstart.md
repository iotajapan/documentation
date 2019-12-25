# Node.js クイックスタート
<!-- # Node.js quickstart -->

**このクイックスタートでは、開発環境の設定からタングルでのライブトランザクションのリッスンまで、Node.js での IOTA 開発の基本を学びます。**
<!-- **In this quickstart, you learn the basics of IOTA development in Node.js, from setting up a development environment to listening for live transaction on the Tangle.** -->

このクイックスタートでは、次の方法を学習します。
<!-- In this quickstart, you will learn how to: -->

1. 開発環境をセットアップする
<!-- 1. Set up a developer environment -->

2. パッケージをインストールする
<!-- 2. Install packages -->

3. ノードに接続する
<!-- 3. Connect to a node -->

## 手順1. 開発環境をセットアップする
<!-- ## Step 1. Set up a developer environment -->

JavaScript クライアントライブラリを使用するには、開発環境を構成するプログラミングツールのセットが必要です。
<!-- To use the JavaScript client library, you need a set of programming tools, which make up a development environment. -->

1. [最新の長期サポート(LTS)バージョンの Node.js](https://nodejs.org/en/download/) をインストールします。
<!-- 1. Install the [latest long-term support (LTS) version of Node.js](https://nodejs.org/en/download/) -->

2. コードエディタをインストールします。[Visual Studio Code](https://code.visualstudio.com/Download) をお勧めしますが、さらに多くのものが利用可能です。
<!-- 2. Install a code editor. We recommend [Visual Studio Code](https://code.visualstudio.com/Download), but many more are available. -->

3. コマンドラインインターフェースを開きます。
<!-- 3. Open a command-line interface -->

  オペレーティングシステムに応じて、コマンドラインインターフェイスは [Windows の PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/getting-started/getting-started-with-windows-powershell?view=powershell-6)、[Linux ターミナル](https://www.howtogeek.com/140679/beginner-geek-how-to-start-using-the-linux-terminal/)、または[macOS のターミナル](https://macpaw.com/how-to/use-terminal-on-mac)になります。
  <!-- Depending on your operating system, a command-line interface could be [PowerShell in Windows](https://docs.microsoft.com/en-us/powershell/scripting/getting-started/getting-started-with-windows-powershell?view=powershell-6), the [Linux Terminal](https://www.howtogeek.com/140679/beginner-geek-how-to-start-using-the-linux-terminal/) or [Terminal for macOS](https://macpaw.com/how-to/use-terminal-on-mac). -->

4. プロジェクトに使用する新しいディレクトリを作成し、そこに移動します。
  <!-- 4. Create a new directory to use for your project and change into it -->

    ```bash
    mkdir myNewProject
    cd myNewProject
    ```

5. 新しいプロジェクトを初期化します。
  <!-- 5. Initialize a new project -->

    ```bash
    npm init
    ```

これで、プロジェクトが依存するパッケージとアプリケーション、およびプロジェクトの名前、説明、作成者などの特定のメタデータを含む `package.json` ファイルが作成されました。パッケージをインストールするたびに、インストールされたパッケージは依存関係として `package.json` ファイルに追加されます。詳細については、この優れた [package.json ガイド](https://flaviocopes.com/package-json/)を参照してください。
<!-- Now you have a `package.json` file, which includes the packages and applications your project depends on, and specific metadata like the project's name, description, and author. Whenever you install packages, those packages will be added to this file as a dependency. For more information, see this excellent [package.json guide](https://flaviocopes.com/package-json/). -->

## 手順2. パッケージをインストールする
<!-- ## Step 2. Install packages -->

JavaScript クライアントライブラリは、関連するメソッドを含むパッケージで構成されています。たとえば、`core` パッケージには、ノードから情報をリクエストし、トランザクションを作成し、トランザクションをノードに送信するためのメソッドが含まれています。
<!-- The JavaScript client library is organized in packages, which contain related methods. For example, the `core` package contains methods for requesting information from nodes, creating transactions, and sending them to nodes. -->

すべてのパッケージは [iota.js GitHub リポジトリ](https://github.com/iotaledger/iota.js/tree/next/packages)にリストされています。
<!-- All the packages are listed on the [iota.js GitHub repository](https://github.com/iotaledger/iota.js/tree/next/packages). -->

ライブラリパッケージをインストールするには、次のいずれかのパッケージマネージャーを使用できます。
<!-- To install the library packages, you can use one of the following package managers: -->

- [npm](https://www.npmjs.com/) (Node.js のダウンロードに含まれています)
<!-- - [npm](https://www.npmjs.com/) (included in Node.js downloads) -->
- [Yarn](https://yarnpkg.com/)

コマンドラインインターフェイスで、プロジェクトを初期化したディレクトリに移動し、パッケージをインストールします。
<!-- In a command-line interface, change into the directory where you initialized your project, and install the packages -->

--------------------
### npm
以下のコマンドは `core` パッケージをインストールします。

```bash
npm install @iota/core
```
---
### Yarn
以下のコマンドは `core` パッケージをインストールします。

```bash
yarn add @iota/core
```
--------------------

すべてがうまくいった場合は、標準出力に次のようなものが表示されるはずです。`npm WARN` メッセージは無視できます。
<!-- If everything went well, you should see something like the following in the output. You can ignore any 'npm WARN' messages. -->

```shell
+ @iota/core@1.0.0-beta.8
added 19 packages from 10 contributors and audited 68 packages in 5.307s
found 0 vulnerabilities
```

パッケージをインストールすると、`core` パッケージコードとその依存関係を含む `node_modules` ディレクトリが作成されます。
<!-- After installing a package, you'll have a `node_modules` directory, which contains the `core` package code and its dependencies. -->

これで、コーディングを開始できます。
<!-- Now you can start coding. -->

## 手順3. ノードに接続する
<!-- ## Step 3. Connect to a node -->

トランザクションの送信を開始する前に、[同期済みノード](root://getting-started/0.1/network/nodes.md#synchronized-nodes)に接続していることを確認することをお勧めします。同期済みノードに接続することで、[タングル](root://getting-started/0.1/network/the-tangle.md)の最新の概観を知ることが出来ます。
<!-- It's best practice to make sure that you're connected to a [synchronized node](root://getting-started/0.1/network/nodes.md#synchronized-nodes) before you start sending transactions to it. This way, you know that it has an up-to-date view of [the Tangle](root://getting-started/0.1/network/the-tangle.md). -->

ノードに接続するたびに、どの[IOTAネットワーク](root://getting-started/0.1/network/iota-networks.md)に接続しているかを知る必要があります。ここでは、テストに使用できるIOTAネットワークであるDevnetのノードに接続します。
<!-- Whenever you connect to a node, you need to know which [IOTA network](root://getting-started/0.1/network/iota-networks.md) it's in. Here, we connect to a node on the Devnet, which is the IOTA networks that you can use for testing. -->

1. IOTA財団の[公式 Discord] に移動し、`botbox` チャンネルに **!milestone** と入力します。
<!-- 1. Go to the IOTA Foundation [Discord](https://discord.iota.org) and enter **!milestone** in the `botbox` channel -->

  ![Entering !milestone on Discord](../images/discord-milestone-check.PNG)

  Discord ボットは、[ノードクォーラム](root://getting-started/0.1/network/nodes.md#node-quorum)から現在の `latestMilestoneIndex` フィールドを返します。
  <!-- The Discord bot should return the current `latestMilestoneIndex` field from a [node quorum](root://getting-started/0.1/network/nodes.md#node-quorum). -->

2. プロジェクトを初期化したディレクトリで、`index.js` という新しいファイルを作成します。
<!-- 2. In the directory where you initialized your project, create a new file called `index.js` -->

3. ノードが同期しているかどうかを確認するには、次のコードをコピーして、`index.js` ファイルにペーストします。
  <!-- 3. To check if your node is synchronized, copy and paste the following code into the `index.js` file -->

    ```js
    // IOTA ライブラリを require します
    const Iota = require('@iota/core');

    // IOTA API オブジェクトの新しいインスタンスを作成します
    // `provider` フィールドを使用して、接続するノードを指定します
    const iota = Iota.composeAPI({
        provider: 'https://nodes.devnet.iota.org:443'
    });

    // ノードとタングルについての情報を得るために `getNodeInfo()` メソッドを呼び出します
    iota.getNodeInfo()
    // 返されたオブジェクトを JSON に変換して、出力を読みやすくします
    .then(info => console.log(JSON.stringify(info, null, 1)))
    .catch(err => {
        // エラーを処理します
        console.log(err);
    });
    ```

4. ファイルを実行します。
  <!-- 4. Execute the file -->

    ```bash
    node index.js
    ```

ノードは次のオブジェクトを返します。
<!-- The node returns the following: -->

```json
{
    "appName": "IRI Testnet",
    "appVersion": "1.5.6-RELEASE",
    "jreAvailableProcessors": 8,
    "jreFreeMemory": 12052395632,
    "jreVersion": "1.8.0_181",
    "jreMaxMemory": 22906667008,
    "jreTotalMemory": 16952328192,
    "latestMilestone": "FPRSBTMKOP9JTTQSHWRGMPT9PBKYWFCCFLZLNWQDFRCXDDHZEFIEDXRIJYIMVGCXYQRHSZQYCTWXJM999",
    "latestMilestoneIndex": 1102841,
    "latestSolidSubtangleMilestone": "FPRSBTMKOP9JTTQSHWRGMPT9PBKYWFCCFLZLNWQDFRCXDDHZEFIEDXRIJYIMVGCXYQRHSZQYCTWXJM999",
    "latestSolidSubtangleMilestoneIndex": 1102841,
    "milestoneStartIndex": 434525,
    "neighbors": 3,
    "packetsQueueSize": 0,
    "time": 1549482118137,
    "tips": 153,
    "transactionsToRequest": 0,
    "features": ["snapshotPruning", "dnsRefresher", "testnet", "zeroMessageQueue", "tipSolidification", "RemotePOW"],
    "coordinatorAddress": "EQQFCZBIHRHWPXKMTOLMYUYPCN9XLMJPYZVFJSAY9FQHCCLWTOLLUGKKMXYFDBOOYFBLBI9WUEILGECYM",
    "duration": 0
}
```

### レスポンスオブジェクトの内容
<!-- ### Reading the response object -->

レスポンスオブジェクトの `latestMilestoneIndex` フィールドが Discord から取得した `latestMilestoneIndex` フィールドと `latestSolidSubtangleMilestoneIndex` フィールドに等しい場合、ノードは同期しています。
<!-- If the `latestMilestoneIndex` field is equal to the one you got from Discord and the `latestSolidSubtangleMilestoneIndex` field, the node is synchronized. -->

そうでない場合は、別のノードに接続してみてください。[iota.dance Web サイト](https://iota.dance/)には、メインネットノードの一覧が含まれています。または、[自分自身のノードを実行](root://node-software/0.1/iri/how-to-guides/quickstart.md)できます。
<!-- If not, try connecting to a different node. The [iota.dance website](https://iota.dance/) includes a list of Mainnet nodes. Or, you can [run your own node](root://node-software/0.1/iri/how-to-guides/quickstart.md). -->

レスポンスオブジェクトの `features` 配列で、このノードは[リモートプルーフオブワーク](root://getting-started/0.1/transactions/proof-of-work.md)（RemotePOW）もサポートしていることがわかります。その結果、このノードを使用して、ローカルデバイスでプルーフオブワークを実行する代わりに、ノードでプルーフオブワークを行うことができます。
<!-- In the `features` array, you can see that this node also support [remote proof of work](root://getting-started/0.1/transactions/proof-of-work.md) (RemotePOW). As a result, you can use this node to do proof of work instead of doing it on your local device. -->

また、このノードではゼロメッセージキュー（ZMQ）が有効になっているため、ZMQ を使用して[ライブトランザクションをリッスン](../how-to-guides/js/listen-for-transactions.md)できます。
<!-- Also, this node has its zero message queue (ZMQ) enabled, so you can use it to [listen for live transactions](../how-to-guides/js/listen-for-transactions.md). -->

これらのフィールドの詳細については、[IRI API リファレンス](root://node-software/0.1/iri/references/api-reference.md#getNodeInfo)を参照してください。
<!-- For more information about these fields, see the [IRI API reference](root://node-software/0.1/iri/references/api-reference.md#getNodeInfo). -->

:::success: おめでとうございます:tada:
同期済みノードへの接続を確認できました。
:::
<!-- :::success: Congratulations :tada: -->
<!-- You've confirmed your connection to a synchronized node. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

[REPL.itツール](https://repl.it)を使用して、ブラウザーでサンプルコードを実行できるようにします。
<!-- We use the [REPL.it tool](https://repl.it) to allow you to run sample code in the browser. -->

緑色のボタンをクリックしてコードを実行し、ウィンドウで結果を確認します。
<!-- Click the green button to run the code and see the results in the window. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Connect-to-a-node?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## プロジェクトをサポートする
<!-- ## Support the project -->

IOTA JavaScript クライアントライブラリが役に立ち、貢献したいと思う場合は、[バグレポート](https://github.com/iotaledger/iota.js/issues/new)、[機能リクエスト](https://github.com/iotaledger/iota.js/issues/new)、または[プルリクエスト](https://github.com/iotaledger/iota.js/pulls/)の投稿を検討してください。
<!-- If the IOTA JavaScript client library has been useful to you and you feel like contributing, consider posting a [bug report](https://github.com/iotaledger/iota.js/issues/new), [feature request](https://github.com/iotaledger/iota.js/issues/new), or a [pull request](https://github.com/iotaledger/iota.js/pulls/). -->

### テストの実行
<!-- ### Running tests -->

単一のパッケージまたは複数のパッケージで変更を行い、ルートディレクトリから次のコマンドを実行してテストします。
<!-- Make your changes on a single package or across multiple packages and test the them by running the following command from the root directory: -->

```bash
npm run test
```
特定のパッケージのテストを実行するには、パッケージのディレクトリに移動して、`npm run test` コマンドを実行します。
<!-- To run tests for a specific package, change into the package's directory and run the `npm run test` command. -->

### ドキュメントの更新
<!-- ### Updating the documentation -->

変更がドキュメントに影響する場合は、[`JSDoc`](http://usejsdoc.org) アノテーションを編集し、ルートディレクトリから `npm run docs` を実行してドキュメントを更新してください。
<!-- If your changes affect the documentation, please update it by editing the [`JSDoc`](http://usejsdoc.org) annotations and running `npm run docs` from the root directory. -->

## 参加する
<!-- ## Get involved -->

[Discordチャンネル](https://discord.iota.org)では、次のことができます。
<!-- [Join our Discord channel](https://discord.iota.org) where you can: -->

- IOTA開発者およびコミュニティとの議論に参加することができます。
<!-- - Take part in discussions with IOTA developers and the community -->
- 助けを求めることができます。
<!-- - Ask for help -->
- 他の人を助けるためにあなたの知識を共有することができます。
<!-- - Share your knowledge to help others -->

次のような多くのチャネルがあります。
<!-- We have many channels, including the following: -->

- `*-dev`：これらのチャンネルは読み取り専用であり、開発者が互いにトピックを議論したり、GitHubからのコードの更新を確認したりできます。
<!-- - `-dev`: These channels are read-only and are where developers discuss topics with each other and where you can see any code updates from GitHub. -->

- `*-discussion`：これらのチャンネルはあなたが参加できる場所です。
<!-- - `-discussion`: These channels are where you can participate. -->

##  次のステップ
<!-- ## Next steps -->

[JavaScript ワークショップ](../how-to-guides/js/get-started.md)で学習を続ける
<!-- Continue learning with our [JavaScript workshop](../how-to-guides/js/get-started.md). -->

[開発者ハンドブック](root://getting-started/0.1/references/quickstart-dev-handbook.md)を読んで、自分自身のノードを実行するかどうか、プライベートIOTAネットワークが必要かどうか、および両方が必要かどうかの参考にしてください。
<!-- Read our [developer's handbook](root://getting-started/0.1/references/quickstart-dev-handbook.md) for guidance on whether you should run your own node, whether you need a private IOTA network, and what you need to consider for both. -->
