# プルーフオブワーク用プロキシサーバをインストールする
<!-- # Install a proof-of-work proxy server -->

**`attachToTangle`エンドポイントはリソースを大量に消費します。結果として、`attachToTangle`エンドポイントへの多くの呼び出しは時々ノードをクラッシュさせることがあります。この問題を解決するには、専用のプロキシサーバーをインストールして、ノードのプルーフオブワーク（PoW）を行います。**
<!-- **The `attachToTangle` endpoint is resource intensive. As a result, many calls to this endpoint can sometimes cause a node to crash. To resolve this problem, you can install a dedicated proxy server to do proof of work (PoW) for your node.** -->

PoWプロキシサーバは、IOTAミドルウェアを使用する[Caddy](https://caddyserver.com/)の実装です。このミドルウェアにより、サーバはIRIノードの`attachToTangle`エンドポイントへの呼び出しを傍受してPoWを実行できます。
<!-- The PoW proxy server is an implementation of [Caddy](https://caddyserver.com/) that uses IOTA middleware. This middleware allows the server to intercept calls to an IRI node's `attachToTangle` endpoint and do the PoW. -->

:::info:
他のIRI APIエンドポイントへのすべてのリクエストはIRIノードに転送されます。
:::
<!-- :::info: -->
<!-- All requests to the other IRI API endpoints are forwarded to the IRI node. -->
<!-- ::: -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完成するには、次のものが必要です。
<!-- To complete this guide, you need the following: -->

* インターネット接続
<!-- * An Internet connection -->
* Goプログラミング言語の少なくともバージョン1.12（最新バージョンをお勧めします）
<!-- * At least version 1.12 of the Go programming language (we recommend the latest version) -->
* GCC：macOSの場合は、[Homebrew](https://brew.sh/)（`brew install gcc`）を使ってGCCをインストールすることができます。Windowsの場合は、[TDM-GCCでインストールできます](http://tdm-gcc.tdragon.net/download)。Linux（Ubuntu 18.04）の場合は、[`build-essential`パッケージ](https://linuxize.com/post/how-to-install-gcc-compiler-on-ubuntu-18-04/)からGCCをインストールできます。
<!-- * GCC: For macOS, you can install GCC using [Homebrew](https://brew.sh/) (`brew install gcc`). For Windows, you can [install TDM-GCC](http://tdm-gcc.tdragon.net/download). For Linux (Ubuntu 18.04), you can [install GCC from the `build-essential` package](https://linuxize.com/post/how-to-install-gcc-compiler-on-ubuntu-18-04/). -->
* [Git](https://git-scm.com/downloads)

## プロキシサーバをダウンロード、ビルド、実行する
<!-- ## Download, build, and run the proxy server -->

1. コマンドプロンプトで、`GOPATH`環境変数を確認します。
  <!-- 1. In the command prompt, check your `GOPATH` environment variable -->

    ```bash
    go env GOPATH
    ````

2. `GOPATH`環境変数内のディレクトリではないディレクトリに、`iotacaddy`のGitHubリポジトリをクローンします。
  <!-- 2. In any directory outside of the one in your `GOPATH` environment variable, clone the `iotacaddy` GitHub repository -->

    ```bash
    git clone https://github.com/luca-moser/iotacaddy.git
    ```

3. `iotacaddy/caddy`ディレクトリに移動します。
  <!-- 3. Change into the `iotacaddy/caddy` directory -->

    ```bash
    cd iotacaddy/caddy
    ```

4. AVXまたはSSE実装を使用して実行可能ファイルをビルドします。
  <!-- 4. Build the executable file with either the AVX or SSE implementation -->

    ```bash
    # AVX
    go build -tags="pow_avx"
    #SSE
    go build -tags="pow_sse"
    ```

    :::info:
    最近のほとんどのCPUはAVXをサポートしていますが、どちらの実装をサポートしているかを確認する必要があります。
    :::
    <!-- :::info: -->
    <!-- Most modern CPUs support AVX, but you should check which implementation your one supports. -->
    <!-- ::: -->

    このアクションは `iotacaddy/caddy`ディレクトリに`caddy`という実行ファイルを作成します。
    <!-- This action will create a executable file called `caddy` in the `iotacaddy/caddy` directory. -->

5. `caddy`実行可能ファイルと同じディレクトリに`Caddyfile`というファイルを作成します。
  <!-- 5. Create a file called `Caddyfile` in the same directory as your `caddy` executable file -->

6. [`Caddyfile`ファイル](https://caddyserver.com/tutorial/caddyfile)を編集して、PoWプロキシサーバを設定します。この例では、ローカルホストでPoWプロキシサーバを実行しています。外部ネットワークからPoWプロキシサーバにアクセスしたい場合は、URLをパブリックIPアドレスに変更し、ポートが開いていることを確認します。
  <!-- 6. Configure your PoW proxy server by [editing the `Caddyfile` file](https://caddyserver.com/tutorial/caddyfile). In this example, we run the PoW proxy server on localhost. If you want to access your PoW proxy server from an external network, change the URL to your public IP address and make sure that the port is open. -->

    ```bash
    # Set the URL of your PoW proxy server
    127.0.0.1:15265 {

    gzip

    # Log requests to the proxy with rotation
    log requests.log {
            rotate_size 100
            rotate_age  90
            rotate_keep 20
            rotate_compress
            }

    #tls /etc/letsencrypt/live/iota-tangle.io/fullchain.pem /etc/letsencrypt/live/iota-tangle.io/privkey.pem

    # Limit request body to 10 megabytes
    limits 10mb

    # Intercept calls that have a maximum MWM of 14 and include a maximum of 20 transactions per call
    iota 14 20

    # Set up a reverse proxy to your IRI node
    # In this example, we connect to a public Devnet node, but you can also connect to your own node
    proxy / https://nodes.devnet.iota.org:443 {
            header_upstream X-IOTA-API-VERSION 1.4
            header_upstream Access-Control-Allow-Origin *
            }
    }
    ```

    :::info:
    クライアントとPoWプロキシサーバ間の接続を安全にしたい場合は、`tls`ディレクティブの前のハッシュ記号（＃）を削除し、あなたのSSL証明書を指すようにパスを変更します。
    :::
    <!-- :::info: -->
    <!-- If you want to the connection between the client and the PoW proxy server to be secure, remove the hash symbol (#) before the `tls` directive and change the paths to point to your SSL certificates. -->
    <!-- ::: -->

7. このファイルをLinuxまたはmacOSで実行するには、`.\caddy`を実行します。Windows上でこのファイルを実行するには、ダブルクリックするか、コマンドプロンプトで`.\caddy`を実行します。
  <!-- 7. To execute this file on Linux or macOS, do `./caddy`. To execute this file on Windows, double click it, or do `.\caddy` in the command prompt. -->

    PoWプロキシサーバが起動すると、次のように表示されます。
    <!-- When the PoW proxy server starts, you should see something like the following: -->

    ```bash
    Activating privacy features... done.
    [iota interceptor] 2019/06/03 12:56:54 iota API call interception configured with max bundle txs limit of 20 and max MWM of 14
    [iota interceptor] 2019/06/03 12:56:54 using PoW implementation: SyncAVX
    Serving HTTPS on port 15265
    http://127.0.0.1:15265
    ```

    :::warning:
    コマンドプロンプトを閉じると、PoWプロキシサーバはシャットダウンします。常に実行されるようにするには、サービスとして実行します。
    :::
    <!-- :::warning: -->
    <!-- If you close the command prompt, the PoW proxy will shut it down. To make sure that it always runs, you can run it as a service. -->
    <!-- ::: -->

8. ローカルのPoWプロキシサーバにトランザクションを送信します。
  <!-- 8. Send a transaction to your local PoW proxy server -->

    :::info:
    初めてトランザクションを送信する場合は、[Node.jsの入門ガイド](root://getting-started/0.1/tutorials/send-a-zero-value-transaction-with-nodejs.md)を参照します。
    :::
    <!-- :::info: -->
    <!-- If this is your first time sending a transaction, follow our [getting started guide with Node.js](root://getting-started/0.1/tutorials/send-a-zero-value-transaction-with-nodejs.md). -->
    <!-- ::: -->

    ```js
    // Require the IOTA libraries
    const Iota = require('@iota/core');
    const Converter = require('@iota/converter');

    // Create a new instance of the IOTA object
    // Use the `provider` field to specify which IRI node to connect to
    const iota = Iota.composeAPI({
    provider: 'http://127.0.0.1:15265'
    });

    const address = 'HELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDD';

    const seed = 'PUEOTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX';

    const message = Converter.asciiToTrytes('Hello World!');
    const transfers = [
    {
    value: 0,
    address: address,
    message: message
    }
    ];

    iota.prepareTransfers(seed, transfers)
    .then(trytes => {
    return iota.sendTrytes(trytes, 3/*depth*/, 9/*MWM*/)
    })
    .then(bundle => {
    var JSONBundle = JSON.stringify(bundle,null,1);
    console.log(`Bundle: ${JSONBundle}`)
    })
    .catch(error => {
    // Catch any errors
    console.log(error);
    });
    ```

    PoWプロキシサーバが`attachToTangle`エンドポイントを処理してそれをコンソールにログ出力することがわかります：
    <!-- You should see that the PoW proxy server handles the `attachToTangle` endpoint and logs it to the console: -->

    ```
    [iota interceptor] 2019/07/17 11:16:15 new attachToTangle request from 127.0.0.1:63167
    [iota interceptor] 2019/07/17 11:16:15 bundle: SBEKOAJCN9NZOECDIIYYAUBZBTGPEIFLUIGTDU9EGDEVS9TGPTGQLFJAFXZBIHLRWLTAZLALRXOFOPTXB
    [iota interceptor] 2019/07/17 11:16:15 doing PoW for bundle with 1 txs...
    [iota interceptor] 2019/07/17 11:16:15 took 17ms to do PoW for bundle with 1 txs
    ```

    :::info:
    Caddyはすべてのリクエストのログを`requests.log`ファイルに保存します。
    :::
    <!-- :::info: -->
    <!-- Caddy saves a log of all requests in the `requests.log` file. -->
    <!-- ::: -->

:::success:おめでとうございます:tada:
IRIノードのプルーフオブワークを処理する専用のプロキシサーバを設置しました。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You have a dedicated proxy server that handles proof of work for your IRI node. -->
<!-- ::: -->
