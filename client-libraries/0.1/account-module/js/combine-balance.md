# Combine your balance in one CDA in JavaScript

**You may want to keep the majority of your balance on as few CDAs as possible. This way, making payments is faster and requires fewer transactions. In this guide, you transfer your entire available balance to a new CDA.**

## Packages

To complete this guide, you need to install the following packages:

--------------------
### npm
```bash
npm install @iota/account @iota/transaction-converter ntp-client
```
---
### Yarn
```bash
yarn add @iota/account @iota/transaction-converter ntp-client
```
--------------------

## IOTA network

In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet).

## Code walkthrough

1. Create a CDA that expects your account's available balance

    ```js
    account.getAvailableBalance()
    .then(balance => {
        const cda = account.generateCDA({
                timeoutAt: Date.now() + 24 * 60 * 60 * 1000,
                expectedAmount: balance
        })
    ```

    :::info:
    Available balance is the total balance of all expired CDAs, which are safe to withdraw from.

    Your account's total balance includes CDAs that are still active and so cannot be withdrawn from.
    :::

2. Send a deposit to your CDA

    ```js
        .then(cda => {
                    account.sendToCDA({
                    ...cda,
                    value: balance
                })
                .then(trytes => {
                    
                    console.log(trytes)
                    // Get the tail transaction and convert it to an object
                    let bundle = TransactionConverter.asTransactionObject(trytes[0]);
                    let bundleHash = bundle.bundle;
                    let address = bundle.address
                    let value = bundle.value;
                    console.log(`Sent ${value} IOTA tokens to ${address} in bundle:  ${bundleHash}`);
                })
            })
        }).catch(error => {
        console.log(error);
        // Close the database and stop any ongoing reattachments
        account.stop();
    });
    ```

    You should see that how many IOTA tokens were sent to your address as well as the bundle hash:

    ```
    Sent 1000 to TIZJIRDCZPRJMMVKSGROPKE9VGIQKOLOUSX9MCUTOEQBBHPMLYBVKBPCXJKY9SDWX9FVMOZTWNMVVEYKX in bundle:  RXIA9CBEOASNY9IRIARZFGDLK9YNGW9ZHJGJLUXOUKVGCZLPNDKALFHZWHZKQQXFTIHEIJJPN9EURO9K9
    ```

## Run the code

To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device.

If you don't have a JavaScript development environment, or if this is your first time using the JavaScript client library, complete our [getting started guide](../../getting-started/js-quickstart.md).

In the command-line, do the following:

```bash
git clone https://github.com/JakeSCahill/iota-samples.git
cd iota-samples/js/account-module
npm i
node combine-balance/combine-balance.js
```

You should see that the deposit was sent.

Your seed state will contain this pending bundle until its tail transaction is confirmed.

## Next steps

[Try exporting your seed state so you back it up or import it onto another device](../js/export-seed-state.md).