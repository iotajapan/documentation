# Java でアカウントを作成する
<!-- # Create an account -->

**このガイドでは、ローカルデータベースでシードステートを追跡するためのアカウントを作成します。**
<!-- **In this guide, you create an account to keep track of your seed state in a local database.** -->

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、以下のネットワーク設定で[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet) with the following network settings: -->

- **[最小重量値](root://getting-started/0.1/network/minimum-weight-magnitude.md)**: 9
<!-- - **[Minimum weight magnitude](root://getting-started/0.1/network/minimum-weight-magnitude.md)**: 9 -->

- **[深度](root://getting-started/0.1/transactions/depth.md)**: 3
<!-- - **[Depth](root://getting-started/0.1/transactions/depth.md)**: 3 -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1\. あたらしいシードを作成し、シードをバックアップします。
<!-- 1\. Create a new seed and back it up -->

:::info:
既存のシードは、シードステートが不明であるため、使用することは安全ではありません。既存のシードは、アカウントが認識していない使用済みアドレスを持っている場合が有ります。
:::
<!-- :::info: -->
<!-- Existing seeds are not safe to use because their state is unknown. As such, these seeds may have spent addresses that the account is not aware of. -->
<!-- ::: -->

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

2\. アカウントを使用するシードを定義します。
<!-- 2\. Define the seed that your account will use -->

```java
String mySeed = "PUEOTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX";
```

3\. ノードに接続します。
<!-- 3\. Connect to a node -->

```java
IotaAPI api = new IotaAPI.Builder()

    .host("nodes.devnet.iota.org")

    .port(443)
    .protocol("https")

    .timeout(500)
    .build();
```

4\. アカウントがシードステートを保存できるストレージオブジェクトを作成します。この例では、シードステートは JSON ファイルに保存されます。
<!-- 4\. Create a storage object to which the account can save the seed state. In this example, the seed state is stored in a JSON file. -->

```java
File file = new File("seed-state-database.json");
AccountStore store = new AccountFileStore(file);
```

5\. アカウントを作成します。カスタム設定を指定しない場合、アカウントは[デフォルト](https://github.com/iotaledger/iota-java/blob/dev/jota/src/main/java/org/iota/jota/config/types/IotaDefaultConfig.java)を使用します。
<!-- 5\. Build your account. If you don't specify any custom settings, the account uses the [defaults](https://github.com/iotaledger/iota-java/blob/dev/jota/src/main/java/org/iota/jota/config/types/IotaDefaultConfig.java). -->

```java
IotaAccount account = new IotaAccount.Builder(mySeed)
    // ノードに接続します
    .api(api)
    // データベースに接続します
    .store(store)
    // デブネット用の最小重量値を設定します（デフォルトの最小重量値は14です）
    .mwm(9)
    // CDAs のセキュリティレベルを設定します（デフォルトのセキュリティレベルは3です）
    .securityLevel(2)
    .build();
```

6\. アカウントを起動します。
<!-- 6\. Start the account -->

```java
account.start();
```

アカウントを開始すると、アカウントのプラグインも開始されます。デフォルトのプラグインには次のものが含まれます。
<!-- When you start the account, you also start any of your account's plugins. The default plugins include the following: -->

- `IncomingTransferChecker`：30秒ごとに、このプラグインはアカウントからの取り出しが確定されたかどうかをチェックします。
<!-- - `IncomingTransferChecker`: Every 30 seconds, this plugin checks whether withdrawals from your account have been confirmed -->

- `OutgoingTransferChecker`：10秒ごとに、このプラグインはアカウントへのデポジットが確定されたかどうかをチェックします。
<!-- - `OutgoingTransferChecker`: Every 10 seconds, this plugin checks whether deposits into your account have been confirmed -->

- `PromoterReattacher`：10秒ごとに、このプラグインは `TransferChecker` プラグインが見つけるペンディング中の取り出しまたはデポジットトランザクションを[プロモートまたは再アタッチ](root://getting-started/0.1/transactions/reattach-rebroadcast-promote.md)します。
<!-- - `PromoterReattacher`: Every 10 seconds, this plugin [promotes or reattaches](root://getting-started/0.1/transactions/reattach-rebroadcast-promote.md) any pending withdrawal or deposit transactions that the `TransferChecker` plugins find -->

:::info:
これらのプラグインの動作をカスタマイズするか、自分自身のプラグインを作成できます。
:::
<!-- :::info: -->
<!-- You can customize the behavior of these plugins or build your own. -->
<!-- ::: -->

7\. アカウントの残高を確認します。
<!-- 7\. Check your account's balance -->

```java
long balance = account.availableBalance();

System.out.print("Your balance is: " + toIntExact(balance));

account.shutdown();
```

:::success:おめでとうございます:tada:
トランザクションを自動的にプロモートおよび再アタッチし、シードステートを管理するアカウントを作成しました。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You've created an account that will automatically promote and reattach transactions as well as manage the state of your seed. -->
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
mvn exec:java -Dexec.mainClass="com.iota.CreateAccount"
```
---
### Windows
```bash
git clone https://github.com/iota-community/account-module.git
cd account-module/java/account-module
mvn clean install
mvn exec:java -D"exec.mainClass"="com.iota.CreateAccount"
```
--------------------

新しいアカウントの残高が表示されます。
<!-- You should see the balance of your new account. -->

また、シードステートを追跡する JSON ファイルもあります。
<!-- You'll also have a JSON file that keeps track of your seed state. -->

## 次のステップ
<!-- ## Next steps -->

アカウントで特定のイベントが発生すると、アカウントはそのイベントを発行するので、発行されたイベントをリッスンすることができます。たとえば、アカウントの新規支払いをモニタリングすることができます。そのためには、[イベントリスナーを作成する](../java/listen-to-events.md)必要があります。
<!-- After certain events happen in your account, it emits them, and allows you to listen for them. For example, you may want to monitor your account for new payments. To do so, you need to [create an event listener](../java/listen-to-events.md). -->

または、イベントを発行する[プラグインを作成する](../java/create-plugin.md)こともできます。
<!-- Or, you can [create a plugin](../java/create-plugin.md) that also emits events. -->
