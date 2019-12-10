# Make payments with your account in Go

**In this guide, you use your account to deposit IOTA tokens into a pre-defined CDA.**

## Packages

To complete this guide, you need to install the following packages (if you're using Go modules, you just need to reference these packages):

```bash
go get github.com/iotaledger/iota.go/account/builder
go get github.com/iotaledger/iota.go/account/deposit
go get github.com/iotaledger/iota.go/account/oracle
go get github.com/iotaledger/iota.go/account/oracle/time
go get github.com/iotaledger/iota.go/account/store/badger
go get github.com/iotaledger/iota.go/account/timesrc
go get github.com/iotaledger/iota.go/api
```

## IOTA network

In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet).

## Step 1. Create an oracle

A CDA may expire during the time it takes for a bundle to be created, sent, and confirmed. So, you need to make a decision about whether to deposit into a CDA, depending on its conditions. To automate this decision-making process, you can create an [oracle](https://github.com/iotaledger/iota.go/tree/master/account/oracle) that returns a decision (true or false) about whether to deposit into it.

Oracles take an oracle source as an argument and return `true` if the oracle source decides that it's safe to deposit into the CDA.

1. Use the [`TimeDecider` oracle source](https://github.com/iotaledger/iota.go/tree/master/account/oracle/time) to check if the CDA's expiration time is at least 30 minutes away. These 30 minutes give the bundle time to be sent and confirmed.

    ```go
    threshold := time.Duration(30)*time.Minute
    // timeDecider is an OracleSource
    timeDecider := oracle_time.NewTimeDecider(timesource, threshold)
    // Create a new SendOracle with the given OracleSources
    sendOracle := oracle.New(timeDecider)
    ```

2. Use the `ParseMagnetLink()` method to deserialize the predefined magnet link into a CDA

    ```go
    magnetLink := "iota://BWNYWGULIIAVRYOOFWZTSDFXFPRCFF9YEHGVBOORLGCPCJSKTHU9OKESUGZGWZXZZDLESFPPTGEHVKTTXG9BQLSIGP/?timeout_at=5174418337&multi_use=1&expected_amount=0"

    cda, err := deposit.ParseMagnetLink(cda)
    handleErr(err)
    ```

    :::info:
    The given magent link is for an example CDA that expires in over 100 years.
    If you want to make a payment to a different CDA, use that one instead.
    :::

3. To call the oracle, pass the CDA to the `OkToSend()` method of the `sendOracle` object

    ```go
    ok, rejectionInfo, err := sendOracle.OkToSend(cda)
    handleErr(err)
    if !ok {
        fmt.Println("Won't send transaction: ", rejectionInfo)
        return
    }
    ```

## Step 2. Make a payment

To make a payment, your account needs to have one or more CDAs that contains IOTA tokens.

1. If you dont have a CDA that contains IOTA tokens, follow [this guide](../go/generate-cda.md)

2. Use the oracle to make sure that the CDA is still active, then send a deposit to it

    ```go
    // Ask the oracle if the CDA is OK to send to
	ok, rejectionInfo, err := sendOracle.OkToSend(cda)
	handleErr(err)
	if !ok {
		fmt.Println("Won't send transaction: ", rejectionInfo)
		return
	}

	// Create and send the bundle
	bundle, err := account.Send(cda.AsTransfer())
	handleErr(err)

	fmt.Printf("Sent deposit to %s in the bundle with the following tail transaction hash %s\n", cda.Address, bundle[len(bundle)-1].Hash)
    ```

    You should see something like the following in the output:

    ```
    Sent deposit to DL9CSYICJVKQRUTWBFUCZJQZ9WNBSRJOA9MGOISQZGGHOCZTXVSKDIZN9HBORNGDWRBBAFTKXGEJIAHKDTMAUX9ILA in the bundle with the following tail transaction hash WZEATTRJYENRALJTWPVGDQZHETIDJXPUROUM9BBPS9RJEELDMU9YNZFBSDGPQHZHMXBVCKITSMDEEQ999
    ```

Your account will reattach and promote your bundle until it's confirmed.

## Run the code

To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device.

If you don't have a Go development environment, or if this is your first time using the Go client library, complete our [getting started guide](../../getting-started/go-quickstart.md).

In the command-line, do the following:

```bash
git clone https://github.com/JakeSCahill/iota-samples.git
cd iota-samples/go/account-module
go mod download
go run make-payment/make-payment.go
```
You should see that the deposit was sent.

Your seed state will contain this pending bundle until it is confirmed.

## Next steps

[Try exporting your seed state so you back it up or import it onto another device](../go/export-seed-state.md).
