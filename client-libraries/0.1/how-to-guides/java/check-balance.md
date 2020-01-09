# Java でアドレスの残高を確認する
<!-- # Check the balance of an address in Java -->

**このガイドでは、[ノード](root://getting-started/0.1/network/nodes.md)から[アドレス](root://getting-started/0.1/clients/addresses.md)の [IOTA トークン](root://getting-started/0.1/clients/token.md)の残高をリクエストします。**
<!-- **In this guide, you request the balance of [IOTA tokens](root://getting-started/0.1/clients/token.md) on [addresses](root://getting-started/0.1/clients/addresses.md) from a [node](root://getting-started/0.1/network/nodes.md).** -->

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. クラスをインポートします。
  <!-- 1. Import the classes -->

    ```java
    package com.iota;

    import org.iota.jota.IotaAPI;
    import org.iota.jota.error.ArgumentException;
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

3. 残高を確認するアドレスを定義します。
  <!-- 3. Define the address whose balance you want to check -->

    ```java
    List<String> addresses = new ArrayList<String>();
        addresses.add("LRAZGXSV9FPCOO9OIUYLRLHBUJSBCCDBZC9UBPNMHQAGGI9BODPVIBMVCIKNCFVWWSALEBQMCFINHIVV9D9LYEQXSA");
    ```

    :::info:
    このアドレスには[チェックサム](root://getting-started/0.1/clients/checksums.md)を含める必要があります。
    :::
    <!-- :::info: -->
    <!-- This address must include a [checksum](root://getting-started/0.1/clients/checksums.md). -->
    <!-- ::: -->

4. `getBalance()` メソッドを使用して、ノードにアドレスの現在の残高を尋ねます。
  <!-- 4. Use the `getBalance()` method to ask the node for the current balance of the address -->

    ```java
    try {
        long balance = api.getBalance(100, address);
        System.out.printf("Your balance is: %s", balance);
    } catch (ArgumentException e) {
        // エラーを処理します
        e.printStackTrace();
    }
    ```

    コンソールに、IOTA トークンの残高が表示されます。
    <!-- In the console, you should see a balance of IOTA tokens: -->

    ```bash
    Your balance is 500
    ```

:::success:おめでとうございます:tada:
アドレスの残高を確認しました。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just checked the balance of an address. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

これらのコードサンプルは [GitHub](https://github.com/JakeSCahill/java-iota-workshop) でホストされています。
<!-- These code samples are hosted on [GitHub](https://github.com/JakeSCahill/java-iota-workshop). -->

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
mvn exec:java -Dexec.mainClass="com.iota.CheckBalance"
```
---
### Windows
```bash
git clone https://github.com/JakeSCahill/java-iota-workshop.git
cd java-iota-workshop
mvn clean install
mvn exec:java -D"exec.mainClass"="com.iota.CheckBalance"
```
--------------------

コンソールに、IOTA トークンの残高が表示されます。
<!-- In the console, you should see a balance of IOTA tokens: -->

```bash
Your balance is 500
```

## 次のステップ
<!-- ## Next steps -->

[タングル上のライブトランザクションをリッスンする](../java/listen-for-transactions.md)。
<!-- [Listen for live transactions on the Tangle](../java/listen-for-transactions.md). -->

[タングルエクスプローラー](https://utils.iota.org)などのユーティリティを使用して、アドレスの残高を確認することもできます。
<!-- You can also check the balance of an address, using a utility such as the [Tangle explorer](https://utils.iota.org). -->
