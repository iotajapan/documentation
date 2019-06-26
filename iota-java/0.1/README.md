# IOTA Javaクライアントライブラリ
<!-- # IOTA Java client library -->

これは**公式**のJavaクライアントライブラリで、次のことができます。
<!-- This is the **official** Java client library, which allows you to do the following: -->
- アカウントを作成、インポート、エクスポート、および管理する
<!-- - Create, import, export, and manage accounts -->
- トランザクションを送信する
<!-- - Send transactions -->
- ペンディングトランザクションの促進と再添付を行う
<!-- - Promote and reattach pending transactions -->
- 条件付預け入れアドレス（CDA）への預け入れをリクエストする
<!-- - Request deposits into conditional deposit addresses (CDA) -->
- イベントをリッスンする
<!-- - Listen to events -->
- プラグインでライブラリ機能を拡張する
<!-- - Extend the library functionality with plug-ins -->
- IRIノードと対話する
<!-- - Interact with an IRI node -->

ライブラリの機能の詳細については、以下をご覧ください。
<!-- To learn more about how the library works: -->

- [アカウントを作成する方法](root://iota-java/0.1/how-to-guides/create-account.md)を確認する。
<!-- - See how you can [work with accounts](root://iota-java/0.1/how-to-guides/create-account.md). -->
- [トランザクションの送受信方法](root://iota-java/0.1/how-to-guides/create-and-manage-cda.md)を確認する。
<!-- - See how you can [send and receive](root://iota-java/0.1/how-to-guides/create-and-manage-cda.md) transactions. -->

[IOTA Java GitHubレポジトリ](https://github.com/iotaledger/iota-java)でライブラリを入手してください。
<!-- Get the library at the [IOTA Java GitHub repository](https://github.com/iotaledger/iota-java). -->

:::warning:ベータソフトウェア
クライアントライブラリは現在ベータ版です。本番環境での使用はサポートされていません。
:::
<!-- :::warning:Beta software -->
<!-- The client libraries are currently in beta. Their use in production is not supported. -->
<!-- ::: -->

問題があれば[issueトラッカー](https://github.com/iotaledger/iota-java/issues/new-issue)で報告してください。
<!-- Please report any issues in our [issue tracker](https://github.com/iotaledger/iota-java/issues/new-issue). -->

## 対象読者
<!-- ## Audience -->

このドキュメントは、Javaプログラミング言語とオブジェクト指向プログラミングの概念に精通している人を対象にしています。[アドレスの再利用](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse)、[バンドルとトランザクション](root://iota-basics/0.1/concepts/bundles-and-transactions.md)など、IOTAの基本概念にも精通している必要があります。
<!-- This documentation is designed for people who are familiar with the Java programming language and object-oriented programming concepts. You should also be familiar with basic IOTA concepts such as [address reuse](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse), [bundles, and transactions](root://iota-basics/0.1/concepts/bundles-and-transactions.md). -->

このガイドは、IOTAを使用したアプリケーションの探求と開発を迅速に開始できるようにすることを目的としています。
<!-- This guide is designed to let you quickly start exploring and developing applications with IOTA. -->

## 前提条件
<!-- ## Prerequisites -->

IOTA Javaクライアントライブラリを使用するには、コンピュータに次の最低条件が必要です。
<!-- To use the IOTA Java client library, your computer must have the following minimum requirement: -->

* Java 6 (もしくは6以上)
<!-- * Java 6 (or higher) -->

## ライブラリをダウンロードする
<!-- ## Download the library -->

IOTA Javaクライアントライブラリは[JitPack](https://jitpack.io/)で入手できます。
<!-- The IOTA Java client library is available on [JitPack](https://jitpack.io/). -->

### 依存関係
<!-- ### Dependencies -->

IOTA Javaクライアントライブラリは、次のサードパーティライブラリを使用します。
<!-- The IOTA Java client library uses the following third-party libraries: -->

* [Retrofit 2](https://square.github.io/retrofit/) 型安全なHTTPクライアント: `2.3.0`
<!-- * [Retrofit 2](https://square.github.io/retrofit/) type-safe HTTP client: `2.3.0` -->
* [Simple Logging Facade for Java](http://www.slf4j.org/): `1.7.25`
* [Apache Commons Lang](http://commons.apache.org/proper/commons-lang/): `3.6`
* [Apache Commons IO](http://commons.apache.org/proper/commons-io/): `2.5`
* [Legion of the Bouncy Castle](https://www.bouncycastle.org/java.html) Java暗号化APIs: `1.58`
<!-- * [Legion of the Bouncy Castle](https://www.bouncycastle.org/java.html) Java cryptography APIs: `1.58` -->

IOTA Javaクライアントライブラリとその依存関係をダウンロードするには、次のいずれかのオプションを使用できます。
<!-- To download the IOTA Java client library and its dependencies, you can use one of the following options: -->

* Gradleでライブラリをダウンロードする
<!-- * Download the library with Gradle -->
* Mavenでライブラリをダウンロードする
<!-- * Download the library with Maven -->
* 手動でライブラリをダウンロードする
<!-- * Download the library manually -->

### Gradleでライブラリをダウンロードする
<!-- ### Download the library with Gradle -->

1. ルートの`build.gradle`ファイルにJitPackリポジトリを追加する。
  <!-- 1. Add the JitPack repository to your root `build.gradle` file -->

    ```gradle
    allprojects {
        repositories {
            maven { url 'https://jitpack.io' }
        }
    }
    ```

2. モジュールの`build.gradle`ファイルにiotaledger依存関係を追加する。
  <!-- 2. Add the iotaledger dependency to your module `build.gradle` file -->

    ```gradle
    dependencies {
        compile 'com.github.iotaledger:iota-java:1.0.0-beta3'
    }
    ```

### Mavenでライブラリをダウンロードする
<!-- ### Download the library with Maven -->

1. ルートの`pom.xml`ファイルにJitPackリポジトリを追加する。
  <!-- 1. Add the JitPack repository to your root `pom.xml` file -->
    ```xml
    <repositories>
        <repository>
            <id>jitpack.io</id>
            <url>https://jitpack.io</url>
        </repository>
    </repositories>
    ```

2. モジュールの`pom.xml`ファイルにiotaledger依存関係を追加する。
  <!-- 2. Add the iotaledger dependency to your module `pom.xml` file -->
    ```xml
    <dependency>
        <groupId>com.github.iotaledger.iota-java</groupId>
        <artifactId>jota</artifactId>
        <classifier>jar-with-dependencies</classifier>
        <version>[VERSION_INFORMATION]</version>
    </dependency>
    ```

3. `<version>タグ</version>`の値をリリース番号またはGitコミットハッシュの最初の10文字のいずれかに変更する。
<!-- 3. Change the value of the `<version>` tag to either a release number or the first 10 characters of a Git commit hash: -->
`<version>a98de8ea50</version>`または`<version>1.0.0-beta3</version>`など。
<!-- `<version>a98de8ea50</version>` or `<version>1.0.0-beta3</version>` -->

**注釈：** [JitPack](https://jitpack.io/#iotaledger/iota-java)のページで最新バージョンを見つけてください。
<!-- **Note:** Find the latest version on the [JitPack](https://jitpack.io/#iotaledger/iota-java) page. -->

### 手動でライブラリをダウンロードする
<!-- ### Download the library manually -->

1. [GitHubリポジトリ](https://github.com/iotaledger/iota-java)をクローンまたはダウンロードする。
  <!-- 1. Clone or download the [GitHub repository](https://github.com/iotaledger/iota-java) -->

  プロジェクト内には、次のディレクトリがあります。
  <!-- Inside the project, you'll have the following directories: -->
  * jota
  * jota-parent

2. プロジェクト内のjotaディレクトリを参照する。
  <!-- 2. Reference the jota directory in your project -->

3. jotaディレクトリで、次のコマンドを実行する。
  <!-- 3. In the jota directory, run the following command: -->

    `mvn clean install`

ライブラリのバージョンに応じて、`jota-[VERSION]-jar-with-dependencies.jar`という.jarファイルがあります。
<!-- You'll have a .jar file called `jota-[VERSION]-jar-with-dependencies.jar`, depending on your version of the library. -->

## はじめに
<!-- ## Get Started -->

ライブラリをダウンロードしたら、IRIノードに接続してトランザクションを送信したり、台帳と対話したりできます。
<!-- After you've downloaded the library, you can connect to an IRI node to send transactions to it and interact with the ledger. -->

ローカルIRIノードに接続するには、以下の手順に従います。
<!-- To connect to a local IRI node, do the following: -->

```java
IotaAPI api = new IotaAPI.Builder().build();
GetNodeInfoResponse response = api.getNodeInfo();
```

リモートIRIノードに接続するには、以下の手順に従います。
<!-- To connect to a remote IRI node, do the following: -->

```java
IotaAPI api = new IotaAPI.Builder()
        .protocol("http")
        .host("URL OF THE REMOTE IRI NODE")
        .port("14265")
        .build();
GetNodeInfoResponse response = api.getNodeInfo();
```

**注釈：** IRIノードの設定を実装から分離するために、[**Javaの.propertiesファイル**](https://en.wikipedia.org/wiki/.properties)やコマンドラインのフラグとしてIRIノードの設定を指定することもできます。これらのオプションは、CIにデプロイされていてコントリビュータにIRIノードの内部構成を見せたくないオープンソースアプリを開発する場合に役立ちます。
<!-- **Note:** To separate your IRI node configuration from the implementation, you can also specify your IRI node configuration in a [**Java `.properties` file**](https://en.wikipedia.org/wiki/.properties) or as command line flags. These options are useful if you develop an open-source app which is deployed on a CI and don't want contributors to see the internal IRI node configuration. -->

**.propertiesファイルの例**
<!-- **Example .properties files** -->

```java
iota.node.protocol=http
iota.node.host=127.0.0.1
iota.node.port=14265
```

ほとんどのAPI呼び出しは**同期的**です。そのため、バックグラウンドスレッドまたはワーカースレッドからAPIを呼び出して、APIがUIやメインスレッドなどの他のスレッドをブロックしないようにすることをお勧めします。
<!-- Most API calls are **synchronous**. Therefore, we recommend that you call the API from a background thread or a worker thread to stop the API from blocking other threads such as the UI or the main thread. -->

## APIリファレンス
<!-- ## API reference -->

IOTA Javaクライアントライブラリ用のAPIコマンドの完全な一覧については、[GitHubページ](https://iotaledger.github.io/iota-java/javadoc/jota/IotaAPICommands.html#enum.constant.summary)にアクセスしてください。
<!-- For a full list of API commands for the IOTA Java client library, go to the [GitHub page](https://iotaledger.github.io/iota-java/javadoc/jota/IotaAPICommands.html#enum.constant.summary). -->

最も一般的に使用されるAPI関数は次のとおりです。
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

## 例
<!-- ## Examples -->

IOTAを使用してアプリケーションを開発する際の参考として使用できるテストケースの一覧が[`src/test/java`ディレクトリ](https://github.com/iotaledger/iota-java/blob/dev/jota/src/test)にあります。
<!-- We have a list of test cases in the [`src/test/java` directory](https://github.com/iotaledger/iota-java/blob/dev/jota/src/test) that you can use as a reference when developing apps with IOTA. -->
良いスターターは[`IotaAPITest`のケース](https://github.com/iotaledger/iota-java/blob/dev/jota/src/test/java/org/iota/jota/IotaAPITest.java)です。
<!-- A good place to start is the [`IotaAPITest` case](https://github.com/iotaledger/iota-java/blob/dev/jota/src/test/java/org/iota/jota/IotaAPITest.java). -->

## 変更ログ
<!-- ## Change logs -->

- Changes in [**1.0.0-beta3**](https://github.com/iotaledger/iota-java/compare/1.0.0-beta2...1.0.0-beta3)
- Changes in [**1.0.0-beta2**](https://github.com/iotaledger/iota-java/compare/1.0.0-beta1...1.0.0-beta2)
- Changes in [**1.0.0-beta1**](https://github.com/iotaledger/iota-java/compare/0.9.10...1.0.0-beta1)
- Changes in [**v0.9.10**](https://github.com/iotaledger/iota-java/compare/v0.9.6...0.9.10)
- Changes in [**v0.9.6**](https://github.com/iotaledger/iota-java/compare/v0.9.5...v0.9.6)
- Changes in [**v0.9.5**](https://github.com/iotaledger/iota-java/compare/v0.9.4...v0.9.5)
- Changes in [**v0.9.4**](https://github.com/iotaledger/iota-java/compare/v0.9.3...v0.9.4)
- Changes in [**v0.9.3**](https://github.com/iotaledger/iota-java/compare/v0.9.2...v0.9.3)
- Changes in [**v0.9.2**](https://github.com/iotaledger/iota-java/compare/v0.9.1...v0.9.2)
- Changes in [**v0.9.1**](https://github.com/iotaledger/iota-java/commits/v0.9.1)

## プロジェクトを支援する
<!-- ## Support the project -->

IOTA Javaクライアントライブラリが役に立ち、貢献したいと思う場合は、[バグレポート](https://github.com/iotaledger/iota-java/issues/new-issue)、[機能リクエスト](https://github.com/iotaledger/iota-java/issues/new-issue)、または[プルリクエスト](https://github.com/iotaledger/iota-java/pulls/)を投稿することをご検討ください。
<!-- If the IOTA Java client library has been useful to you and you feel like contributing, consider posting a [bug report](https://github.com/iotaledger/iota-java/issues/new-issue), feature request or a [pull request](https://github.com/iotaledger/iota-java/pulls/). -->
コードベースの安定性と一貫性を保つための[基本的な貢献ガイドライン](https://github.com/iotaledger/iota-java/blob/dev/CONTRIBUTING.md)がいくつかあります。
<!-- We have some [basic contribution guidelines](https://github.com/iotaledger/iota-java/blob/dev/CONTRIBUTING.md) to keep our code base stable and consistent. -->

## ディスカッションに参加する
<!-- ## Join the Discussion -->

コミュニティに参加したい場合、セットアップの手助けが必要な場合、ライブラリに関する問題がある場合、あるいはブロックチェーン、分散型台帳、およびIoTについて他の人と話したい場合は、遠慮なく[Discord](https://discordapp.com/invite/fNGZXvh)に参加してください。
<!-- If you want to get involved in the community, need help with getting setup, have any issues related with the library or just want to discuss blockchain, distributed ledgers, and IoT with other people, feel free to join our [Discord](https://discordapp.com/invite/fNGZXvh). -->

## ライセンス
<!-- ## License -->

Apache 2.0ライセンスは[ここ](https://github.com/iotaledger/iota-java/blob/dev/LICENSE)にあります。
<!-- The Apache 2.0 license can be found [here](https://github.com/iotaledger/iota-java/blob/dev/LICENSE). -->
