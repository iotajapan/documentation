# アカウントに既存のシードステートをインポートする
<!-- # Import an existing seed state into an account -->

**既存のシードステートを他の端末のアカウントにインポートすることをお勧めします。そのためには、まずシードステートをエクスポートして正しい形式にする必要があります。**
<!-- **You may want to import an existing seed state into an account on another device. To do so, you first need to export the seed state so that it's in the correct format.** -->

:::danger:重要
複数のデバイスで同じシードステートを更新しないでください。複数のデバイスがシードステートを変更すると、そのステートは一貫性がなくなり、署名済みアドレスからの取り出しが発生する可能性があります。
:::
<!-- :::danger:Important -->
<!-- Never update the same seed state on multiple devices. If more than one device changes the seed state, the state is no longer consistent and may lead to withdrawals from spent addresses. -->
<!-- ::: -->

## 前提条件
<!-- ## Prerequisites -->

[アカウントを作成します](../how-to-guides/create-account.md)。
<!-- [Create an account](../how-to-guides/create-account.md). -->

## シードステートをエクスポートしてインポートする
<!-- ## Export then import your seed state -->

別のデバイスでシードステートを使用するには、新しいデバイスにインポートする前にシードステートをエクスポートして保存する必要があります。
<!-- To use your seed state on another device, you need to export and store it before importing it onto the new device. -->

1. アカウントのIDを`store.ExportAccount()`メソッドに渡してシードステートをエクスポートします。
  <!-- 1. Export your seed state by passing your account's ID to the `store.ExportAccount()` method -->

    ```go
    ID := account.ID()

    acc, err := store.ExportAccount(ID)
    handleErr(err)

    fmt.Println(acc)
    ```

2. シードステートをJSONなどのデータ構造にシリアル化します。
  <!-- 2. Serialize your seed state into a data structure such as JSON -->

    ```go
    jsonacc, err := json.Marshal(acc)
    handleErr(err)
    ```

    :::info:
    ここでは、エクスポートされたシードステートを変数に格納します。ただし、後で読み込むことができるように、シードステートをファイルに保存することをお勧めします。
    :::
    <!-- :::info: -->
    <!-- Here, we store the exported seed state in a variable. But, we recommend that you save the seed state in a file so that you can later read from it. -->
    <!-- ::: -->

3. JSONシードステートを`ExportedAccountState`型に逆シリアル化します。
  <!-- 3. Deserialize your JSON seed state into an `ExportedAccountState` type -->

    ```go
    a := Store.ExportedAccountState{}
    err = json.Unmarshal(jsonacc, &a)
    handleErr(err)
    ```

4. `exportedAccountState`構造体を`store.ImportAccount()`メソッドに渡すことでシードステートをアカウントのデータベースにインポートします。
  <!-- 4. Import your seed state into your account's database by passing the `ExportedAccountState` struct to the `store.ImportAccount()` method -->

    ```go
    store.ImportAccount(a)
    ```

    :::warning:
    シードステートをインポートすると、アカウントのデータベース内の既存のシードステートが上書きされます。
    :::
    <!-- :::warning: -->
    <!-- When you import a seed state, you overwrite any existing seed state in your account's database. -->
    <!-- ::: -->
