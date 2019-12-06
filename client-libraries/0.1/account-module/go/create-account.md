# Create an account in Go

**In this guide, you create an account to keep track of your seed state in a local database and print your account's balance to the console.**

## Packages

To complete this guide, you need to install the following packages (if you're using Go modules, you just need to reference these packages):

```bash
go get github.com/iotaledger/iota.go/api
go get github.com/iotaledger/iota.go/badger
go get github.com/iotaledger/iota.go/builder
go get github.com/iotaledger/iota.go/timesrc
go get github.com/iotaledger/iota.go/trinary
```

## IOTA network

In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet).

## Code walkthrough

1\. Create a new seed and back it up

--------------------
### Linux
```bash
cat /dev/urandom |tr -dc A-Z9|head -c${1:-81}
```
---
### macOS
```bash
cat /dev/urandom |LC_ALL=C tr -dc 'A-Z9' | fold -w 81 | head -n 1
```
---
### Windows PowerShell
```bash
$b=[byte[]] (1..81);(new-object Security.Cryptography.RNGCryptoServiceProvider).GetBytes($b);-join($b|%{[char[]] (65..90+57..57)[$_%27]})
```
--------------------

2\. Define the seed that your account will use

```go
var seed = trinary.Trytes("PUEOTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX")
```

:::info:
If you want to use a seed from a particular location, for example a hardware wallet, create a custom `SeedProvider` object, and pass it to the `WithSeedProvider()` method.
:::

3\. Connect to a node
   
```go
apiSettings := api.HTTPClientSettings{URI: "https://nodes.devnet.iota.org:443"}
iotaAPI, err := api.ComposeAPI(apiSettings)
handleErr(err)
```

4\. Create a storage object to which the account can save the seed state. In this example, the seed state is stored in a BadgerDB database. Change `db` to the path where you want to save the database directory.

```go
store, err := badger.NewBadgerStore("db")
handleErr(err)

// Make sure the database closes when the code stops
defer store.Close()
```

5\. Create a `timesource` object that returns an accurate time, which the account will use to decide if your CDAs are still active. In this example, the time source is a [Google NTP (network time protocol) server](https://developers.google.com/time/faq).

```go
timesource := timesrc.NewNTPTimeSource("time.google.com")
```

6\. Build your account. If you don't specify any custom settings, the account uses the [defaults](https://github.com/iotaledger/iota.go/blob/master/.docs/iota.go/reference/account_default_settings.md).

```go
account, err := builder.NewBuilder().
    // Connect to a node
    WithAPI(iotaAPI).
    // Create the database
    WithStore(store).
    WithSeed(seed).
    // Set the minimum weight magnitude for the Devnet
    WithMWM(9).
    WithTimeSource(timesource).
    // Load the default plugins that enhance the functionality of the account
    WithDefaultPlugins().
    Build()
handleErr(err)
```

The default plugins include the `transfer-poller` and the `promoter-reattacher` plugins.

Every 30 seconds, the `transfer-poller` plugin checks whether withdrawals have been confirmed or whether any deposits are pending.

The `promoter-reattacher` plugin [promotes or reattaches](root://getting-started/0.1/transactions/reattach-rebroadcast-promote.md) any pending withdrawal transactions that the `transfer-poller` finds.

:::info:
You can customize the behavior of these plugins or build your own.
:::

7\. Start the account and the plugins

```go
handleErr(account.Start())

defer account.Shutdown()
```

8\. Check your account's balance

```go
balance, err := account.AvailableBalance()
handleErr(err)
fmt.Println("Total available balance: ")
fmt.Println(balance)
```

You should see your balance.

:::success:Congratulations! :tada:
You've created an account that will automatically promote and reattach transactions as well as manage the state of your seed.
:::

## Run the code

To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device.

If you don't have a Go development environment, or if this is your first time using the Go client library, complete our [getting started guide](../../getting-started/go-quickstart.md).

In the command-line, do the following:

```bash
git clone https://github.com/JakeSCahill/getting-started-go-accounts.git
cd getting-started-go-accounts
go mod download
go run create-account/create-account.go
```
You should see the balance of your new account.

You'll also have a database file that keeps track of your seed state.

## Next steps

After certain events happen in your account, it emits them, and allows you to listen for them. For example, you may want to monitor your account for new payments. To do so, you need to [create an event listener](../go/listen-to-events.md).

Or, you can [create a plugin](../go/create-plugin.md) that also emits events.
