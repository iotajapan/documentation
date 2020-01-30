# Java で条件付きデポジットアドレスを生成する
<!-- # Generate a conditional deposit address in Java -->

**このガイドでは，条件付きデポジットアドレス（CDA）を生成し，マグネットリンクにシリアル化し，テスト IOTA トークンを送信します．**
<!-- **In this guide, you generate a conditional deposit address (CDA), serialize it into a magnet link, and send test IOTA tokens to it.** -->

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは，[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します．
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. 持ちたい条件付きデポジットアドレスの[条件を計画します](../introduction/overview.md#advice-for-creating-cdas)．
<!-- 1. [Plan the conditions](../introduction/overview.md#advice-for-creating-cdas) that you would like your conditional deposit address to have -->

2. 新しい CDA を作成します．この CDA は翌日に期限切れになります．
  <!-- 2. Create a new CDA. This one expires tomorrow. -->

    ```java
    // 明日の同じ時間を定義します
    Date timeoutAt = new Date(System.currentTimeMillis() + 24000 * 60 * 60);

    // CDA を生成します
    ConditionalDepositAddress cda = account.newDepositAddress(timeoutAt, false,0).get();
    ```

    :::info:
    デフォルトでは，この方法はアカウントのセキュリティレベル設定を使用して CDA を生成します．異なるセキュリティレベルで CDA を生成するには，アカウント設定を更新する必要があります．
    :::
    <!-- :::info: -->
    <!-- By default, this method generates a CDA, using your account's security level settings. To generate a CDA with a different security level, you need to update your account settings. -->
    <!-- ::: -->

3. CDA をマグネットリンクにシリアル化し，コンソールに印刷します．
  <!-- 3. Serialize the CDA into a magnet link and print it to the console -->

    ```java
    String magnet = (String) DepositFactory.get().build(cda, MagnetMethod.class);

    System.out.println(magnet);
    ```

    :::info:
    CDA の最後の9つのトライトはチェックサムであり，これはアドレスとそのアドレスのすべての条件のハッシュです．
    :::
    <!-- :::info: -->
    <!-- The last 9 trytes of a CDA are the checksum, which is a hash of the address and all of its conditions. -->
    <!-- ::: -->

4. アドレスをコピーして[デブネット蛇口](https://faucet.devnet.iota.org)にペーストし，トークンがアドレスに転送されるのを待ちます．
  <!-- 4. Copy and paste your address into the [Devnet faucet](https://faucet.devnet.iota.org), then wait for the tokens to be transferred to your address -->

    :::info:
    デブネット蛇口から IOTA トークンをリクエストする前に，チェックサムを必ず削除してください．
    :::
    <!-- :::info: -->
    <!-- Make sure to remove the checksum before requesting IOTA tokens from the Devnet faucet. -->
    <!-- ::: -->

    アドレスの例
    <!-- For example: -->

    ```bash
    DL9CSYICJVKQRUTWBFUCZJQZ9WNBSRJOA9MGOISQZGGHOCZTXVSKDIZN9HBORNGDWRBBAFTKXGEJIAHKD
    ```

:::success:
これで IOTA トークンを含む CDA ができたので，支払いを行うことができます．
:::
<!-- :::success: -->
<!-- Now you have a CDA that contains IOTA tokens, you can make payments to it. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

これらのコードサンプルは[GitHub](https://github.com/iota-community/account-module) でホストされています．
<!-- These code samples are hosted on [GitHub](https://github.com/iota-community/account-module). -->

開始するには，デバイスに [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) がインストールされている必要があります．
<!-- To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device. -->

また，[Maven](https://maven.apache.org/download.cgi) ビルドツールを使用する Java 開発環境も必要です．Java クライアントライブラリを初めて使用する場合は，[スタートガイド](../../getting-started/java-quickstart.md)を完了し，Maven でライブラリをインストールするための指示に従ってください．
<!-- You also need a Java development environment that uses the [Maven](https://maven.apache.org/download.cgi) build tool. If this is your first time using the Java client library, complete our [getting started guide](../../getting-started/java-quickstart.md), and follow the instructions for installing the library with Maven. -->

コマンドラインで，次を実行します．
<!-- In the command-line, do the following: -->

--------------------
### Linux and macOS
```bash
git clone https://github.com/iota-community/account-module.git
cd account-module/java/account-module
mvn clean install
mvn exec:java -Dexec.mainClass="com.iota.GenerateCDA"
```
---
### Windows
```bash
git clone https://github.com/iota-community/account-module.git
cd account-module/java/account-module
mvn clean install
mvn exec:java -D"exec.mainClass"="com.iota.GenerateCDA"
```
--------------------

コンソールにマグネットリンクが表示されます．
<!-- You should see the magnet link in the console. -->

```bash
iota://DL9CSYICJVKQRUTWBFUCZJQZ9WNBSRJOA9MGOISQZGGHOCZTXVSKDIZN9HBORNGDWRBBAFTKXGEJIAHKDJUYJJCFHC/?timeout_at=1574514007&multi_use=false&expected_amount=0
```

このマグネットリンクをコピーして他の人に送信すると，IOTA トークンをマグネットリンクのアドレスにデポジットすることができます．
<!-- You can copy this magnet link and send it to someone else so they can deposit IOTA tokens into it. -->

## 次のステップ
<!-- ## Next steps -->

[アカウントで支払いを行う](../java/make-payment.md)．
<!-- [Start making payments with your account](../java/make-payment.md). -->
