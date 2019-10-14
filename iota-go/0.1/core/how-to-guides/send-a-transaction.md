###トランザクションを作成してブロードキャストする
<!-- ### Create and broadcast transactions -->

この例では、`PrepareTransfers()`メソッドを呼び出し、準備されたバンドルを`SendTrytes()`メソッドにパイプすることにより、トランザクションを作成してIRIノードに送信する方法を示します。
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

// 81トライトの長さで、本当にランダムでなければなりません。
var seed = trinary.Trytes("AAAA....")

// タングルにトランザクションを添付するために必要なプルーフオブワークの困難さ
const mwm = 14

// ランダムウォークを開始するためにマイルストーンを戻る数
const depth = 3

// 90トライト（チェックサム付き）長にできます。
const recipientAddress = "BBBB....."

func main() {

	//利用可能な最高のPoW実装を取得します。
	_, proofOfWorkFunc := pow.GetFastestProofOfWorkImpl()

	// 新しいAPIインスタンスを作成します。
	api, err := ComposeAPI(HTTPClientSettings{
		URI: endpoint,
		//（！）PoWFuncが提供されない場合、接続されたノードは
		// AttachToTangle() API呼び出しを介してPoWを実行するように要求されます。
		LocalProofOfWorkFunc: proofOfWorkFunc,
	})
	must(err)

	// 指定された受信者アドレスへの転送を作成します。
	// オプションでメッセージとタグを定義します。
	transfers := bundle.Transfers{
		{
			Address: recipientAddress,
			Value:   80,
		},
	}

	// 転送の入力を作成します。
	inputs := []Input{
		{
			Address:  "CCCCC....",
			Security: SecurityLevelMedium,
			KeyIndex: 0,
			Balance:  100,
		},
	}

	// リマインダーアドレスを作成します。
	// この場合、入力アドレスから100を消費し、
	// 80のみを受信者に送信するため、残りは20のiotasになります。
	remainderAddress, err := address.GenerateAddress(seed, 1, SecurityLevelMedium)
	must(err)

	// 入力アドレスおよびリマインダーアドレスを提供するため、
	// オプションでセキュリティレベルまたはタイムスタンプを設定する必要はありません。
	prepTransferOpts := PrepareTransfersOptions{Inputs: inputs, RemainderAddress: &remainderAddress}

	// 指定された転送と入力でバンドルを作成して、転送を準備します。
	// その結果、PoWの準備が整ったトライトになります。
	trytes, err := api.PrepareTransfers(seed, transfers, prepTransferOpts)
	must(err)

	// バンドルをブロードキャストする前にアドレスを確認することで、
	// 使用済みのアドレスに送信する可能性を減らすことができます。
	spent, err := api.WereAddressesSpentFrom(transfers[0].Address)
	must(err)

	if spent[0] {
		fmt.Println("recipient address is spent from, aborting transfer")
		return
	}

	// この時点で、バンドルトライトが署名されます。
	// ここで必要なこと：
	// 1. 2つのチップを選択します。
	// 2. プルーフオブワークを行います。
	// 3. バンドルをブロードキャストします。
	// 4. バンドルを保存します。
	// SendTrytes()は便利に上記の手順を実行します。
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
