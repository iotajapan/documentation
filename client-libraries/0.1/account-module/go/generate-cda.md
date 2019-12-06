# Generate a conditional deposit address

**In this guide, you generate a conditional deposit address, serialize it into a magnet link, and send test IOTA tokens to it.**

## Packages

To complete this guide, you need the following packages (if you're using Go modules, you just need to reference these packages):

```bash
go get github.com/iotaledger/iota.go/account/builder
go get github.com/iotaledger/iota.go/account/deposit
go get github.com/iotaledger/iota.go/account/store/badger
go get github.com/iotaledger/iota.go/account/timesrc
go get github.com/iotaledger/iota.go/api
go get github.com/iotaledger/iota.go/trinary
```

## IOTA network

In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet).

## Code walkthrough

1. [Plan the conditions](../introduction/overview.md#advice-for-creating-cdas) you would like your conditional deposit address to have

2. Create a new CDA. This one expires tomorrow.

    ```go
    // Get the current time
	now, err := timesource.Time()
	handleErr(err)

	// Define the same time tomorrow
	now = now.Add(time.Duration(24) * time.Hour)

	// Specify the conditions
	conditions := &deposit.Conditions{TimeoutAt: &now, MultiUse: true}

	// Generate the CDA
	cda, err := account.AllocateDepositAddress(conditions)
	handleErr(err)

	fmt.Println(cda.AsMagnetLink())
    ```

3. Use the `AsMagnetLink()` method to serialize the CDA into a magnet link and print it to the console

    ```go
    fmt.Println(cda.AsMagnetLink())
    ```

    :::info:
    The last 9 trytes of a CDA are the checksum, which is a hash of the address and all of its conditions.
    :::

4. Copy and paste your address into the [Devnet faucet](https://faucet.devnet.iota.org), then wait for the tokens to be transferred to your address

    :::info:
    Make sure to remove the checksum before requesting IOTA tokens from the Devnet faucet.
    :::

    For example:

    ```bash
    DL9CSYICJVKQRUTWBFUCZJQZ9WNBSRJOA9MGOISQZGGHOCZTXVSKDIZN9HBORNGDWRBBAFTKXGEJIAHKD
    ```

## Run the code

To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device.

If you don't have a Go development environment, or if this is your first time using the Go client library, complete our [getting started guide](../../getting-started/go-quickstart.md).

In the command-line, do the following:

```bash
git clone https://github.com/JakeSCahill/getting-started-go-accounts.git
cd getting-started-go-accounts
go mod download
go run generate-cda/generate-cda.go
```
You should see the magnet link in the console.

```bash
iota://DL9CSYICJVKQRUTWBFUCZJQZ9WNBSRJOA9MGOISQZGGHOCZTXVSKDIZN9HBORNGDWRBBAFTKXGEJIAHKDJUYJJCFHC/?timeout_at=1574514007&multi_use=1&expected_amount=0
```

You can copy this magnet link and send it to someone else so they can deposit IOTA tokens into it.

## Next steps

[Start making payments with your account](../go/make-payment.md).