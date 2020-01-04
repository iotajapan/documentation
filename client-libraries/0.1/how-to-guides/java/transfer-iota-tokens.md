# Java でマイクロペイメントを送信する
<!-- # Send a micropayment in Java -->

**このガイドでは、[転送バンドル](root://getting-started/0.1/transactions/bundles.md)を[ノード](root://getting-started/0.1/network/nodes.md)に送信して、1 IOTA のマイクロペイメントを送信します。**
<!-- **In this guide, you send a micropayment of 1 IOTA by sending a [transfer bundle](root://getting-started/0.1/transactions/bundles.md) to a [node](root://getting-started/0.1/network/nodes.md).** -->

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、以下のネットワーク設定で[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet) with the following network settings: -->

- **[最小重量値](root://getting-started/0.1/network/minimum-weight-magnitude.md)**: 9
<!-- - **[Minimum weight magnitude](root://getting-started/0.1/network/minimum-weight-magnitude.md)**: 9 -->

- **[深さ](root://getting-started/0.1/transactions/depth.md)**: 3
<!-- - **[Depth](root://getting-started/0.1/transactions/depth.md)**: 3 -->

## 手順1. テスト IOTA トークンを取得する
<!-- ## Step 1. Get test IOTA tokens -->

テスト IOTA トークンをデブネットで送信するには、ノードはシードに属するアドレスの1つにおいて0より大きい残高のレコードを持っている必要があります。デブネットで使用するテスト IOTA トークンを取得するには、デブネット蛇口を使用できます。
<!-- To send test IOTA tokens on the Devnet, the nodes must have a record of a greater than 0 balance for one of the addresses that belongs to your seed. To get test IOTA tokens to use on the Devnet, you can use the Devnet faucet. -->

1\. 新しいシードを作成し、シードをパックアップします。
  <!-- 1\. Create a new seed and back it up -->

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

2\. [シードの新しいアドレスを生成します](../java/generate-an-address.md)。
<!-- 2\. [Generate a new address for your seed](../java/generate-an-address.md) -->

3\. アドレスをコピーして[デブネット蛇口]にペーストし、テスト IOTA トークンがアドレスに転送されるのを待ちます。
<!-- 3\. Copy and paste your address into the [Devnet faucet](https://faucet.devnet.iota.org), then wait for the tokens to be transferred to your address -->

## 手順2. 転送バンドルを作成し、送信する
<!-- ## Step 2. Create and send a transfer bundle -->

あるアドレスから別のアドレスにテスト IOTA トークンを転送するには、転送バンドルを作成してノードに送信する必要があります。この転送バンドルには、アドレスから IOTA トークンを取り出すための入力トランザクションと、それらを別のアドレスに預け入れるための出力トランザクションが必要です。
<!-- To transfer your test tokens from one address to another, you need to create and send a transfer bundle. This bundle needs an input transaction to withdraw the IOTA tokens from your address and an output transaction to deposit them into another address. -->

1. クラスをインポートします。
  <!-- 1. Import the classes -->

    ```java
    package com.iota;

    import java.util.ArrayList;

    import org.iota.jota.IotaAPI;
    import org.iota.jota.dto.response.SendTransferResponse;
    import org.iota.jota.error.ArgumentException;
    import org.iota.jota.model.Transfer;
    ```

2. ノードに接続します。
  <!-- 2. Connect to a node -->

    ```java
    IotaAPI api = new IotaAPI.Builder()
            .protocol("https")
            .host("nodes.devnet.thetangle.org")
            .port(443)
            .build();
    ```

3. 深さと最小重量値を定義します。
  <!-- 3. Define the depth and the minimum weight magnitude -->

    ```java
    int depth = 3;
    int minimumWeightMagnitude = 9;
    ```

4. IOTA トークンを取り出すアドレスのシードとセキュリティレベルを定義します。このシードを、テスト IOTA トークンを持つアドレスを所有するシードに置き換えます。
  <!-- 4. Define your seed and the security level of the address from which you want to withdraw the IOTA token. Replace this seed with one that owns an address with test IOTA tokens -->

    ```java
    String mySeed = "JBN9ZRCOH9YRUGSWIQNZWAIFEZUBDUGTFPVRKXWPAUCEQQFS9NHPQLXCKZKRHVCCUZNF9CZZWKXRZVCWQ";
    ```

    :::info:
    このバンドルは IOTA トークンを転送するため、シードを使用してバンドルに署名します。したがって、このシードのアドレスの1つには少なくとも1つの IOTA トークンが含まれている必要があります。
    :::
    <!-- :::info: -->
    <!-- Because this bundle transfers IOTA tokens, the seed is used to sign it. Therefore, one of this seed's addresses must contain at least 1 IOTA token. -->
    <!-- ::: -->

5. IOTA トークンを送信するアドレスを定義します。
  <!-- 5. Define the address to which you want to send your IOTA token -->

    ```java
    String address = "ZLGVEQ9JUZZWCZXLWVNTHBDX9G9KZTJP9VEERIIFHY9SIQKYBVAHIMLHXPQVE9IXFDDXNHQINXJDRPFDXNYVAPLZAW";
    ```

6. 転送する IOTA トークンの量と送信先のアドレスを指定する `transfers` オブジェクトを作成します。
  <!-- 6. Create a `Transfers` object that specifies the amount of IOTA tokens you want to transfer and the address to send the tokens to -->

    ```java
    int value = 1;

    Transfer Transaction = new Transfer(address, value);

    ArrayList<Transfer> transfers = new ArrayList<Transfer>();

    transfers.add(Transaction);
    ```

7. `Transfers` オブジェクトから転送バンドルを作成するには、`Transfers` オブジェクトを[チップ選択](root://node-software/0.1/iri/concepts/tip-selection.md)、[リモートプルーフオブワーク](root://getting-started/0.1/transactions/proof-of-work.md)を処理する [`sendTransfer()`](https://github.com/iotaledger/iota-java/blob/dev/docs/iota-java/sendTransfer.md) メソッドに渡し、バンドルをノードに送信します。
  <!-- 7. To create a transfer bundle from your `Transfers` object, pass it to the [`sendTransfer()`](https://github.com/iotaledger/iota-java/blob/dev/docs/iota-java/sendTransfer.md) method, which handles [tip selection](root://node-software/0.1/iri/concepts/tip-selection.md), [remote proof of work](root://getting-started/0.1/transactions/proof-of-work.md), and sending the bundle to the node -->

    ```java
    try {
        System.out.printf("Sending 1 i to %s", address);
        SendTransferResponse response = api.sendTransfer(mySeed, securityLevel, depth, minimumWeightMagnitude, transfers, null, null, false, false, null);
        System.out.println(response.getTransactions());
    } catch (ArgumentException e) {
        // Handle error
        // エラーを処理します
        e.printStackTrace();
    }
    ```

    このメソッドは、指定されたセキュリティレベルを持つシードのアドレスの残高を確認するようにノードに要求します。アドレスに転送を完了するのに十分なIOTAトークンがある場合、メソッドは入力トランザクションを作成して、転送を実行するために十分な全残高をアドレスから取り出します。次に、メソッドはこれらのトランザクションを転送バンドルに追加し、IOTA トークンが取り出されるすべてのアドレスの秘密鍵でバンドルに署名します。
    <!-- This method asks the node to check the balance of your seed's addresses that have the given security level. If your addresses have enough IOTA tokens to complete the transfer, the method creates input transactions to withdraw the full balance from enough of your addresses to fulfill the transfer. Then, the method adds those transactions to the transfer bundle and signs the bundle with the private keys of any withdrawn addresses. -->

    :::info:
    シードがデバイスから離れることはありません。ライブラリはアドレスを生成し、アドレスのみをノードに送信します。
    :::
    <!-- :::info: -->
    <!-- Your seed never leaves your device. The library generates addresses and sends them to the node. -->
    <!-- ::: -->

    転送する金額が取り出すアドレスの残高より少ない場合、メソッドは別の出力トランザクションを作成して、残りの IOTA トークンをシードに属する未使用のアドレスに転送します。
    <!-- If the amount you want to transfer is less than the balance of your withdrawn addresses, the method creates another output transaction to transfer the remainder to an unspent address that belongs to your seed. -->

    コンソールに、送信したばかりのトランザクションのバンドルが表示されます。
    <!-- In the console, you'll see the bundle of transactions that you just sent. -->

:::success:おめでとうございます:tada:
最初の転送バンドルを送信しました。トランザクションはタングルにアタッチされ、ネットワークの残りの部分に転送されます。これで、トランザクションが確定し、残高が更新されるのを待つだけです。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just sent your first transfer bundle. Your transactions are attached to the Tangle and will be forwarded to the rest of the network. Now, you just need to wait until the transaction is confirmed for your balance to be updated. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

開始するには、デバイスに [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) がインストールされている必要があります。
<!-- To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device. -->

また、[Maven](https://maven.apache.org/download.cgi) ビルドツールを使用する Java 開発環境も必要です。Java クライアントライブラリを初めて使用する場合は、[スタートガイド](../../getting-started/java-quickstart.md)を完了し、Maven でライブラリをインストールするための指示に従ってください。
<!-- You also need a Java development environment that uses the [Maven](https://maven.apache.org/download.cgi) build tool. If this is your first time using the Java client library, complete our [getting started guide](../../getting-started/java-quickstart.md), and follow the instructions for installing the library with Maven. -->

コマンドラインで、次を実行します。
<!-- In the command-line, do the following: -->

--------------------
### Linux and macOS
```bash
git clone https://github.com/JakeSCahill/java-iota-workshop.git
cd java-iota-workshop
mvn clean install
mvn exec:java -Dexec.mainClass="com.iota.SendTokens"
```
---
### Windows
```bash
git clone https://github.com/JakeSCahill/java-iota-workshop.git
cd java-iota-workshop
mvn clean install
mvn exec:java -D"exec.mainClass"="com.iota.SendTokens"
```
--------------------

コンソールに、送信したばかりのトランザクションのバンドルが表示されます。
<!-- In the console, you'll see the bundle of transactions that you just sent. -->

## 次のステップ
<!-- ## Next steps -->

[アドレスの残高を確認する](../java/check-balance.md)。
<!-- [Check the balance of your address](../java/check-balance.md). -->

このシナリオでは、バンドルの作成と送信にかかる時間の間にアドレスが使用されるかどうかを事前に知ることができません。
<!-- In this scenario, you wouldn't know in advance whether the address is spent during the time it takes to create and send your bundle. -->

たとえば、オンラインショッピングをしていて、チェックアウトには QR コードがあり、IOTA トークンで支払うオプションがあったとします。この QR コードには、トリニティに自動入力されるアドレスが含まれています。
<!-- For example, you are online shopping and the checkout has a QR code that gives you the option to pay in IOTA tokens. This QR code contains an address that is auto-populated in Trinity. -->

チェックアウトを完了して転送バンドルを送信するまでに、Web サイトの所有者が QR コードのアドレスから IOTA トークンを取り出したとします。これで QR コードのアドレスは使用済みアドレスとなり、支払者はこの使用済みアドレスへ IOTA トークンを送信してしまうことになります。
<!-- During the time it takes you to complete the checkout and send your transfer bundle, the website owner withdraws IOTA tokens from the address in the QR code. Now that address is spent, and you have just sent IOTA tokens to it. -->

これを防ぐには、[アカウントモジュール](../../account-module/introduction/overview.md)を使用して、アクティブか期限切れかを指定する条件付き預け入れアドレスを作成することをお勧めします。
<!-- To help stop this from happening, we recommend using the [account module](../../account-module/introduction/overview.md) to create conditional deposit addresses that specify whether they are active or expired. -->
