# Java クイックスタート
<!-- # Java quickstart -->

**このクイックスタートでは、開発環境の設定からタングルでのライブトランザクションのリッスンまで、Java での IOTA 開発の基本を学びます。**
<!-- **In this quickstart, you learn the basics of IOTA development in Java, from setting up a development environment to listening for live transaction on the Tangle.** -->

このクイックスタートでは、次の方法を学習します。
<!-- In this quickstart, you will learn how to: -->

1. 開発環境のセットアップをする
<!-- 1. Set up a developer environment -->

2. パッケージをインストールする
<!-- 2. Install packages -->

3. ノードに接続する
<!-- 3. Connect to a node -->

## 手順1. 開発環境をセットアップする
<!-- ## Step 1. Set up a developer environment -->

Java クライアントライブラリを使用するには、開発環境を構成するプログラミングツールのセットが必要です。
<!-- To use the Java client library, you need a set of programming tools, which make up a development environment. -->

1. コマンドラインインターフェイスを開き、プロジェクトのディレクトリに移動します。
  <!-- 1. Open a command-line interface and change into your project's directory -->

    オペレーティングシステムに応じて、コマンドラインインターフェイスは[ Windows の PowerShell ](https://docs.microsoft.com/en-us/powershell/scripting/getting-started/getting-started-with-windows-powershell?view=powershell-6)、[Linux ターミナル](https://www.howtogeek.com/140679/beginner-geek-how-to-start-using-the-linux-terminal/)、または[macOS のターミナル](https://macpaw.com/how-to/use-terminal-on-mac)になります。
    <!-- Depending on your operating system, a command-line interface could be [PowerShell in Windows](https://docs.microsoft.com/en-us/powershell/scripting/getting-started/getting-started-with-windows-powershell?view=powershell-6), the [Linux Terminal](https://www.howtogeek.com/140679/beginner-geek-how-to-start-using-the-linux-terminal/) or [Terminal for macOS](https://macpaw.com/how-to/use-terminal-on-mac). -->

2. Java Development Kit（JDK）がインストールされているかどうかわからない場合は、次のコマンドを実行して、使用しているバージョンを確認します。
  <!-- 2. If you don't know if you have the Java Development Kit (JDK) installed, run the following command to see which version you have: -->

    ```bash
    javac -version
    ```

    次の出力に類似した何かを見るはずです。
    <!-- You should see something similar to this output: -->

    ```bash
    javac 1.8.0_92
    ```

JDK がない場合は、先に進む前に JDK をインストールする必要があります。[OpenJDK ダウンロードページ](https://openjdk.java.net/install/)から、自分のプラットフォーム（Windows、Mac、Linux）に適合する Java OpenJDK をインストールするための指示に従ってください。
<!-- If you don't have the JDK, you'll need to install it before going any further. Follow the directions for installing the Java OpenJDK for your platform (Windows, Mac, Linux) from the [OpenJDK download page](https://openjdk.java.net/install/). -->

これで、パッケージのインストールを開始する準備ができました。
<!-- Now you're ready to start installing packages. -->

## 手順2. パッケージをインストールする
<!-- ## Step 2. Install packages -->

Java クライアントライブラリは、関連するメソッドを含むパッケージで構成されています。
<!-- The Java client library is organized in packages, which contain related methods. -->

IOTA Javaクライアントライブラリとその依存関係をインストールするには、次のオプションのいずれかを使用できます。
<!-- To install the IOTA Java client library and its dependencies, you can use one of the following options: -->

- Gradle でライブラリをインストールする
<!-- - Install the library with Gradle -->
- Maven でライブラリをインストールする
<!-- - Install the library with Maven -->
- 手動でライブラリをインストールする
<!-- - Install the library manually -->

Java 統合開発環境（IDE）を使用してアプリケーションを構築している場合、依存関係のグループID、アーティファクトID、およびバージョン番号を指定できます。
<!-- If you are using a Java integrated development environment (IDE) to build your application, you can specify the group ID, artifact ID, and version number for your dependencies. -->

使用しているパッケージマネージャーに関係なく、[JitPack](https://jitpack.io/#iotaledger/iota-java) ページにある Jota ライブラリの最新バージョンを指定することを忘れないでください。
<!-- Regardless of the package manager you are using, remember to specify the latest version of the Jota library, which is on the [JitPack](https://jitpack.io/#iotaledger/iota-java) page. -->

--------------------
### Gradle

[Gradle](https://gradle.org/install/) ビルドツールを使用するには、Gradle をインストールする必要があります。

1. JitPack リポジトリをルートの `build.gradle` ファイルに追加します。

    ```gradle
    allprojects {
        repositories {
            maven { url 'https://jitpack.io' }
        }
    }
    ```

2. モジュールの `build.gradle` ファイルに iotaledger 依存関係を追加します。

    ```gradle
    dependencies {
        compile 'com.github.iotaledger:iota-java:[VERSION]'
    }
    ```
---
### Maven

[Maven](https://maven.apache.org/download.cgi) ビルドツールを使用するには、Maven をインストールする必要があります。

1. JitPack リポジトリをルートの `pom.xml` ファイルに追加します。

    ```xml
    <repositories>
        <repository>
            <id>jitpack.io</id>
            <url>https://jitpack.io</url>
        </repository>
    </repositories>
    ```

2. iotaledger 依存関係をモジュールの `pom.xml` ファイルに追加します。`[VERSION]`プレースホルダーを、`1.0.0-beta3`のような最新のリリース番号、または`a98de8ea50`のような Git コミットハッシュの最初の10文字に置き換えます。

    ```xml
    <dependency>
      <groupId>org.iota</groupId>
      <artifactId>jota</artifactId>
      <version>[VERSION]</version>
    </dependency>
    ```
---
### 手動でインストール

1. [GitHub リポジトリ](https://github.com/iotaledger/iota-java)をクローンまたはダウンロードします。

プロジェクト内には、`jota` および `jota-parent` ディレクトリがあります。

2. プロジェクトの `jota` ディレクトリを参照します。

3. `jota` ディレクトリに依存関係をインストールします。

    ```bash
    mvn install -DskipTests
    ```

    次のようなものが表示されるはずです。

    ```bash
    [INFO] ーーーーーーーーーーーー
    [INFO] BUILD SUCCESS
    [INFO] ーーーーーーーーーーーー
    [INFO] Total time:  03:41 min
    [INFO] Finished at: 2019-11-27T10:14:45Z
    [INFO] ーーーーーーーーーーーー
    ```

これで、ライブラリのバージョンに応じて、`jota-[VERSION]-jar-with-dependencies.jar` ファイルが作成されます。
--------------------

これで、コーディングを開始できます。
<!-- Now you can start coding. -->

## 手順3. ノードに接続する
<!-- ## Step 3. Connect to a node -->

トランザクションの送信を開始する前に、[同期済みノード](root://getting-started/0.1/network/nodes.md#synchronized-nodes)に接続していることを確認することをお勧めします。同期済みノードに接続することで、[タングル](root://getting-started/0.1/network/the-tangle.md)の最新の概観を知ることが出来ます。
<!-- It's best practice to make sure that you're connected to a [synchronized node](root://getting-started/0.1/network/nodes.md#synchronized-nodes) before you start sending transactions to it. This way, you know that it has an up-to-date view of [the Tangle](root://getting-started/0.1/network/the-tangle.md). -->

ノードに接続するたびに、どの[IOTAネットワーク](root://getting-started/0.1/network/iota-networks.md)に接続しているかを知る必要があります。ここでは、テストに使用できるIOTAネットワークであるDevnetのノードに接続します。
<!-- Whenever you connect to a node, you need to know which [IOTA network](root://getting-started/0.1/network/iota-networks.md) it's in. Here, we connect to a node on the Devnet, which is the IOTA networks that you can use for testing. -->

1\. IOTA財団の[公式 Discord] に移動し、`botbox` チャンネルに **!milestone** と入力します。
<!-- 1\. Go to the IOTA Foundation [Discord](https://discord.iota.org) and enter **!milestone** in the `botbox` channel -->

![Entering !milestone on Discord](../images/discord-milestone-check.PNG)

Discord ボットは、[ノードクォーラム](root://getting-started/0.1/network/nodes.md#node-quorum)から現在の `latestMilestoneIndex` フィールドを返します。
<!-- The Discord bot should return the current `latestMilestoneIndex` field from a [node quorum](root://getting-started/0.1/network/nodes.md#node-quorum). -->

2\. `ConnectToNode.java` という新しいファイルを作成します。
<!-- 2\. Create a new file called `ConnectToNode.java` -->

3\. ノードが同期されているかどうかを確認するには、次のコードをコピーして `ConnectToNode.java` ファイルにペーストします。
<!-- 3\. To check if your node is synchronized, copy and paste the following code into the `ConnectToNode.java` file -->

```java
package com.iota;

import org.iota.jota.IotaAPI;
import org.iota.jota.dto.response.GetNodeInfoResponse;

class ConnectToNode {
public static void main(String[] args) throws ArgumentException {

        // API オブジェクトの新しいインスタンスを作成し、
        // 指定するノードに接続します
        IotaAPI api = new IotaAPI.Builder()
            .protocol("https")
            .host("nodes.devnet.thetangle.org")
            .port(443)
            .build();

        // ノードとタングルに関する情報を得るために `getNodeInfo()` メソッドを呼び出します
        GetNodeInfoResponse response = api.getNodeInfo();
        System.out.println(response);
    }
}
```

4\. ライブラリを手動でインストールした場合は、変更を保存し、コマンドラインからこの Java クラスをコンパイルします。
<!-- 4\. If you installed the library manually, save your changes and compile this Java class from the command line -->

```bash
javac -cp jota-[VERSION]-jar-with-dependencies.jar ConnectToNode.java
```

5\. ライブラリを手動でインストールした場合は、コマンドラインからコンパイルされたコードを実行します。
<!-- 5\. If you installed the library manually, run the compiled code from the command line -->

--------------------
### macOS と Linux
```bash
java -cp .:jota-[VERSION]-jar-with-dependencies.jar ConnectToNode
```
---
### Windows
```bash
java -cp .;jota-[VERSION]-jar-with-dependencies.jar ConnectToNode
```
--------------------

ノードは次を返します。
<!-- The node returns the following: -->

```json
{
    "appName": "IRI Testnet",
    "appVersion": "1.5.6-RELEASE",
    "jreAvailableProcessors": 8,
    "jreFreeMemory": 12052395632,
    "jreVersion": "1.8.0_181",
    "jreMaxMemory": 22906667008,
    "jreTotalMemory": 16952328192,
    "latestMilestone": "FPRSBTMKOP9JTTQSHWRGMPT9PBKYWFCCFLZLNWQDFRCXDDHZEFIEDXRIJYIMVGCXYQRHSZQYCTWXJM999",
    "latestMilestoneIndex": 1102841,
    "latestSolidSubtangleMilestone": "FPRSBTMKOP9JTTQSHWRGMPT9PBKYWFCCFLZLNWQDFRCXDDHZEFIEDXRIJYIMVGCXYQRHSZQYCTWXJM999",
    "latestSolidSubtangleMilestoneIndex": 1102841,
    "milestoneStartIndex": 434525,
    "neighbors": 3,
    "packetsQueueSize": 0,
    "time": 1549482118137,
    "tips": 153,
    "transactionsToRequest": 0,
    "features": ["snapshotPruning", "dnsRefresher", "testnet", "zeroMessageQueue", "tipSolidification", "RemotePOW"],
    "coordinatorAddress": "EQQFCZBIHRHWPXKMTOLMYUYPCN9XLMJPYZVFJSAY9FQHCCLWTOLLUGKKMXYFDBOOYFBLBI9WUEILGECYM",
    "duration": 0
}
```

### レスポンスオブジェクトの内容
<!-- ### Reading the response object -->

レスポンスオブジェクトの `latestMilestoneIndex` フィールドが Discord から取得した `latestMilestoneIndex` フィールドと `latestSolidSubtangleMilestoneIndex` フィールドに等しい場合、ノードは同期しています。
<!-- If the `latestMilestoneIndex` field is equal to the one you got from Discord and the `latestSolidSubtangleMilestoneIndex` field, the node is synchronized. -->

そうでない場合は、別のノードに接続してみてください。[iota.dance Web サイト](https://iota.dance/)には、メインネットノードの一覧が含まれています。または、[自分自身のノードを実行](root://node-software/0.1/iri/how-to-guides/quickstart.md)できます。
<!-- If not, try connecting to a different node. The [iota.dance website](https://iota.dance/) includes a list of Mainnet nodes. Or, you can [run your own node](root://node-software/0.1/iri/how-to-guides/quickstart.md). -->

レスポンスオブジェクトの `features` 配列で、このノードは[リモートプルーフオブワーク](root://getting-started/0.1/transactions/proof-of-work.md)（RemotePOW）もサポートしていることがわかります。その結果、このノードを使用して、ローカルデバイスでプルーフオブワークを実行する代わりに、ノードでプルーフオブワークを行うことができます。
<!-- In the `features` array, you can see that this node also support [remote proof of work](root://getting-started/0.1/transactions/proof-of-work.md) (RemotePOW). As a result, you can use this node to do proof of work instead of doing it on your local device. -->

また、このノードではゼロメッセージキュー（ZMQ）が有効になっているため、ZMQ を使用してライブトランザクションをリッスンできます。
<!-- Also, this node has its zero message queue (ZMQ) enabled, so you can use it to listen for live transactions. -->

これらのフィールドの詳細については、[IRI API リファレンス](root://node-software/0.1/iri/references/api-reference.md#getNodeInfo)を参照してください。
<!-- For more information about these fields, see the [IRI API reference](root://node-software/0.1/iri/references/api-reference.md#getNodeInfo). -->

:::success: おめでとうございます:tada:
同期済みノードへの接続を確認できました。
:::
<!-- :::success: Congratulations :tada: -->
<!-- You've confirmed your connection to a synchronized node. -->
<!-- ::: -->

## プロジェクトをサポートする
<!-- ## Support the project -->

Java ライブラリが役に立ち、貢献したいと思うなら、[バグレポート](https://github.com/iotaledger/iota-java/issues/new-issue)、機能リクエスト、または[プルリクエスト](https://github.com/iotaledger/iota-java/pulls/)の投稿を検討してください。
<!-- If the Java library has been useful to you and you feel like contributing, consider posting a [bug report](https://github.com/iotaledger/iota-java/issues/new-issue), feature request or a [pull request](https://github.com/iotaledger/iota-java/pulls/). -->

コードベースの安定性と一貫性を保つために、[基本的な貢献ガイドライン](https://github.com/iotaledger/iota-java/blob/dev/CONTRIBUTING.md)があります。
<!-- We have some [basic contribution guidelines](https://github.com/iotaledger/iota-java/blob/dev/CONTRIBUTING.md) to keep our code base stable and consistent. -->

変更内容がドキュメントに影響する場合は、ドキュメントを更新してください。
<!-- If your changes affect the documentation, please update it. -->

## 参加する
<!-- ## Get involved -->

[Discordチャンネル](https://discord.iota.org)では、次のことができます。
<!-- [Join our Discord channel](https://discord.iota.org) where you can: -->

- IOTA開発者およびコミュニティとの議論に参加することができます。
<!-- - Take part in discussions with IOTA developers and the community -->
- 助けを求めることができます。
<!-- - Ask for help -->
- 他の人を助けるためにあなたの知識を共有することができます。
<!-- - Share your knowledge to help others -->

次のような多くのチャネルがあります。
<!-- We have many channels, including the following: -->

- `*-dev`：これらのチャンネルは読み取り専用であり、開発者が互いにトピックを議論したり、GitHubからのコードの更新を確認したりできます。
<!-- - `-dev`: These channels are read-only and are where developers discuss topics with each other and where you can see any code updates from GitHub. -->

- `*-discussion`：これらのチャンネルはあなたが参加できる場所です。
<!-- - `-discussion`: These channels are where you can participate. -->

## 次のステップ
<!-- ## Next steps -->

[開発者ハンドブック](root://getting-started/0.1/references/quickstart-dev-handbook.md)を読んで、自分自身のノードを実行するかどうか、プライベートIOTAネットワークが必要かどうか、および両方が必要かどうかの参考にしてください。
<!-- Read our [developer's handbook](root://getting-started/0.1/references/quickstart-dev-handbook.md) for guidance on whether you should run your own node, whether you need a private IOTA network, and what you need to consider for both. -->
