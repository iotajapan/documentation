# Go でアカウントで支払いを行う
<!-- # Make payments with your account in Go -->

**このガイドでは、アカウントを使用して IOTA トークンを事前定義された CDA にデポジットします。**
<!-- **In this guide, you use your account to deposit IOTA tokens into a pre-defined CDA.** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには、以下のパッケージをインストールする必要があります（Go モジュールを使用している場合は、以下のパッケージを参照するだけです）。
<!-- To complete this guide, you need to install the following packages (if you're using Go modules, you just need to reference them): -->

```bash
go get github.com/iotaledger/iota.go/account/builder
go get github.com/iotaledger/iota.go/account/deposit
go get github.com/iotaledger/iota.go/account/oracle
go get github.com/iotaledger/iota.go/account/oracle/time
go get github.com/iotaledger/iota.go/account/store/badger
go get github.com/iotaledger/iota.go/account/timesrc
go get github.com/iotaledger/iota.go/api
```

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## 手順1. オラクルを作成する
<!-- ## Step 1. Create an oracle -->

CDA は、バンドルが作成、送信、および確定されるまでに有効期限が切れる場合があります。そのため、条件に応じて、CDA にデポジットするかどうかを決定する必要があります。この意思決定プロセスを自動化するために、デポジットするかどうかの決定（true または false）を返す[オラクル](https://github.com/iotaledger/iota.go/tree/master/account/oracle)を作成できます。
<!-- A CDA may expire during the time it takes for a bundle to be created, sent, and confirmed. So, you need to make a decision about whether to deposit into a CDA, depending on its conditions. To automate this decision-making process, you can create an [oracle](https://github.com/iotaledger/iota.go/tree/master/account/oracle) that returns a decision (true or false) about whether to deposit into it. -->

オラクルは、オラクルソースを引数として受け取り、オラクルソースが CDA にデポジットしても安全だと判断した場合、`true` を返します。
<!-- Oracles take an oracle source as an argument and return `true` if the oracle source decides that it's safe to deposit into the CDA. -->

1. [`TimeDecider` オラクルソース](https://github.com/iotaledger/iota.go/tree/master/account/oracle/time)を使用して、CDA の有効期限が少なくとも30分離れているかどうかを確認します。これらの30分は、バンドルを送信して確認する時間を与えます。
  <!-- 1. Use the [`TimeDecider` oracle source](https://github.com/iotaledger/iota.go/tree/master/account/oracle/time) to check if the CDA's expiration time is at least 30 minutes away. These 30 minutes give the bundle time to be sent and confirmed. -->

    ```go
    threshold := time.Duration(30)*time.Minute
    // timeDecider はオラクルソースです
    timeDecider := oracle_time.NewTimeDecider(timesource, threshold)
    // Create a new SendOracle with the given OracleSources
    // 指定されたオラクルソースで新しい SendOracle を作成します
    sendOracle := oracle.New(timeDecider)
    ```

2. `ParseMagnetLink()` メソッドを使用して、事前定義されたマグネットリンクを CDA にデシリアライズします。
  <!-- 2. Use the `ParseMagnetLink()` method to deserialize the predefined magnet link into a CDA -->

    ```go
    magnetLink := "iota://BWNYWGULIIAVRYOOFWZTSDFXFPRCFF9YEHGVBOORLGCPCJSKTHU9OKESUGZGWZXZZDLESFPPTGEHVKTTXG9BQLSIGP/?timeout_at=5174418337&multi_use=1&expected_amount=0"

    cda, err := deposit.ParseMagnetLink(cda)
    handleErr(err)
    ```

    :::info:
    指定されたマグネットリンクは、100年以上で期限切れになる CDA の例です。別の CDA に支払いを行う場合は、代わりにその CDA を使用してください。
    :::
    <!-- :::info: -->
    <!-- The given magent link is for an example CDA that expires in over 100 years. If you want to make a payment to a different CDA, use that one instead. -->
    <!-- ::: -->

3. オラクルを呼び出すには、CDA を `sendOracle` オブジェクトの `OkToSend()` メソッドに渡します。
  <!-- 3. To call the oracle, pass the CDA to the `OkToSend()` method of the `sendOracle` object -->

    ```go
    ok, rejectionInfo, err := sendOracle.OkToSend(cda)
    handleErr(err)
    if !ok {
        fmt.Println("Won't send transaction: ", rejectionInfo)
        return
    }
    ```

## 手順2. 支払いを行う
<!-- ## Step 2. Make a payment -->

支払いを行うには、アカウントに IOTA トークンを含む1つ以上の CDA が必要です。
<!-- To make a payment, your account needs to have one or more CDAs that contains IOTA tokens. -->

1. IOTA トークンを含む CDA がない場合は、[このガイド](../go/generate-cda.md)に従ってください。
<!-- 1. If you dont have a CDA that contains IOTA tokens, follow [this guide](../go/generate-cda.md) -->

2. オラクルを使用して CDA がまだアクティブであることを確認してから、デポジットを送信します。
  <!-- 2. Use the oracle to make sure that the CDA is still active, then send a deposit to it -->

    ```go
    // CDA に送信してもよいかどうかをオラクルに確認します
    ok, rejectionInfo, err := sendOracle.OkToSend(cda)
    handleErr(err)
    if !ok {
        fmt.Println("Won't send transaction: ", rejectionInfo)
        return
    }

    // バンドルを作成し、送信します
    bundle, err := account.Send(cda.AsTransfer())
    handleErr(err)

    fmt.Printf("Sent deposit to %s in the bundle with the following tail transaction hash %s\n", cda.Address, bundle[len(bundle)-1].Hash)
    ```

    出力に次のようなものが表示されるはずです。
    <!-- You should see something like the following in the output: -->

    ```bash
    Sent deposit to DL9CSYICJVKQRUTWBFUCZJQZ9WNBSRJOA9MGOISQZGGHOCZTXVSKDIZN9HBORNGDWRBBAFTKXGEJIAHKDTMAUX9ILA in the bundle with the following tail transaction hash WZEATTRJYENRALJTWPVGDQZHETIDJXPUROUM9BBPS9RJEELDMU9YNZFBSDGPQHZHMXBVCKITSMDEEQ999
    ```

アカウントは、確定されるまでバンドルを再アタッチしてプロモートします。
<!-- Your account will reattach and promote your bundle until it's confirmed. -->

## コードを実行する
<!-- ## Run the code -->

これらのコードサンプルは [GitHub](https://github.com/iota-community/account-module) でホストされています。
<!-- These code samples are hosted on [GitHub](https://github.com/iota-community/account-module). -->

開始するには、デバイスに [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) がインストールされている必要があります。
<!-- To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device. -->

Go 開発環境がない場合、または Go クライアントライブラリを初めて使用する場合は、[スタートガイド](../../getting-started/go-quickstart.md)を完了してください。
<!-- If you don't have a Go development environment, or if this is your first time using the Go client library, complete our [getting started guide](../../getting-started/go-quickstart.md). -->

コマンドラインで、次を実行します。
<!-- In the command-line, do the following: -->

```bash
git clone https://github.com/iota-community/account-module.git
cd account-module/go/account-module
go mod download
go run make-payment/make-payment.go
```

デポジットが送信されたことがわかります。
<!-- You should see that the deposit was sent. -->

シードステートには、確定されるまでこのペンディング中のバンドルが含まれます。
<!-- Your seed state will contain this pending bundle until it is confirmed. -->

## 次のステップ
<!-- ## Next steps -->

[シードステートをエクスポートして、バックアップするか、別のデバイスにインポートする](../go/export-seed-state.md)。
<!-- [Try exporting your seed state so you back it up or import it onto another device](../go/export-seed-state.md). -->
