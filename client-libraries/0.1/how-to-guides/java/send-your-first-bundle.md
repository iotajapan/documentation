# Java で "hello world" トランザクションを送信する
<!-- # Send a "hello world" transaction in Java -->

**このガイドでは，ゼロトークン[トランザクション](root://getting-started/0.1/transactions/transactions.md)で "hello world" メッセージを送信します．ゼロトークントランザクションは，[IOTA トークン](root://getting-started/0.1/clients/token.md)を送信することなく[タングル](root://getting-started/0.1/network/the-tangle.md)にメッセージを保存するのに役立ちます．**
<!-- **In this guide, you send a "hello world" message in a zero-value [transaction](root://getting-started/0.1/transactions/transactions.md). These transactions are useful for storing messages on the [Tangle](root://getting-started/0.1/network/the-tangle.md) without having to send any [IOTA tokens](root://getting-started/0.1/clients/token.md).** -->

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは，以下のネットワーク設定で[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します．
<!-- In this guide, we connect to a [node](root://getting-started/0.1/network/nodes.md) on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet) with the following network settings: -->

- **[最小重量値](root://getting-started/0.1/network/minimum-weight-magnitude.md)**：9
<!-- - **[Minimum weight magnitude](root://getting-started/0.1/network/minimum-weight-magnitude.md)**: 9 -->

- **[深度](root://getting-started/0.1/transactions/depth.md)**：3
<!-- - **[Depth](root://getting-started/0.1/transactions/depth.md)**: 3 -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. クラスをインポートします．
  <!-- 1. Import the classes -->

    ```java
    package com.iota;

    import java.util.ArrayList;

    import org.iota.jota.IotaAPI;
    import org.iota.jota.dto.response.SendTransferResponse;
    import org.iota.jota.error.ArgumentException;
    import org.iota.jota.model.Transfer;
    import org.iota.jota.utils.SeedRandomGenerator;
    import org.iota.jota.utils.TrytesConverter;
    ```

2. ノードに接続します．
  <!-- 2. Connect to a node -->

    ```java
    IotaAPI api = new IotaAPI.Builder()
            .protocol("https")
            .host("nodes.devnet.thetangle.org")
            .port(443)
            .build();
    ```

3. 深さと最小重量値を定義します．
  <!-- 3. Define the depth and the minimum weight magnitude -->

    ```java
    int depth = 3;
    int minimumWeightMagnitude = 9;
    ```

4. メッセージの送信先[アドレス](root://getting-started/0.1/clients/addresses.md)を定義します．
  <!-- 4. Define an [address](root://getting-started/0.1/clients/addresses.md) to which you want to send a message -->

    ```java
    String address = "ZLGVEQ9JUZZWCZXLWVNTHBDX9G9KZTJP9VEERIIFHY9SIQKYBVAHIMLHXPQVE9IXFDDXNHQINXJDRPFDXNYVAPLZAW";
    ```

    :::info:
    このアドレスは誰のものである必要はありません．アドレスが有効であるには，81[トライト](root://getting-started/0.1/introduction/ternary.md)で構成される必要があります．
    :::
    <!-- :::info: -->
    <!-- This address does not have to belong to anyone. To be valid, the address just needs to consist of 81 [trytes](root://getting-started/0.1/introduction/ternary.md). -->
    <!-- ::: -->

5. シードとセキュリティレベルを定義します．
  <!-- 5. Define a seed and a security level -->

    ```java
    String myRandomSeed = SeedRandomGenerator.generateNewSeed();

    int securityLevel = 2;
    ```

    :::info:
    これはゼロトークントランザクションであるため，シードとセキュリティレベルは使用されません．ただし，ライブラリは有効なシードを想定しているため，81文字のランダムな文字列を使用します．81文字未満で構成されるシードを入力すると，ライブラリはその末尾に9を追加して81文字を作成します．
    :::
    <!-- :::info: -->
    <!-- Because this is a zero-value transaction, the seed and the security level are not used. However, the library expects a valid seed, so we use a random string of 81 characters. If you enter a seed that consists of less than 81 characters, the library will append 9s to the end of it to make 81 characters. -->
    <!-- ::: -->

6. アドレスに送信するメッセージを作成し，メッセージをトライトに変換します．
  <!-- 6. Create a message that you want to send to the address and convert it to trytes -->

    ```java
    String message = TrytesConverter.asciiToTrytes("Hello world");
    String tag = "HELLOWORLD";
    ```

    :::info:
    `AsciiToTrytes()` メソッドは[基本的な ASCII 文字](https://en.wikipedia.org/wiki/ASCII#Printable_characters)のみをサポートします．その結果，アクセントやウムラウトなどの発音区別記号や日本語はサポートされず，`INVALID_ASCII_CHARS` エラーが発生します．
    :::
    <!-- :::info: -->
    <!-- The `AsciiToTrytes()` method supports only [basic ASCII characters](https://en.wikipedia.org/wiki/ASCII#Printable_characters). As a result, diacritical marks such as accents and umlauts aren't supported and result in an `INVALID_ASCII_CHARS` error. -->
    <!-- ::: -->

7. アドレスにメッセージを送信するゼロトークントランザクションを定義します．
  <!-- 7. Define a zero-value transaction that sends the message to the address -->

    ```java
    int value = 0;

    Transfer zeroValueTransaction = new Transfer(address, value, message, tag);

    ArrayList<Transfer> transfers = new ArrayList<Transfer>();

    transfers.add(zeroValueTransaction);
    ```

8. `Transfers` オブジェクトからバンドルを作成するには，`Transfers` オブジェクトを[チップ選択](root://node-software/0.1/iri/concepts/tip-selection.md)，[リモートプルーフオブワーク](root://getting-started/0.1/transactions/proof-of-work.md)を処理する [`sendTransfer()`](https://github.com/iotaledger/iota-java/blob/dev/docs/iota-java/sendTransfer.md) メソッドに渡し，バンドルをノードに送信します．
  <!-- 8. To create a bundle from your `Transfers` object, pass it to the [`sendTransfer()`](https://github.com/iotaledger/iota-java/blob/dev/docs/iota-java/sendTransfer.md) method, which handles [tip selection](root://node-software/0.1/iri/concepts/tip-selection.md), [remote proof of work](root://getting-started/0.1/transactions/proof-of-work.md), and sending the bundle to the node -->

    ```java
    try {
        SendTransferResponse response = api.sendTransfer(myRandomSeed, securityLevel, depth, minimumWeightMagnitude, transfers, null, null, false, false, null);
        System.out.println(response.getTransactions());
    } catch (ArgumentException e) {
        // エラーを処理します
        e.printStackTrace();
     }
    ```

    コンソールに，送信したばかりのテールトランザクションが表示されます．
    <!-- In the console, you should see the tail transaction that you just sent. -->

:::success:おめでとうございます:tada:
最初のゼロトークンランザクションを送信しました．トランザクションはタングルにアタッチされ，ネットワークの残りの部分に転送されます．

このテールトランザクションハッシュを使用して，タングルからトランザクションを読み取ることができます．
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just sent your first zero-value transaction. Your transaction is attached to the Tangle, and will be forwarded to the rest of the network. -->

<!-- You can use this tail transaction hash to read the transaction from the Tangle. -->
<!-- ::: -->

:::warning:
ノードは，タングルのローカルコピーから古いトランザクションを削除できます．したがって，ノードからトランザクションをリクエストするときが来るかもしれませんが，ノードは古いトランザクションをもう持っていないかもしれません．

長期間タングルにデータを保存する場合は，[自分自身のノードの実行](root://node-software/0.1/iri/how-to-guides/quickstart.md)または[クロニクル](root://node-software/0.1/chronicle/introduction/overview.md)などのパーマノードの実行をお勧めします．
:::
<!-- :::warning: -->
<!-- Nodes can delete old transactions from their local copies of the Tangle. Therefore, a time may come where you request your transaction from a node, but the node doesn't have it anymore. -->

<!-- If you want to store data on the Tangle for extended periods of time, we recommend either [running your own node](root://node-software/0.1/iri/how-to-guides/quickstart.md) or running a permanode such as [Chronicle](root://node-software/0.1/chronicle/introduction/overview.md). -->
:::

## コードを実行する
<!-- ## Run the code -->

これらのコードサンプルは [GitHub](https://github.com/iota-community/java-iota-workshop) でホストされています．
<!-- These code samples are hosted on [GitHub](https://github.com/iota-community/java-iota-workshop). -->

開始するには，デバイスに [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) がインストールされている必要があります．
<!-- To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device. -->

また，[Maven](https://maven.apache.org/download.cgi) ビルドツールを使用する Java 開発環境も必要です．Java クライアントライブラリを初めて使用する場合は，[スタートガイド](../../getting-started/java-quickstart.md)を完了し，Maven でライブラリをインストールするための指示に従ってください．
<!-- You also need a Java development environment that uses the [Maven](https://maven.apache.org/download.cgi) build tool. If this is your first time using the Java client library, complete our [getting started guide](../../getting-started/java-quickstart.md), and follow the instructions for installing the library with Maven. -->

コマンドラインで，次を実行します．
<!-- In the command-line, do the following: -->

--------------------
### Linux and macOS
```bash
git clone https://github.com/iota-community/java-iota-workshop.git
cd java-iota-workshop
mvn clean install
mvn exec:java -Dexec.mainClass="com.iota.SendData"
```
---
### Windows
```bash
git clone https://github.com/iota-community/java-iota-workshop.git
cd java-iota-workshop
mvn clean install
mvn exec:java -D"exec.mainClass"="com.iota.SendData"
```
--------------------

コンソールに，送信したバンドルのテールトランザクションハッシュが表示されます．
<!-- In the console, you should see the tail transaction hash of the bundle you just sent. -->

## 次のステップ
<!-- ## Next steps -->

トランザクションのハッシュをメモして，[タングルからトランザクションを読み取る](../java/read-transactions.md)ことでメッセージを確認できるようにします．
<!-- Make a note of the transaction's hash so you can [read the transaction from the Tangle](../java/read-transactions.md) to see your message. -->

[タングルエクスプローラー](https://utils.iota.org)などのユーティリティを使用して，トランザクションを読み取ることができます．
<!-- You can also read your transaction, using a utility such as the [Tangle explorer](https://utils.iota.org). -->
