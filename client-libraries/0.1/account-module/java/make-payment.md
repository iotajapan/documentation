# Make payments with your account in Java

**In this guide, you use your account to deposit IOTA tokens into a pre-defined CDA.**

## IOTA network

In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet).

## Code walkthrough

1. Use the `ParseMagnetLink()` method to deserialize the predefined magnet link into a CDA

    ```java
    String magnet = "iota://BWNYWGULIIAVRYOOFWZTSDFXFPRCFF9YEHGVBOORLGCPCJSKTHU9OKESUGZGWZXZZDLESFPPTGEHVKTTXG9BQLSIGP/?timeout_at=5174418337&multi_use=1&expected_amount=0";

    ConditionalDepositAddress cda = DepositFactory.get().parse(magnet, MagnetMethod.class);
    ```

    :::info:
    The given magent link is for an example CDA that expires in over 100 years.
    If you want to make a payment to a different CDA, use that one instead.
    :::


2. If you dont have a CDA that contains IOTA tokens, follow [this guide](../java/generate-cda.md)

3. After making sure that the CDA is still active, send a deposit to it

    ```java
    Bundle bundle = account.send(
        cda.getDepositAddress().getHashCheckSum(),
        cda.getRequest().getExpectedAmount(),
        Optional.of("Thanks for the pizza"),
        Optional.of("ACCOUNTMODULETEST")).get();
    System.out.printf("Sent deposit to %s in the bundle with the following tail transaction hash %s\n",
    bundle.getTransactions().get(bundle.getLength() - 1).getAddress(), bundle.getTransactions().get(bundle.getLength() - 1).getHash());
    ```

    You should see something like the following in the output:

    ```
    Sent deposit to DL9CSYICJVKQRUTWBFUCZJQZ9WNBSRJOA9MGOISQZGGHOCZTXVSKDIZN9HBORNGDWRBBAFTKXGEJIAHKDTMAUX9ILA in the bundle with the following tail transaction hash WZEATTRJYENRALJTWPVGDQZHETIDJXPUROUM9BBPS9RJEELDMU9YNZFBSDGPQHZHMXBVCKITSMDEEQ999
    ```

Your account will reattach and promote your bundle until it's confirmed.

## Run the code

To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device.

You also need a Java development environment that uses the [Maven](https://maven.apache.org/download.cgi) build tool. If this is your first time using the Java client library, complete our [getting started guide](../../getting-started/java-quickstart.md), and follow the instructions for installing the library with Maven.

In the command-line, do the following:

--------------------
### Linux and macOS
```bash
git clone https://github.com/JakeSCahill/iota-samples.git
cd iota-samples/java/account-module
mvn clean install
mvn exec:java -Dexec.mainClass="com.iota.MakePayment"
```
---
### Windows
```bash
git clone https://github.com/JakeSCahill/iota-samples.git
cd iota-samples/java/account-module
mvn clean install
mvn exec:java -D"exec.mainClass"="com.iota.MakePayment"
```
--------------------

You should see that the deposit was sent.

Your seed state will contain this pending bundle until it is confirmed.

## Next steps

[Try exporting your seed state so you back it up or import it onto another device](../java/export-seed-state.md).
