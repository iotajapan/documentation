# Goコアライブラリ入門
<!-- # Get started with the Go core library -->

**Goコアクライアントライブラリには、タングルと対話するための低レベルのメソッドが含まれています。このライブラリを使用して、コアIOTAプロトコルを使用できます。たとえば、ノードに接続し、バンドルを作成し、ペンディング中のトランザクションを促進および再添付できます。**
<!-- **The Go core client library includes low-level methods for interacting with the Tangle. You can use this library to use the core IOTA protocol. For example, you can connect to nodes, create bundles, and promote and reattach pending transactions.** -->

:::warning:ベータソフトウェアです！
クライアントライブラリは2019年現在ベータ版であり、運用環境では使用しないでください。
:::
<!-- :::warning:Beta software -->
<!-- The client libraries are currently in beta and you should not use them in production environments. -->
<!-- ::: -->

## 対象読者
<!-- ## Audience -->

このドキュメントは、Goプログラミング言語とオブジェクト指向プログラミングの概念に精通している開発者向けです。また、[バンドル](root://getting-started/0.1/transactions/bundles.md)、[トランザクション](root://getting-started/0.1/transactions/transactions.md)、[シード](root://getting-started/0.1/clients/seeds.md)や[アドレス](root://getting-started/0.1/clients/addresses.md)などの[基本的なIOTAの概念](root://getting-started/0.1/introduction/overview.md)にも精通している必要があります。
<!-- This documentation is for developers who are familiar with the Go programming language and object-oriented programming concepts. You should also be familiar with [basic IOTA concepts](root://getting-started/0.1/introduction/overview.md) such as [bundles](root://getting-started/0.1/transactions/bundles.md), [transactions](root://getting-started/0.1/transactions/transactions.md), [seeds](root://getting-started/0.1/clients/seeds.md), and [addresses](root://getting-started/0.1/clients/addresses.md). -->

## 前提条件
<!-- ## Prerequisites -->

Goライブラリとその依存関係をダウンロードするには、[vgo modules](https://github.com/golang/go/wiki/Modules)（Go 1.11以降）を使用してプロジェクトの依存関係を管理することをお勧めします。
<!-- To download the Go library and its dependencies, we recommend that you use [vgo modules](https://github.com/golang/go/wiki/Modules) (since Go 1.11) to manage dependencies in your project. -->

<a name="install-the-library"></a>
## ライブラリをインストールする
<!-- ## Install the library -->

Goライブラリとその依存関係をインストールするには、次の手順を実行します。
<!-- To install the Go library and its dependencies, do the following: -->

1. `$GOPATH`以外のディレクトリで、プロジェクトのディレクトリを作成し、初期化します。`<your-module-path>`プレースホルダーを、`github.com/me/awesome-project`などのプロジェクトのパスに変更します。
  <!-- 1. In any directory outside of $GOPATH, create a directory for your project and initialize it. Change the `<your-module-path>` placeholder to the path of your project such as `github.com/me/awesome-project`. -->

    ```bash
    go mod init <your-module-path>
    ```

2. ライブラリをダウンロードします。
  <!-- 2. Download the library -->

    ```bash
    go get github.com/iotaledger/iota.go/api
    ```

このコマンドはIOTA Goクライアントライブラリの最新バージョンをダウンロードし、そのバージョンを`go.mod`ファイルに書き込みます。
<!-- This command downloads the latest version of the IOTA Go client library and writes the version into the `go.mod` file. -->

## 始める
<!-- ## Get started -->

[ライブラリをインストール](#install-the-library)した後、ノードに接続して、タングルに関する情報を要求することができます。
<!-- After you've [installed the library](#install-the-library), you can connect to a node to request information from it about the Tangle. -->

```go
package main

import (
	. "github.com/iotaledger/iota.go/api"
	"fmt"
)

var endpoint = "<node-url>"

func main() {
	// 新しいAPIインスタンスを作成します。
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

## APIリファレンス
<!-- ## API reference -->

利用可能なすべてのAPIメソッドの詳細については、[APIリファレンス](https://github.com/iotaledger/iota.go/tree/master/api)を参照してください。
<!-- For details on all available API methods, see the [API reference](https://github.com/iotaledger/iota.go/tree/master/api). -->

### ネイティブコードとPoW
<!-- ### Native code and PoW -->

ライブラリがCGOを有効にしてコンパイルされている場合、Curlの`transform()`メソッドなどの特定の関数はネイティブCコードを実行して速度を向上させます。
<!-- If the library is compiled with CGO enabled, certain functions such as Curl's `transform()` method will run native C code for increased speed. -->

プログラムのコンパイル中に正しいフラグが渡されると、特定のPoW実装が有効になります。
<!-- Certain PoW implementations are enabled if the correct flags are passed while compiling your program: -->

- AVXベースのPoW用の`pow_avx`
<!-- - `pow_avx` for AVX based PoW -->
- SSEベースのPoWの`pow_sse`
<!-- - `pow_sse` for SSE based PoW -->
- PoWを使用したC int128ベースの`pow_c128`
<!-- - `pow_c128` for C int128 based using PoW -->
- ARM64 int128 CベースのPoW用の`pow_arm_c128`
<!-- - `pow_arm_c128` for ARM64 int128 C based PoW -->
- CベースのPoW用の`pow_c`
<!-- - `pow_c` for C based PoW -->

GoでのPoW実装は常に利用可能です。ローカルPoWを使用する場合は、`HTTPClientSettings`などのプロバイダー設定で`LocalProofOfWorkFunc`を必ず定義してください。
<!-- PoW implementation in Go is always available. -->
<!-- If you want to use local PoW, make sure you define `LocalProofOfWorkFunc` in your provider settings such as `HTTPClientSettings`. -->

## プロジェクトをサポートする
<!-- ## Support the project -->

Goライブラリが役に立ち、貢献したいと思う場合は、[バグレポート](https://github.com/iotaledger/iota.go/issues/new-issue)、機能リクエスト、または[プルリクエスト](https://github.com/iotaledger/iota.go/pulls/)の投稿を検討してください。。
<!-- If the Go library has been useful to you and you feel like contributing, consider posting a [bug report](https://github.com/iotaledger/iota.go/issues/new-issue), feature request or a [pull request](https://github.com/iotaledger/iota.go/pulls/). -->

皆様の貢献に感謝します。 プルリクエストを受け入れるには、次の基準を満たしている必要があります。
<!-- We thank everyone for their contributions. In order for your pull requests to be accepted, they must fulfill the following criteria: -->
- Ginkgoで追加するテストを作成する必要があります。
<!-- - You must write tests for your additions with Ginkgo -->
- 追加するパラメーターと機能を説明するサンプルコードを記述する必要があります。
<!-- - You must write example code that desribes the parameters and the functionality of your additions -->
- プルリクエストは継続的インテグレーション設定に合格する必要があります。
<!-- - Your pull request must pass the continuous integration configuration -->

### Ginkgoでテストを書く
<!-- ### Write tests with Ginkgo -->

プルリクエストが受け入れられる前に、Ginkgoでコードをテストする必要があります。
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

3. テストファイルをを生成します。
  <!-- 3. Generate a new testing file -->

    ```bash
    ginkgo generate <package-name>
    ```

テストファイルを作成すると、次の2つのファイルが作成されます。
<!-- After creating a testing file, you'll have following two files: -->

- `<package-name>_suite_test.go`
- `<package-name>_test.go`

:::info:
Ginkgoテストの作成方法に関するリファレンスとして既存のテストを使用するか、[Ginkgoのドキュメント](https://onsi.github.io/ginkgo/)を読むことができます。
:::
<!-- :::info: -->
<!-- You can use the existing tests as a reference on how to write Ginkgo tests or -->
<!-- you can [read the documentation](https://onsi.github.io/ginkgo/). -->
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

### ドキュメントを更新する
<!-- ### Update documentation -->

変更内容がドキュメントに影響する場合は、更新してください。
<!-- If your changes affect the documentation, please update it. -->

1. 存在しない場合、新しく作成したパッケージに`.examples`ディレクトリを追加します
  <!-- 1. If non existent, add a `.examples` directory in your newly created package -->

2. 次の規則で新しいファイルを作成します：`.examples`ディレクトリ内に`<package-name>_examples_test.go`
  <!-- 2. Create a new file with the following convention: `<package-name>_examples_test.go` inside the `.examples` directory -->

3. 次のスキーマで例を作成します。
  <!-- 3. Write examples in the following schema: -->

    ```go
    // i req: s, トライトに変換するASCII文字列。
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

| **シンボル** | **説明** |
| :------- | :--- |
| i req | 関数のパラメーターを記述します。 |
| i | 関数のオプションのパラメーターを記述します。 |
| o | 関数の戻り値を記述します。 |

<!-- | **Symbol**     | **Description** | -->
<!-- |:---------------|:--------| -->
<!-- | i req | Describes a parameter to the function. | -->
<!-- | i | Describes an optional parameter to the function. | -->
<!-- | o | Describes a return value of the function. | -->

構文：
<!-- Syntax: -->

- パラメーターの場合：`<symbol>:<parameter_name>, <description>.`
- 戻り値の場合：`<symbol>:<type>, <description>.`
- 関数の例：`Example<OriginFunctionName>`
<!-- - For parameters: `<symbol>: <parameter_name>, <description>.` -->
<!-- - For return values: `<symbol>: <type>, <description>.` -->
<!-- - Example function: `Example<OriginFunctionName>` -->

## ディスカッションに参加する
<!-- ## Join the discussion -->

[Discord](https://discord.iota.org)に参加して、コミュニティに参加したり、助けを求めたり、技術について話し合ったりしてください。
<!-- Join our [Discord](https://discord.iota.org) to get involved in the community, ask for help, or to discuss the technology. -->
