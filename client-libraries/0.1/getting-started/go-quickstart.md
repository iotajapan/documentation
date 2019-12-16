# Goクイックスタート
<!-- # Go quickstart -->

**このクイックスタートでは、開発環境の設定からタングル上のライブトランザクションのリッスンまで、GoでのIOTA開発の基本を学びます。**
<!-- **In this quickstart, you learn the basics of IOTA development in Go, from setting up a development environment to listening for live transaction on the Tangle.** -->

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

Goクライアントライブラリを使用するには、開発環境を構成するプログラミングツールのセットが必要です。
<!-- To use the Go client library, you need a set of programming tools, which make up a development environment. -->

Goライブラリとその依存関係をダウンロードするには、[vgo modules](https://github.com/golang/go/wiki/Modules)（Go 1.11以上）を使用してプロジェクトの依存関係を管理することをお勧めします。
<!-- To download the Go library and its dependencies, we recommend that you use [vgo modules](https://github.com/golang/go/wiki/Modules) (since Go 1.11) to manage dependencies in your project. -->

1. [Go1.11以上をインストールします](https://golang.org/doc/install)。
<!-- 1. [Install Go version 1.11 or later](https://golang.org/doc/install) -->

2. コードエディターをインストールします。[Visual Studio Code](https://code.visualstudio.com/Download)をお勧めしますが、さらに多くのものが利用可能です。
<!-- 2. Install a code editor. We recommend [Visual Studio Code](https://code.visualstudio.com/Download), but many more are available. -->

3. コマンドラインインターフェースを開きます。
<!-- 3. Open a command-line interface -->

  オペレーティングシステムに応じて、コマンドラインインターフェイスは[WindowsのPowerShell](https://docs.microsoft.com/en-us/powershell/scripting/getting-started/getting-started-with-windows-powershell?view=powershell-6)、[Linuxターミナル](https://www.howtogeek.com/140679/beginner-geek-how-to-start-using-the-linux-terminal/)、または[macOSのターミナル](https://macpaw.com/how-to/use-terminal-on-mac)になります。
<!-- Depending on your operating system, a command-line interface could be [PowerShell in Windows](https://docs.microsoft.com/en-us/powershell/scripting/getting-started/getting-started-with-windows-powershell?view=powershell-6), the [Linux Terminal](https://www.howtogeek.com/140679/beginner-geek-how-to-start-using-the-linux-terminal/) or [Terminal for macOS](https://macpaw.com/how-to/use-terminal-on-mac). -->

4. `$GOPATH`以外のディレクトリで、プロジェクト用のディレクトリを作成し、初期化します。`&lt;your-module-path&gt;`プレースホルダーを、`github.com/me/awesome-project`などのプロジェクトへのパスに置き換えます。
  <!-- 4. In any directory outside of `$GOPATH`, create a directory for your project and initialize it. Replace the `<your-module-path>` placeholder with the path to your project such as `github.com/me/awesome-project`. -->

    ```bash
    go mod init <your-module-path>
    ```

これで、`go.mod`ファイルと`go.sum`ファイルが作成され、パッケージのインストールを開始する準備が整いました。
<!-- Now you have a `go.mod` file and a `go.sum` file, and you're ready to start installing packages. -->

## 手順2. パッケージをインストールする
<!-- ## Step 2. Install packages -->

Goクライアントライブラリは、関連するメソッドを含むパッケージで構成されています。たとえば、`api`パッケージには、ノードから情報をリクエストし、トランザクションを作成し、それらをノードに送信するためのメソッドが含まれています。
<!-- The Go client library is organized in packages, which contain related methods. For example, the `api` package contains methods for requesting information from nodes, creating transactions, and sending them to nodes. -->

すべてのパッケージは[Go GitHub repository](https://github.com/iotaledger/iota.go/)にリストされています。
<!-- All the packages are listed on the [Go GitHub repository](https://github.com/iotaledger/iota.go/). -->

以下のコマンドで`api`パッケージをインストールします。
<!-- This command installs the `api` package -->

```bash
go get github.com/iotaledger/iota.go/api
```

すべてがうまくいった場合は、標準出力に次のようなものが表示されるはずです。
<!-- If everything went well, you should see something like the following in the output: -->

```shell
go: finding github.com/iotaledger/iota.go/api latest
go: finding github.com/iotaledger/iota.go v1.0.0-beta.10
go: downloading github.com/iotaledger/iota.go v1.0.0-beta.10
```

`api`パッケージをインストールした後、`api`パッケージはモジュールの`go.mod`ファイルに依存関係として追加され、`api`パッケージの依存関係が`go.sum`ファイルに追加されます。
<!-- After installing the `api` package, it is added as a dependency in your module's `go.mod` file, and the package's dependencies are added to you `go.sum` file. -->

これで、コーディングを開始できます。
<!-- Now you can start coding. -->

## 手順3. ノードに接続する
<!-- ## Step 3. Connect to a node -->

トランザクションの送信を開始する前に、[同期済みのノード](root://getting-started/0.1/network/nodes.md#synchronized-nodes)に接続していることを確認することをお勧めします。同期済みのノードに接続しているときにのみ、あなたは最新の[タングル](root://getting-started/0.1/network/the-tangle.md)の概観が分かります。
<!-- It's best practice to make sure that you're connected to a [synchronized node](root://getting-started/0.1/network/nodes.md#synchronized-nodes) before you start sending transactions to it. This way, you know that it has an up-to-date view of [the Tangle](root://getting-started/0.1/network/the-tangle.md). -->

ノードに接続するたびに、どの[IOTAネットワーク](root://getting-started/0.1/network/iota-networks.md)に入っているかを知る必要があります。ここでは、テストに使用できるIOTAネットワークであるデブネットのノードに接続します。
<!-- Whenever you connect to a node, you need to know which [IOTA network](root://getting-started/0.1/network/iota-networks.md) it's in. Here, we connect to a node on the Devnet, which is the IOTA networks that you can use for testing. -->

1. IOTA財団の公式[Discord](https://discord.iota.org)に移動し、`botbox`チャンネルに**!milestone **と入力します
  <!-- 1. Go to the IOTA Foundation [Discord](https://discord.iota.org) and enter **!milestone** in the `botbox` channel -->

    ![Entering !milestone on Discord](../images/discord-milestone-check.PNG)

    Discordボットは、[ノードクォーラム](root://getting-started/0.1/network/nodes.md#node-quorum)から現在の`latestMilestoneIndex`フィールドを返します。
    <!-- The Discord bot should return the current `latestMilestoneIndex` field from a [node quorum](root://getting-started/0.1/network/nodes.md#node-quorum). -->

2. プロジェクトを初期化したディレクトリで、`connectToANode.go`という新しいファイルを作成します。
<!-- 2. In the directory where you initialized your project, create a new file called `connectToANode.go` -->

3. ノードが同期されているかどうかを確認するには、次のコードをコピーして`connectToANode.go`ファイルにペーストします。
  <!-- 3. To check if your node is synchronized, copy and paste the following code into the `connectToANode.go` file -->

    ```go
    package main

    import (
        . "github.com/iotaledger/iota.go/api"
        "fmt"
    )

    var node = "https://nodes.devnet.iota.org:443"

    func main() {
        // IOTA APIオブジェクトの新しいインスタインスを作成し、
        // 接続するノードを指定します
        api, err := ComposeAPI(HTTPClientSettings{URI: node})
        must(err)

        // ノードとタングルについての情報を得るために `getNodeInfo()`メソッドを呼び出します
        nodeInfo, err := api.GetNodeInfo()
        must(err)

        fmt.Println(nodeInfo)

    }

    func must(err error) {
        if err != nil {
            panic(err)
        }
    }
    ```

4. ファイルを実行します
  <!-- 4. Execute the file -->

    ```bash
    go run connectToANode.go
    ```

ノードは次を返します。
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

レスポンスオブジェクトの`latestMilestoneIndex`フィールドがDiscordから取得した`latestMilestoneIndex`フィールドと`latestSolidSubtangleMilestoneIndex`フィールドに等しい場合、ノードは同期しています。
<!-- If the `latestMilestoneIndex` field is equal to the one you got from Discord and the `latestSolidSubtangleMilestoneIndex` field, the node is synchronized. -->

そうでない場合は、別のノードに接続してみてください。[iota.dance](https://iota.dance/)には、メインネットノードの一覧が含まれています。または、[自分自身のノードを実行](root://node-software/0.1/iri/how-to-guides/quickstart.md)できます。
<!-- If not, try connecting to a different node. The [iota.dance website](https://iota.dance/) includes a list of Mainnet nodes. Or, you can [run your own node](root://node-software/0.1/iri/how-to-guides/quickstart.md). -->

`features`配列で、このノードは[リモートプルーフオブワーク](root://getting-started/0.1/transactions/proof-of-work.md)（RemotePOW）もサポートしていることがわかります。その結果、ローカルデバイスでプルーフオブワークを実行する代わりに、このノードを使用して、プルーフオブワークを行うことができます。
<!-- In the `features` array, you can see that this node also support [remote proof of work](root://getting-started/0.1/transactions/proof-of-work.md) (RemotePOW). As a result, you can use this node to do proof of work instead of doing it on your local device. -->

また、このノードではゼロメッセージキュー（ZMQ）が有効になっているため、ZMQを使用して[ライブトランザクションをリッスン](../how-to-guides/go/listen-for-transactions.md)できます。
<!-- Also, this node has its zero message queue (ZMQ) enabled, so you can use it to [listen for live transactions](../how-to-guides/go/listen-for-transactions.md). -->

これらのフィールドの詳細については、[IRI APIリファレンス](root://node-software/0.1/iri/references/api-reference.md#getNodeInfo)を参照してください。
<!-- For more information about these fields, see the [IRI API reference](root://node-software/0.1/iri/references/api-reference.md#getNodeInfo). -->

:::success:おめでとうございます:tada:
同期済みノードへの接続を確認しました。
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

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Connect-to-a-node-Go?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## プロジェクトをサポートする
<!-- ## Support the project -->

Goライブラリが役に立ち、貢献したいと思う場合は、[バグレポート](https://github.com/iotaledger/iota.go/issues/new-issue)、機能リクエスト、または[プルリクエスト](https://github.com/iotaledger/iota.go/pulls/)の投稿を検討してください。
<!-- If the Go library has been useful to you and you feel like contributing, consider posting a [bug report](https://github.com/iotaledger/iota.go/issues/new-issue), feature request or a [pull request](https://github.com/iotaledger/iota.go/pulls/). -->

皆様の貢献に感謝します。プルリクエストが受け入れるには、次の基準を満たしている必要があります。
<!-- We thank everyone for their contributions. In order for your pull requests to be accepted, they must fulfill the following criteria: -->
- Ginkgoで追加するテストを作成する必要があります。
- You must write tests for your additions with Ginkgo
- 追加のパラメーターと機能を説明するサンプルコードを記述する必要があります。
<!-- - You must write example code that describes the parameters and the functionality of your additions -->
- プルリクエストは継続的インテグレーション設定に合格する必要があります。
<!-- - Your pull request must pass the continuous integration configuration -->

### Ginkgoでテストを書く
<!-- ### Writing tests with Ginkgo -->

プルリクエストが受け入れる前に、Ginkgoでコードをテストする必要があります。
<!-- Before your pull requests can be accepted, you must test your code in Ginkgo. -->

1. Ginkgoをダウンロードします。
  <!-- 1. Download Ginkgo -->

    ```bash
    go get github.com/onsi/ginkgo/ginkgo
    go get github.com/onsi/gomega/...
    ```

2. 新しいパッケージを作成した場合は、対応するテストスイートファイルを生成します。
  <!-- 2. If you've written a new package, generate a corresponding test-suite file -->

    ```bash
    cd <dir-of-your-package>
    ginkgo bootstrap
    ```

3. 新しいテストファイルを生成します。
  <!-- 3. Generate a new testing file -->

    ```bash
    ginkgo generate <package-name>
    ```

テストファイルを作成すると、次の2つのファイルが作成されます。
<!-- After creating a testing file, you'll have following two files: -->

- `<package-name>_suite_test.go`
- `<package-name>_test.go`

:::info:
Ginkgoテストの作成方法に関するリファレンスとして既存のテストを使用するか、[ドキュメントを読む](https://onsi.github.io/ginkgo/)こともできます。
:::
<!-- :::info: -->
<!-- You can use the existing tests as a reference on how to write Ginkgo tests or you can [read the documentation](https://onsi.github.io/ginkgo/). -->
<!-- ::: -->

4. テストを実行します。
  <!-- 4. Run your tests -->

    ```bash
    go test -v
    === RUN   TestAddress
    Running Suite: Address Suite
    ============================
    Random Seed: 1542616006
    Will run 11 of 11 specs

    •••••••••••
    Ran 11 of 11 Specs in 0.261 seconds
    SUCCESS! -- 11 Passed | 0 Failed | 0 Pending | 0 Skipped
    --- PASS: TestAddress (0.26s)
    PASS
    ok  	github.com/iotaledger/iota.go/address	0.264s
    ```

### ドキュメントの更新
<!-- ### Updating the documentation -->

変更内容がドキュメントに影響する場合は、ドキュメントを更新してください。
<!-- If your changes affect the documentation, please update it. -->

1. 存在しない場合、新しく作成したパッケージに `.examples`ディレクトリを追加します。
<!-- 1. If non existent, add a `.examples` directory in your newly created package -->

2. 次の規則で新しいファイルを作成します。`.examples`ディレクトリ内に`&lt;package-name&gt;_examples_test.go`を作成します。
<!-- 2. Create a new file with the following convention: `<package-name>_examples_test.go` inside the `.examples` directory -->

3. 次のスキーマで例を作成します。
  <!-- 3. Write examples in the following schema: -->

    ```go
    // i req: s, トライトへ変換するASCII文字列。
    // o: Trytes, 入力ASCII文字列のトライト表現。
    // o: error, 非ASCII文字列入力に対して返されます。
    func ExampleASCIIToTrytes() {
        trytes, err := converter.ASCIIToTrytes("IOTA")
        if err != nil {
            // エラーを処理します。
            return
        }
        fmt.Println(trytes) // output: "SBYBCCKB"
    }
    ```

| **シンボル** | **説明**                                       |
| :----------- | :--------------------------------------------- |
| i req        | 関数へのパラメーターを記述します。             |
| i            | 関数へのオプションのパラメーターを記述します。 |
| o            | 関数の戻り値を記述します。                     |

構文：
<!-- Syntax: -->

- パラメーターの場合： `&lt;symbol&gt;: &lt;parameter_name&gt;, &lt;description&gt;.`
<!-- - For parameters: `<symbol>: <parameter_name>, <description>.` -->
- 返り値の場合： `&lt;symbol&gt;: &lt;type&gt;, &lt;description&gt;.`
<!-- - For return values: `<symbol>: <type>, <description>.` -->
- Example関数： `Example&lt;OriginFunctionName&gt;`
<!-- - Example function: `Example<OriginFunctionName>` -->

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

## 次のステップ
<!-- ## Next steps -->

[Goワークショップ](../how-to-guides/go/get-started.md)で学習を続ける。
<!-- Continue learning with our [Go workshop](../how-to-guides/go/get-started.md). -->

[開発者ハンドブック](root://getting-started/0.1/references/quickstart-dev-handbook.md)を読んで、自分自身のノードを実行する必要があるかどうか、プライベートIOTAネットワークが必要かどうか、および両方について考慮する必要があるかについてのガイダンスを参照してください。
<!-- Read our [developer's handbook](root://getting-started/0.1/references/quickstart-dev-handbook.md) for guidance on whether you should run your own node, whether you need a private IOTA network, and what you need to consider for both. -->
