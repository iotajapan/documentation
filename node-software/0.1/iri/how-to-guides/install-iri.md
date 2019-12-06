# IRIノードを実行する
<!-- # Run IRI -->

**IRIノードを実行することにより、[タングル](root://getting-started/0.1/network/the-tangle.md)上で[トランザクション](root://getting-started/0.1/transactions/transactions.md)を検証することにより、[IOTAネットワーク](root://getting-started/0.1/network/iota-networks.md)がより分散されるように支援します。**
<!-- **By running an IRI node, you help an [IOTA network](root://getting-started/0.1/network/iota-networks.md) to become more distributed by validating [transactions](root://getting-started/0.1/transactions/transactions.md) in the [Tangle](root://getting-started/0.1/network/the-tangle.md).** -->

IRIを実行するには2つのオプションがあります。
<!-- You have two options for running IRI: -->

- DockerコンテナでIRIを実行する
<!-- - Run IRI in a Docker container -->
- Linux UbuntuサーバーでIRIをビルドして実行する
<!-- - Build and run IRI on a Linux Ubuntu server -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完成するには、次のものが必要です。
<!-- To complete this guide, you need the following: -->

- 4GB RAM
- 64ビットプロセッサ
<!-- - 64-bit processor -->
- 静的な、または[duckdns.org](https://www.duckdns.org)などの動的DNSサービスに接続されている[パブリックIPアドレス](root://general/0.1/how-to-guides/expose-your-local-device.md)
<!-- - A [public IP address](root://general/0.1/how-to-guides/expose-your-local-device.md) that's either static or connected to a dynamic DNS service such as [duckdns.org](https://www.duckdns.org) -->
- ノードを実行しているデバイスに[次のポートを転送します](root://general/0.1/how-to-guides/expose-your-local-device.md)。
<!-- - [Forward the following ports](root://general/0.1/how-to-guides/expose-your-local-device.md) to the device that's running the node: -->

    - **TCP隣接ノードのピアリングポート：** 15600
    <!-- - **TCP neighbor peering port:** 15600 -->
    - **TCPのAPIポート：** 14265
    <!-- - **TCP API port:** 14265 -->

## Run IRI in a Docker container

In this guide, you download the IRI Docker image and run it in a Docker container.

### Prerequisites

To complete this guide, you need [Docker](https://docs.docker.com/install/#supported-platforms).

:::info:
If you're using a Debian-based operating system, add `sudo` before all the commands in the following tasks.
:::

### Optional: Build the IRI Docker container from the source code

Instead of using the pre-built Docker image, you may want to build the file from the source code for any of the following reasons:
- You want to be sure that the code you run is the same as the source code
- You want to modify the code before you run it

1. [Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

2. Make sure that Git is installed

    ```bash
    git --version
    ```

    You should see the version number of your Git installation.

3. Build the latest version of IRI

    ```bash
    git clone https://github.com/iotaledger/iri.git
    cd iri
    export TAG=$(git describe --tags $(git rev-list --tags --max-count=1))
    git checkout ${TAG}
    docker build -t iri .
    ```

### Run IRI

To run IRI, you download and run the IRI Docker image.

1\. [Find some neighbors](../how-to-guides/find-neighbor-iri-nodes.md) and make a note of their URL or IP addresses

2\. Create an IRI configuration file in the same directory as your IRI Java file. Replace `jake` with your Linux username.

```bash
nano /home/jake/node/config.ini
```

These are some example configurations.

--------------------
### Permanode

This command configures IRI to be compatible with the Mainnet, exposes its API on port 14265, and keeps all transactions in the ledger instead of doing a local snapshot.

You can also store all transactions in a separate database with the [Chronicle node software](root://node-software/0.1/chronicle/introduction/overview.md).

Replace the neighbor URLs with your own neighbors.

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

### Devnet

This command configures IRI to be compatible with the Devnet, exposes its API on port 14265, and does local snapshots.

These neighbors have auto-peering enabled, so they will automatically add you as a neighbor.

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

3\. Download the IRI Docker image and run it with the [command line options](../references/iri-configuration-options.md) that you want to use

```bash
docker run --name iri iotaledger/iri:latest -c /path/to/conf/config.ini
```

:::info:
If you built the Docker container from source, change the value of the `-name` option to `iri iri:latest`.
:::

:::info:
To have the IRI Docker container restart on every reboot, add the `--restart=always` flag to the `docker run` command.
:::

:::success:Congratulations :tada:
IRI is running in the background! Now, you can use its API to start creating user accounts.
:::

4\. Call the [getNodeInfo](../references/api-reference.md#getnodeinfo) endpoint to request general information about the IRI node

    ```bash
    curl -s http://localhost:14265 -X POST -H 'X-IOTA-API-Version: 1' -H 'Content-Type: application/json' -d '{"command": "getNodeInfo"}' | jq
    ```

    You should see something like the following in the output:

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
    "neighbors":0,
    "packetsQueueSize":0,
    "time":1548407444641,
    "tips":0,
    "transactionsToRequest":0,
    "features":["snapshotPruning","dnsRefresher","tipSolidification"],
    "coordinatorAddress": "EQSAUZXULTTYZCLNJNTXQTQHOMOFZERHTCGTXOLTVAHKSA9OGAZDEKECURBRIXIJWNPFCQIOVFVVXJVD9",
    "duration": 0
    }
    ```

Now that your node is up and running, it'll start to synchronize its ledger with the network. Give your node some time to synchronize, or read our troubleshooting guide if your IRI node isn't synchronizing.

## Install and run IRI on a Linux Ubuntu server

In this guide, you install and run IRI on Ubuntu 18.04.

### Prerequisites

To complete this guide, you need a Linux server (this guide has been tested on [Ubuntu 18.04](http://releases.ubuntu.com/18.04)).

### Step 1. Download the IRI Java file

The IRI is Java software, so it must be run in a Java runtime environment (JRE).

You have two options for downloading the latest IRI software:
- Download the pre-built Java file from GitHub (quickest option)
- Build the Java file from the source code on GitHub

#### Download the pre-built IRI Java file

The pre-built IRI Java file is available on the [IOTA GitHub repository](https://github.com/iotaledger/iri/releases). Downloading this file is the quickest and simplest way to install the IRI.

1. Install the latest security patches for your system

    ```bash
    sudo apt-get update
    sudo apt-get upgrade -y
    ```

2. IRI Javaファイルをダウンロードするディレクトリを作成します。`jake`を自分のユーザー名に変更します。
  <!-- 2. Make a directory in which to download the IRI Java file. Replace `jake` with your username. -->

    ```bash
    mkdir /home/jake/node
    ```

    :::info:
    標準出力に`mkdir：ディレクトリを作成できません...`と表示されている場合、「jake」をLinuxユーザー名に変更せずにコマンドをコピーして貼り付けている可能性があります。
    :::
    <!-- :::info: -->
    <!-- If you see 'mkdir: cannot create directory...' in the output, you may have copied and pasted the command without changing `jake` to your Linux username. -->
    <!-- ::: -->

3. Java 8 OpenJDKをダウンロードしてインストールします。
  <!-- 3. Download and install the Java 8 OpenJDK -->

    ```bash
    sudo add-apt-repository universe
    sudo apt-get install -y software-properties-common --no-install-recommends
    sudo apt-get install openjdk-8-jdk
    sudo apt-get update
    ```

    :::info:
    Javaがインストールされていることを確認するには、コマンドラインに`java -version`と入力します。標準出力にバージョン番号が表示されます。
    :::
    <!-- :::info: -->
    <!-- To check that Java is installed, enter `java -version` in the command line. You should see a version number in the output. -->
    <!-- ::: -->

4. 最新のIRI Javaファイルを自分の`node`ディレクトリにダウンロードします。`jake`を自分のユーザー名に置き換えて、`${VERSION}`変数を[最新バージョン](https://github.com/iotaledger/iri/releases)のIRIに置き換えます。
<!-- 4. Download the latest IRI Java file into your `node` directory. Replace `jake` with your username and replace the `${VERSION}` variable with the [latest version](https://github.com/iotaledger/iri/releases) of the IRI. -->

    ```bash
    sudo wget -O /home/jake/node/iri-${VERSION}.jar https://github.com/iotaledger/iri/releases/download/v${VERSION}/iri-${VERSION}.jar
    ```

    :::info:
    `${VERSION}`にはバージョン全体（例えば`1.7.0-RELEASE`）を含めるようにしてください。
    :::
    <!-- :::info: -->
    <!-- Make sure that you include the whole version, for example 1.6.0-RELEASE. -->
    <!-- ::: -->

ダウンロードにはしばらく時間がかかります。すべてうまくいけば、標準出力に次のようなものが表示されるはずです。
<!-- The download may take some time. You should see something like the following in the output if everything went well: -->

```bash
HTTP request sent, awaiting response ... 200 OK
'/home/jake/node/iri-1.6.0-RELEASE.jar' saved [175441686/175441686]
```

:::info:初めてのノードですか？
[IOTA財団のWebサイト](https://dbfiles.iota.org/?prefix=mainnet/spent-addresses/)、[IOTA Partners Webサイト](https://iota.partners/#database)、または[IOTA Playbook](https://iri-playbook.readthedocs.io/en/master/faq.html#where-can-i-get-a-fully-synced-database-to-help-kick-start-my-node)から`spent-addresses-db`ディレクトリをダウンロードする必要があります。

ディレクトリをダウンロードしたら、手順4のIRI Javaファイルと同じディレクトリに解凍します。この例では、ファイルは`/home/jake/node/`ディレクトリにあります。
:::
<!-- :::info:Is this your first node? -->
<!-- You need to download the `spent-addresses-db` directory from [our website](https://dbfiles.iota.org/?prefix=mainnet/spent-addresses/), [the IOTA Partners website](https://iota.partners/#database), or the [IOTA Playbook](https://iri-playbook.readthedocs.io/en/master/faq.html#where-can-i-get-a-fully-synced-database-to-help-kick-start-my-node). -->
<!--  -->
<!-- After you've downloaded the directory, extract it into the same directory as your IRI Java file from step 4. For this example, the file is in the `/home/jake/node/` directory. -->
<!-- ::: -->

これで、IRI Javaファイルと`spent-addresses-db`ディレクトリがサーバに保存されたので、実行する前に[IRIを設定](#configure-the-iri)します。
<!-- Now that the IRI Java file and the `spent-addresses-db` directory are saved on your server, [configure the IRI](#configure-the-iri) before running it. -->

<a name="build-the-iri-java-file-from-the-source-code"></a>
#### ソースコードからIRI Javaファイルをビルドする
<!-- #### Build the IRI Java file from the source code -->

ビルド済みのIRI Javaファイルをダウンロードする代わりに、次のいずれかの理由でソースコードからファイルをビルドすることをお勧めします。
<!-- Instead of downloading the pre-built IRI Java file, you may want to build the file from the source code for any of the following reasons: -->

- 実行するコードがソースコードと同じであることを確認したい場合。
<!-- - You want to be sure that the code you run is the same as the source code -->
- IRIを実行する前にコードを修正したい場合。
<!-- - You want to modify the code before you run it -->

1. Java 8 OpenJDKをダウンロードしてインストールします。
  <!-- 1. Download and install the Java 8 OpenJDK -->

    ```bash
    sudo apt-get install -y software-properties-common --no-install-recommends
    sudo apt-get install openjdk-8-jdk
    sudo apt-get update
    ```

2. [Maven](https://maven.apache.org/what-is-maven.html)ビルドツールをインストールします。`USER_HOME_DIR`変数をご自身のディレクトリパスに変更します。
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
    SHA256チェックサムは、[Apache Webサイト](https://archive.apache.org/dist/maven/maven-3/3.5.4/binaries/apache-maven-3.5.4-bin.tar.gz.sha256)でも入手できます。
    :::
    <!-- :::info: -->
    <!-- The SHA256 checksum is also available on the [Apache website](https://archive.apache.org/dist/maven/maven-3/3.5.4/binaries/apache-maven-3.5.4-bin.tar.gz.sha256). -->
    <!-- ::: -->

3. Gitをインストールします。
  <!-- 3. Install Git -->

    ```bash
    sudo apt-get update && apt-get install -y --no-install-recommends git
    ```

4. GitHubリポジトリをクローンしてチェックアウトします。
  <!-- 4. Clone and check out the GitHub repository -->

    ```bash
    git clone https://github.com/iotaledger/iri.git
    cd iri

    # Checkout the latest tag

    export TAG=$(git describe --tags $(git rev-list --tags --max-count=1))
    git checkout ${TAG}
    ```

5. IRI Javaファイルをビルドします。
  <!-- 5. Build the IRI Java file -->

    ```bash
    /usr/share/maven/bin/mvn clean package
    ```

    :::info:
    IRI Javaファイルは、`target`というディレクトリにあります。
    :::
    <!-- :::info: -->
    <!-- The IRI Java file is in a directory called `target`. -->
    <!-- ::: -->

### 手順2. IRIを設定する
<!-- ### Step 2. Configure IRI -->

IRIはJava仮想マシンで動作します。したがって、IRIを実行する前に、いくつかのJava変数を設定する必要があります。
<!-- The IRI runs in a Java virtual machine. Therefore, before you run IRI, you need to set up some Java variables. -->

1\. Java仮想マシンを最適化するためにいくつかのJava変数を定義します。
  <!-- 1\. Define some Java variables to optimize the Java virtual machine -->

    ```bash
    export JAVA_OPTIONS="-XX:+UnlockExperimentalVMOptions -XX:+DisableAttachMechanism -XX:InitiatingHeapOccupancyPercent=60 -XX:G1MaxNewSizePercent=75 -XX:MaxGCPauseMillis=10000 -XX:+UseG1GC"
    export JAVA_MIN_MEMORY=2G
    export JAVA_MAX_MEMORY=4G
    ```

    **JAVA_OPTIONS：** Java仮想マシンを最適化するコマンド
    <!-- **JAVA_OPTIONS:** Commands that optimize the Java virtual machine -->

    **JAVA_MIN_MEMORY：** Java仮想マシンの初期メモリ割り当て
    <!-- **JAVA_MIN_MEMORY:** The initial memory allocation for the Java virtual machine -->

    **JAVA_MAX_MEMORY：** Java仮想マシンの最大メモリ割り当て
    <!-- **JAVA_MAX_MEMORY:** the maximum memory allocation for the Java virtual machine -->

2\. [いくつかの隣接ノードを見つけ](../how-to-guides/find-neighbor-iri-nodes.md)、その隣接ノードのURLまたはIPアドレスをメモします。
<!-- 2\. [Find some neighbors](../how-to-guides/find-neighbor-iri-nodes.md) and make a note of their URL or IP addresses -->

3\. IRI Javaファイルと同じディレクトリにIRI設定ファイルを作成します。`jake`を自分のユーザー名に置き換えます。
  <!-- 3\. Create an IRI configuration file in the same directory as your IRI Java file. Replace `jake` with your Linux username. -->

```bash
nano /home/jake/node/config.ini
```

以下はいくつかの設定例です。
<!-- These are some example configurations. -->

--------------------
### パーマノード
<!-- ### Permanode -->

This command configures IRI to be compatible with the Mainnet, exposes its API on port 14265, and keeps all transactions in the ledger instead of doing a local snapshot.

You can also store all transactions in a separate database with the [Chronicle node software](root://node-software/0.1/chronicle/introduction/overview.md).

Replace the neighbor URLs with your own neighbors.

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

### Devnet

This command configures IRI to be compatible with the Devnet, exposes its API on port 14265, and does local snapshots.

These neighbors have auto-peering enabled, so they will automatically add you as a neighbor.

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

<a name="run-the-iri"></a>
### 手順3. IRIを実行する
<!-- ### Step 3. Run IRI -->

IRIをダウンロードして設定したら、実行します。
<!-- When you've downloaded, and configured IRI, it's time to run it. -->
>>>>>>> upstream/develop:node-software/0.1/iri/how-to-guides/install-iri.md

1. データベースとIXI（IOTA交換インターフェース）を保存するディレクトリを作成します。`jake`を自分のユーザー名に置き換えます。
  <!-- 1. Make a directory to keep the database and the IXI (IOTA exchange interface) modules. Change `jake` to your Linux username. -->

    ```bash
    mkdir -p /home/jake/node/data
    cd /home/jake/node/data
    ```

    :::info:
    データベースの定期的なバックアップを作成することをお勧めします。これにより、データベースが破損した場合や、他の種類のノードが誤動作した場合に、ノードを復元できます。そのためには、毎日データベースを別のボリュームにコピーするcronジョブを作成します。
    :::
    <!-- :::info: -->
    <!-- We recommend making regular backups of your database. This way, you can restore your node in case of a corrupted database or another type of node malfunction. To do so, you create a cron job that copies the database to a different volume every day. -->
    <!-- ::: -->

2. IRIを実行します。`jake`をご自身のLinuxユーザー名に、`$VERSION`をダウンロードしたIRIのバージョンに置き換えます。
  <!-- 2. Run IRI. Replace `jake` with your Linux username and `$VERSION` with the version of the IRI that you downloaded. -->

    ```bash
    java ${JAVA_OPTIONS} -Xms${JAVA_MIN_MEMORY} -Xmx${JAVA_MAX_MEMORY} -Djava.net.preferIPv4Stack=true -jar /home/jake/node/iri-${VERSION}.jar -c /home/jake/node/config.ini
    ```

    IRIはアクティビティログを出力します。
    <!-- The IRI should start to log its activity to the output. -->

    :::success:おめでとうございます:tada:
    IRIノードを実行しています！
    :::
    <!-- :::success:Congratulations :tada: -->
    <!-- You're now running an IRI node! -->
    <!-- ::: -->

3. Linuxサーバで新しいターミナルウィンドウを開き、CurlとJQをインストールします。CurlはREST APIリクエストをIRIノードに送信するために使用します。JQは、JSONデータを読みやすい形式で表示するコマンドラインプロセッサです。
  <!-- 3. Open a new terminal window on your Linux server and install Curl and JQ. Curl is used to send REST API requests to your IRI node. JQ is a command-line processor that displays JSON data in an easy-to-read format. -->

    ```bash
    sudo apt install curl jq
    ```

4. [`getNodeInfo`](../references/api-reference.md#getNodeInfo)APIエンドポイントを呼び出して、IRIノードに関する一般情報をリクエストします。
  <!-- 4. Call the [`getNodeInfo`](../references/api-reference.md#getNodeInfo) API endpoint to request general information about the IRI node -->

    ```bash
    curl -s http://localhost:14265 -X POST -H 'X-IOTA-API-Version: 1' -H 'Content-Type: application/json' -d '{"command": "getNodeInfo"}' | jq
    ```

    標準出力に次のようなものが表示されるはずです。
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

ノードが起動して実行されているので、IOTAネットワークを使い台帳の同期を開始します。ノードに同期をとる時間を与えるか、IRIノードが同期していない場合は、トラブルシューティングガイドをお読みください。
<!-- Now that your node is up and running, it'll start to synchronize its ledger with the network. Give your node some time to synchronize, or read our troubleshooting guide if your IRI node isn't synchronizing. -->

<a name="check-that-the-iri-is-synchronized"></a>
## ノードが同期していることを確認する
<!-- ## Check that the node is synchronized -->

`latestMilestoneIndex`フィールドが`latestSolidSubtangleMilestoneIndex`フィールドと等しい場合、ノードは同期していると見なされます。
<!-- A node is considered synchronized when the `latestMilestoneIndex` field is equal to the `latestSolidSubtangleMilestoneIndex` field. -->

`latestMilestoneIndex`フィールドは、ノードが隣接ノードから受け取った最新のマイルストーンのインデックスです。
<!-- The `latestMilestoneIndex` field is the index of the latest milestone that the node has received from its neighbors. -->

`latestSolidSubtangleMilestoneIndex`フィールドは、ノードがマイルストーンを凝固（マイルストーンが直接および間接的に参照するすべてのトランザクションをIRIノードが持った状態）にした最新のマイルストーンのインデックスです。
<!-- The `latestSolidSubtangleMilestoneIndex` field is the index of the latest milestone for which the node's ledger has all the transactions that the milestone directly and indirectly references. -->

`latestMilestoneIndex`フィールドと`latestSolidSubtangleMilestoneIndex`フィールドは、ノードが同期済み隣接ノードに接続されている場合にのみ正確です。
<!-- The `latestMilestoneIndex` and `latestSolidSubtangleMilestoneIndex` fields are accurate only when the node is connected to synchronized neighbors. -->

1. 現在の`latestMilestoneIndex`フィールドを確認するには、[Discord](https://discord.iota.org)に移動し、`#botbox`チャンネルに**!milestone**と入力してください。
  <!-- 1. To check the current `latestMilestoneIndex` field, go to our [Discord](https://discord.iota.org) and enter **!milestone** in the `botbox` channel -->

    ![Entering !milestone on Discord](../images/discord-milestone-check.PNG)

2. IRIノードの`latestMilestoneIndex`フィールドと`latestSolidSubtangleMilestoneIndex`フィールドを確認するには、`getNodeInfo` APIエンドポイントを呼び出します。
  <!-- 2. To check these fields for your IRI node, call the `getNodeInfo` API endpoint -->

    ```bash
    sudo apt install curl jq
    curl -s http://localhost:14265 -X POST -H 'X-IOTA-API-Version: 1' -H 'Content-Type: application/json' -d '{"command": "getNodeInfo"}' | jq
    ```

## Troubleshooting

These are known issues that you may find while following this guide and some suggested steps to resolve them.

## The IRI won't synchronize with its neighbors

It may take some time for node to synchronize, so we recommend that you wait a while to see if it synchronizes by itself.

- [Make sure that you have 6 or 7 neighbors](../how-to-guides/find-neighbor-iri-nodes.md)

- Make sure that you're running the [latest version of the IRI](https://github.com/iotaledger/iri/releases)

- Make sure that your node's neighbors are sending data among each other. Call the `getNeighbors` endpoint to see both the incoming transactions (`numberOfAllTransactions`) and the number of outgoing transactions (`numberOfSentTransactions`). If your neighbors aren't sending you data, find new neighbors to connect to.

    ```bash
    curl http://localhost:14265 -X POST -H 'Content-Type: application/json' -H 'X-IOTA-API-Version: 1' -d '{"command": "getNeighbors"}'
    ```

- Ask for more support on [Discord](https://discord.iota.org) in our #help and #fullnodes channels

:::info:
[dbfiles.iota.org](https://dbfiles.iota.org/?prefix=)から最新のデータベースファイルをダウンロードできます。
最新のデータベースファイルをダウンロードして抽出することにより、ノードは近隣ノードとより速く同期できます。
:::
<!-- :::info: -->
<!-- You can download the latest database files from [dbfiles.iota.org](https://dbfiles.iota.org/?prefix=). -->
<!-- By downloading and extracting the latest database files, your node can synchronize faster with its neighbors. -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

[API入門](../how-to-guides/get-started-with-the-api.md)。
[Get started with the API](../how-to-guides/get-started-with-the-api.md).

IRIノードに対して[リバースプロキシを設定する](../how-to-guides/set-up-a-reverse-proxy.md)ことをお勧めします。これにより、ノードに対するリクエストをより詳細に制御できるようになります。
<!-- We recommend [setting up a reverse proxy](../how-to-guides/set-up-a-reverse-proxy.md) for your IRI node so that you can have more control over the requests that are made to it. -->
