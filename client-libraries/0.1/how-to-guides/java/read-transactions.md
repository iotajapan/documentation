# Java でタングルからトランザクションを読み取る
<!-- # Read transactions from the Tangle in Java -->

**このガイドでは、[ノード](root://getting-started/0.1/network/nodes.md)に末尾トランザクションハッシュを与えることで、タングルから "hello world" [トランザクション](root://getting-started/0.1/transactions/transactions.md)を読み取ります。**
<!-- **In this guide, you read your "hello world" [transaction](root://getting-started/0.1/transactions/transactions.md) from the Tangle by giving a [node](root://getting-started/0.1/network/nodes.md) your tail transaction hash.** -->

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. クラスをインポートします。
  <!-- 1. Import the classes -->

    ```java
    import java.io.IOException;
    import java.util.List;

    import com.fasterxml.jackson.core.JsonParseException;
    import com.fasterxml.jackson.databind.JsonMappingException;

    import org.iota.jota.IotaAPI;
    import org.iota.jota.error.ArgumentException;
    import org.iota.jota.model.Transaction;
    import org.iota.jota.utils.TrytesConverter;
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

3. バンドルの末尾トランザクションハッシュを定義します。
<!-- 3. Define the tail transaction hash of the bundle -->

    ```java
    String tailTransactionHash = "GUQXLBNYXSEVNAPNJ9L9ADZIHBHRVEH9VZURPUHGQEVAFCWIDKQWTKYPOFALYZNOWGRJQURNQBGFGQDM9";
    ```

    :::info:
    `signatureMessageFragment` フィールドはハッシュの一部であるため、テールトランザクションハッシュを使用します。したがって、トランザクション内のメッセージはイミュータブルです。

    バンドルハッシュを使用する場合、誰でもテールトランザクションのメッセージを変更し、バンドルのコピーをタングルにアタッチできるため、別のメッセージが表示される場合があります。
    :::

    <!-- :::info: -->
    <!-- We use the tail transaction hash because the `signatureMessageFragment` field is part of the hash. Therefore, the message in the transaction is immutable. -->

    <!-- If you were to use the bundle hash, you may see a different message because anyone can change the message in the tail transaction and attach a copy of the bundle to the Tangle. -->
    <!-- ::: -->

4. `getBundle()`メソッドを使用して、末尾トランザクションのバンドル内のすべてのトランザクションを取得します。次に、最初のトランザクションの `signatureMessageFragment` フィールドでメッセージを取得し、デコードして、コンソールに出力します
  <!-- 4. Use the `getBundle()` method to get all transactions in the tail transaction's bundle. Then, get the message in the first transaction's `signatureMessageFragment` field, decode it, and print it to the console -->

    ```java
    try {
        GetBundleResponse response = api.getBundle("GUQXLBNYXSEVNAPNJ9L9ADZIHBHRVEH9VZURPUHGQEVAFCWIDKQWTKYPOFALYZNOWGRJQURNQBGFGQDM9");
        System.out.println(TrytesConverter.trytesToAscii(response.get(0).getSignatureFragments().substring(0,2186)));
    } catch (ArgumentException e) {
        // エラーを処理します
        e.printStackTrace();
    }
    ```

    コンソールにメッセージが表示されます。
    <!-- In the console, you should see your message: -->

    ```json
    "Hello world"
    ```

:::success:おめでとうございます:tada:
タングルからトランザクションを見つけて読み取りました。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just found and read a transaction from the Tangle. -->
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
mvn exec:java -Dexec.mainClass="com.iota.ReadData"
```
---
### Windows
```bash
git clone https://github.com/JakeSCahill/java-iota-workshop.git
cd java-iota-workshop
mvn clean install
mvn exec:java -D"exec.mainClass"="com.iota.ReadData"
```
--------------------

コンソールにメッセージが表示されます。
<!-- In the console, you should see your message: -->

```json
"Hello world"
```

## 次のステップ
<!-- ## Next steps -->

[新しいアドレスを生成する](../java/generate-an-address.md)。
<!-- [Generate a new address](../java/generate-an-address.md). -->
