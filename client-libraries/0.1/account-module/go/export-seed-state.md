# Go で既存のシードステートをインポート/エクスポートする
<!-- # Import/export an existing seed state in Go -->

**アカウントを使用して支払いを行うと、アカウントはシードステートを更新します。このガイドでは、アカウントのシードステートをエクスポートして、別のデバイスにインポートしたり、単にバックアップしたりできるようにする方法を学習します。**
<!-- **When you use your account to make payments, your account updates your seed state. In this guide, you learn how to export your account's seed state so that you can import it on another device or simply back it up.** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには、以下のパッケージをインストールする必要があります（Go モジュールを使用している場合は、以下のパッケージを参照するだけです）。
<!-- To complete this guide, you need to install the following packages (if you're using Go modules, you just need to reference them): -->

```bash
go get github.com/iotaledger/iota.go/account/store
go get github.com/iotaledger/iota.go/account/builder
go get github.com/iotaledger/iota.go/account/store/badger
go get github.com/iotaledger/iota.go/account/timesrc
go get github.com/iotaledger/iota.go/api
```

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## 手順1. シートステーをエクスポートする
<!-- ## Step 1. Export your seed state -->

1. アカウントIDを `ExportAccount()` メソッドに渡すことでシードステートをエクスポートします。
  <!-- 1. Export your seed state by passing your account's ID to the `ExportAccount()` method -->

    ```go
    ID := account.ID()

	acc, err := store.ExportAccount(ID)
	handleErr(err)

    fmt.Println(acc)
    ```

2. シードステートを保存する JSON ファイルを作成します。
  <!-- 2. Create a JSON file to which to save your seed state -->

    ```go
    f, err := os.OpenFile("exported-seed-state.json", os.O_CREATE, 0755);
    handleErr(err)

    defer f.Close();
    ```

3. シードシードをシリアル化し、ファイルに保存します。
  <!-- 3. Serialize your seed state and save it to the file -->

    ```go
    jsonacc, err := json.Marshal(acc)
    handleErr(err)

    f.Write(jsonacc)
    f.Close()
    ```

    :::info:
    定期的にシードステートをバックアップすることをお勧めします。
    :::
    <!-- :::info: -->
    <!-- It's best practice to back up your seed state at regular intervals. -->
    <!-- ::: -->

:::success:おめでとうございます:tada:
シードステートをエクスポートしました。これで、バックアップしたり、別のデバイスのアカウントにインポートしたりできます。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You've exported your seed state. Now, you can back it up or import it into an account on another device. -->
<!-- ::: -->

## 手順2. シードステートをインポートする
<!-- ## Step 2. Import your seed state -->

1. エクスポートしたシードステートを読み込みます。
  <!-- 1. Read your exported seed state -->

    ```go
    file, err := os.Open("exported-seed-state.json")
    handleErr(err)

    defer file.Close()

    fileinfo, err := file.Stat()
    handleErr(err)

    filesize := fileinfo.Size()
    buffer := make([]byte, filesize)

    jsonSeedState, err := file.Read(buffer)
    handleErr(err)
    ```

2. JSON シードステートを `ExportedAccountState` タイプにデシリアライズします。
  <!-- 2. Deserialize your JSON seed state into an `ExportedAccountState` type -->

    ```go
    a := Store.ExportedAccountState{}
    err = json.Unmarshal(jsonSeedState, &a)
	handleErr(err)
    ```

3. `ExportedAccountState` 構造体を `ImportAccount()` メソッドに渡すことにより、シードステートをアカウントのデータベースにインポートします。
  <!-- 3. Import your seed state into your account's database by passing the `ExportedAccountState` struct to the `ImportAccount()` method -->

    ```go
    store.ImportAccount(a)
    ```

    :::warning:
    シードステートをインポートすると、アカウントのデータベース内の既存のシードステートが上書きされます。
    :::
    <!-- :::warning: -->
    <!-- When you import a seed state, you overwrite any existing seed state in your account's database. -->
    <!-- ::: -->

:::success:おめでとうございます:tada:
シードステートをエクスポートおよびインポートする方法を学習しました。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You've learned how to export and import your seed state. -->
<!-- ::: -->

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
go run export-account/export-account.go
```

シードステートを含む `exported-seed-state.json` ファイルが必要です。このファイルを使用して、シードステートを別のデバイスにインポートできます。
<!-- You should have an `exported-seed-state.json` file that contains your seed state. You can use this file to import your seed state on another device. -->

## 次のステップ
<!-- ## Next steps -->

[APIリファレンス](https://github.com/iotaledger/iota.go/tree/master/.docs/iota.go/reference)を参照し、アカウントモジュールについて引き続き学習する。
<!-- Take a look at the [API reference](https://github.com/iotaledger/iota.go/tree/master/.docs/iota.go/reference) to continue learning about the account module. -->
