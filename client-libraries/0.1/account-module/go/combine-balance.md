# Go で残高を1つの CDA にまとめる
<!-- # Combine your balance into one CDA in Go -->

**残高の大半をできるだけ少ない条件付きデポジットアドレス（CDA）に保持することをお勧めします．これにより，支払いがより高速になり，必要なトランザクションが少なくなります．このガイドでは，利用可能な残高全体を新しい CDA に移行します．**
<!-- **You may want to keep the majority of your balance on as few conditional deposit addresses (CDA) as possible. This way, making payments is faster and requires fewer transactions. In this guide, you transfer your entire available balance to a new CDA.** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには，以下のパッケージをインストールする必要があります（Go モジュールを使用している場合は，以下のパッケージを参照するだけです）．
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

このガイドでは，[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します．
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. アカウントの利用可能残高を期待する CDA を作成します．
  <!-- 1. Create a CDA that expects your account's available balance -->

    ```go
    // 現在の時刻を取得します
    now, err := timesource.Time()
    handleErr(err)

    now = now.Add(time.Duration(24) * time.Hour)

    // 条件を指定します
    conditions := &deposit.Conditions{TimeoutAt: &now, MultiUse: false, ExpectedAmount: account.AvailableBalance() }

    cda, err := account.AllocateDepositAddress(conditions)
    handleErr(err)
    ```

    :::info:
    アカウントの利用可能残高は，すべての期限切れの CDA の合計残高です．期限切れの CDA に IOTA トークンを送信してはならないため，この残高は取り出しても安全です．

    アカウントの合計残高には，期限切れと同様にアクティブな CDA が含まれています．
    :::
    <!-- :::info: -->
    <!-- You account's available balance is the total balance of all expired CDAs. This balance is safe to withdraw because no one should send IOTA tokens to an expired CDA. -->

    <!-- Your account's total balance includes CDAs that are still active as well as expired. -->
    <!-- ::: -->

2. オラクルを使用して CDA がまだアクティブであることを確認してから，デポジットを送信します．
  <!-- 2. Use the oracle to make sure that the CDA is still active, then send a deposit to it -->

    ```go
    // CDA を送信してもよいかどうかをオラクルに確認します
    ok, rejectionInfo, err := sendOracle.OkToSend(cda)
    handleErr(err)
    if !ok {
        fmt.Println("Won't send transaction: ", rejectionInfo)
        return
    }

    // バンドルを作成し，送信します
    bundle, err := account.Send(cda.AsTransfer())
    handleErr(err)

    fmt.Printf("Sent deposit to %s in the bundle with the following tail transaction hash %s\n", cda.Address, bundle[len(bundle)-1].Hash)
    ```

:::success:
現在，利用可能な合計残高は単一のアドレスにあります．
:::
<!-- :::success: -->
<!-- Now your total available balance is in a single address. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

これらのコードサンプルは [GitHub](https://github.com/iota-community/account-module) でホストされています．
<!-- These code samples are hosted on [GitHub](https://github.com/iota-community/account-module). -->

開始するには，デバイスに [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) がインストールされている必要があります．
<!-- To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device. -->

Go 開発環境がない場合，または Go クライアントライブラリを初めて使用する場合は，[スタートガイド](../../getting-started/go-quickstart.md)を完了してください．
<!-- If you don't have a Go development environment, or if this is your first time using the Go client library, complete our [getting started guide](../../getting-started/go-quickstart.md). -->

コマンドラインで，次を実行します．
<!-- In the command-line, do the following: -->

```bash
git clone https://github.com/iota-community/account-module.git
cd account-module/go/account-module
go mod download
go run combine-balance/combine-balance.go
```
デポジットが送信されたことがわかります．
<!-- You should see that the deposit was sent. -->

シードステートには，確定されるまでこのペンディング中のバンドルが含まれます．
<!-- Your seed state will contain this pending bundle until it is confirmed. -->

## 次のステップ
<!-- ## Next steps -->

[シードステートをエクスポートして，バックアップするか，別のデバイスにインポートする](../go/export-seed-state.md)．
<!-- [Try exporting your seed state so you back it up or import it onto another device](../go/export-seed-state.md). -->
