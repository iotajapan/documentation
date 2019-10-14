# アカウントモジュール入門
<!-- # Get started with the account module -->

**アカウントは、トランザクションの送受信を容易にするオブジェクトです。アカウントは、アドレスやペンディング中のバンドルハッシュなどのデータをローカルデータベースに保存します。このデータにより、使用済みアドレスからの取り出しやペンディング中のトランザクションの促進と再添付を心配することなく、IOTAネットワークと対話できます。**
<!-- **An account is an object that makes it easier to send and receive transactions. Accounts store data such as addresses and pending bundle hashes in a local database. This data allows you to interact with an IOTA network without worrying about withdrawing from spent addresses or promoting and reattaching pending transactions.** -->

アカウントはIOTAプロトコルの複雑さを抽象化し、支払いに集中できるようにします。アカウントでは、支払いは次の2つのタイプのいずれかになります。
<!-- Accounts abstract the complexity of the IOTA protocol and allow you to focus on making payments. In accounts, a payment can be one of two types: -->

* **入金：**アカウントにIOTAトークンを預け入れるバンドル
* **支払：**アカウントからIOTAトークンを取り出すバンドル
<!-- * **Incoming payment:** A bundle that deposits IOTA tokens into an account -->
<!-- * **Outgoing payment:** A bundle that withdraws IOTA tokens from an account -->

:::warning:ベータソフトウェアです！
クライアントライブラリは2019年現在ベータ版であり、運用環境では使用しないでください。
:::
<!-- :::warning:Beta software -->
<!-- The client libraries are currently in beta and you should not use them in production environments. -->
<!-- ::: -->

## 対象読者
<!-- ## Audience -->

このドキュメントは、Javaプログラミング言語とオブジェクト指向プログラミングの概念に精通している開発者向けに設計されています。 また、[アドレスの再利用](root://dev-essentials/0.1/concepts/addresses-and-signatures.md#address-reuse)、[バンドル、トランザクション](root://dev-essentials/0.1/concepts/bundles-and-transactions.md)などのIOTAの基本的な概念にも精通している必要があります。
<!-- This documentation is designed for developers who are familiar with the Java programming language and object-oriented programming concepts. You should also be familiar with basic IOTA concepts such as [address reuse](root://dev-essentials/0.1/concepts/addresses-and-signatures.md#address-reuse), [bundles, and transactions](root://dev-essentials/0.1/concepts/bundles-and-transactions.md). -->

## 前提条件
<!-- ## Prerequisites -->

このライブラリを使用するには、Java 6（以上）が必要です。
<!-- To use this library, you must have Java 6 (or later). -->

## ライブラリをインストールする
<!-- ## Install the library -->

IOTA Javaクライアントライブラリは[JitPack](https://jitpack.io/)で入手できます。
<!-- The IOTA Java client library is available on [JitPack][https://jitpack.io/]. -->

### 依存関係
<!-- ### Dependencies -->

Javaライブラリは、次のサードパーティライブラリを使用します。
<!-- The Java library uses the following third-party libraries: -->

* [Retrofit 2](https://square.github.io/retrofit/) type-safe HTTP client: `2.3.0`
* [Simple Logging Facade for Java](http://www.slf4j.org/): `1.7.25`
* [Apache Commons Lang](http://commons.apache.org/proper/commons-lang/): `3.6`
* [Apache Commons IO](http://commons.apache.org/proper/commons-io/): `2.5`
* [Legion of the Bouncy Castle](https://www.bouncycastle.org/java.html) Java cryptography APIs: `1.58`

IOTA Javaクライアントライブラリとその依存関係をダウンロードするには、次のオプションのいずれかを使用できます。
<!-- To download the IOTA Java client library and its dependencies, you can use one of the following options: -->

* [Gradleでライブラリをダウンロードする](#install-the-library-with-gradle)
<!-- * Download the library with Gradle -->
* [Mavenでライブラリをダウンロードする](#install-the-library-with-maven)
<!-- * Download the library with Maven -->
* [ライブラリを手動でダウンロードする](#install-the-library-manually)
<!-- * Download the library manually -->

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

ライブラリをインストールしたら、[新しいアカウントを作成](../how-to-guides/create-account.md)できます。
<!-- After you've installed the library, you can [create a new account](../how-to-guides/create-account.md). -->

ほとんどのAPI呼び出しは**同期**です。そのため、APIをバックグラウンドスレッドまたはワーカースレッドから呼び出して、APIがUIやメインスレッドなどの他のスレッドをブロックしないようにすることをお勧めします。
<!-- Most API calls are **synchronous**. As a result, we recommend that you call the API from a background thread or a worker thread to stop the API from blocking other threads such as the UI or the main thread. -->

## APIリファレンス
<!-- ## API reference -->

JavaライブラリのAPIコマンドの完全なリストについては、[iotaledgerのGitHubページ](https://github.com/iotaledger/iota-java/tree/dev/jota/src/main/java/org/iota/jota/account)にアクセスしてください。
<!-- For a full list of API commands for the Java library, go to the [iotaledger GitHub page](https://github.com/iotaledger/iota-java/tree/dev/jota/src/main/java/org/iota/jota/account). -->

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
