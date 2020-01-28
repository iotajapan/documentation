# IRI ノードを実行する
<!-- # Run IRI -->

**このガイドでは，デバイスを IOTA ネットワーク上のノードに変える IRI ノードソフトウェアをインストールして実行します．**
<!-- **In this guide, you install and run the IRI node software, which turns your device into a node on an IOTA network.** -->

IRI を実行するには2つのオプションがあります．
<!-- You have two options for running IRI: -->

- Docker コンテナで IRI を実行する（Linux, macOS, Windows 用）
<!-- - Run IRI in a Docker container (for Linux, macOS, and Windows) -->
- Linux Ubuntu サーバーでIRIをビルドして実行する
<!-- - Build and run IRI on a Linux Ubuntu server -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完成するには，次のものが必要です．
<!-- To complete this guide, you need the following: -->

- 4GB RAM
- 64ビットプロセッサ
<!-- - 64-bit processor -->
- 静的な，または [duckdns.org](https://www.duckdns.org) などの動的 DNS サービスに接続されている[パブリック IP アドレス](root://general/0.1/how-to-guides/expose-your-local-device.md)
<!-- - A [public IP address](root://general/0.1/how-to-guides/expose-your-local-device.md) that's either static or connected to a dynamic DNS service such as [duckdns.org](https://www.duckdns.org) -->
- ポート15600および14265は開いている必要があります．ポートを開く1つの方法は，ノードを実行しているデバイスに[ポートを転送](root://general/0.1/how-to-guides/expose-your-local-device.md)することです．
<!-- - Ports 15600 and 14265 must be open. One way of opening ports is by [forwarding them](root://general/0.1/how-to-guides/expose-your-local-device.md) to the device that's running the node. -->

## Docker コンテナで IRI を実行する
<!-- ## Run IRI in a Docker container -->

このガイドでは，IRI Docker イメージをダウンロードし，Docker コンテナで実行します．
<!-- In this guide, you download the IRI Docker image and run it in a Docker container. -->

### 前提条件
<!-- ### Prerequisites -->

このガイドを完了するには，[Docker](https://docs.docker.com/install/#supported-platforms) が必要です．
<!-- To complete this guide, you need [Docker](https://docs.docker.com/install/#supported-platforms). -->

:::info:
Debian ベースのオペレーティングシステムを使用している場合は，次のタスクのコマンドの前に `sudo` を追加します．
:::
<!-- :::info: -->
<!-- If you're using a Debian-based operating system, add `sudo` before the commands in the following tasks. -->
<!-- ::: -->

### IRI を実行する
<!-- ### Run IRI -->

IRI を実行するには，[IRI Docker イメージ](https://hub.docker.com/r/iotaledger/iri)をダウンロードして実行します．
<!-- To run IRI, you download and run the [IRI Docker image](https://hub.docker.com/r/iotaledger/iri). -->

1\. [IRI の構成方法を計画します](../how-to-guides/configure-iri.md)．
<!-- 1\. [Plan how you want to configure IRI](../how-to-guides/configure-iri.md) -->

2\. 選択した IOTA ネットワークで実行されている[隣接ノードを見つける](../how-to-guides/find-neighbor-iri-nodes.md)と，隣接ノードの URL または IP アドレスを書き留めます．
<!-- 2\. [Find some neighbors](../how-to-guides/find-neighbor-iri-nodes.md) that are running on your chosen IOTA network and make a note of their URLs or IP addresses -->

3\. IRI Java ファイルと同じディレクトリに構成ファイルを作成し，構成オプションを追加します．`jake` を Linux ユーザー名に置き換えます．
<!-- 3\. Create a configuration file in the same directory as your IRI Java file, and add your configuration options to it. Replace `jake` with your Linux username. -->

```bash
nano /home/jake/node/config.ini
```

以下はいくつかの設定例です．
<!-- These are some example configurations: -->

--------------------
### メインネット

このファイルは，メインネットで実行するように IRI を構成し，ポート14265で API を公開し，[ローカルスナップショット](root://getting-started/0.1/network/nodes.md#local-snapshots)を行う代わりにすべてのトランザクションを台帳に保持します．

```bash
[IRI]
PORT = 14265
NEIGHBORING_SOCKET_PORT = 15600
NEIGHBORS = tcp://my.favorite.com:15600 tcp://my.other.favorite.com:15600
IXI_DIR = ixi
DEBUG = false
DB_PATH = mainnetdb
LOCAL_SNAPSHOTS_PRUNING_ENABLED = false
MWM = 14
```
---

### デブネット

このファイルは，IRI をデブネットで実行するように構成し，ポート14265で API を公開し，ローカルスナップショットを実行します．

以下の隣接ノードは自動ピアリングが有効になっているため，自動的に隣接ノードとして追加されます．
<!-- These neighbors have auto-peering enabled, so they will automatically add you as a neighbor. -->

```shell
[IRI]
PORT = 14265
TESTNET = true
NEIGHBORING_SOCKET_PORT = 15600
NEIGHBORS = tcp://p101.testnet.iota.cafe:14666 tcp://p102.testnet.iota.cafe:14666 tcp://p103.testnet.iota.cafe:14666 tcp://p104.testnet.iota.cafe:14666
IXI_DIR = ixi
DEBUG = false
LOCAL_SNAPSHOTS_ENABLED = true
LOCAL_SNAPSHOTS_PRUNING_ENABLED = true
```
--------------------

4\. IRI Docker イメージをダウンロードして実行し，構成ファイルを渡します．
<!-- 4\. Download the IRI Docker image and run it, passing in your configuration file -->

```bash
docker run --name iri iotaledger/iri:latest -c /path/to/conf/config.ini
```

:::info:
再起動のたびに IRI Docker コンテナを再起動するには，`--restart = always` フラグを `docker run` コマンドに追加します．
:::
<!-- :::info: -->
<!-- To have the IRI Docker container restart on every reboot, add the `--restart=always` flag to the `docker run` command. -->
<!-- ::: -->

:::success:おめでとうございます:tada:
IRI はバックグラウンドで実行されています！これで，IRI API を使用してタングルとの対話を開始できます．
:::
<!-- :::success:Congratulations :tada: -->
<!-- IRI is running in the background! Now, you can use the IRI API to start interacting with the Tangle. -->
<!-- ::: -->

5\. [getNodeInfo](../references/api-reference.md#getnodeinfo) エンドポイントを呼び出して，IRI ノードに関する一般情報をリクエストします．
  <!-- 5\. Call the [getNodeInfo](../references/api-reference.md#getnodeinfo) endpoint to request general information about the IRI node -->

    ```bash
    curl -s http://localhost:14265 -X POST -H 'X-IOTA-API-Version: 1' -H 'Content-Type: application/json' -d '{"command": "getNodeInfo"}' | jq
    ```

    標準出力に次のようなものが表示されるはずです．
    <!-- You should see something like the following in the output: -->

    ```json
    {
    "appName": "IRI",
    "appVersion": "1.7.0-RELEASE",
    "jreAvailableProcessors": 8,
    "jreFreeMemory": 2115085674,
    "jreVersion": "1.8.0_191",
    "jreMaxMemory": 20997734400,
    "jreTotalMemory": 4860129502,
    "latestMilestone": "CUOENIPTRCNECMVOXSWKOONGZJICAPH9FIG9F9KYXF9VYXFUKTNDCCLLWRZNUHZIGLJZFWPOVCIZA9999",
    "latestMilestoneIndex": 1050373,
    "latestSolidSubtangleMilestone": "CUOENIPTRCNECMVOXSWKOONGZJICAPH9FIG9F9KYXF9VYXFUKTNDCCLLWRZNUHZIGLJZFWPOVCIZA9999",
    "latestSolidSubtangleMilestoneIndex": 1050373,
    "milestoneStartIndex": 1039138,
    "lastSnapshottedMilestoneIndex": 1039138,
    "neighbors":6,
    "packetsQueueSize":0,
    "time":1548407444641,
    "tips":0,
    "transactionsToRequest":0,
    "features":["snapshotPruning","dnsRefresher","tipSolidification"],
    "coordinatorAddress": "EQSAUZXULTTYZCLNJNTXQTQHOMOFZERHTCGTXOLTVAHKSA9OGAZDEKECURBRIXIJWNPFCQIOVFVVXJVD9",
    "duration": 0
    }
    ```

ノードが稼働しているので，台帳をネットワークと同期し始めます．ノードに同期する時間を与えます． [ノードが同期されていることを確認する](#step-5-check-that-the-node-is-synchronized)を参照してください．
<!-- Now that your node is up and running, it'll start to synchronize its ledger with the network. Give your node some time to synchronize. See [Check that the node is synchronized](#step-5-check-that-the-node-is-synchronized). -->

## Linux Ubuntu サーバーに IRI をインストールし，実行する
<!-- ## Install and run IRI on a Linux Ubuntu server -->

このガイドでは，IRI を Linux デバイスのサービスとしてインストールして実行します．このガイドは [Ubuntu 18.04](http://releases.ubuntu.com/18.04) でテストされています．
<!-- In this guide, you install and run IRI as a service on a Linux device. This guide has been tested on [Ubuntu 18.04](http://releases.ubuntu.com/18.04). -->

### 手順1. IRI Java ファイルをダウンロードする
<!-- ### Step 1. Download the IRI Java file -->

IRI は Java ソフトウェアであるため，Java ランタイム環境（JRE）で実行する必要があります．
<!-- The IRI is Java software, so it must be run in a Java runtime environment (JRE). -->

最新の IRI ソフトウェアをダウンロードするには，2つのオプションがあります．
<!-- You have two options for downloading the latest IRI software: -->
- GitHub からビルド済みの Java ファイルをダウンロードします（最も速いオプション）
<!-- - Download the pre-built Java file from GitHub (quickest option) -->
- GitHub のソースコードから Java ファイルをビルドする
<!-- - Build the Java file from the source code on GitHub -->

#### ビルド済みの IRI Java ファイルをダウンロードする
<!-- #### Download the pre-built IRI Java file -->

ビルド済みの IRI Java ファイルは [IOTA GitHub リポジトリ](https://github.com/iotaledger/iri/releases)で入手できます．このファイルをダウンロードすることは，IRI をインストールする最も簡単な方法です．
<!-- The pre-built IRI Java file is available on the [IOTA GitHub repository](https://github.com/iotaledger/iri/releases). Downloading this file is the quickest and simplest way to install the IRI. -->

1. システムに最新のセキュリティパッチをインストールします．
  <!-- 1. Install the latest security patches for your system -->

    ```bash
    sudo apt-get update
    sudo apt-get upgrade -y
    ```

2. IRI Java ファイルをダウンロードするディレクトリを作成します．`jake` を自分のユーザー名に変更します．
  <!-- 2. Make a directory in which to download the IRI Java file. Replace `jake` with your username. -->

    ```bash
    mkdir /home/jake/node
    ```

    :::info:
    標準出力に `mkdir：ディレクトリを作成できません...`と表示されている場合，`jake` を Linux ユーザー名に変更せずにコマンドをコピーして貼り付けている可能性があります．
    :::
    <!-- :::info: -->
    <!-- If you see 'mkdir: cannot create directory...' in the output, you may have copied and pasted the command without changing `jake` to your Linux username. -->
    <!-- ::: -->

3. Java 8 OpenJDK をダウンロードしてインストールします．
  <!-- 3. Download and install the Java 8 OpenJDK -->

    ```bash
    sudo add-apt-repository universe
    sudo apt-get install -y software-properties-common --no-install-recommends
    sudo apt-get install openjdk-8-jdk
    sudo apt-get update
    ```

    :::info:
    Java がインストールされていることを確認するには，コマンドラインに `java -version` と入力します．標準出力にバージョン番号が表示されます．
    :::
    <!-- :::info: -->
    <!-- To check that Java is installed, enter `java -version` in the command line. You should see a version number in the output. -->
    <!-- ::: -->


4. `node` ディレクトリで，最新の IRI Java ファイルをダウンロードします．`jake` をユーザー名に置き換え，`$ {VERSION}` 変数を IRI の[最新バージョン](https://github.com/iotaledger/iri/releases)に置き換えます．
  <!-- 4. In your `node` directory, download the latest IRI Java file. Replace `jake` with your username and replace the `${VERSION}` variable with the [latest version](https://github.com/iotaledger/iri/releases) of the IRI. -->

    ```bash
    cd node
    sudo wget -O /home/jake/node/iri-${VERSION}.jar https://github.com/iotaledger/iri/releases/download/v${VERSION}/iri-${VERSION}.jar
    ```

    :::info:
    `${VERSION}` にはバージョン全体（例えば`1.7.0-RELEASE`）を含めるようにしてください．
    :::
    <!-- :::info: -->
    <!-- Make sure that you include the whole version, for example 1.6.0-RELEASE. -->
    <!-- ::: -->

ダウンロードには時間がかかる場合があります．すべてがうまくいった場合，標準出力に次のようなものが表示されるはずです．
<!-- The download may take some time. If everything went well, you should see something like the following in the output: -->

```bash
HTTP request sent, awaiting response ... 200 OK
'/home/jake/node/iri-1.8.4-RELEASE.jar' saved [175441686/175441686]
```

これで [IRI を構成](#configure-the-iri)できます．
<!-- Now you can [configure IRI](#configure-the-iri). -->

<a name="build-the-iri-java-file-from-the-source-code"></a>
#### ソースコードからIRI Javaファイルをビルドする
<!-- #### Build the IRI Java file from the source code -->

ビルド済みの IRI Java ファイルをダウンロードする代わりに，次のいずれかの理由でソースコードからファイルをビルドすることをお勧めします．
<!-- Instead of downloading the pre-built IRI Java file, you may want to build the file from the source code for any of the following reasons: -->

- 実行するコードがソースコードと同じであることを確認したい場合．
<!-- - You want to be sure that the code you run is the same as the source code -->
- IRI を実行する前にコードを修正したい場合．
<!-- - You want to modify the code before you run it -->

1. Java 8 OpenJDK をダウンロードしてインストールします．
  <!-- 1. Download and install the Java 8 OpenJDK -->

    ```bash
    sudo apt-get install -y software-properties-common --no-install-recommends
    sudo apt-get install openjdk-8-jdk
    sudo apt-get update
    ```

2. [Maven](https://maven.apache.org/what-is-maven.html) ビルドツールをインストールします．`USER_HOME_DIR` 変数をご自身のディレクトリパスに変更します．
  <!-- 2. Install the [Maven](https://maven.apache.org/what-is-maven.html) build tool. Change the `USER_HOME_DIR` variable to your chosen path. -->

    ```bash
    export MAVEN_VERSION=3.5.4
    export USER_HOME_DIR="/root"
    export SHA=ce50b1c91364cb77efe3776f756a6d92b76d9038b0a0782f7d53acf1e997a14d
    export BASE_URL=https://apache.osuosl.org/maven/maven-3/${MAVEN_VERSION}/binaries
    sudo apt-get update && apt-get install -y --no-install-recommends curl
    sudo mkdir -p /usr/share/maven /usr/share/maven/ref
    sudo curl -fsSL -o /tmp/apache-maven.tar.gz ${BASE_URL}/apache-maven-${MAVEN_VERSION}-bin.tar.gz

    # Check the sha256 checksum, the output should read 'OK' if the checksum is correct

    echo "${SHA} /tmp/apache-maven.tar.gz" | sha256sum -c -
    sudo tar -xzf /tmp/apache-maven.tar.gz -C /usr/share/maven --strip-components=1
    sudo rm -f /tmp/apache-maven.tar.gz
    export MAVEN_HOME=/usr/share/maven
    export MAVEN_CONFIG="${USER_HOME_DIR}/.m2"
    ```

    :::info:
    SHA256 チェックサムは，[Apache Web サイト](https://archive.apache.org/dist/maven/maven-3/3.5.4/binaries/apache-maven-3.5.4-bin.tar.gz.sha256)でも入手できます．
    :::
    <!-- :::info: -->
    <!-- The SHA256 checksum is also available on the [Apache website](https://archive.apache.org/dist/maven/maven-3/3.5.4/binaries/apache-maven-3.5.4-bin.tar.gz.sha256). -->
    <!-- ::: -->

3. Git をインストールします．
  <!-- 3. Install Git -->

    ```bash
    sudo apt-get update && apt-get install -y --no-install-recommends git
    ```

4. GitHub リポジトリをクローンしてチェックアウトします．
  <!-- 4. Clone and check out the GitHub repository -->

    ```bash
    git clone https://github.com/iotaledger/iri.git
    cd iri

    # Checkout the latest tag

    export TAG=$(git describe --tags $(git rev-list --tags --max-count=1))
    git checkout ${TAG}
    ```

5. IRI Java ファイルをビルドします．
  <!-- 5. Build the IRI Java file -->

    ```bash
    /usr/share/maven/bin/mvn clean package
    ```

    :::info:
    IRI Java ファイルは，`target` というディレクトリにあります．
    :::
    <!-- :::info: -->
    <!-- The IRI Java file is in a directory called `target`. -->
    <!-- ::: -->

これで IRI を構成できます．
<!-- Now you can configure IRI. -->

<a name="configure-the-iri"></a>
### 手順2. IRI を構成する
<!-- ### Step 2. Configure IRI -->

IRI は Java 仮想マシンで実行されます．Java 仮想マシンは，いくつかの Java 変数を設定することで最適化できます．
<!-- IRI runs in a Java virtual machine, which you can optimize by setting some Java variables. -->

1\. Java 仮想マシンを最適化するために Java 変数を定義します．
  <!-- 1\. Define the Java variables to optimize the Java virtual machine -->

    ```bash
    export JAVA_OPTIONS="-XX:+UnlockExperimentalVMOptions -XX:+DisableAttachMechanism -XX:InitiatingHeapOccupancyPercent=60 -XX:G1MaxNewSizePercent=75 -XX:MaxGCPauseMillis=10000 -XX:+UseG1GC"
    export JAVA_MIN_MEMORY=2G
    export JAVA_MAX_MEMORY=4G
    ```

    **JAVA_OPTIONS：**Java 仮想マシンを最適化するコマンド
    <!-- **JAVA_OPTIONS:** Commands that optimize the Java virtual machine -->

    **JAVA_MIN_MEMORY：**Java 仮想マシンの初期メモリ割り当て
    <!-- **JAVA_MIN_MEMORY:** The initial memory allocation for the Java virtual machine -->

    **JAVA_MAX_MEMORY：**Java 仮想マシンの最大メモリ割り当て
    <!-- **JAVA_MAX_MEMORY:** the maximum memory allocation for the Java virtual machine -->

2\. [IRI の構成を計画します](../how-to-guides/configure-iri.md)．
<!-- 2\. [Plan how you want to configure IRI](../how-to-guides/configure-iri.md) -->

3\. 選択した IOTA ネットワークで実行されている[いくつかの隣接ノードを探して](../how-to-guides/find-neighbor-iri-nodes.md)，隣接ノードの URL または IP アドレスをメモします．
<!-- 3\. [Find some neighbors](../how-to-guides/find-neighbor-iri-nodes.md) that are running on your chosen IOTA network and make a note of their URL or IP addresses -->

4\. IRI Java ファイルと同じディレクトリに構成ファイルを作成し，構成オプションを追加します．`jake` をLinux ユーザー名に置き換えます．
<!-- 4\. Create a configuration file in the same directory as your IRI Java file, and add your configuration options to it. Replace `jake` with your Linux username. -->

```bash
nano /home/jake/node/config.ini
```

以下はいくつかの設定例です．
<!-- These are some example configurations: -->

--------------------
### メインネット

このファイルは，メインネットで実行するように IRI を構成し，ポート14265で API を公開し，[ローカルスナップショット](root://getting-started/0.1/network/nodes.md#local-snapshots)を行う代わりにすべてのトランザクションを台帳に保持します．

```bash
[IRI]
PORT = 14265
NEIGHBORING_SOCKET_PORT = 15600
NEIGHBORS = tcp://my.favorite.com:15600 tcp://my.other.favorite.com:15600
IXI_DIR = ixi
DEBUG = false
DB_PATH = mainnetdb
LOCAL_SNAPSHOTS_PRUNING_ENABLED = false
MWM = 14
```
---

### デブネット

このコマンドは，IRI をデブネットと互換性があるように構成し，ポート14265で API を公開し，ローカルスナップショットを実行します．

これらの隣接ノードは自動ピアリングが有効になっているため，自動的に隣接ノードとして追加されます．

```shell
[IRI]
PORT = 14265
TESTNET = true
NEIGHBORING_SOCKET_PORT = 15600
NEIGHBORS = tcp://p101.testnet.iota.cafe:14666 tcp://p102.testnet.iota.cafe:14666 tcp://p103.testnet.iota.cafe:14666 tcp://p104.testnet.iota.cafe:14666
IXI_DIR = ixi
DEBUG = false
LOCAL_SNAPSHOTS_ENABLED = true
LOCAL_SNAPSHOTS_PRUNING_ENABLED = true
```
--------------------

5\. デブネットおよびメインネット IOTA ネットワークの最新データを含む最新の使用済みアドレスファイルとスナップショットファイルをダウンロードします．このディレクトリは [IOTA 財団の Web サイト](https://dbfiles.iota.org/?prefix=mainnet/iri/local-snapshots-and-spent-addresses/)で入手できます．
  <!-- 5\. Download the latest spent addresses file and snapshot files, which contains the latest data for the Devnet and Mainnet IOTA networks. This directory is available on [the IOTA Foundation's website](https://dbfiles.iota.org/?prefix=mainnet/iri/local-snapshots-and-spent-addresses/) -->

    :::info:
    選択した IOTA ネットワークの正しいディレクトリを必ずダウンロードしてください．
    :::
    <!-- :::info: -->
    <!-- Make sure you download the correct directory for your chosen IOTA network. -->
    <!-- ::: -->

6\. IRI Java ファイルと同じディレクトリにダウンロードしたディレクトリを展開します．`jake` を Linux ユーザー名に置き換え，`$FILE_NAME` プレースホルダーをダウンロードしたファイルの名前に置き換えます．
  <!-- 6\. Extract the directories in the same directory as your IRI Java file. Replace `jake` with your Linux username, and replace the `$FILE_NAME` placeholder with the name of the file you downloaded. -->

    ```bash
    tar -xzvf /home/jake/node/$FILE_NAME
    ```

### 手順3. IRI を実行する
<!-- ### Step 3. Run IRI -->

IRI をダウンロードして構成したら，次は IRI を実行します．
<!-- When you've downloaded, and configured IRI, it's time to run it. -->

1. IRI を実行します．`jake` を Linux ユーザー名に置き換え，`$VERSION` プレースホルダーをダウンロードした IRI のバージョンに置き換えます．
  <!-- 1. Run IRI. Replace `jake` with your Linux username, and replace the `$VERSION` placeolder with the version of the IRI that you downloaded. -->

    ```bash
    java ${JAVA_OPTIONS} -Xms${JAVA_MIN_MEMORY} -Xmx${JAVA_MAX_MEMORY} -Djava.net.preferIPv4Stack=true -jar /home/jake/node/iri-${VERSION}.jar -c /home/jake/node/config.ini
    ```

    IRIはアクティビティログをコンソールにログします．
    <!-- The IRI should start to log its activity in the console. -->

    :::info:
    データベースファイルの定期的なバックアップを作成することをお勧めします．このようにして，データベースが破損した場合や別の種類のノードの故障した場合にノードを復元できます．そのためには，データベースを毎日別のボリュームにコピーする cron ジョブを作成できます．
    :::
    <!-- :::info: -->
    <!-- We recommend making regular backups of your database files. This way, you can restore your node in case of a corrupted database or another type of node malfunction. To do so, you can create a cron job that copies the database to a different volume every day. -->
    <!-- ::: -->

    :::success:おめでとうございます:tada:
    IRI ノードを実行しています！
    :::
    <!-- :::success:Congratulations :tada: -->
    <!-- You're now running an IRI node! -->
    <!-- ::: -->

2. Linux サーバーで新しいターミナルウィンドウを開き，Curl と JQ をインストールします．Curl は REST API リクエストを IRI ノードに送信するために使用します．JQ は，JSON データを読みやすい形式で表示するコマンドラインプロセッサです．
  <!-- 2. Open a new terminal window on your Linux server and install Curl and JQ. Curl is used to send REST API requests to your IRI node. JQ is a command-line processor that displays JSON data in an easy-to-read format. -->

    ```bash
    sudo apt install curl jq
    ```

3. [`getNodeInfo`](../references/api-reference.md#getNodeInfo) API エンドポイントを呼び出して，IRI ノードに関する一般情報をリクエストします．
  <!-- 3. Call the [`getNodeInfo`](../references/api-reference.md#getNodeInfo) API endpoint to request general information about the IRI node -->

    ```bash
    curl -s http://localhost:14265 -X POST -H 'X-IOTA-API-Version: 1' -H 'Content-Type: application/json' -d '{"command": "getNodeInfo"}' | jq
    ```

    標準出力に次のようなものが表示されるはずです．
    <!-- You should see something like the following in the output: -->

    ```json
     {
    "appName": "IRI",
    "appVersion": "1.7.0-RELEASE",
    "jreAvailableProcessors": 8,
    "jreFreeMemory": 2115085674,
    "jreVersion": "1.8.0_191",
    "jreMaxMemory": 20997734400,
    "jreTotalMemory": 4860129502,
    "latestMilestone": "CUOENIPTRCNECMVOXSWKOONGZJICAPH9FIG9F9KYXF9VYXFUKTNDCCLLWRZNUHZIGLJZFWPOVCIZA9999",
    "latestMilestoneIndex": 1050373,
    "latestSolidSubtangleMilestone": "CUOENIPTRCNECMVOXSWKOONGZJICAPH9FIG9F9KYXF9VYXFUKTNDCCLLWRZNUHZIGLJZFWPOVCIZA9999",
    "latestSolidSubtangleMilestoneIndex": 1050373,
    "milestoneStartIndex": -1,
    "lastSnapshottedMilestoneIndex": 1039138,
    "neighbors":6,
    "packetsQueueSize":0,
    "time":1548407444641,
    "tips":0,
    "transactionsToRequest":0,
    "features":["snapshotPruning","dnsRefresher","tipSolidification"],
    "coordinatorAddress": "EQSAUZXULTTYZCLNJNTXQTQHOMOFZERHTCGTXOLTVAHKSA9OGAZDEKECURBRIXIJWNPFCQIOVFVVXJVD9",
    "duration": 0
    }
    ```

ノードが稼働しているので，ノードは自身の台帳をネットワークと同期し始めます．ノードに同期する時間を与えます． [ノードが同期していることを確認する](#step-5-check-that-the-node-is-synchronized)を参照してください．
<!-- Now that your node is up and running, it'll start to synchronize its ledger with the network. Give your node some time to synchronize. See [Check that the node is synchronized](#step-5-check-that-the-node-is-synchronized). -->

## 手順4. systemd サービスを作成してノードを制御する
<!-- ## Step 4. Create a systemd service to control your node -->

Linux システムが起動すると，`systemd` サービスが実行されます．`systemd` サービスを使用することで，簡単な1行のコマンドで IRI のログを開始，停止，再起動，制御できます．また，デバイスを再起動したときに IRI が自動的に起動することを確認できます．
<!-- A `systemd` service runs when a Linux system boots up. By using a `systemd` service, you can start, stop, restart, and control logging for IRI with simple one-line commands. You can also make sure that IRI starts automatically when you restart your device. -->

1. ノードを実行している場合は，**Ctrl**+**C** を押してノードを止めます．
<!-- 1. If you're node is running, press **Ctrl**+**C** to stop it -->

2. 新しい `systemd` ファイルを作成します．
  <!-- 2. Create a new `systemd` file -->

    ```bash
    sudo nano /etc/systemd/system/iri.service
    ```

3. 以下をコピーしてファイルにペーストします．`jake` を Linux ユーザー名に置き換え，`${VERSION}` プレースホルダーをダウンロードした IRI のバージョンに置き換えます．
  <!-- 3. Copy and paste the following into the file. Replace `jake` with your Linux username, and replace the `${VERSION}` placeholder with the version of the IRI that you downloaded. -->

    ```
    [Unit]
    Description=IOTA IRI Service
    After=network-online.target

    [Service]
    WorkingDirectory=/home/jake/node
    ExecStart=/usr/bin/java -XX:+UnlockExperimentalVMOptions -XX:+DisableAttachMechanism -XX:InitiatingHeapOccupancyPercent=60 -XX:G1MaxNewSizePercent=75 -XX:MaxGCPauseMillis=10000 -XX:+UseG1GC -Xms2G -Xmx4G -Djava.net.preferIPv4Stack=true -jar /home/jake/node/iri-${VERSION}.jar -c /home/jake/node/config.ini
    KillMode=process
    Type=simple
    User=jake
    StandardOutput=inherit
    StandardError=inherit

    [Install]
    WantedBy=multi-user.target
    ```

4. **Ctrl**+**X**，**y** そして **Enter**を押してファイルを保存します．
<!-- 4. Save the file by pressing **Ctrl**+**X**, **y** then **Enter** -->

5. `iri` サービスを実行する許可を自分に与え，ブート時に開始できるようにします．
  <!-- 5. Give yourself permission to execute the `iri` service and enable it to start at boot -->

    ```bash
    sudo chmod u+x /etc/systemd/system/iri.service
    sudo systemctl daemon-reload
    sudo systemctl enable iri
    ```

6. IRI を実行し，IRI が実行されていることを確認します．
  <!-- 6. Start IRI and check that it's running -->

    ```bash
    sudo systemctl start iri
    systemctl status iri
    ```

    IRI が実行されている場合，出力に `active (running)` メッセージが表示されます．
    <!-- If IRI is running, you should see the `active (running)` message in the output. -->

7. ログメッセージをディスプレイに表示します．
  <!-- 7. Display the log messages -->

    ```bash
    journalctl -fu iri
    ```

8. **Ctrl**+**C** を押して，ログメッセージのティスプレイ表示を止めます．IRI はバックグラウンドで起動し続けています．
<!-- 8. To stop displaying the log messages press **Ctrl**+**C**. IRI will continue running in the background -->

### IRI を止めて，再起動する
<!-- ### Stop and restart IRI -->

次の `systemd` コマンドを使用して，IRI を開始，停止，および再起動できます．
<!-- You can use the following `systemd` commands to start, stop, and restart IRI: -->

```bash
# Start
sudo systemctl start iri
# Restart
sudo systemctl restart iri
# Stop
sudo systemctl stop iri
```

### ログメッセージを見る
<!-- ### View log messages -->

次の操作を行うことで，ログメッセージが書き込まれたときに表示できます．
<!-- You can view the log messages as they are written by doing the following: -->

```bash
journalctl -fu iri
```

より詳細なログメッセージが必要な場合は，IRI を停止し，`--debug` フラグを使用して IRI Java ファイルを実行できます．`jake` を Linux ユーザー名に置き換え，`${VERSION}` プレースホルダーをダウンロードした IRI のバージョンに置き換えます．
<!-- If you want more detailed log messages, you can stop IRI and execute the IRI Java file with the `--debug` flag. Replace `jake` with your Linux username, and replace the `${VERSION}` placeholder with the version of the IRI that you downloaded. -->

```bash
sudo systemctl stop iri
java -jar /home/jake/node/iri${VERSION}.jar --debug
```

<a name="step-5-check-that-the-node-is-synchronized"></a>
## 手順5. ノードが同期しているかを確認する
<!-- ## Step 5. Check that the node is synchronized -->

`latestMilestoneIndex` フィールドが `latestSolidSubtangleMilestoneIndex` フィールドと等しい場合，ノードは同期済とと見なされます．
<!-- A node is considered synchronized when the `latestMilestoneIndex` field is equal to the `latestSolidSubtangleMilestoneIndex` field: -->

- `latestMilestoneIndex`：ノードが隣接ノードから受け取った最新のマイルストーンのインデックス．このフィールドは，ノードが同期済の隣接ノードに接続されている場合にのみ正確です．
<!-- - `latestMilestoneIndex`: Index of the latest milestone that the node has received from its neighbors. This field is accurate only when the node is connected to synchronized neighbors. -->

- `latestSolidSubtangleMilestoneIndex`：ノードの台帳にある最新の凝固済マイルストーンのインデックス
<!-- - `latestSolidSubtangleMilestoneIndex`: Index of the latest solid milestone that's in the node's ledger -->

1. 現在の`latestMilestoneIndex` フィールドを確認するには，[Discord](https://discord.iota.org)に移動し，`#botbox` チャネルに **!milestone** と入力します．
  <!-- 1. To check the current `latestMilestoneIndex` field, go to our [Discord](https://discord.iota.org) and enter **!milestone** in the #botbox channel -->

    ![Entering !milestone on Discord](../images/discord-milestone-check.PNG)

2. IRI ノードの `latestMilestoneIndex` フィールドと `latestSolidSubtangleMilestoneIndex` フィールドを確認するには，`getNodeInfo` API エンドポイントを呼び出します．
  <!-- 2. To check these fields for your IRI node, call the `getNodeInfo` API endpoint -->

    ```bash
    sudo apt install curl jq
    curl -s http://localhost:14265 -X POST -H 'X-IOTA-API-Version: 1' -H 'Content-Type: application/json' -d '{"command": "getNodeInfo"}' | jq
    ```

`latestMilestoneIndex` フィールドが `latestSolidSubtangleMilestoneIndex` フィールドと等しいことを確認してください．
<!-- Make sure that the `latestMilestoneIndex` field is equal to the `latestSolidSubtangleMilestoneIndex` field. -->

これらのフィールドが等しくない場合，ノードは同期していません．ヘルプについては，トラブルシューティングを参照してください．
<!-- If these fields aren't equal, your node is not synchronized. See Troubleshooting for help. -->

## トラブルシューティング
<!-- ## Troubleshooting -->

以下は，このガイドの使用中に見つかる可能性のある既知の問題と，その問題を解決するためのいくつかの推奨手順です．
<!-- These are known issues that you may find while following this guide and some suggested steps to resolve them. -->

## IRI が隣接ノードと同期しない
<!-- ## The IRI won't synchronize with its neighbors -->

ノードの同期には時間がかかる場合があるため，ノードが自身の力で同期するまでしばらく待つことをお勧めします．
<!-- It may take some time for node to synchronize, so we recommend that you wait a while to see if it synchronizes by itself. -->

- [6〜7個の隣接ノードがいることを確認してください](../how-to-guides/find-neighbor-iri-nodes.md)．
<!-- - [Make sure that you have 6 or 7 neighbors](../how-to-guides/find-neighbor-iri-nodes.md) -->

- [最新バージョンの IRI](https://github.com/iotaledger/iri/releases) を実行していることを確認してください．
<!-- - Make sure that you're running the [latest version of the IRI](https://github.com/iotaledger/iri/releases) -->

- ノードと隣接ノードが相互にデータを送信していることを確認してください． `getNeighbors` エンドポイントを呼び出して，着信トランザクション（`numberOfAllTransactions`）と発信トランザクションの数（`numberOfSentTransactions`）の両方を確認します．隣接ノードがデータを送信していない場合は，接続する新しい隣接ノードを見つけます．
  <!-- - Make sure that your node's neighbors are sending data among each other. Call the `getNeighbors` endpoint to see both the incoming transactions (`numberOfAllTransactions`) and the number of outgoing transactions (`numberOfSentTransactions`). If your neighbors aren't sending you data, find new neighbors to connect to. -->

    ```bash
    curl http://localhost:14265 -X POST -H 'Content-Type: application/json' -H 'X-IOTA-API-Version: 1' -d '{"command": "getNeighbors"}'
    ```

- [ヘルプ]または [Discord](https://discord.iota.org) の `#fullnodes` チャンネルでサポートを依頼してください．
<!-- - Ask for more support on [Discord](https://discord.iota.org) either in our #help or #fullnodes channel -->

:::info:
[dbfiles.iota.org](https://dbfiles.iota.org/?prefix=) から最新のデータベースファイルをダウンロードできます．最新のデータベースファイルをダウンロードして展開することにより，ノードは隣接ノードとより速く同期できます．
:::
<!-- :::info: -->
<!-- You can download the latest database files from [dbfiles.iota.org](https://dbfiles.iota.org/?prefix=). -->
<!-- By downloading and extracting the latest database files, your node can synchronize faster with its neighbors. -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

[API 入門](../how-to-guides/get-started-with-the-api.md)．
<!-- [Get started with the API](../how-to-guides/get-started-with-the-api.md). -->

IRI ノードの[リバースプロキシのセットアップ](../how-to-guides/set-up-a-reverse-proxy.md)を使用して，HTTPS サポート，レート制限，および負荷分散を構成する．
<!-- [Set up a reverse proxy](../how-to-guides/set-up-a-reverse-proxy.md) for your IRI node so that you can configure HTTPS support, rate limiting, and load balancing. -->

[クロニクルノードソフトウェア](root://node-software/0.1/chronicle/introduction/overview.md)をセットアップして，ノードをパーマノードに変更する．
<!-- Set up the [Chronicle node software](root://node-software/0.1/chronicle/introduction/overview.md) to turn your node into a permanode. -->
