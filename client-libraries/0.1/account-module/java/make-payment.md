# Java でアカウントで支払いを行う
<!-- # Make payments with your account in Java -->

**このガイドでは、アカウントを使用して IOTA トークンを事前定義された CDA にデポジットします。**
<!-- **In this guide, you use your account to deposit IOTA tokens into a pre-defined CDA.** -->

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. `ParseMagnetLink()` メソッドを使用して、事前定義されたマグネットリンクを CDA にデシリアライズします。
  <!-- 1. Use the `ParseMagnetLink()` method to deserialize the predefined magnet link into a CDA -->

    ```java
    String magnet = "iota://BWNYWGULIIAVRYOOFWZTSDFXFPRCFF9YEHGVBOORLGCPCJSKTHU9OKESUGZGWZXZZDLESFPPTGEHVKTTXG9BQLSIGP/?timeout_at=5174418337&multi_use=1&expected_amount=0";

    ConditionalDepositAddress cda = DepositFactory.get().parse(magnet, MagnetMethod.class);
    ```

    :::info:
    指定されたマグネットリンクは、100年以上で期限切れになる CDA の例です。別の CDA に支払いを行う場合は、代わりにその CDA を使用してください。
    :::
    <!-- :::info: -->
    <!-- The given magent link is for an example CDA that expires in over 100 years. If you want to make a payment to a different CDA, use that one instead. -->
    <!-- ::: -->


2. IOTA トークンを含む CDA がない場合は、[このガイド](../java/generate-cda.md)に従ってください。
<!-- 2. If you dont have a CDA that contains IOTA tokens, follow [this guide](../java/generate-cda.md) -->

3. CDA がまだアクティブであることを確認した後、デポジットを送信します。
  <!-- 3. After making sure that the CDA is still active, send a deposit to it -->

    ```java
    Bundle bundle = account.send(
        cda.getDepositAddress().getHashCheckSum(),
        cda.getRequest().getExpectedAmount(),
        Optional.of("Thanks for the pizza"),
        Optional.of("ACCOUNTMODULETEST")).get();
    System.out.printf("Sent deposit to %s in the bundle with the following tail transaction hash %s\n",
    bundle.getTransactions().get(bundle.getLength() - 1).getAddress(), bundle.getTransactions().get(bundle.getLength() - 1).getHash());
    ```

    標準出力に次のようなものが表示されるはずです。
    <!-- You should see something like the following in the output: -->

    ```bash
    Sent deposit to DL9CSYICJVKQRUTWBFUCZJQZ9WNBSRJOA9MGOISQZGGHOCZTXVSKDIZN9HBORNGDWRBBAFTKXGEJIAHKDTMAUX9ILA in the bundle with the following tail transaction hash WZEATTRJYENRALJTWPVGDQZHETIDJXPUROUM9BBPS9RJEELDMU9YNZFBSDGPQHZHMXBVCKITSMDEEQ999
    ```

アカウントは、確定されるまでバンドルを再アタッチしてプロモートします。
<!-- Your account will reattach and promote your bundle until it's confirmed. -->

## コードを実行する
<!-- ## Run the code -->

これらのコードサンプルは[GitHub](https://github.com/iota-community/account-module) でホストされています。
<!-- These code samples are hosted on [GitHub](https://github.com/iota-community/account-module). -->

開始するには、デバイスに [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) がインストールされている必要があります。
<!-- To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device. -->

また、[Maven](https://maven.apache.org/download.cgi) ビルドツールを使用する Java 開発環境も必要です。Java クライアントライブラリを初めて使用する場合は、[スタートガイド](../../getting-started/java-quickstart.md)を完了し、Maven でライブラリをインストールするための指示に従ってください。
<!-- You also need a Java development environment that uses the [Maven](https://maven.apache.org/download.cgi) build tool. If this is your first time using the Java client library, complete our [getting started guide](../../getting-started/java-quickstart.md), and follow the instructions for installing the library with Maven. -->

コマンドラインで、次を実行します。
<!-- In the command-line, do the following: -->

--------------------
### Linux and macOS
```bash
git clone https://github.com/iota-community/account-module.git
cd account-module/java/account-module
mvn clean install
mvn exec:java -Dexec.mainClass="com.iota.MakePayment"
```
---
### Windows
```bash
git clone https://github.com/iota-community/account-module.git
cd account-module/java/account-module
mvn clean install
mvn exec:java -D"exec.mainClass"="com.iota.MakePayment"
```
--------------------

デポジットが送信されたことがわかります。
<!-- You should see that the deposit was sent. -->

シードステートには、確定されるまでこのペンディング中のバンドルが含まれます。
<!-- Your seed state will contain this pending bundle until it is confirmed. -->

## 次のステップ
<!-- ## Next steps -->

[シードステートをエクスポートして、バックアップするか、別のデバイスにインポートする](../java/export-seed-state.md)。
<!-- [Try exporting your seed state so you back it up or import it onto another device](../java/export-seed-state.md). -->
