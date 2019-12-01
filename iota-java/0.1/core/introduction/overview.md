# Javaコアライブラリ入門
<!-- # Get started with the Java core library -->

**Javaコアクライアントライブラリには、タングルと対話するための低レベルのメソッドが含まれています。このライブラリを使用して、コアIOTAプロトコルを使用できます。たとえば、ノードに接続し、バンドルを作成し、ペンディング中のトランザクションを促進および再添付できます。**
<!-- **The Java core client library includes low-level methods for interacting with the Tangle. You can use this library to use the core IOTA protocol. For example, you can connect to nodes, create bundles, and promote and reattach pending transactions.** -->

:::warning:ベータソフトウェアです！
クライアントライブラリは2019年現在ベータ版であり、運用環境では使用しないでください。
:::
<!-- :::warning:Beta software -->
<!-- The client libraries are currently in beta and you should not use them in production environments. -->
<!-- ::: -->

## 対象読者
<!-- ## Audience -->

このドキュメントは、Javaプログラミング言語とオブジェクト指向プログラミングの概念に精通している開発者向けです。また、[バンドル](root://getting-started/0.1/transactions/bundles.md)、[トランザクション](root://getting-started/0.1/transactions/transactions.md)、[シード](root://getting-started/0.1/clients/seeds.md)や[アドレス](root://getting-started/0.1/clients/addresses.md)などの[基本的なIOTAの概念](root://getting-started/0.1/introduction/overview.md)にも精通している必要があります。
<!-- This documentation is for developers who are familiar with the Java programming language and object-oriented programming concepts. You should also be familiar with [basic IOTA concepts](root://getting-started/0.1/introduction/overview.md) such as [bundles](root://getting-started/0.1/transactions/bundles.md), [transactions](root://getting-started/0.1/transactions/transactions.md), [seeds](root://getting-started/0.1/clients/seeds.md), and [addresses](root://getting-started/0.1/clients/addresses.md). -->

## 前提条件
<!-- ## Prerequisites -->

このライブラリを使用するには、Java 6（以上）が必要です。
<!-- To use this library, you must have Java 6 (or later). -->

## ライブラリをインストールする
<!-- ## Install the library -->

IOTA Javaクライアントライブラリは[JitPack](https://jitpack.io/)で入手できます。
<!-- The IOTA Java client library is available on [JitPack](https://jitpack.io/). -->

### 依存関係
<!-- ### Dependencies -->

Javaライブラリは、次のサードパーティライブラリを使用します。
<!-- The Java library uses the following third-party libraries: -->

- [Retrofit 2](https://square.github.io/retrofit/) type-safe HTTP client: `2.3.0`
- [Simple Logging Facade for Java](http://www.slf4j.org/): `1.7.25`
- [Apache Commons Lang](http://commons.apache.org/proper/commons-lang/): `3.6`
- [Apache Commons IO](http://commons.apache.org/proper/commons-io/): `2.5`
- [Legion of the Bouncy Castle](https://www.bouncycastle.org/java.html) Java cryptography APIs: `1.58`

IOTA Javaクライアントライブラリとその依存関係をダウンロードするには、次のオプションのいずれかを使用できます。
<!-- To download the IOTA Java client library and its dependencies, you can use one of the following options: -->

- [Gradleでライブラリをダウンロードする](#install-the-library-with-gradle)
<!-- - Download the library with Gradle -->
- [Mavenでライブラリをダウンロードする](#install-the-library-with-maven)
<!-- - Download the library with Maven -->
- [ライブラリを手動でダウンロードする](#install-the-library-manually)
<!-- - Download the library manually -->

<a name="install-the-library-with-gradle"></a>
### Gradleでライブラリをダウンロードする
<!-- ### Install the library with Gradle -->

1. JitPackリポジトリをルートの`build.gradle`ファイルに追加します。
  <!-- 1. Add the JitPack repository to your root `build.gradle` file -->

    ```gradle
    allprojects {
        repositories {
            maven { url 'https://jitpack.io' }
        }
    }
    ```

2. iotaledger依存関係をモジュールの`build.gradle`ファイルに追加します。
  <!-- 2. Add the iotaledger dependency to your module `build.gradle` file -->

    ```gradle
    dependencies {
        compile 'com.github.iotaledger:iota-java:1.0.0-beta3'
    }
    ```

<a name="install-the-library-with-maven"></a>
### Mavenでライブラリをダウンロードする
<!-- ### Install the library with Maven -->

1. JitPackリポジトリをルートの`pom.xml`ファイルに追加します。
  <!-- 1. Add the JitPack repository to your root `pom.xml` file -->

    ```xml
    <repositories>
        <repository>
            <id>jitpack.io</id>
            <url>https://jitpack.io</url>
        </repository>
    </repositories>
    ```

2. iotaledger依存関係をモジュールの`pom.xml`ファイルに追加します。
  <!-- 2. Add the iotaledger dependency to your module `pom.xml` file -->

    ```xml
    <dependency>
        <groupId>com.github.iotaledger.iota-java</groupId>
        <artifactId>jota</artifactId>
        <classifier>jar-with-dependencies</classifier>
        <version>[VERSION_INFORMATION]</version>
    </dependency>
    ```

3. `<version>`タグの値をリリース番号またはGitコミットハッシュの最初の10文字に変更します：`<version>a98de8ea50</version>`または`<version>1.0.0-beta3</version>`
  <!-- 3. Change the value of the `<version>` tag to either a release number or the first 10 characters of a Git commit hash: `<version>a98de8ea50</version>` or `<version>1.0.0-beta3</version>` -->

:::info:
[JitPack](https://jitpack.io/#iotaledger/iota-java)ページで最新バージョンを見つけます。
:::
<!-- :::info: -->
<!-- Find the latest version on the [JitPack](https://jitpack.io/#iotaledger/iota-java) page. -->
<!-- ::: -->

<a name="install-the-library-manually"></a>
### ライブラリを手動でダウンロードする
<!-- ### Install the library manually -->

1. [GitHubリポジトリ](https://github.com/iotaledger/iota-java)をクローンするかダウンロードします。
  <!-- 1. Clone or download the [GitHub repository](https://github.com/iotaledger/iota-java) -->

    プロジェクト内には、次のディレクトリがあります。
    <!-- Inside the project, you'll have the following directories: -->
    * jota
    * jota-parent

2. プロジェクトのjotaディレクトリを参照します。
  <!-- 2. Reference the jota directory in your project -->

3. `jota`ディレクトリに依存関係をインストールします。
  <!-- 3. In the `jota` directory, install the dependencies -->

    ```bash
    mvn clean install
    ```

これで、ライブラリのバージョンに応じて、`jota-[VERSION]-jar-with-dependencies.jar`ファイルが作成されました。
<!-- Now, you have a `jota-[VERSION]-jar-with-dependencies.jar` file, depending on your version of the library. -->

## 始める
<!-- ## Get started -->

ライブラリをダウンロードした後、ノードに接続し、そこからタングルに関する情報をリクエストできます。
<!-- After you've downloaded the library, you can connect to a node and request information from it about the Tangle. -->

```java
IotaAPI api = new IotaAPI.Builder()
        .protocol("https")
        .host("nodes.devnet.thetangle.org")
        .port("443")
        .build();
GetNodeInfoResponse response = api.getNodeInfo();
```

IRIノード設定を実装から分離するには、IRIノード設定を[**Java `.properties`ファイル**](https://en.wikipedia.org/wiki/.properties)またはコマンドラインフラグとして指定することもできます。これらのオプションは、CIにデプロイされるオープンソースアプリケーションを開発し、貢献者に内部ノード構成を表示させたくない場合に役立ちます。
<!-- To separate your IRI node configuration from the implementation, you can also specify your IRI node configuration in a [**Java `.properties` file**](https://en.wikipedia.org/wiki/.properties) or as command line flags. These options are useful if you develop an open-source application that's deployed on a CI and don't want contributors to see the internal node configuration. -->

### `.properties`ファイルの例
<!-- ### Example .properties file -->

```java
iota.node.protocol=http
iota.node.host=127.0.0.1
iota.node.port=14265
```

ほとんどのAPI呼び出しは**同期**です。そのため、APIをバックグラウンドスレッドまたはワーカースレッドから呼び出して、APIがUIやメインスレッドなどの他のスレッドをブロックしないようにすることをお勧めします。
<!-- Most API calls are **synchronous**. As a result, we recommend that you call the API from a background thread or a worker thread to stop the API from blocking other threads such as the UI or the main thread. -->

## APIリファレンス
<!-- ## API reference -->

JavaライブラリのAPIコマンドの完全なリストについては、[iotaledgerのGitHubページ](https://iotaledger.github.io/iota-java/javadoc/jota/IotaAPICommands.html#enum.constant.summary)にアクセスしてください。
<!-- For a full list of API commands for the Java library, go to the [iotaledger GitHub page](https://iotaledger.github.io/iota-java/javadoc/jota/IotaAPICommands.html#enum.constant.summary). -->

最も一般的に使用されるAPI関数の一部を次に示します。
<!-- Here are some of the most commonly used API functions: -->

- [`getTransactionsObjects`](https://iotaledger.github.io/iota-java/javadoc/jota/IotaAPI.html#getTransactionsObjects-java.lang.String:A-)
- [`findTransactionObjects`](https://iotaledger.github.io/iota-java/javadoc/jota/IotaAPI.html#findTransactionObjects-java.lang.String:A-)
- [`getTransactionsObjects`](https://iotaledger.github.io/iota-java/javadoc/jota/IotaAPI.html#getTransactionsObjects-java.lang.String:A-)
- [`getLatestInclusion`](https://iotaledger.github.io/iota-java/javadoc/jota/IotaAPI.html#getLatestInclusion-java.lang.String:A-)
- [`broadcastAndStore`](https://iotaledger.github.io/iota-java/javadoc/jota/IotaAPI.html#broadcastAndStore-java.lang.String...-)
- [`getNewAddress`](https://iotaledger.github.io/iota-java/javadoc/jota/IotaAPI.html#getNewAddress-java.lang.String-int-int-boolean-int-boolean-)
- [`getInputs`](https://iotaledger.github.io/iota-java/javadoc/jota/IotaAPI.html#getInputs-java.lang.String-int-int-int-long-)
- [`prepareTransfers`](https://iotaledger.github.io/iota-java/javadoc/jota/IotaAPI.html#prepareTransfers-java.lang.String-int-java.util.List-java.lang.String-java.util.List-boolean-)
- [`sendTrytes`](https://iotaledger.github.io/iota-java/javadoc/jota/IotaAPI.html#sendTrytes-java.lang.String:A-int-int-)
- [`sendTransfer`](https://iotaledger.github.io/iota-java/javadoc/jota/IotaAPI.html#sendTransfer-java.lang.String-int-int-int-java.util.List-jota.model.Input:A-java.lang.String-)
- [`replayBundle`](https://iotaledger.github.io/iota-java/javadoc/jota/IotaAPI.html#replayBundle-java.lang.String-int-int-)
- [`getBundle`](https://iotaledger.github.io/iota-java/javadoc/jota/IotaAPI.html#getBundle-java.lang.String-)
- [`getTransfers`](https://iotaledger.github.io/iota-java/javadoc/jota/IotaAPI.html#getTransfers-java.lang.String-int-java.lang.Integer-java.lang.Integer-java.lang.Boolean-)
- [`initiateTransfer`](https://iotaledger.github.io/iota-java/javadoc/jota/IotaAPI.html#initiateTransfer-int-java.lang.String-java.lang.String-java.util.List-boolean-)
- [`getAccountData`](https://iotaledger.github.io/iota-java/javadoc/jota/IotaAPI.html#getAccountData-java.lang.String-int-int-boolean-int-boolean-int-int-boolean-long-)

## プロジェクトをサポートする
<!-- ## Support the project -->

Javaライブラリが役に立ち、貢献したいと思う場合は、[バグレポート](https://github.com/iotaledger/iota-java/issues/new-issue)、機能要求、または[プルリクエスト](https://github.com/iotaledger/iota-java/pulls/)の投稿を検討してください。コードベースの安定性と一貫性を保つために、[基本的な貢献ガイドライン](https://github.com/iotaledger/iota-java/blob/dev/CONTRIBUTING.md)があります。
<!-- If the Java library has been useful to you and you feel like contributing, consider posting a [bug report](https://github.com/iotaledger/iota-java/issues/new-issue), feature request or a [pull request](https://github.com/iotaledger/iota-java/pulls/). -->
<!-- We have some [basic contribution guidelines](https://github.com/iotaledger/iota-java/blob/dev/CONTRIBUTING.md) to keep our code base stable and consistent. -->

### ドキュメントを更新する
<!-- ### Update documentation -->

変更内容がドキュメントに影響する場合は、ドキュメントを更新してください。
<!-- If your changes affect the documentation, please update it. -->

## ディスカッションに参加する
<!-- ## Join the discussion -->

[Discord](https://discord.iota.org)に参加して、コミュニティに参加したり、助けを求めたり、技術について話し合ったりしてください。
<!-- Join our [Discord](https://discord.iota.org) to get involved in the community, ask for help, or to discuss the technology. -->
