# IOTA GOクライアントライブラリ
<!-- # IOTA Go Library -->

これは**公式**のGoクライアントライブラリで、次のことができます。
<!-- This is the **official** Go client library, which allows you to do the following: -->
- アカウントを作成、インポート、エクスポート、および管理する
<!-- - Create, import, export, and manage accounts -->
- トランザクションを送信する
<!-- - Send transactions -->
- ペンディングトランザクションの促進と再添付を行う
<!-- - Promote and reattach pending transactions -->
- 条件付預け入れアドレス（CDA）への預け入れをリクエストする
<!-- - Request deposits into conditional deposit addresses (CDA) -->
- イベントをリッスンする
<!-- - Listen to events -->
- プラグインでライブラリ機能を拡張する
<!-- - Extend the library functionality with plug-ins -->
- IRIノードと対話する
<!-- - Interact with an IRI node -->

ライブラリの機能の詳細については、以下をご覧ください。
<!-- To learn more about how the library works: -->

- [アカウントを作成する方法](root://iota-go/0.1/how-to-guides/create-account.md)を確認する。
<!-- - See how you can [work with accounts](root://iota-go/0.1/how-to-guides/create-account.md). -->
- [トランザクションの送受信方法](root://iota-go/0.1/how-to-guides/create-and-manage-cda.md)を確認する。
<!-- - See how you can [send and receive](root://iota-go/0.1/how-to-guides/create-and-manage-cda.md) transactions. -->

[IOTA Go GitHubレポジトリ](https://github.com/iotaledger/iota.go)でライブラリを入手してください。
<!-- Get the library at the [IOTA Go GitHub repository](https://github.com/iotaledger/iota.go). -->

:::warning:ベータソフトウェア
クライアントライブラリは現在ベータ版です。本番環境での使用はサポートされていません。
:::
<!-- :::warning:Beta software -->
<!-- The client libraries are currently in beta. Their use in production is not supported. -->
<!-- ::: -->

問題があれば[issueトラッカー](https://github.com/iotaledger/iota.go/issues/new)で報告してください。
<!-- Please report any issues in our [issue tracker](https://github.com/iotaledger/iota.go/issues/new). -->

## 対象読者
<!-- ## Audience -->

このドキュメントは、Goプログラミング言語とオブジェクト指向プログラミングの概念に精通している人を対象にしています。[アドレスの再利用](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse)、[バンドルとトランザクション](root://iota-basics/0.1/concepts/bundles-and-transactions.md)など、IOTAの基本概念にも精通している必要があります。
<!-- This documentation is designed for people who are familiar with the Go programming language and object-oriented programming concepts. You should also be familiar with basic IOTA concepts such as [address reuse](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse), [bundles, and transactions](root://iota-basics/0.1/concepts/bundles-and-transactions.md). -->

このガイドは、IOTAを使用したアプリケーションの探求と開発を迅速に開始できるようにすることを目的としています。
<!-- This guide is designed to let you quickly start exploring and developing applications with IOTA. -->

## 前提条件
<!-- ## Prerequisites -->

IOTA Goクライアントライブラリとその依存関係をダウンロードするには、プロジェクト内の依存関係を管理する[vgoモジュール](https://github.com/golang/go/wiki/Modules)（Go 1.11以降）を使用することをお勧めします。
<!-- To download the IOTA Go client library and its dependencies, we recommend that you use [vgo modules](https://github.com/golang/go/wiki/Modules) -->
<!-- (since Go 1.11) to manage dependencies in your project. -->

## ライブラリをダウンロードする
<!-- ## Download the library -->

IOTA Goクライアントライブラリとその依存関係をダウンロードするには、次の手順に従います。
<!-- To download the IOTA Go client library and its dependencies, do the following: -->

1. $GOPATH以外のディレクトリに、プロジェクト用のディレクトリを作成して初期化します。`&lt;your-module-path&gt;`プレースホルダを`github.com/me/awesome-project`などのプロジェクトのパスに変更します。
  <!-- 1. In any directory outside of $GOPATH, create a directory for your project and initialize it. Change the `<your-module-path>` placeholder to the path of your project such as `github.com/me/awesome-project`. -->

    ```bash
    go mod init <your-module-path>
    ```

2. ライブラリをダウンロードします。
  <!-- 2. Download the library -->

    ```bash
    go get github.com/iotaledger/iota.go/api
    ```

このコマンドは、最新バージョンのIOTA Goクライアントライブラリをダウンロードし、そのバージョンを`go.mod`ファイルに書き込みます。
<!-- This command downloads the latest version of the IOTA Go client library and writes the version into the `go.mod` file. -->

## はじめに
<!-- ## Get started -->

[ライブラリをダウンロード](#ライブラリをダウンロードする)したら、IRIノードに接続してトランザクションを送信したり、台帳と対話したりできます。
<!-- After you've [downloaded the library](#download-the-library), you can connect to an IRI node to send transactions to it and interact with the ledger. -->

1. ローカルIRIノードに接続するには、以下の手順に従います。
  <!-- 1. To connect to a local IRI node, do the following: -->

    ```go
    package main

    import (
    	. "github.com/iotaledger/iota.go/api"
    	"fmt"
    )

    var endpoint = "<node-url>"

    func main() {
    	// compose a new API instance
    	api, err := ComposeAPI(HTTPClientSettings{URI: endpoint})
    	must(err)

    	nodeInfo, err := api.GetNodeInfo()
    	must(err)

    	fmt.Println("latest milestone index:", nodeInfo.LatestMilestoneIndex)
    }

    func must(err error) {
    	if err != nil {
    		panic(err)
    	}
    }
    ```

# APIリファレンス
<!-- ## API reference -->

利用可能なすべてのAPIメソッドの詳細については、[APIフォルダ](https://github.com/iotaledger/iota.go/tree/master/api)を参照してください。
<!-- For details on all available API methods, see the [API folder](https://github.com/iotaledger/iota.go/tree/master/api). -->

## 例
<!-- ## Examples -->

以下の例と同様に、IOTA財団の[例フォルダ](https://github.com/iotaledger/iota.go/tree/master/api/.examples)もご覧ください。
<!-- As well as the following examples, you can take a look at our [examples folder](https://github.com/iotaledger/iota.go/tree/master/api/.examples) for more. -->

### トランザクションを作成してブロードキャストする
<!-- ### Create and broadcast transactions -->

この例では、`PrepareTransfers()`メソッドを呼び出して準備済みバンドルを`SendTrytes()`メソッドにパイプ処理することによって、トランザクションを作成してIRIノードに送信する方法を示します。
<!-- This example shows you how to create and send a transaction to an IRI node by calling the `PrepareTransfers()` method and piping the prepared bundle to the `SendTrytes()` method. -->

```go
package main

import (
	"fmt"
	"github.com/iotaledger/iota.go/address"
	. "github.com/iotaledger/iota.go/api"
	"github.com/iotaledger/iota.go/bundle"
	. "github.com/iotaledger/iota.go/consts"
	"github.com/iotaledger/iota.go/pow"
	"github.com/iotaledger/iota.go/trinary"
)

var endpoint = "<node-url>"

// must be 81 trytes long and truly random
var seed = trinary.Trytes("AAAA....")

// difficulty of the proof of work required to attach a transaction on the tangle
const mwm = 14

// how many milestones back to start the random walk from
const depth = 3

// can be 90 trytes long (with checksum)
const recipientAddress = "BBBB....."

func main() {

	// get the best available PoW implementation
	_, proofOfWorkFunc := pow.GetFastestProofOfWorkImpl()

	// create a new API instance
	api, err := ComposeAPI(HTTPClientSettings{
		URI: endpoint,
		// (!) if no PoWFunc is supplied, then the connected node is requested to do PoW for us
		// via the AttachToTangle() API call.
		LocalProofOfWorkFunc: proofOfWorkFunc,
	})
	must(err)

	// create a transfer to the given recipient address
	// optionally define a message and tag
	transfers := bundle.Transfers{
		{
			Address: recipientAddress,
			Value:   80,
		},
	}

	// create inputs for the transfer
	inputs := []Input{
		{
			Address:  "CCCCC....",
			Security: SecurityLevelMedium,
			KeyIndex: 0,
			Balance:  100,
		},
	}

	// create an address for the remainder.
	// in this case we will have 20 iotas as the remainder, since we spend 100 from our input
	// address and only send 80 to the recipient.
	remainderAddress, err := address.GenerateAddress(seed, 1, SecurityLevelMedium)
	must(err)

	// we don't need to set the security level or timestamp in the options because we supply
	// the input and remainder addresses.
	prepTransferOpts := PrepareTransfersOptions{Inputs: inputs, RemainderAddress: &remainderAddress}

	// prepare the transfer by creating a bundle with the given transfers and inputs.
	// the result are trytes ready for PoW.
	trytes, err := api.PrepareTransfers(seed, transfers, prepTransferOpts)
	must(err)

	// you can decrease your chance of sending to a spent address by checking the address before
	// broadcasting your bundle.
	spent, err := api.WereAddressesSpentFrom(transfers[0].Address)
	must(err)

	if spent[0] {
		fmt.Println("recipient address is spent from, aborting transfer")
		return
	}

	// at this point the bundle trytes are signed.
	// now we need to:
	// 1. select two tips
	// 2. do proof-of-work
	// 3. broadcast the bundle
	// 4. store the bundle
	// SendTrytes() conveniently does the steps above for us.
	bndl, err := api.SendTrytes(trytes, depth, mwm)
	must(err)

	fmt.Println("broadcasted bundle with tail tx hash: ", bundle.TailTransactionHash(bndl))
}

func must(err error) {
	if err != nil {
		panic(err)
	}
}
```

### ネイティブコードとPoW
<!-- ### Native code and PoW -->

ライブラリがCGOを有効にしてコンパイルされている場合、Curlの`transform()`メソッドなどの特定の関数はネイティブCコードを実行して速度を上げます。
<!-- If the library is compiled with CGO enabled, certain functions such as Curl's `transform()` method will -->
<!-- run native C code for increased speed. -->

プログラムのコンパイル中に正しいフラグが渡されると、特定のPoW実装が有効になります。
<!-- Certain PoW implementations are enabled if the correct flags are passed while compiling your program: -->

- `pow_avx`： AVXベースのPoW用
<!-- - `pow_avx` for AVX based PoW -->
- `pow_sse`： SSEベースのPoW用
<!-- - `pow_sse` for SSE based PoW -->
- `pow_c128`： C int128ベースのPoW用
<!-- - `pow_c128` for C int128 based using PoW -->
- `pow_arm_c128`： ARM64 int128 CベースのPoW用
<!-- - `pow_arm_c128` for ARM64 int128 C based PoW -->
- `pow_c`： CベースのPoW用
<!-- - `pow_c` for C based PoW -->

GoでのPoW実装はいつでも利用可能です。
<!-- PoW implementation in Go is always available. -->
ローカルPoWを使用する場合は、必ず`HTTPClientSettings`などのプロバイダ設定で`LocalProofOfWorkFunc`を定義してください。
<!-- If you want to use local PoW, make sure you define `LocalProofOfWorkFunc` in your provider settings such as `HTTPClientSettings`. -->

## プロジェクトを支援する
<!-- ## Support the project -->

皆さんの貢献に感謝します。プルリクエストが受け入れられるようにするには、次の基準を満たす必要があります。
<!-- We thank everyone for their contributions. In order for your pull requests to be accepted, -->
<!-- they must fulfill the following criteria: -->
- Ginkgoで追加のためのテストを書かなければなりません。
<!-- - You must write tests for your additions with Ginkgo -->
- 追加のパラメータと機能を説明するサンプルコードを書く必要があります。
<!-- - You must write example code that desribes the parameters and the functionality of your additions -->
- プルリクエストは継続的インテグレーション設定に合格する必要があります。
<!-- - Your pull request must pass the continuous integration configuration -->

### Ginkgoでテストを書く
<!-- ### Write tests with Ginkgo -->

プルリクエストを出す前に、Ginkgoでコードをテストする必要があります。
<!-- Before your pull requests can be accepted, you must test your code in Ginkgo. -->

1. Ginkgoをダウンロードします。
  <!-- 1. Download Ginkgo -->

    ```bash
    go get github.com/onsi/ginkgo/ginkgo
    go get github.com/onsi/gomega/...
    ```

2. 新しいパッケージを書いた場合は、対応するテストスイートファイルを生成してください。
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

テストファイルを作成したら、次の2つのファイルが作成されます。
<!-- After creating a testing file, you'll have following two files: -->

- `&lt;package-name &rt; _suite_test.go`
- `&lt;package-name &rt;_test.go`

**注釈：** 既存のテストをGinkgoのテストの書き方のリファレンスとして使うことも、[Ginkgoのドキュメント](https://onsi.github.io/ginkgo/)を読むこともできます。
<!-- **Note:** You can use the existing tests as a reference on how to write Ginkgo tests or -->
<!-- you can [read the documentation](https://onsi.github.io/ginkgo/). -->

4. テストを実行する
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

### ドキュメントとサンプルコードを書く
<!-- ### Write documentation and example code -->

godoc.orgはすでに十分なパッケージのドキュメンテーションを提供していますが、IOTA Foundationのドキュメンテーションポータルにはパラメータの説明や例などの追加情報が必要です。
<!-- While godoc.org gives a good enough documentation of the package already, the IOTA Foundation's -->
<!-- documentation portal needs additional information, such as parameter description, examples and so on. -->

1. 存在しない場合は、新しく作成したパッケージに`.examples`ディレクトリを追加します。
  <!-- 1. If non existent, add a `.examples` directory in your newly created package -->

2. 次の規則に従って新しいファイルを作成します。
`.examples`ディレクトリ内に`&lt;package-name&rt; _examples_test.go`
  <!-- 2. Create a new file with the following convention: `<package-name>_examples_test.go` inside -->
  <!-- the `.examples` directory -->

3. 次のスキーマで例を書きます。
  <!-- 3. Write examples in the following schema: -->
    ```go
    // i req: s, The ASCII string to convert to Trytes.
    // o: Trytes, The Trytes representation of the input ASCII string.
    // o: error, Returned for non ASCII string inputs.
    func ExampleASCIIToTrytes() {
        trytes, err := converter.ASCIIToTrytes("IOTA")
        if err != nil {
            // handle error
            return
        }
        fmt.Println(trytes) // output: "SBYBCCKB"
    }
    ```

    | シンボル | 説明 |
    | :------- | :--- |
    | i req    | 関数へのパラメータを記述します。 |
    | i        | 関数に対するオプションのパラメータを記述します。 |
    | o        | 関数の戻り値を記述します。 |

    構文:
    - パラメータ用： `<シンボル>: <パラメータ名>, <説明>.`
    - 戻り値用： `\<シンボル\>: \<型\>, \<説明\>.`
    - 関数例： `Example\<オリジナルの関数名\>`

## ディスカッションに参加する
<!-- ## Join the discussion -->

コミュニティに参加したい場合、セットアップの手助けが必要な場合、ライブラリに関する問題がある場合、あるいはブロックチェーン、分散型台帳、およびIoTについて他の人と話したい場合は、遠慮なく[Discord](https://discordapp.com/invite/fNGZXvh)に参加してください。
<!-- If you want to get involved in the community, need help with getting setup, have any issues related with the library or just want to discuss blockchain, distributed ledgers, and IoT with other people, feel free to join our [Discord](https://discordapp.com/invite/fNGZXvh). -->
