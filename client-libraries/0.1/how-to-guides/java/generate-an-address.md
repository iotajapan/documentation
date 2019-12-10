# Generate an address in Java

**In this guide, you learn how to generate a new address for a [seed](root://getting-started/0.1/clients/seeds.md) with a given [security level](root://getting-started/0.1/clients/security-levels.md).**

## IOTA network

In this guide, we connect to a [node](root://getting-started/0.1/network/nodes.md) on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet).

## Code walkthrough

1. Import the classes

    ```java
    package com.iota;

    import org.iota.jota.IotaAPI;
    import org.iota.jota.builder.AddressRequest;
    import org.iota.jota.dto.response.GetNewAddressResponse;
    import org.iota.jota.error.ArgumentException;
    ```
2. Connect to a node

    ```java
    IotaAPI api = new IotaAPI.Builder()
            .protocol("https")
            .host("nodes.devnet.thetangle.org")
            .port(443)
            .build();
    ```

3. Define the security level that you want to use for your address

    ```java
    int securityLevel = 2;
    ```

4. Define a seed for which to generate an address

    ```java
    String mySeed = "PUPTTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX";
    ```

5. Use the [`generateNewAddresses()`](https://github.com/iotaledger/iota-java/blob/dev/docs/iota-java/generateNewAddresses.md) method to generate an unspent address

    ```java
    try { 
        GetNewAddressResponse response = api.generateNewAddresses(new AddressRequest.Builder(mySeed, securityLevel).amount(1).checksum(true).build());
        System.out.printf("Your address is %s", response.getAddresses());
    } catch (ArgumentException e) { 
        // Handle error
        e.printStackTrace(); 
    }
    ```

Starting from the given index, the connected node checks the following:

- If any input transactions (pending or confirmed) in its view of the Tangle withdraw from the address
- If the address is in the node's list of spent addresses (addresses that were withdrawn from in confirmed transactions)

If an address with the given index is spent, the index is incremented until the node finds one that isn't spent.

:::warning:
This way of generating addresses replies on the node to return valid data about your addresses. To have more control over your addresses, we recommend using the [account module](../../account-module/introduction/overview.md) to keep track of spent addresses in your own local database.
:::

In the console, you should see an address.

```
Your address is: WKJDF9LVQCVKEIVHFAOMHISHXJSGXWBJFYEQPOQKSVGZZFLTUUPBACNQZTAKXR9TFVKBGYSNSPHRNKKHA
```

:::success:Congratulations :tada:
You've just generated a new unspent address. You can share this address with anyone who wants to send you a transaction.
:::

## Run the code

To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device.

You also need a Java development environment that uses the [Maven](https://maven.apache.org/download.cgi) build tool. If this is your first time using the Java client library, complete our [getting started guide](../../getting-started/java-quickstart.md), and follow the instructions for installing the library with Maven.

In the command-line, do the following:

--------------------
### Linux and macOS
```bash
git clone https://github.com/JakeSCahill/java-iota-workshop.git
cd java-iota-workshop
mvn clean install
mvn exec:java -Dexec.mainClass="com.iota.GenerateAddress"
```
---
### Windows
```bash
git clone https://github.com/JakeSCahill/java-iota-workshop.git
cd java-iota-workshop
mvn clean install
mvn exec:java -D"exec.mainClass"="com.iota.GenerateAddress"
```
--------------------

In the console, you should see an address.

```
Your address is: WKJDF9LVQCVKEIVHFAOMHISHXJSGXWBJFYEQPOQKSVGZZFLTUUPBACNQZTAKXR9TFVKBGYSNSPHRNKKHA
```

## Next steps

[Send test IOTA tokens to your new address](../java/transfer-iota-tokens.md).
