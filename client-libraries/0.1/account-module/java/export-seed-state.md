# Java でシードステートをインポート/エクスポートする
<!-- # Import/export an existing seed state in Java -->

**アカウントを使用して支払いを行うと、アカウントはシードステートを更新します。このガイドでは、アカウントのシードステートをエクスポートして、別のデバイスにインポートしたり、単にバックアップしたりできるようにする方法を学習します。**
<!-- **When you use your account to make payments, your account updates your seed state. In this guide, you learn how to export your account's seed state so that you can import it on another device or simply back it up.** -->

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. アカウントのIDを `exportAccount()` メソッドに渡すことでシードステートをエクスポートします。
  <!-- 1. Export your seed state by passing your account's ID to the `exportAccount()` method -->

    ```java
    ExportedAccountState state = store.exportAccount(account.getId());
    ```

2. シードステートを保存する JSON ファイルを作成します。
  <!-- 2. Create a JSON file to which to save your seed state -->

    ```java
    BufferedWriter writer = new BufferedWriter(new FileWriter("exported-seed-state-database.json"));
    ```

3. シードステートをシリアル化し、ファイルに保存します。
  <!-- 3. Serialize your seed state and save it to a file -->

    ```java
    ObjectMapper mapper = new ObjectMapper();
        try {
        String json = mapper.writeValueAsString(state);
        System.out.println("ResultingJSONstring = " + json);
        writer.write(json);
        writer.close();
        } catch (JsonProcessingException e) {
            e.printStackTrace();
            account.shutdown();
    }
    ```

    :::info:
    定期的にシードステートをバックアップすることをお勧めします。
    :::
    <!-- :::info: -->
    <!-- It's best practice to back up your seed state at regular intervals. -->
    <!-- ::: -->

4. エクスポートされたシードステートを読み取り、デシリアライズし、アカウントにインポートします。
  <!-- 4. Read your exported seed state, deserialize it, and import it into your account -->

    ```java
    mapper = new ObjectMapper();
    // 新しいファイルを無視します
    mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);

    try {

        FileReader readState = new FileReader("exported-seed-state-database.json");

        state = mapper.readValue(readState, ExportedAccountState.class);

        store.importAccount(state);

        System.out.println("Seed state imported");
    } catch (JsonProcessingException e) {
        e.printStackTrace();
    }
    ```

    :::warning:
    シードステートをインポートすると、アカウントのデータベース内の既存のシードステートが上書きされます。
    :::
    <!-- :::warning: -->
    <!-- When you import a seed state, you overwrite any existing seed state in your account's database. -->
    <!-- ::: -->

:::success:おめでとうございます:tada:
シードステートをエクスポートおよびインポートする方法を学習しました。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You've learned how to export and import your seed state. -->
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
mvn exec:java -Dexec.mainClass="com.iota.ExportAccount"
```
---
### Windows
```bash
git clone https://github.com/iota-community/account-module.git
cd account-module/java/account-module
mvn clean install
mvn exec:java -D"exec.mainClass"="com.iota.ExportAccount"
```
--------------------

シードステートを含む `exported-seed-state.json` ファイルが必要です。このファイルを使用して、シードステートを別のデバイスにインポートできます。
<!-- You should have an `exported-seed-state.json` file that contains your seed state. You can use this file to import your seed state on another device. -->

## 次のステップ
<!-- ## Next steps -->

[ソースコード](https://github.com/iotaledger/iota-java/tree/dev/jota/src/main/java/org/iota/jota)を見て、アカウントモジュールについて引き続き学習する。
<!-- Take a look at the [source code](https://github.com/iotaledger/iota-java/tree/dev/jota/src/main/java/org/iota/jota) to continue learning about the account module. -->
