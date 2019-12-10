# Generate a conditional deposit address in Java

**In this guide, you generate a conditional deposit address, serialize it into a magnet link, and send test IOTA tokens to it.**

## IOTA network

In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet).

## Code walkthrough

1. [Plan the conditions](../introduction/overview.md#advice-for-creating-cdas) that you would like your conditional deposit address to have

2. Create a new CDA. This one expires tomorrow.

    ```java
	// Define the same time tomorrow
	Date timeoutAt = new Date(System.currentTimeMillis() + 24000 * 60 * 60);

	// Generate the CDA
    ConditionalDepositAddress cda = account.newDepositAddress(timeoutAt, false,0).get();
    ```

    :::info:
    By default, this method generates a CDA, using your account's security level settings. To generate a CDA with a different security level, you need to update your account settings.
    :::

3. Serialize the CDA into a magnet link and print it to the console

    ```java
    String magnet = (String) DepositFactory.get().build(cda, MagnetMethod.class);
    
    System.out.println(magnet);
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

:::success:
Now you have a CDA that contains IOTA tokens, you can make payments to it.
:::

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
mvn exec:java -Dexec.mainClass="com.iota.GenerateCDA"
```
---
### Windows
```bash
git clone https://github.com/JakeSCahill/iota-samples.git
cd iota-samples/java/account-module
mvn clean install
mvn exec:java -D"exec.mainClass"="com.iota.GenerateCDA"
```
--------------------

You should see the magnet link in the console.

```bash
iota://DL9CSYICJVKQRUTWBFUCZJQZ9WNBSRJOA9MGOISQZGGHOCZTXVSKDIZN9HBORNGDWRBBAFTKXGEJIAHKDJUYJJCFHC/?timeout_at=1574514007&multi_use=false&expected_amount=0
```

You can copy this magnet link and send it to someone else so they can deposit IOTA tokens into it.

## Next steps

[Start making payments with your account](../java/make-payment.md).