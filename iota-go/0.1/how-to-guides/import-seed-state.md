## Import an existing seed state into an account

You may want to import an existing seed state into an account on another device. To do so, you first need to export the seed state so that it's in the correct format.

:::danger:Important
Never update the same seed state on multiple devices. If more than one device changes the seed state, the state is no longer consistent and may lead to withdrawals from spent addresses.
:::

## Prerequisites

This guide assumes that you've followed our [Getting started guide](../README.md) and are using the [Go modules](https://github.com/golang/go/wiki/Modules) to manage dependencies in your project.

## Export then import your seed state

To use your seed state on another device, you need to export, store, then import it.

1. Export your seed state by passing your account's ID to the `store.ExportAccount()` method

    ```go
    ID := account.ID()

	acc, err := store.ExportAccount(ID)
	handleErr(err)

    fmt.Println(acc)
    ```

3. Serialize your seed state into a data structure such as JSON

    ```go
    jsonacc, err := json.Marshal(acc)
    handleErr(err)
    ```

    :::info:
    Here, we store the exported seed state in a variable. But, we recommend that you save the seed state in a file so that you can later read from it.
    :::

4. Deserialize your JSON seed state into an `ExportedAccountState` type

    ```go
    a := Store.ExportedAccountState{}
    err = json.Unmarshal(jsonacc, &a)
	handleErr(err)
    ```

5. Import your seed state into your account's database by passing the `ExportedAccountState` struct to the `store.ImportAccount()` method

    ```go
    store.ImportAccount(a)
    ```

    :::warning:
    When you import a seed state, you overwrite any existing seed state in your account's database.
    :::