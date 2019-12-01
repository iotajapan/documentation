# 自分自身の IRI ノードを実行する（Docker）
<!-- # Run your own IRI node -->

**IRI [ノード](../network/nodes.md)は、IRI ノードソフトウェアを実行するサーバーです。IRI ノードソフトウェアは、[Mainnet と Devnet ネットワーク](../network/iota-networks.md)でトランザクションを検証および保存するために使用されます。**
<!-- **An IRI [node](../network/nodes.md) is a server that runs the IRI node software. This software is used to validate and store transactions on the [Mainnet and Devnet networks](../network/iota-networks.md).** -->

自分自身のIRIノードを実行すると、次の利点があります。
<!-- By running your own IRI node, you have the following benefits: -->

- 誰かのノードに接続する代わりに、タングルに直接アクセスできます。
<!-- - You have your own direct access to the Tangle instead of having to connect to someone else's node -->
- 他のユーザーのトランザクションを検証することにより、IOTA ネットワークの分散化を支援します。
<!-- - You help the IOTA network to become more distributed by validating other users' transactions -->

このガイドでは、[Docker](https://www.docker.com/) コンテナで自分自身の IRI ノードを実行します。
<!-- In this guide, you run your own IRI node in a [Docker](https://www.docker.com/) container. -->

## 前提条件
<!-- ## Prerequisites -->

このチュートリアルを完了するには、コマンドラインインターフェイスと次のいずれかのオペレーティングシステムにアクセスする必要があります。
<!-- To complete this tutorial, you need access to a command-line interface and one of the following operating systems: -->

- Linux

- MacOSX

- Windows

Linuxオペレーティングシステムを使用している場合は、次の手順のコマンドの前に`sudo`を追加します。
<!-- If you're using a Linux operating system, add `sudo` before the commands in the following steps. -->

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、[Mainnet](../network/iota-networks.md#mainnet)でノードを実行します。
<!-- In this guide, we run a node on the [Mainnet](../network/iota-networks.md#mainnet). -->

---
1. [Dockerをインストール](https://docs.docker.com/install/#supported-platforms)します。システム要件よりも古いバージョンの Mac または Windows を実行している場合は、代わりに[Docker Toolbox](https://docs.docker.com/toolbox/overview/)をインストールします。
<!-- 1. [Install Docker](https://docs.docker.com/install/#supported-platforms). If you're running a version of Mac or Windows that's older than the system requirements, install the [Docker toolbox](https://docs.docker.com/toolbox/overview/) instead. -->

2. Docker がインストールされていることを確認します。
  <!-- 2. Make sure that Docker is installed -->

    ```bash
    docker run hello-world
    ```

    以下のような Docker 情報が表示されるはずです。
    <!-- You should see some Docker information like the following: -->

    ```bash
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

3. IRI をビルド済みの Docker コンテナをダウンロードします。
  <!-- 3. Download the pre-built Docker container -->

    ```bash
    docker pull iotaledger/iri:latest
    ```

4. IRI を実行します。
  <!-- 4. Run the IRI -->

    ```bash
    docker run --name iri iotaledger/iri:latest --remote true -p 14265
    ```

    IRI ノードは現在実行中で、次の URL：`http://localhost.com:14265` で[IRI API](root://node-software/0.1/iri/references/api-reference.md)を通して IRI ノードと対話することができます。
    <!-- Your IRI node is now running and you can interact with it through the [IRI API](root://node-software/0.1/iri/references/api-reference.md) at the following URL: -->
    <!-- http://localhost.com:14265 -->

5. cURL を使用して[`getNodeInfo` endpoint](root://node-software/0.1/iri/references/api-reference.md#getNodeInfo)にリクエストを送信します。
  <!-- 5. Use cURL to send a request to the [`getNodeInfo` endpoint](root://node-software/0.1/iri/references/api-reference.md#getNodeInfo) -->
    ```bash
    curl http://localhost:14265 -X POST -H 'Content-Type: application/json' -H 'X-IOTA-API-Version: 1' -d '{"command": "getNodeInfo"}'
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

出力では、`neighbors`フィールドの値は0です。これは、IRI ノードが IOTA ネットワークに接続されていないことを意味します。そのためには、隣接 IRI ノードに接続する必要があります。
<!-- In the output, the value of the `neighbors` field is 0. This means that your node is not connected to an IOTA network. To do so, you need to connect to neighbors. -->

隣接 IRI ノードと繋がるには、[Discord](https://discord.iota.org)の #help または #nodesharing チャンネルにアクセスしてください。
<!-- For help connecting to neighbors, go to the #help or #nodesharing channel on our [Discord](https://discord.iota.org). -->

## 次のステップ
<!-- ## Next steps -->

[ノードの実行に関する詳細なガイドを読む](root://node-software/0.1/iri/introduction/overview.md)。
<!-- [Read more in-depth guides about running a node](root://node-software/0.1/iri/introduction/overview.md). -->
