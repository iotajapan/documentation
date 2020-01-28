# プルーフオブワーク用プロキシサーバーをインストールする
<!-- # Install a proof-of-work proxy server -->

**`attachToTangle` エンドポイントはリソースを大量に消費します。結果として、`attachToTangle` エンドポイントへの多くの呼び出しは時々ノードをクラッシュさせることがあります。この問題を解決するには、専用のプロキシサーバーをインストールして、ノードのプルーフオブワーク（PoW）を行います。**
<!-- **The `attachToTangle` endpoint is resource intensive. As a result, many calls to this endpoint can sometimes cause a node to crash. To resolve this problem, you can install a dedicated proxy server to do proof of work (PoW) for your node.** -->

PoW プロキシサーバーは、IOTA ミドルウェアを使用する [Caddy](https://caddyserver.com/) の実装です。このミドルウェアにより、サーバーは IRI ノードの `attachToTangle` エンドポイントへの呼び出しを傍受して PoW を実行できます。
<!-- The PoW proxy server is an implementation of [Caddy](https://caddyserver.com/) that uses IOTA middleware. This middleware allows the server to intercept calls to an IRI node's `attachToTangle` endpoint and do the PoW. -->

:::info:
他の IRI API エンドポイントへのすべてのリクエストは IRI ノードに転送されます。
:::
<!-- :::info: -->
<!-- All requests to the other IRI API endpoints are forwarded to the IRI node. -->
<!-- ::: -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完成するには、次のものが必要です。
<!-- To complete this guide, you need the following: -->

- Go プログラミング言語の少なくともバージョン1.12（最新バージョンをお勧めします）
<!-- - At least version 1.12 of the Go programming language (we recommend the latest version) -->
- GCC：macOS の場合は、[Homebrew](https://brew.sh/)（`brew install gcc`）を使って GCC をインストールすることができます。Windows の場合は、[TDM-GCC でインストールできます](http://tdm-gcc.tdragon.net/download)。Linux（Ubuntu 18.04）の場合は、[`build-essential` パッケージ](https://linuxize.com/post/how-to-install-gcc-compiler-on-ubuntu-18-04/)から GCC をインストールできます。
<!-- - GCC: For macOS, you can install GCC using [Homebrew](https://brew.sh/) (`brew install gcc`). For Windows, you can [install TDM-GCC](http://tdm-gcc.tdragon.net/download). For Linux (Ubuntu 18.04), you can [install GCC from the `build-essential` package](https://linuxize.com/post/how-to-install-gcc-compiler-on-ubuntu-18-04/). -->
- [Git](https://git-scm.com/downloads)

## プロキシサーバーをダウンロード、ビルド、実行する
<!-- ## Download, build, and run the proxy server -->

1. コマンドラインインターフェイスで、`GOPATH` 環境変数を確認します。
  <!-- 1. In the command-line interface, check your `GOPATH` environment variable -->

    ```bash
    go env GOPATH
    ````

2. `GOPATH` 環境変数内のディレクトリではないディレクトリに、`iotacaddy` の GitHub リポジトリをクローンします。
  <!-- 2. In any directory outside of the one in your `GOPATH` environment variable, clone the `iotacaddy` GitHub repository -->

    ```bash
    git clone https://github.com/luca-moser/iotacaddy.git
    ```

3. `iotacaddy/caddy` ディレクトリに移動します。
  <!-- 3. Change into the `iotacaddy/caddy` directory -->

    ```bash
    cd iotacaddy/caddy
    ```

4. AVX または SSE 実装を使用して実行可能ファイルをビルドします。
  <!-- 4. Build the executable file with either the AVX or SSE implementation -->

    ```bash
    # AVX
    go build -tags="pow_avx"
    #SSE
    go build -tags="pow_sse"
    ```

    :::info:
    最近のほとんどの CPU は AVX をサポートしていますが、どちらの実装をサポートしているかを確認する必要があります。
    :::
    <!-- :::info: -->
    <!-- Most modern CPUs support AVX, but you should check which implementation your one supports. -->
    <!-- ::: -->

    このアクションは `iotacaddy/caddy` ディレクトリに `caddy` という実行ファイルを作成します。
    <!-- This action will create a executable file called `caddy` in the `iotacaddy/caddy` directory. -->

5. `caddy` 実行可能ファイルと同じディレクトリに `Caddyfile` というファイルを作成します。
  <!-- 5. Create a file called `Caddyfile` in the same directory as your `caddy` executable file -->

6. [`Caddyfile` ファイル](https://caddyserver.com/tutorial/caddyfile)を編集して、PoW プロキシサーバーを設定します。この例では、ローカルホストで PoW プロキシサーバーを実行しています。外部ネットワークから PoW プロキシサーバーにアクセスしたい場合は、URL をパブリック IP アドレスに変更し、ポートが開いていることを確認します。
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
    クライアントと PoW プロキシサーバー間の接続を安全にしたい場合は、`tls` ディレクティブの前のハッシュ記号（#）を削除し、あなたの SSL 証明書を指すようにパスを変更します。
    :::
    <!-- :::info: -->
    <!-- If you want to the connection between the client and the PoW proxy server to be secure, remove the hash symbol (#) before the `tls` directive and change the paths to point to your SSL certificates. -->
    <!-- ::: -->

7. このファイルを Linux または macOS で実行するには、`.\caddy` を実行します。Windows 上でこのファイルを実行するには、ダブルクリックするか、コマンドラインインターフェイスで `.\caddy` を実行します。
  <!-- 7. To execute this file on Linux or macOS, do `./caddy`. To execute this file on Windows, double click it, or do `.\caddy` in the command-line interface. -->

    PoW プロキシサーバーが起動すると、次のように表示されます。
    <!-- When the PoW proxy server starts, you should see something like the following: -->

    ```bash
    Activating privacy features... done.
    [iota interceptor] 2019/06/03 12:56:54 iota API call interception configured with max bundle txs limit of 20 and max MWM of 14
    [iota interceptor] 2019/06/03 12:56:54 using PoW implementation: SyncAVX
    Serving HTTPS on port 15265
    http://127.0.0.1:15265
    ```

    :::warning:
    コマンドラインインターフェイスを閉じると、PoW プロキシサーバーはシャットダウンします。常に実行されるようにするには、サービスとして実行します。
    :::
    <!-- :::warning: -->
    <!-- If you close the command-line interface, the PoW proxy will shut it down. To make sure that it always runs, you can run it as a service. -->
    <!-- ::: -->

8. ローカルの PoW プロキシサーバーにトランザクションを送信します。
  <!-- 8. Send a transaction to your local PoW proxy server -->

    ```js
    // パッケージをリクワイアします
    const Iota = require('@iota/core');
    const Converter = require('@iota/converter');

    // IOTA オブジェクトの新しいインスタンスを作成します
    // `provider` フィールドを使用して、接続する IRI ノードを指定します
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
      // エラーを処理します
      console.log(error);
    });
    ```

    PoW プロキシサーバーが `attachToTangle` エンドポイントを処理してそれをコンソールにログ出力することがわかります：
    <!-- You should see that the PoW proxy server handles the `attachToTangle` endpoint and logs it to the console: -->

    ```bash
    [iota interceptor] 2019/07/17 11:16:15 new attachToTangle request from 127.0.0.1:63167
    [iota interceptor] 2019/07/17 11:16:15 bundle: SBEKOAJCN9NZOECDIIYYAUBZBTGPEIFLUIGTDU9EGDEVS9TGPTGQLFJAFXZBIHLRWLTAZLALRXOFOPTXB
    [iota interceptor] 2019/07/17 11:16:15 doing PoW for bundle with 1 txs...
    [iota interceptor] 2019/07/17 11:16:15 took 17ms to do PoW for bundle with 1 txs
    ```

    :::info:
    Caddy はすべてのリクエストのログを `requests.log` ファイルに保存します。
    :::
    <!-- :::info: -->
    <!-- Caddy saves a log of all requests in the `requests.log` file. -->
    <!-- ::: -->

:::success:おめでとうございます:tada:
IRI ノードのプルーフオブワークを処理する専用のプロキシサーバーを設置しました。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You have a dedicated proxy server that handles proof of work for your IRI node. -->
<!-- ::: -->
