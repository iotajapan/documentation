# Java で残高を1つの CDA にまとめる
<!-- # Combine your balance into one CDA in Java -->

**残高の大半をできるだけ少ない条件付きデポジットアドレス（CDA）に保持することをお勧めします。これにより、支払いがより高速になり、必要なトランザクションが少なくなります。このガイドでは、利用可能な残高全体を新しい CDA に移行します。**
<!-- **You may want to keep the majority of your balance on as few conditional deposit addresses (CDA) as possible. This way, making payments is faster and requires fewer transactions. In this guide, you transfer your entire available balance to a new CDA.** -->

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. アカウントの利用可能な合計残高を予想額として持つ CDA を作成します。
  <!-- 1. Create a CDA that has your account's total available balance as its expected amount -->

    ```java
	Date timeoutAt = new Date(System.currentTimeMillis() + 24000 * 60 * 60);

    ConditionalDepositAddress cda = account.newDepositAddress(timeoutAt, true, account.availableBalance()).get();
    ```

    :::info:
    アカウントの利用可能残高は、すべての期限切れの CDA の合計残高です。期限切れの CDA に IOTA トークンを送信してはならないため、この残高は取り出しても安全です。

    アカウントの合計残高には、期限切れと同様にアクティブな CDA が含まれています。
    :::
    <!-- :::info: -->
    <!-- You account's available balance is the total balance of all expired CDAs. This balance is safe to withdraw because no one should send IOTA tokens to an expired CDA. -->

    <!-- Your account's total balance includes CDAs that are still active as well as expired. -->
    <!-- ::: -->

2. 利用可能な合計残高を CDA に転送します。
  <!-- 2. Transfer your total available balance to the CDA -->

    ```java
    Bundle bundle = account.send(
        cda.getDepositAddress().getHashCheckSum(),
        cda.getRequest().getExpectedAmount(),
        Optional.of("Sweep of all addresses"),
        Optional.of("IOTA9SWEEP")).get();
    System.out.printf("Sent deposit to %s in the bundle with the following tail transaction hash %s\n",
    bundle.getTransactions().get(bundle.getLength() - 1).getAddress(), bundle.getTransactions().get(bundle.getLength() - 1).getHash());
    ```

:::success:
現在、利用可能な合計残高は単一のアドレスにあります。
:::
<!-- :::success: -->
<!-- Now your total available balance is in a single address. -->
<!-- ::: -->

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
mvn exec:java -Dexec.mainClass="com.iota.CombineBalance"
```
---
### Windows
```bash
git clone https://github.com/iota-community/account-module.git
cd account-module/java/account-module
mvn clean install
mvn exec:java -D"exec.mainClass"="com.iota.CombineBalance"
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
