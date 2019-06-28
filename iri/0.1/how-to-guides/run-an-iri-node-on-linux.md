# LinuxサーバーでIRIノードを実行する
<!-- # Run an IRI node on a Linux server -->

**Linuxサーバー上でIRIを実行すると、LinuxサーバーがIOTAネットワークへ直接アクセスを可能にするノードになります。ノードを実行することで、台帳の数を増やし、隣接ノードのトランザクションを検証することで、IOTAネットワークがより分散されるようになります。**
<!-- **When you run the IRI on a Linux server, it becomes a node that gives you direct access to an IOTA network. By running a node, you help the IOTA network to become more distributed by adding to the number of ledgers and validating your neighbors' transactions.** -->

## 前提条件
<!-- ## Prerequisites -->

* IRIを実行するには、サーバーが以下の最小要件を満たしている必要があります。
  <!-- * To run the IRI, your server must meet the following minimum requirements: -->
    * 4GB RAM
    * 64ビットプロセッサ
    <!-- * 64-bit processor -->
    * 静的[パブリックIPアドレス](root://general/0.1/how-to-guides/expose-your-local-device.md)か、[duckdns.org](https://www.duckdns.org)などの動的DNSサービスに接続されている[パブリックIPアドレス](root://general/0.1/how-to-guides/expose-your-local-device.md)
    <!-- * A [public IP address](root://general/0.1/how-to-guides/expose-your-local-device.md) that's either static or connected to a dynamic DNS service such as [duckdns.org](https://www.duckdns.org) -->

* デフォルトでは、IRIは次のポートを使用します。ローカルネットワークでLinuxサーバーを実行している場合は、これらのポートをコンピューターの[パブリックIPアドレス](root://general/0.1/how-to-guides/expose-your-local-device.md)に転送する必要があります。
<!-- * By default, the IRI uses the following ports. If you're running a Linux server on your local network, you must [forward these ports to your computer's public IP address](root://general/0.1/how-to-guides/expose-your-local-device.md). -->

    * **UDP隣接ノードのピアリングポート：** 14600
    <!-- * **UDP neighbor peering port:** 14600 -->
    * **TCP隣接ノードのピアリングポート：** 14600
    <!-- * **TCP neighbor peering port:** 14600 -->
    * **TCPのAPIポート：** 14265
    <!-- * **TCP API port:** 14265 -->

---

:::info:
サーバーのRAMが4GB未満など、リソースが限られている場合は、IRIではなくcIRIを実行することをお勧めします。
:::
<!-- :::info: -->
<!-- If your server has limited resources such as less than 4 GB of RAM, we recommend running the cIRI instead of the IRI. -->
<!-- ::: -->

IRIはJavaソフトウェアなので、Javaランタイム環境（JRE）で実行する必要があります。
<!-- The IRI is Java software, so it must be run in a Java runtime environment (JRE). -->

最新のIRIソフトウェアをダウンロードする方法は2つあります。
<!-- You have two options for downloading the latest IRI software: -->
* GitHubからのビルド済みJavaファイルのダウンロードする。（最速オプション）
<!-- * Download the pre-built Java file from GitHub (quickest option) -->
* GitHub上のソースコードからJavaファイルを構築する。
<!-- * Build the Java file from the source code on GitHub -->

### ビルド済みのIRI Javaファイルをダウンロードする
<!-- ### Download the pre-built IRI Java file -->

ビルド済みのIRI Javaファイルは、[IOTA GitHubレポジトリ](https://github.com/iotaledger/iri/releases)にあります。このファイルをダウンロードするのがIRIをインストールするための最も早くて簡単な方法です。
<!-- The pre-built IRI Java file is available on the [IOTA GitHub repository](https://github.com/iotaledger/iri/releases). Downloading this file is the quickest and simplest way to install the IRI. -->

1. システムに最新のセキュリティパッチをインストールします。
  <!-- 1. Install the latest security patches for your system -->

    ```bash
    sudo apt-get update
    sudo apt-get upgrade -y
    ```

2. IRI Javaファイルをダウンロードするディレクトリを作成します。`jake`をご自身のLinuxユーザー名に変更してください。
  <!-- 2. Make a directory in which to download the IRI Java file. Change `jake` to your Linux username. -->

    ```bash
    mkdir /home/jake/node
    ```

    :::info:
    出力に「mkdir：ディレクトリを作成できません...」と表示された場合は、おそらくjakeをご自分のLinuxユーザー名に変更せずにコマンドをコピーして貼り付けたはずです。
    :::
    <!-- :::info: -->
    <!-- If you see 'mkdir: cannot create directory...' in the output, you probably copied and pasted the command without changing `jake` to your Linux username. -->
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
    Javaがインストールされていることを確認するには、`java -version`を実行します。出力にバージョン番号が表示されます。
    :::
    <!-- :::info: -->
    <!-- To check that Java is installed, do `java -version`. You should see a version number in the output. -->
    <!-- ::: -->

4. 最新のIRI Javaファイルを自分の`node`ディレクトリにダウンロードします。 `jake`をご自身のLinuxユーザー名に変更して、`${VERSION}`変数を[最新バージョン](https://github.com/iotaledger/iri/releases)のIRIに置き換えます。
  <!-- 4. Download the latest IRI Java file into your `node` directory. Change `jake` to your Linux username and replace the `${VERSION}` variable with the [latest version](https://github.com/iotaledger/iri/releases) of the IRI. -->

    ```bash
    sudo wget -O /home/jake/node/iri-${VERSION}.jar https://github.com/iotaledger/iri/releases/download/v${VERSION}/iri-${VERSION}.jar
    ```

    :::info:
    `${VERSION}`にはバージョン全体（例えば`1.7.0-RELEASE`）を含めるようにしてください。
    :::
    <!-- :::info: -->
    <!-- Make sure that you include the whole version, for example 1.6.0-RELEASE. -->
    <!-- ::: -->

ダウンロードにはしばらく時間がかかります。すべてうまくいけば、出力に次のようなものが表示されるはずです。
<!-- The download may take some time. You should see something like the following in the output if everything went well: -->

```bash
HTTP request sent, awaiting response ... 200 OK
'/home/jake/node/iri-1.6.0-RELEASE.jar' saved [175441686/175441686]
```

:::info:初めてのノードですか？
スクラッチからノードを起動する場合は、[IOTA財団のWebサイト](https://dbfiles.iota.org/?prefix=mainnet/spent-addresses/)、[IOTA Partners Webサイト](https://iota.partners/#database)、または[IOTA Playbook](https://iri-playbook.readthedocs.io/en/master/faq.html#where-can-i-get-a-fully-synced-database-to-help-kick-start-my-node)から`spent-addresses-db`ディレクトリをダウンロードする必要があります。

ディレクトリをダウンロードしたら、手順4のIRI Javaファイルと同じディレクトリに解凍します。この例では、ファイルは`/home/jake/node/`ディレクトリにあります。
:::
<!-- :::info:Is this your first node? -->
<!-- If you are starting a node from scratch, you need to download the `spent-addresses-db` directory from [our website](https://dbfiles.iota.org/?prefix=mainnet/spent-addresses/), [the IOTA Partners website](https://iota.partners/#database), or the [IOTA Playbook](https://iri-playbook.readthedocs.io/en/master/faq.html#where-can-i-get-a-fully-synced-database-to-help-kick-start-my-node). -->
<!--  -->
<!-- After you've downloaded the directory, extract it into the same directory as your IRI Java file from step 4. For this example, the file is in the `/home/jake/node/` directory. -->
<!-- ::: -->

これで、IRI Javaファイルと`spent-addresses-db`ディレクトリがサーバに保存されたので、実行する前に[IRIを設定](#configure-the-iri)します。
<!-- Now that the IRI Java file and the `spent-addresses-db` directory are saved on your server, [configure the IRI](#configure-the-iri) before running it. -->

### ソースコードからIRI Javaファイルをビルドする
<!-- ### Build the IRI Java file from the source code -->

ビルド済みのIRI Javaファイルをダウンロードする代わりに、次のいずれかの理由でソースコードからファイルをビルドすることをお勧めします。
<!-- Instead of downloading the pre-built IRI Java file, you may want to build the file from the source code the any of the following reasons: -->
* 実行するコードがソースコードと同じであることを確認したい。
<!-- * You want to be sure that the code you run is the same as the source code -->
* IRIを実行する前にコードを修正したい。
<!-- * You want to modify the code before you run it -->

1. Java 8 OpenJDKをダウンロードしてインストールします。
  <!-- 1. Download and install the Java 8 OpenJDK -->

    ```bash
    sudo apt-get install -y software-properties-common --no-install-recommends
    sudo apt-get install openjdk-8-jdk
    sudo apt-get update
    ```

2. [Maven](https://maven.apache.org/what-is-maven.html)ルドツールをインストールします。`USER_HOME_DIR`変数をご自身のディレクトリパスに変更します。
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

4. IRI GitHubリポジトリをクローンしてチェックアウトします。
  <!-- 4. Clone and check out the IRI GitHub repository -->

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
    IRI Javaファイルは、`target`というディレクトリにビルドされています。
    :::
    <!-- :::info: -->
    <!-- The IRI Java file is built in a directory called `target`. -->
    <!-- ::: -->

## IRIを設定する
<!-- ## Configure the IRI -->

IRIはJava仮想マシンで動作します。したがって、IRIを実行する前に、いくつかのJava変数を設定する必要があります。
<!-- The IRI runs in a Java virtual machine. Therefore, before you run the IRI, you need to set up some Java variables. -->

1. Java仮想マシンでIRIを実行するために使用されるJava変数を作成します。
  <!-- 1. Create the Java variables that'll be used to run the IRI in the Java virtual machine -->

    ```bash
    export JAVA_OPTIONS="-XX:+UnlockExperimentalVMOptions -XX:+DisableAttachMechanism -XX:InitiatingHeapOccupancyPercent=60 -XX:G1MaxNewSizePercent=75 -XX:MaxGCPauseMillis=10000 -XX:+UseG1GC"
    export JAVA_MIN_MEMORY=2G
    export JAVA_MAX_MEMORY=4G
    ```

    **JAVA_OPTIONS：** Java仮想マシンを最適化するコマンド
    <!-- **JAVA_OPTIONS:** Commands that optimise the Java virtual machine -->

    **JAVA_MIN_MEMORY：** Java仮想マシンの初期メモリ割り当て
    <!-- **JAVA_MIN_MEMORY:** The initial memory allocation for the Java virtual machine -->

    **JAVA_MAX_MEMORY：** Java仮想マシンの最大メモリ割り当て
    <!-- **JAVA_MAX_MEMORY:** the maximum memory allocation for the Java virtual machine -->

2. IRI Javaファイルと同じディレクトリにIRI設定ファイルを作成します。`jake`をご自身のLinuxユーザー名に変更してください。
  <!-- 2. Create an IRI configuration file in the same directory as your IRI Java file. Change `jake` to your Linux username. -->

    ```bash
    nano /home/jake/node/config.ini
    ```

    今のところ設定ファイルを空のままにします。ここでのセットアップでは、デフォルトの[IRI設定オプション](../references/iri-configuration-options.md)が適しています。設定オプションを変更したい場合は、config.iniファイルを編集して、変更したい設定オプションを追加してください。
    <!-- Leave the file empty for now. The default [IRI configuration options](../references/iri-configuration-options.md) are fine for this setup. If you want to change the configuration options, edit the config.ini file and add the configuration options that you want to change. -->

### permanodeを設定する
<!-- ### Configure a permanode -->

permanode（すべてのトランザクションを台帳に保持するノード）を実行する場合は、[`LOCAL_SNAPSHOTS_PRUNING_ENABLED`設定パラメータ](../references/iri-configuration-options.md#local-snapshots-enabled)を`false`に設定します。
<!-- If you want to run a permanode (keep all transactions in the ledger), set the [`LOCAL_SNAPSHOTS_PRUNING_ENABLED` configuration parameter](../references/iri-configuration-options.md#local-snapshots-enabled) to `false`. -->

### Devnetノードを設定する
<!-- ### Configure a Devnet node -->

Devnetノードを実行する場合は、`TESTNET`設定オプションを`true`に設定し、他のDevnetノードを[`NEIGHBORS`](../references/iri-configuration-options.md#neighbors)設定パラメータに追加する必要があります。
<!-- If you want to run a Devnet node, you must set the `TESTNET` configuration option to `true`, and add other Devnet nodes to the [`NEIGHBORS`](../references/iri-configuration-options.md#neighbors) configuration parameter. -->

次のDevnetノードは自動ピアリングを有効にしているので、自動的に隣接ノードとしてあなたのノードを追加します。
<!-- The following Devnet nodes have auto-peering enabled, so they will automatically add you as neighbors: -->

* udp://p101.testnet.iota.cafe:14666

* udp://p102.testnet.iota.cafe:14666

* udp://p103.testnet.iota.cafe:14666

* udp://p104.testnet.iota.cafe:14666

### Spamnetノードを設定する
<!-- ### Configure a Spamnet node -->

Spamnetノードを実行したい場合は、設定ファイルに以下の設定パラメータのみを含める必要があります。
<!-- If you want to run a Spamnet node, you must include only the following configuration parameters in your configuration file: -->

```
[IRI]
ZMQ_ENABLED = TRUE
TESTNET = TRUE
DB_PATH = spamnetdb
DB_LOG_PATH = spamnetdb.log
MWM = 7
SNAPSHOT_FILE = spamnet.txt
COORDINATOR = H9FXUMSYAWNZPVFINVTXOTYKFZXR9OBKA9KSTVWXTWHIZZRISFYZMXIMOQFXDXXQHNAJXAZFP9IHSFXRH
NUMBER_OF_KEYS_IN_A_MILESTONE = 20
SNAPSHOT_TIME = 1535760000
MILESTONE_START_INDEX = 2
DONT_VALIDATE_TESTNET_MILESTONE_SIG = true
NEIGHBORS = udp://p101.spamnet.iota.cafe:14600 udp://p102.spamnet.iota.cafe:14600
```

Spamnet上のトークンの総供給量を含むアドレスを定義するためのスナップショットファイルも作成しなければなりません。このファイルの場所は、`SNAPSHOT_FILE`設定パラメータで設定する必要があります。
<!-- You must also create a snapshot file to define an address that contains the entire supply of tokens on the Spamnet. The location of this file must be set in the `SNAPSHOT_FILE` configuration parameter. -->

```
WYF9OOFCQJRTLTRMREDWPOBQ9KNDMFVZSROZVXACAWKUMXAIYTFQCPAYZHNGKIWZZGKCSHSSTRDHDAJCW;2779530283277761
```

## IRIを実行する
<!-- ## Run the IRI -->

IRIをダウンロードして設定したら、実行します。
<!-- When you've downloaded, and configured the IRI, it's time to run it. -->

1. データベースとIXI（IOTA交換インターフェースフォルダ）を保存するディレクトリを作成します。`jake`をご自身のLinuxユーザー名に変更してください。
  <!-- 1. Make a directory to keep the database and the IXI (IOTA exchange interface folders). Change `jake` to your Linux username. -->

    ```bash
    mkdir -p /home/jake/node/data
    cd /home/jake/node/data
    ```

2. IRIを実行します。`jake`をご自身のLinuxユーザー名に変更し、`$VERSION`をダウンロードしたIRIのバージョンに変更します。
  <!-- 2. Run the IRI. Change `jake` to your Linux username and `$VERSION` to the version of the IRI that you downloaded. -->

    ```bash
    java ${JAVA_OPTIONS} -Xms${JAVA_MIN_MEMORY} -Xmx${JAVA_MAX_MEMORY} -Djava.net.preferIPv4Stack=true -jar /home/jake/node/iri-${VERSION}.jar
    ```
    IRIはアクティビティログを標準出力に出力します。
    <!-- The IRI should start to log its activity to the output. -->

    IRIに`config.ini`ファイルを読み込ませるには、`-c`フラグの後にパスを追加します。<br>例えば：
    <!-- To make the IRI read your `config.ini` file, add the path to it after the `-c` flag. For example: -->

    ```bash
    java ${JAVA_OPTIONS} -Xms${JAVA_MIN_MEMORY} -Xmx${JAVA_MAX_MEMORY} -Djava.net.preferIPv4Stack=true -jar /home/jake/node/iri-${VERSION}.jar -c /home/jake/node/config.ini
    ```

    :::success:おめでとうございます:tada:
    IRIノードを実行しています！
    :::
    <!-- :::success:Congratulations :tada: -->
    <!-- You're now running an IRI node! -->
    <!-- ::: -->

3. Linuxサーバーで新しいターミナルウィンドウを開き、CurlとJQをインストールします。CurlはREST APIリクエストをIRIノードに送信するために使用されます。JQは、JSONデータを読みやすい形式で表示するコマンドラインプロセッサです。
  <!-- 3. Open a new terminal window on your Linux server, and install Curl and JQ. Curl is used to send REST API requests to your IRI node. JQ is a command-line processor that displays JSON data in an easy-to-read format. -->

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

    標準出力には、`neighbors`フィールドの値が0であることがわかります。IRIノードはまだIOTAネットワークに接続されていません。IOTAネットワークに接続するには、[隣接IRIノード](../concepts/neighbor-iri-node.md)に接続する必要があります。
    <!-- You'll notice in the output that the value of the `neighbors` field is 0. The IRI node is not yet connected to an IOTA network. To do so, you need to connect to [neighbor IRI nodes](../concepts/neighbor-iri-node.md). -->

5. [隣接ノードを見つけて](../how-to-guides/find-neighbor-iri-nodes.md)、隣接ノードのURLまたはIPアドレスをconfig.iniファイルに追加します。
  <!-- 5. [Find neighbors](../how-to-guides/find-neighbor-iri-nodes.md) and add their URL or IP addresses to your config.ini file -->

    :::info:
    IRIノードに対して行われるリクエストをより細かく制御できるように、IRIノードに[リバースプロキシを設定する](../how-to-guides/set-up-a-reverse-proxy.md)ことをお勧めします。
    :::
    <!-- :::info: -->
    <!-- We recommend [setting up a reverse proxy](../how-to-guides/set-up-a-reverse-proxy.md) for your IRI node so that you can have more control over the requests that are made to it. -->
    <!-- ::: -->

ノードが起動して実行されているので、[IOTAネットワークを使い台帳の同期](../concepts/the-ledger.md#ledger-synchronization)を開始します。ノードに同期をとる時間を与えます。IRIノードが同期していない場合は、[トラブルシューティングガイド](../references/troubleshooting.md)をお読みください。
<!-- Now that your node is up and running, it'll start to [synchronize its ledger with the network](../concepts/the-ledger.md#ledger-synchronization). Give your node some time to synchronize, or read our troubleshooting guide if your IRI node isn't synchronizing. -->

## IRIが同期していることを確認します
<!-- ## Check that the IRI is synchronized -->

`latestMilestoneIndex`フィールドが`latestSolidSubtangleMilestoneIndex`フィールドと等しい場合、IRIは同期していると見なされます。
<!-- The IRI is considered synchronized when the `latestMilestoneIndex` field is equal to the `latestSolidSubtangleMilestoneIndex` field. -->

`latestMilestoneIndex`フィールドは、IRIが隣接ノードから受け取った最新のマイルストーンのインデックスです。
<!-- The `latestMilestoneIndex` field is the index of the latest milestone that the IRI has received from its neighbors. -->

`latestSolidSubtangleMilestoneIndex`フィールドは、IRIノードがマイルストーンを凝固にした（マイルストーンが直接および間接的に参照するすべてのトランザクションをIRIノードが持った）最新のマイルストーンのインデックスです。
<!-- The `latestSolidSubtangleMilestoneIndex` field is the index of the latest milestone for which the IRI node's ledger has all the transactions that the milestone directly and indirectly references. -->

`latestMilestoneIndex`フィールドと`latestSolidSubtangleMilestoneIndex`フィールドは、IRIノードが同期済み隣接ノードに接続されている場合にのみ正確です。
<!-- The `latestMilestoneIndex` and `latestSolidSubtangleMilestoneIndex` fields are accurate only when the IRI node is connected to synchronized neighbors. -->

1. 実際の`latestMilestoneIndex`フィールドを確認するには、[Discord](https://discordapp.com/invite/fNGZXvh)に移動し、いずれかのチャンネルに**!milestone**と入力してください。
  <!-- 1. To check the actual `latestMilestoneIndex` field, go to our [Discord](https://discordapp.com/invite/fNGZXvh) and enter **!milestone** in one of the channels -->

    ![Entering !milestone on Discord](../images/discord-milestone-check.PNG)

2. IRIノードの`latestMilestoneIndex`フィールドと`latestSolidSubtangleMilestoneIndex`フィールドを確認するには、`getNodeInfo` APIエンドポイントを呼び出します。
  <!-- 2. To check these fields for your IRI node, call the `getNodeInfo` API endpoint -->

    ```bash
    sudo apt install curl jq
    curl -s http://localhost:14265 -X POST -H 'X-IOTA-API-Version: 1' -H 'Content-Type: application/json' -d '{"command": "getNodeInfo"}' | jq
    ```

:::info:
IRIが同期するまでに時間がかかることがあります。問題がある場合は、[トラブルシューティングガイド](../references/troubleshooting.md)をお読みください。
:::
<!-- :::info: -->
<!-- It may take some time for the IRI to synchronize. For help with any issues, read our [troubleshooting guide](../references/troubleshooting.md). -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

* [IRIノードのイベントを購読する](../how-to-guides/subscribe-to-events-in-an-iri-node.md)。
<!-- * [Subscribe to events in an IRI node](../how-to-guides/subscribe-to-events-in-an-iri-node.md) -->
