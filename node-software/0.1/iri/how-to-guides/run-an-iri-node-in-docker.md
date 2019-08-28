# DockerコンテナでIRIノードを実行する
<!-- # Run an IRI node in a Docker container -->

**Dockerコンテナ内でIRIを実行すると、デバイスはIRIノード用のLinuxサーバになり、タングルに直接アクセスできるようになります。 IRIノードを実行することで、台帳の数を増やし、隣接IRIノードのトランザクションを検証することで、IOTAネットワークをより分散させることができます。**
<!-- **When you run the IRI in a Docker container, your device becomes a Linux server for an IRI node, which gives you direct access to the Tangle. By running an IRI node, you help the IOTA network to become more distributed by adding to the number of ledgers and validating your neighbor IRI node's transactions.** -->

IRI Dockerコンテナは、以下のオペレーティングシステムに適しています。
<!-- The IRI Docker container is suitable for the following operating systems: -->
* Linux
* Mac
* Windows

Linuxオペレーティングシステムを使用している場合は、このガイドのコマンドの前に`sudo`を追加します。
<!-- If you're using a Linux operating system, add `sudo` before the commands in this guide. -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完成するには、次のものが必要です。
<!-- To complete this guide, you need the following: -->

* 4GB RAM
* 64ビットプロセッサ
<!-- * 64-bit processor -->
* インターネット接続
<!-- * An Internet connection -->
* 静的な、または[duckdns.org](https://www.duckdns.org)などの動的DNSサービスに接続されている[パブリックIPアドレス](root://general/0.1/how-to-guides/expose-your-local-device.md)
<!-- * A [public IP address](root://general/0.1/how-to-guides/expose-your-local-device.md) that's either static or connected to a dynamic DNS service such as [duckdns.org](https://www.duckdns.org) -->
* ノードを実行しているデバイスに[次のポートを転送します](root://general/0.1/how-to-guides/expose-your-local-device.md)。
  <!-- * [Forward the following ports](root://general/0.1/how-to-guides/expose-your-local-device.md) to the device that's running the node: -->

    * **TCP隣接ノードのピアリングポート：** 15600
    <!-- * **TCP neighbor peering port:** 15600 -->
    * **TCPのAPIポート：** 14265
    <!-- * **TCP API port:** 14265 -->

Dockerコンテナは、以下のオペレーティングシステムに適しています。
<!-- The Docker container is suitable for the following operating systems: -->
* Linux
* macOS
* Windows

:::info:
Linuxオペレーティングシステムを使用している場合は、以下のタスクのすべてのコマンドの前に`sudo`を追加します。
:::
<!-- :::info: -->
<!-- If you're using a Linux operating system, add `sudo` before all the commands in the following tasks. -->
<!-- ::: -->

## 手順1. Dockerをインストールする
<!-- ## Step 1. Install Docker -->

Dockerコンテナーをビルドするには、Docker 17.05（マルチステージビルドサポート用）をデバイスにインストールする必要があります。
<!-- To build the Docker container, you must install Docker 17.05+ (for multi-stage build support) on your device. -->

1. [Dockerをインストールします](https://docs.docker.com/install/#supported-platforms)。システム要件よりも古いバージョンのmacOSまたはWindowsを実行している場合は、代わりに[Dockerツールボックス](https://docs.docker.com/toolbox/overview/)をインストールしてください。
  <!-- 1. [Install Docker](https://docs.docker.com/install/#supported-platforms). If you're running a version of macOS or Windows that's older than the system requirements, install the [Docker toolbox](https://docs.docker.com/toolbox/overview/) instead. -->

2. Dockerがインストールされていることを確認します。
  <!-- 2. Make sure that Docker is installed -->

    ```bash
    docker run hello-world
    ```

    以下のようなDocker情報が表示されるはずです。
    <!-- You should see some Docker information like the following: -->

    ```
    Unable to find image 'hello-world:latest' locally
    latest: Pulling from library/hello-world
    1b930d010525: Pull complete
    Digest: sha256:2557e3c07ed1e38f26e389462d03ed943586f744621577a99efb77324b0fe535
    Status: Downloaded newer image for hello-world:latest

    Hello from Docker!
    This message shows that your installation appears to be working correctly.

    To generate this message, Docker took the following steps:
    1. The Docker client contacted the Docker daemon.
    2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
        (amd64)
    3. The Docker daemon created a new container from that image which runs the
        executable that produces the output you are currently reading.
    4. The Docker daemon streamed that output to the Docker client, which sent it
        to your terminal.

    To try something more ambitious, you can run an Ubuntu container with:
    docker run -it ubuntu bash

    Share images, automate workflows, and more with a free Docker ID:
    https://hub.docker.com/

    For more examples and ideas, visit:
    https://docs.docker.com/get-started/
    ```

## 手順.2 IRI Dockerコンテナをダウンロードする
<!-- ## Step 2. Download the IRI Docker container -->

IRIはJavaソフトウェアなので、Javaランタイム環境（JRE）で実行する必要があります。
<!-- The IRI is Java software, so it must be run in a Java runtime environment (JRE). -->
IRI Dockerコンテナには、IRIを実行するために必要なソフトウェアが含まれています。
<!-- The IRI Docker container contains the necessary software to run the IRI. -->

IRI Dockerコンテナをダウンロードする方法は2つあります。
<!-- You have two options for downloading the IRI Docker container: -->
* [ビルド済みのDockerコンテナをダウンロードする](#download-the-pre-built-iri-docker-container)（最速のオプション）。
<!-- * [Download the pre-built Docker container](#download-the-pre-built-iri-docker-container)(quickest option) -->
* [ソースコードからDockerコンテナをビルドする](#build-the-iri-docker-container-from-the-source-code)。
<!-- * [Build the Docker container from the source code](#build-the-iri-docker-container-from-the-source-code) -->

<a name="download-the-pre-built-iri-docker-container"></a>
## ビルド済みのIRI Dockerコンテナをダウンロードする
<!-- ### Download the pre-built IRI Docker container -->

ビルド済みのIRI Javaファイル用のDockerコンテナは、IOTA GitHubリポジトリにあります。
<!-- The Docker container for the pre-built IRI Java file is available on the IOTA GitHub repository. -->

```bash
docker pull iotaledger/iri:latest
```

<a name="build-the-iri-docker-container-from-the-source-code"></a>
## ソースコードからIRI Dockerコンテナをビルドする
<!-- ### Build the IRI Docker container from the source code -->

ビルド済みのDockerコンテナをダウンロードする代わりに、次のいずれかの理由でソースコードからファイルをビルドすることをお勧めします。
<!-- Instead of downloading the pre-built Docker container, you may want to build the file from the source code for any of the following reasons: -->
* 実行するコードがソースコードと同じであることを確認したい場合。
<!-- * You want to be sure that the code you run is the same as the source code -->
* IRIを実行する前にコードを修正したい場合。
<!-- * You want to modify the code before you run it -->

1. [Gitをインストールします](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)。
  <!-- 1. [Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) -->

2. Gitがインストールされていることを確認します。
  <!-- 2. Make sure that Git is installed -->

    ```bash
    git --version
    ```

    Gitのバージョンをぜひご確認ください。
    <!-- You should see the version number of your Git installation. -->

3. 最新バージョンのIRIをビルドします。
  <!-- 3. Build the latest version of the IRI -->

    ```bash
    git clone https://github.com/iotaledger/iri.git
    cd iri
    export TAG=$(git describe --tags $(git rev-list --tags --max-count=1))
    git checkout ${TAG}
    docker build -t iri .
    ```

## 手順2. IRIを実行する
<!-- ## Step 2. Run the IRI -->

`run`コマンドに[設定オプション](../references/iri-configuration-options.md)をフラグとして渡すことでノードの設定ができます。
<!-- You can configure the node by passing [configuration options](../references/iri-configuration-options.md) to the `run` command as flags. -->

1. APIポートを指定するために`-p`フラグを指定してIRIを実行します。
  <!-- 1. Run the IRI with the `-p` flag to specify the API port -->

    ```bash
    docker run --name iri iotaledger/iri:latest --remote -p 14265
    ```

    設定オプションをIRI設定ファイルに保存したい場合は、IRI設定ファイルへのパスを`-c`フラグと一緒に渡す必要があります。たとえば、Dockerホストの`/path/to/conf/config.ini`にconfig.iniファイルを保存した場合は、DOCKER RUNコマンドに`-c /path/to/conf/config.ini`を追加します。
    <!-- If you want to save your configuration options in an IRI configuration file, you must pass the path to that file along with the `-c` flag. For example, if you save a config.ini file in the `/path/to/conf/config.ini` on your Docker host, then add `-c /path/to/conf/config.ini` to the DOCKER RUN command. -->

    :::info:
    ソースコードからIRI Dockerコンテナをビルドした場合は、`-name`フラグの値を`iri iri：latest`に変更する必要があります。
    :::
    <!-- :::info: -->
    <!-- If you built the IRI Docker container from the source code, you must change the value of the `-name` flag to `iri iri:latest`. -->
    <!-- ::: -->
    :::info:
    Dockerホストの再起動のたびにIRI Dockerコンテナも再起動するには、DOCKER RUNコマンドに`--restart = always`フラグを追加します。
    :::
    <!-- :::info: -->
    <!-- To have the IRI Docker container restart on every reboot, add the `--restart=always` flag to the DOCKER RUN command. -->
    <!-- ::: -->

2. [getNodeInfo](../references/api-reference.md#getnodeinfo)エンドポイントを呼び出して、IRIノードに関する一般情報をリクエストします。
  <!-- 2. Call the [getNodeInfo](../references/api-reference.md#getnodeinfo) endpoint to request general information about the IRI node -->

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

    標準出力には、`neighbors`フィールドの値が0であることがわかります。IRIノードはまだIOTAネットワークに接続されていません。IOTAネットワークへの接続を行うには、[隣接IRIノード](../concepts/neighbor-iri-node.md)に接続する必要があります。
    <!-- You'll notice in the output that the value of the `neighbors` field is 0. The IRI node is not yet connected to an IOTA network. To do so, you need to connect to [neighbor IRI nodes](../concepts/neighbor-iri-node.md). -->

3. [隣接ノードを見つけて](../how-to-guides/find-neighbor-iri-nodes.md)、隣接ノードのURLまたはIPアドレスをconfig.iniファイルに追加します。
  <!-- 3. [Find neighbors](../how-to-guides/find-neighbor-iri-nodes.md) and add their URL or IP addresses to your config.ini file -->

  <!-- Now that your node is up and running, it'll start to [synchronize its ledger with the network](../concepts/the-ledger.md#ledger-synchronization). Give your node some time to synchronize, or read our troubleshooting guide if your IRI node isn't synchronizing. -->

## 手順3. ノードが同期していることを確認する
<!-- ## Step 3. Check that the node is synchronized -->

`latestMilestoneIndex`フィールドが`latestSolidSubtangleMilestoneIndex`フィールドと等しい場合、ノードは同期していると見なされます。
<!-- A node is considered synchronized when the `latestMilestoneIndex` field is equal to the `latestSolidSubtangleMilestoneIndex` field. -->

`latestMilestoneIndex`フィールドは、ノードが隣接ノードから受け取った最新のマイルストーンのインデックスです。
<!-- The `latestMilestoneIndex` field is the index of the latest milestone that the node has received from its neighbors. -->

`latestSolidSubtangleMilestoneIndex`フィールドは、ノードがマイルストーンを凝固（マイルストーンが直接および間接的に参照するすべてのトランザクションをノードが持った状態）にした最新のマイルストーンのインデックスです。
<!-- The `latestSolidSubtangleMilestoneIndex` field is the index of the latest milestone for which the node's ledger has all the transactions that the milestone directly and indirectly references. -->

`latestMilestoneIndex`フィールドと`latestSolidSubtangleMilestoneIndex`フィールドは、ノードが同期済み隣接ノードに接続されている場合にのみ正確です。
<!-- The `latestMilestoneIndex` and `latestSolidSubtangleMilestoneIndex` fields are accurate only when the node is connected to synchronized neighbors. -->

1. 現在の`latestMilestoneIndex`フィールドを確認するには、[Discord](https://discord.iota.org)に移動し、いずれかのチャンネルに**!milestone**と入力してください。
  <!-- 1. To check the current `latestMilestoneIndex` field, go to our [Discord](https://discord.iota.org) and enter **!milestone** in one of the channels -->

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

* [IRIノードと対話する](../how-to-guides/interact-with-an-iri-node.md)。
<!-- * [Interact with an IRI node](../how-to-guides/interact-with-an-iri-node.md) -->
* [IRIノードのイベントを購読する](../how-to-guides/subscribe-to-events-in-an-iri-node.md)。
<!-- * [Subscribe to events in an IRI node](../how-to-guides/subscribe-to-events-in-an-iri-node.md) -->
