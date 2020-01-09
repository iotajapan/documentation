# Make payments with your account in JavaScript

**In this guide, you use your account to deposit IOTA tokens into a pre-defined CDA.**

## Packages

To complete this guide, you need to install the following packages:

--------------------
### npm
```bash
npm install @iota/account @iota/cda @iota/transaction-converter ntp-client
```
---
### Yarn
```bash
yarn add @iota/account @iota/cda @iota/transaction-converter ntp-client
```
--------------------

## IOTA network

In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet).

## Code walkthrough

To make a payment, your account needs to have one or more CDAs that contains IOTA tokens.

1. If you dont have a CDA that contains IOTA tokens, follow this guide to [generate a CDA](../js/generate-cda.md) and send test IOTA tokens to it

2. Use the `parseCDAMagnet()` method to deserialize the magnet link into a CDA

    ```js
    const magnetLink = "iota://BWNYWGULIIAVRYOOFWZTSDFXFPRCFF9YEHGVBOORLGCPCJSKTHU9OKESUGZGWZXZZDLESFPPTGEHVKTTXG9BQLSIGP/?timeout_at=5174418337&multi_use=1&expected_amount=0";

    const cda = CDA.parseCDAMagnet(
        magnetLink
    );
    ```

    :::info:
    The given magent link is for an example CDA that expires in over 100 years.
    If you want to make a payment to a different CDA, use that one instead.
    :::

3. Make sure that the CDA is still active

    ```js
    // Get the current time to use to compare to the CDA's timeout
    ntpClient.getNetworkTime("time.google.com", 123, function(err, date) {
        if(err) {
            console.error(err);
            return;
        } else if (!(CDA.isAlive(date, cda))) {
            isActive = false
        }
    });
    ```

3. Send a deposit to the CDA

    ```js
    // Send the payment only if the CDA is active
    if (isActive) {
        account.sendToCDA({
            ...cda,
            value: 1000
        })
        .then((trytes) => {
            // Get the tail transaction and convert it to an object
            let bundle = TransactionConverter.asTransactionObject(trytes[trytes.length - 1]);
            let bundleHash = bundle.bundle;
            let address = bundle.address
            let value = bundle.value;
            console.log(`Sent ${value} IOTA tokens to ${address} in bundle:  ${bundleHash}`);
        })
        .catch(error => {
            console.log(error);
            // Close the database and stop any ongoing reattachments
            account.stop();
        });

    } else {
        console.log('CDA is expired. Use an active CDA.');
        // Close the database and stop any ongoing reattachments
        account.stop();
        return;
    }
    ```

    You should see that how many IOTA tokens were sent to your address as well as the bundle hash:

    ```
    Sent 1000 to TIZJIRDCZPRJMMVKSGROPKE9VGIQKOLOUSX9MCUTOEQBBHPMLYBVKBPCXJKY9SDWX9FVMOZTWNMVVEYKX in bundle:  RXIA9CBEOASNY9IRIARZFGDLK9YNGW9ZHJGJLUXOUKVGCZLPNDKALFHZWHZKQQXFTIHEIJJPN9EURO9K9
    ```

Your account will reattach and promote your bundle until it's confirmed.

You can stop the reattachment routine by calling the `stopAttaching()` method.

To restart the reattachment routine, call the `startAttaching()` method with your network settings.

```js
account.stopAttaching();

account.startAttaching({
    depth: 3,
    minWeightMagnitude: 9,
    delay: 30 * 1000
    maxDepth: 6
});
```

## Run the code

These code samples are hosted on [GitHub](https://github.com/iota-community/account-module).

To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device.

If you don't have a JavaScript development environment, or if this is your first time using the JavaScript client library, complete our [getting started guide](../../getting-started/js-quickstart.md).

In the command-line, do the following:

```bash
git clone https://github.com/iota-community/account-module.git
cd account-module/js/account-module
npm i
node make-payment/make-payment.js
```

You should see that the deposit was sent.

Your seed state will contain this pending bundle until it is confirmed.

## Next steps

[Try exporting your seed state so you back it up or import it onto another device](../js/export-seed-state.md).
