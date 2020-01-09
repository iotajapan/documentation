# Java でアドレスを生成する
<!-- # Generate an address in Java -->

**このガイドでは、[セキュリティレベル](root://getting-started/0.1/clients/security-levels.md)を指定して[シード](root://getting-started/0.1/clients/seeds.md)の新しいアドレスを生成する方法を学習します。**
<!-- **In this guide, you learn how to generate a new address for a [seed](root://getting-started/0.1/clients/seeds.md) with a given [security level](root://getting-started/0.1/clients/security-levels.md).** -->

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a [node](root://getting-started/0.1/network/nodes.md) on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. クラスをインポートします。
  <!-- 1. Import the classes -->

    ```java
    package com.iota;

    import org.iota.jota.IotaAPI;
    import org.iota.jota.builder.AddressRequest;
    import org.iota.jota.dto.response.GetNewAddressResponse;
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

3. アドレスに使用するセキュリティレベルを定義します。
  <!-- 3. Define the security level that you want to use for your address -->

    ```java
    int securityLevel = 2;
    ```

4. 生成するアドレスのシードを定義します。
  <!-- 4. Define a seed for which to generate an address -->

    ```java
    String mySeed = "PUPTTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX";
    ```

5. [`generateNewAddresses()`](https://github.com/iotaledger/iota-java/blob/dev/docs/iota-java/generateNewAddresses.md) メソッドを使用して、未使用のアドレスを生成します。
  <!-- 5. Use the [`generateNewAddresses()`](https://github.com/iotaledger/iota-java/blob/dev/docs/iota-java/generateNewAddresses.md) method to generate an unspent address -->

    ```java
    try {
        GetNewAddressResponse response = api.generateNewAddresses(new AddressRequest.Builder(mySeed, securityLevel).amount(1).checksum(true).build());
        System.out.printf("Your address is %s", response.getAddresses());
    } catch (ArgumentException e) {
        // エラーを処理します
        e.printStackTrace();
    }
    ```

接続されたノードは、指定されたインデックスから開始して、次の操作を実行してアドレスが使用されているかどうかを確認します。
<!-- Starting from the given index, the connected node checks if the address is spent by doing the following: -->

- アドレスから IOTA トークンを取り出す入力トランザクションがタングルの概観内にあるかどうかを検索する。
<!-- - Search its view of the Tangle for input transactions that withdraw from the address -->
- 使用済みアドレスのリスト内にアドレスがあるかどうかを検索する。
<!-- - Search for the address in the list of spent addresses -->

指定されたインデックスのアドレスが使用されていると、ノードは使用されていないアドレスを見つけるまでインデックスを増分します。
<!-- If an address with the given index is spent, the index is incremented until the node finds one that isn't spent. -->

:::warning:
ノードでアドレス応答を生成するこの方法は、アドレスに関する有効なデータを返します。アドレスをより細かく制御するには、[アカウントモジュール](../../account-module/introduction/overview.md)を使用して、ローカルデータベース内に保存した使用済みアドレスを追跡することをお勧めします。
:::
:::warning:
This way of generating addresses replies on the node to return valid data about your addresses. To have more control over your addresses, we recommend using the [account module](../../account-module/introduction/overview.md) to keep track of spent addresses in your own local database.
:::

コンソールにアドレスが表示されます。
<!-- In the console, you should see an address. -->

```bash
Your address is: WKJDF9LVQCVKEIVHFAOMHISHXJSGXWBJFYEQPOQKSVGZZFLTUUPBACNQZTAKXR9TFVKBGYSNSPHRNKKHA
```

:::success:おめでとうございます:tada:
新しい未使用のアドレスが生成されました。このアドレスは、トランザクションを送信したい人と共有できます。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just generated a new unspent address. You can share this address with anyone who wants to send you a transaction. -->
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

コンソールにアドレスが表示されます。
<!-- In the console, you should see an address. -->

```bash
Your address is: WKJDF9LVQCVKEIVHFAOMHISHXJSGXWBJFYEQPOQKSVGZZFLTUUPBACNQZTAKXR9TFVKBGYSNSPHRNKKHA
```

## 次のステップ
<!-- ## Next steps -->

[アドレスにテスト IOTA トークンを送信する](../java/transfer-iota-tokens.md)。
<!-- [Send test IOTA tokens to your new address](../java/transfer-iota-tokens.md). -->
