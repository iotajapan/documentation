# C クイックスタート
<!-- # C quickstart -->

**このクイックスタートでは，開発環境の設定からタングルでのライブトランザクションのリッスンまで，C での IOTA 開発の基本を学びます．**
<!-- **In this quickstart, you learn the basics of IOTA development in C, from setting up a development environment to listening for live transaction on the Tangle.** -->

このクイックスタートでは，次の方法を学習します．
<!-- In this quickstart, you will learn how to: -->

1. 開発環境をセットアップする
<!-- 1. Set up a developer environment -->

2. パッケージをインストールする
<!-- 2. Install packages -->

3. ノードに接続する
<!-- 3. Connect to a node -->

## 手順1. 開発環境をセットアップする
<!-- ## Step 1. Set up a developer environment -->

C クライアントライブラリを使用するには，開発環境を構成するプログラミングツールのセットが必要です．
<!-- To use the C client library, you need a set of programming tools, which make up a development environment. -->

:::info:
ガイドでは，Linux [Ubuntu 18.04 LTS](https://www.ubuntu.com/download/server) オペレーティングシステムを使用しています．Windows または Mac オペレーティングシステムを使用している場合は，これらの例を Linux [Ubuntu 18.04 LTS](https://www.ubuntu.com/download/server) 以外のオペレーティングシステムで試して実行するか，[仮想マシンで Linux サーバーを作成する](root://general/0.1/how-to-guides/set-up-virtual-machine.md)ことができます．
:::
<!-- :::info: -->
<!-- In our guides, we use a Linux [Ubuntu 18.04 LTS](https://www.ubuntu.com/download/server) operating system. If you are on a Windows or Mac operating system, you can try and run these examples on your operating system, or you can [create a Linux server in a virtual machine](root://general/0.1/how-to-guides/set-up-virtual-machine.md). -->
<!-- ::: -->

1. [Bazel ビルドツールをインポートします](https://docs.bazel.build/versions/master/install.html)．
  <!-- 1. [Install the Bazel build tool](https://docs.bazel.build/versions/master/install.html) -->

    :::info:
    Bazel を初めて使用する場合は，[入門ドキュメント](https://docs.bazel.build/versions/master/getting-started.html)を読むことをお勧めします．
    :::
    <!-- :::info: -->
    <!-- If you're new to [Bazel](https://docs.bazel.build/versions/master/getting-started.html), we recommend reading their getting-started documentation. -->
    <!-- ::: -->

2. [Linux ターミナル](https://www.howtogeek.com/140679/beginner-geek-how-to-start-using-the-linux-terminal/)を開きます．
<!-- 2. Open the [Linux Terminal](https://www.howtogeek.com/140679/beginner-geek-how-to-start-using-the-linux-terminal/) -->

3. プロジェクトのディレクトリを作成します．
  <!-- 3. Create a directory for your project -->

    ```bash
    sudo mkdir my-c-iota-project
    ```

これで，パッケージのインストールを開始する準備ができました．
<!-- Now you're ready to start installing packages. -->

## 手順2. パッケージをインストールする
<!-- ## Step 2. Install packages -->

C クライアントライブラリは，関連するメソッドを含むパッケージで構成されています．ノードからの情報のリクエスト，トランザクションの作成，ノードへの送信など，IOTA 関連のすべてのメソッドは [`api` パッケージ](https://github.com/iotaledger/entangled/tree/develop/cclient/api)にあります．
<!-- The C client library is organized in packages, which contain related methods. All the IOTA-related methods such as requesting information from nodes, creating transactions, and sending them to nodes, are located in the [`api` package](https://github.com/iotaledger/entangled/tree/develop/cclient/api). -->

1. プロジェクトディレクトリのルートで，`WORKSPACE` というファイルを作成し，次のコンテンツを追加します．これにより，ライブラリの依存関係がロードされます．
  <!-- 1. In the root of your project directory, create a file called `WORKSPACE` and add the following content, which loads the library's dependencies. -->

    `$ENTANGLED_COMMIT_HASH` プレースホルダーを `entangled` リポジトリの最新の Git コミットハッシュに置き換えます．
    <!-- Replace the `$ENTANGLED_COMMIT_HASH` placeholder with the latest Git commit hash of the `entangled` repository. -->

    `$RULES_IOTA_COMMIT_HASH` プレースホルダーを `rules_iota` リポジトリの最新の Git コミットハッシュに置き換えます．
    <!-- Replace the `$RULES_IOTA_COMMIT_HASH` placeholder with the latest Git commit hash of the `rules_iota` repository. -->

    ```bash
    git_repository(
        name = "entangled",
        commit = "$ENTANGLED_COMMIT_HASH",
        remote = "https://github.com/iotaledger/entangled.git",
    )

    # 外部ライブラリのビルドルール
    git_repository(
        name = "rules_iota",
        commit = "$RULES_IOTA_COMMIT_HASH",
        remote = "https://github.com/iotaledger/rules_iota.git",
    )

    load("@rules_iota//:defs.bzl", "iota_deps")

    iota_deps()
    ```

2. 構成ファイルとネットワークコードを保存する `iota_client_service` ディレクトリを作成します．
<!-- 2. Create an `iota_client_service` directory in which to store configuration files and the networking code -->

3. `iota_client_service` ディレクトリで，`config.h` ファイルを作成し，以下を追加します．
  <!-- 3. In the `iota_client_service` directory, create a `config.h` file and add the following: -->

    ```cpp
    // 接続先の IOTA ノードとそのポート
    #define CONFIG_IRI_NODE_URI "nodes.devnet.thetangle.org"

    #define CONFIG_IRI_NODE_PORT 443

    // サーバーで TLS が有効になっているかどうか
    #define CONFIG_ENABLE_HTTPS

    // サーバーで TLS が有効になっている場合，この定数は TLS 証明書を定義します
    #define TLS_CERTIFICATE_PEM \
            "-----BEGIN CERTIFICATE-----\r\n" \
            "MIIDQTCCAimgAwIBAgITBmyfz5m/jAo54vB4ikPmljZbyjANBgkqhkiG9w0BAQsF\r\n" \
            "ADA5MQswCQYDVQQGEwJVUzEPMA0GA1UEChMGQW1hem9uMRkwFwYDVQQDExBBbWF6\r\n" \
            "b24gUm9vdCBDQSAxMB4XDTE1MDUyNjAwMDAwMFoXDTM4MDExNzAwMDAwMFowOTEL\r\n" \
            "MAkGA1UEBhMCVVMxDzANBgNVBAoTBkFtYXpvbjEZMBcGA1UEAxMQQW1hem9uIFJv\r\n" \
            "b3QgQ0EgMTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBALJ4gHHKeNXj\r\n" \
            "ca9HgFB0fW7Y14h29Jlo91ghYPl0hAEvrAIthtOgQ3pOsqTQNroBvo3bSMgHFzZM\r\n" \
            "9O6II8c+6zf1tRn4SWiw3te5djgdYZ6k/oI2peVKVuRF4fn9tBb6dNqcmzU5L/qw\r\n" \
            "IFAGbHrQgLKm+a/sRxmPUDgH3KKHOVj4utWp+UhnMJbulHheb4mjUcAwhmahRWa6\r\n" \
            "VOujw5H5SNz/0egwLX0tdHA114gk957EWW67c4cX8jJGKLhD+rcdqsq08p8kDi1L\r\n" \
            "93FcXmn/6pUCyziKrlA4b9v7LWIbxcceVOF34GfID5yHI9Y/QCB/IIDEgEw+OyQm\r\n" \
            "jgSubJrIqg0CAwEAAaNCMEAwDwYDVR0TAQH/BAUwAwEB/zAOBgNVHQ8BAf8EBAMC\r\n" \
            "AYYwHQYDVR0OBBYEFIQYzIU07LwMlJQuCFmcx7IQTgoIMA0GCSqGSIb3DQEBCwUA\r\n" \
            "A4IBAQCY8jdaQZChGsV2USggNiMOruYou6r4lK5IpDB/G/wkjUu0yKGX9rbxenDI\r\n" \
            "U5PMCCjjmCXPI6T53iHTfIUJrU6adTrCC2qJeHZERxhlbI1Bjjt/msv0tadQ1wUs\r\n" \
            "N+gDS63pYaACbvXy8MWy7Vu33PqUXHeeE6V/Uq2V8viTO96LXFvKWlJbYK8U90vv\r\n" \
            "o/ufQJVtMVT8QtPHRh8jrdkPSHCa2XV4cdFyQzR1bldZwgJcJmApzyMZFo6IQ6XU\r\n" \
            "5MsI+yMRQ+hDKXJioaldXgjUkK642M4UwtBV8ob2xJNDd2ZhwLnoQdeXeGADbkpy\r\n" \
            "rqXRfboQnoZsG4q5WTP468SQvvG5\r\n" \
            "-----END CERTIFICATE-----\r\n"

    // チップ選択に使用するノードの適切な深度
    #define DEPTH 3

    // デブネットの最小重量値（メインネットの場合は14を使用）
    #define MINIMUM_WEIGHT_MAGNITUDE 9

    // アドレスに使用するセキュリティレベル
    #define SECURITY_LEVEL 2
    ```

    この例では，[デブネット](root://getting-started/0.1/network/iota-networks.md) [ノード](root://getting-started/0.1/network/nodes.md)に接続するようにプロジェクトを構成します．したがって，9の[最小重量値](root://getting-started/0.1/network/minimum-weight-magnitude.md)を使用します．また，アドレスの生成に使用する[セキュリティレベル](root://getting-started/0.1/clients/security-levels.md)2を定義します．
    <!-- In this example, we configure our project to connect to a [Devnet](root://getting-started/0.1/network/iota-networks.md) [node](root://getting-started/0.1/network/nodes.md), so we use a [minimum weight magnitude](root://getting-started/0.1/network/minimum-weight-magnitude.md) of 9. We also define a [security level](root://getting-started/0.1/clients/security-levels.md) of 2 to use for generating addresses. -->

3. `iota_client_service` ディレクトリで，`client_service.c` ファイルを作成し，以下を追加します．
  <!-- 3. In the `iota_client_service` directory, create a `client_service.c` file and add the following: -->

    ```cpp
    #include "cclient/api/core/core_api.h"
    #include "cclient/api/extended/extended_api.h"

    #include "common/trinary/tryte_ascii.h"
    #include <inttypes.h>

    #include "config.h"
    #include "client_service.h"

    void init_iota_client(iota_client_service_t *const service)
    {
        service->http.path = "/";
        service->http.content_type = "application/json";
        service->http.accept = "application/json";
        service->http.host = CONFIG_IRI_NODE_URI;
        service->http.port = CONFIG_IRI_NODE_PORT;
        service->http.api_version = 1;
    #ifdef CONFIG_ENABLE_HTTPS
        service->http.ca_pem = TLS_CERTIFICATE_PEM;
    #else
        service->http.ca_pem = NULL;
    #endif
        service->serializer_type = SR_JSON;
        iota_client_core_init(service);
        iota_client_extended_init();
    }
    ```

    このコードは，ノードへの[サービス](https://github.com/iotaledger/entangled/blob/develop/cclient/service.h)接続を処理します．
    <!-- This code handles the [service](https://github.com/iotaledger/entangled/blob/develop/cclient/service.h) connection to the node. -->

4. `iota_client_service` ディレクトリで，`client_service.h` ファイルを作成し，以下を追加します．
  <!-- 4. In the `iota_client_service` directory, create a `client_service.h` file and add the following: -->

    ```cpp
    #include "cclient/api/core/core_api.h"
    #include "cclient/api/extended/extended_api.h"

    void init_iota_client(iota_client_service_t *const service);
    ```

    このコードは `init_iota_client()` 関数を宣言します．このようにして，コンパイルされたプログラムには `init_iota_client()` 関数のコピーが1つだけ含まれ，プログラム内のすべてのモジュールが `init_iota_client()` 関数を使用できます．
    <!-- This code declares the `init_iota_client()` function. This way, your compiled program will contain just one copy of the function, and every module in your program can use it. -->

5. `iota_client_service` ディレクトリで，`BUILD` ファイルを作成し，以下を追加します．
  <!-- 5. In the `iota_client_service` directory, create a `BUILD` file and add the following: -->

    ```bash
    package(default_visibility = ["//visibility:public"])

    cc_library(
        name = "service",
        srcs = [
            "client_service.c",
        ],
        hdrs = [
            "client_service.h",
            "config.h"
        ],
        deps = [
            "@entangled//cclient/api",
        ],
    )
    ```

    このコードは，作成した構成およびサービスターゲットを構築するためのルールを作成します．
    <!-- This code creates the rules for building the configuration and service targets that you just wrote. -->

これで，ノードから情報をリクエストできます．
<!-- Now you can request information from the node. -->

## 手順3. ノードに接続する
<!-- ## Step 3. Connect to a node -->

トランザクションの送信を開始する前に，[同期済みノード](root://getting-started/0.1/network/nodes.md#synchronized-nodes)に接続していることを確認することをお勧めします．同期済みのノードに接続しているときにのみ，あなたは最新の[タングル](root://getting-started/0.1/network/the-tangle.md)の概観が分かります．
<!-- It's best practice to make sure that you're connected to a [synchronized node](root://getting-started/0.1/network/nodes.md#synchronized-nodes) before you start sending transactions to it. This way, you know that it has an up-to-date view of [the Tangle](root://getting-started/0.1/network/the-tangle.md). -->

ノードに接続するたびに，どの [IOTA ネットワーク](root://getting-started/0.1/network/iota-networks.md)に接続しているかを知る必要があります．ここでは，テストに使用できる IOTA ネットワークであるデブネットのノードに接続します．
<!-- Whenever you connect to a node, you need to know which [IOTA network](root://getting-started/0.1/network/iota-networks.md) it's in. Here, we connect to a node on the Devnet, which is the IOTA networks that you can use for testing. -->

1. IOTA 財団の公式 [Discord](https://discord.iota.org) に移動し，`botbox` チャンネルに **!milestone ** と入力します
  <!-- 1. Go to the IOTA Foundation [Discord](https://discord.iota.org) and enter **!milestone** in the `botbox` channel -->

    ![Entering !milestone on Discord](../images/discord-milestone-check.PNG)

    Discord ボットは，[ノードクォーラム](root://getting-started/0.1/network/nodes.md#node-quorum)から現在の `latestMilestoneIndex` フィールドを返します．
    <!-- The Discord bot should return the current `latestMilestoneIndex` field from a [node quorum](root://getting-started/0.1/network/nodes.md#node-quorum). -->

2. プロジェクトディレクトリのルートに，`examples` という新しいディレクトリを作成します．
<!-- 2. In the root of your project directory, create a new directory called `examples` -->

3. `examples` ディレクトリで，`hello_world.c` というファイルを作成し，以下を追加します．
  <!-- 3. In the `examples` directory, create a file called `hello_world.c` and add the following: -->

    ```cpp
    #include "cclient/api/core/core_api.h"
    #include "cclient/api/extended/extended_api.h"

    #include <inttypes.h>
    #include "iota_client_service/config.h"
    #include "iota_client_service/client_service.h"

    retcode_t get_iota_node_info(iota_client_service_t *iota_client_service, get_node_info_res_t *node_response) {
        retcode_t ret = RC_ERROR;
        // ノードへ接続します
        ret = iota_client_get_node_info(iota_client_service, node_response);

        // 変数を定義して，トリット変換が成功するかどうかを判断します
        trit_t trytes_out[NUM_TRYTES_HASH + 1];
        size_t trits_count = 0;
        // ノードがデータを返した場合，コンソールに出力します
        if (ret == RC_OK) {
            printf("appName %s \n", node_response->app_name->data);
            printf("appVersion %s \n", node_response->app_version->data);

            // 返されたトリットをトライトへ変換します
            // トリットとトライトの詳細については，IOTA ドキュメントポータルを参照してください：https://docs.iota.org/docs/getting-started/0.1/introduction/ternary
            trits_count = flex_trits_to_trytes(trytes_out, NUM_TRYTES_HASH,
                                            node_response->latest_milestone, NUM_TRITS_HASH,
                                            NUM_TRITS_HASH);
            if (trits_count == 0) {
                printf("trit converting failed\n");
                goto done;
            }
            // この文字列を空にします：もう必要ありません
            trytes_out[NUM_TRYTES_HASH] = '\0';

            printf("latestMilestone %s \n", trytes_out);
            printf("latestMilestoneIndex %u\n", (uint32_t) node_response->latest_milestone_index);
            printf("latestSolidSubtangleMilestoneIndex %u\n", (uint32_t)
            node_response->latest_solid_subtangle_milestone_index);

            printf("milestoneStartIndex %u\n", node_response->milestone_start_index);
            printf("neighbors %d \n", node_response->neighbors);
            printf("packetsQueueSize %d \n", node_response->packets_queue_size);
            printf("tips %u \n", node_response->tips);
            printf("transactionsToRequest %u\n", node_response->transactions_to_request);
            size_t num_features = get_node_info_req_features_num(node_response);
            for (; num_features > 0; num_features--) {
              printf("%s, ", get_node_info_res_features_at(node_response, num_f$
              printf("\n");
            }

        } else {
            printf("Failed to connect to the node.");
        }

        done:

        // レスポンスオブジェクトを解放します
        get_node_info_res_free(&node_response);
        return ret;
    }

    int main(void) {
        // クライアントサービスを作成します
        iota_client_service_t iota_client_service;
        init_iota_client(&iota_client_service);
        // レスポンスオブジェクトを割り当てます
        get_node_info_res_t *node_response = get_node_info_res_new();
        // getNodeInfo エンドポイントを呼び出します
        get_iota_node_info(&iota_client_service, node_response);
    }
    ```

4. `examples` ディレクトリで，コードをビルドする `BUILD` ファイルを作成します．
  <!-- 4. In the `examples` directory, create a `BUILD` file that builds your code -->

    ```bash
    package(default_visibility = ["//visibility:public"])

    cc_binary(
        name = "hello_world",
        srcs = ["e01_hello_world.c"],
        copts = ["-DLOGGER_ENABLE"],
        linkopts = ["-pthread"],
        deps = [
            "//iota_client_service:service",
        ],
    )
    ```

5. ファイルを実行します
  <!-- 5. Execute the file -->

    ```bash
    bazel run -c opt examples:hello_world
    ```

ノードは次のようなものを返します．
<!-- The node returns something like the following: -->

```bash
appName IRI Devnet
appVersion 1.8.1
latestMilestone SFQXAJ9MXLHTVHWVKGYYOOCECPIMSZUYYCFORHBRRQUCWLV9SJHOOYNLSSHIJHUKAJAEIBSBOFBCNR999
latestMilestoneIndex 1429040
latestSolidSubtangleMilestoneIndex 1429040
milestoneStartIndex 434527
neighbors 8
packetsQueueSize 0
tips 4478
transactionsToRequest 0
features: testnet,
RemotePOW,
snapshotPruning,
loadBalancer
```

### レスポンスオブジェクトの内容
<!-- ### Reading the response object -->

レスポンスオブジェクトの `latestMilestoneIndex` フィールドが Discord から取得した `latestMilestoneIndex` フィールドと `latestSolidSubtangleMilestoneIndex` フィールドに等しい場合，ノードは同期しています．
<!-- If the `latestMilestoneIndex` field is equal to the one you got from Discord and the `latestSolidSubtangleMilestoneIndex` field, the node is synchronized. -->

そうでない場合は，別のノードに接続してみてください．[iota.dance](https://iota.dance/) には，メインネットノードの一覧が含まれています．または，[自分自身のノードを実行](root://node-software/0.1/iri/how-to-guides/quickstart.md)できます．
<!-- If not, try connecting to a different node. The [iota.dance website](https://iota.dance/) includes a list of Mainnet nodes. Or, you can [run your own node](root://node-software/0.1/iri/how-to-guides/quickstart.md). -->

`features` リストで，このノードは[リモートプルーフオブワーク](root://getting-started/0.1/transactions/proof-of-work.md)（RemotePOW）もサポートしていることがわかります．その結果，ローカルデバイスでプルーフオブワークを実行する代わりに，このノードを使用して，プルーフオブワークを行うことができます．
<!-- In the `features` list, you can see that this node also support [remote proof of work](root://getting-started/0.1/transactions/proof-of-work.md) (RemotePOW). As a result, you can use this node to do proof of work instead of doing it on your local device. -->

これらのフィールドの詳細については，[IRI API リファレンス](root://node-software/0.1/iri/references/api-reference.md#getNodeInfo)を参照してください．
<!-- For more information about these fields, see the [IRI API reference](root://node-software/0.1/iri/references/api-reference.md#getNodeInfo). -->

:::success:おめでとうございます:tada:
同期済みノードへの接続を確認しました．
:::
<!-- :::success: Congratulations :tada: -->
<!-- You've confirmed your connection to a synchronized node. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

これらのコードサンプルは [GitHub](https://github.com/JakeSCahill/java-iota-workshop) でホストされています．
<!-- These code samples are hosted on [GitHub](https://github.com/JakeSCahill/java-iota-workshop). -->

開始するには，デバイスに [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) がインストールされている必要があります．]
<!-- To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device. -->

C開発環境も必要です．
<!-- You also need a C development environment. -->

コマンドラインで，次を実行します．
<!-- In the command-line, do the following: -->

```bash
git clone https://github.com/iota-community/c-iota-workshop.git
cd c-iota-workshop
bazel run -c opt examples:hello_world
```

コンソールに，レスポンスオブジェクトが表示されます．
<!-- In the console, you should see the response object. -->

## 議論に参加する
<!-- ## Get involved -->

[Discord チャンネル](https://discord.iota.org)では，次のことができます．
<!-- [Join our Discord channel](https://discord.iota.org) where you can: -->

- IOTA 開発者およびコミュニティとの議論に参加することができます．
<!-- - Take part in discussions with IOTA developers and the community -->
- 助けを求めることができます．
<!-- - Ask for help -->
- 他の人を助けるためにあなたの知識を共有することができます．
<!-- - Share your knowledge to help others -->

次のような多くのチャネルがあります．
<!-- We have many channels, including the following: -->

- `-dev`：これらのチャンネルは読み取り専用であり，開発者が互いにトピックを議論したり，GitHub からのコードの更新を確認したりできます．
<!-- - `-dev`: These channels are read-only and are where developers discuss topics with each other and where you can see any code updates from GitHub. -->

- `-discussion`：これらのチャンネルはあなたが参加できる場所です．
<!-- - `-discussion`: These channels are where you can participate. -->

## 次のステップ
<!-- ## Next steps -->

[C ワークショップ](../how-to-guides/c/get-started.md)で学習を続ける．
<!-- Continue learning with our [C workshop](../how-to-guides/c/get-started.md). -->

[開発者ハンドブック](root://getting-started/0.1/references/quickstart-dev-handbook.md)を読んで，自分自身のノードを実行する必要があるかどうか，プライベート IOTA ネットワークが必要かどうか，および両方について考慮する必要があるかどうかについてのガイダンスを参照してください．
<!-- Read our [developer's handbook](root://getting-started/0.1/references/quickstart-dev-handbook.md) for guidance on whether you should run your own node, whether you need a private IOTA network, and what you need to consider for both. -->
