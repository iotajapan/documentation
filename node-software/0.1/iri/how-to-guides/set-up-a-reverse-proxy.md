# リバースプロキシサーバーを設定する
<!-- # Set up a reverse proxy server -->

**クライアントは，API リクエストをスパミングすることによって，IRI ノードのオープン API ポートを悪用することができます．IP アドレスで API リクエストを制限したり，API リクエストの数を制限したりするには，IRI ノードをリバースプロキシサーバーに接続します．このガイドでは，[Nginx](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/) を IRI ノードと同じ Linux サーバーにインストールします．**
<!-- **Clients can abuse the open API port of an IRI node by spamming API requests to it. To restrict API requests by IP address or to limit the number of API requests, you can connect your IRI node to a reverse proxy server. In this guide, you'll install [Nginx](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/) on the same Linux server as your IRI node.** -->

## リバースプロキシサーバーとして Nginx をインストールする
<!-- ## Install Nginx as a reverse proxy server -->

1. Linux サーバーで，ターミナルウィンドウを開いて，Nginx をインストールします．
  <!-- 1. On your Linux server, open a terminal window, and install Nginx -->

    ```bash
    sudo apt-get update
    sudo apt-get install nginx
    ```
    :::info:
    インストールプロセスの最後に，Ubuntu は Nginx を起動します．再起動のたびに Nginx を自動的に起動させたくない場合は，次のコマンドを実行します．

    ```bash
    sudo systemctl disable nginx
    ```

    設定を元に戻すには，次のコマンドを実行します．

    ```bash
    sudo systemctl enable nginx
    ```
    :::
    <!-- :::info: -->
    <!-- At the end of the installation process, Ubuntu starts Nginx. If you don't want Nginx to start automatically on every restart, do the following: -->
    <!--  -->
    <!-- ```bash -->
    <!-- sudo systemctl disable nginx -->
    <!-- ``` -->
    <!--  -->
    <!-- To re-enable Nginx to start, do the following: -->
    <!--  -->
    <!-- ```bash -->
    <!-- sudo systemctl enable nginx -->
    <!-- ``` -->
    <!-- ::: -->

2. Nginx サーバーが稼働していることを確認します．
  <!-- 2. Check that the Nginx server is running -->

    ```bash
    systemctl status nginx
    ```
    標準出力に次のようなものが表示されるはずです．
    <!-- You should see something like the following in the output: -->

    ```shell
    nginx.service - A high performance web server and a reverse proxy server
    Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
    Active: active (running) since Mon 2019-02-04 16:14:00 UTC; 4min 2s ago
    Main PID: 12857 (nginx)
    CGroup: /system.slice/nginx.service
            ├─12857 nginx: master process /usr/sbin/nginx -g daemon on; master_process on
            └─12858 nginx: worker process
    ```

3. Web ブラウザを開き，アドレスバーに Linux サーバーの IP アドレスを入力します．
  <!-- 3. Go to a web browser and enter the IP address of your Linux server in the address bar -->

    Nginx の Web ページが表示されます．このページは Nginx に含まれており，サーバーが稼働していることを示しています．次に，IRI ノードのリバースプロキシとして Nginx を設定する必要があります．
    <!-- You'll see the Nginx webpage. This page is included with Nginx to show you that the server is running. Now, you need to configure Nginx as a reverse proxy for your IRI node. -->

    :::info:
    仮想プライベートサーバー（VPS）を使用している場合，VPS プロバイダーの既定の Web ページのみが表示される場合があります．
    :::
    <!-- :::info: -->
    <!-- If you're using a virtual private server (VPS), you may only see the default webpage of your VPS provider. -->
    <!-- ::: -->

4. `iri.conf` というカスタム構成ファイルを作成します．
  <!-- 4. Create a custom configuration file called iri.conf -->

    ```bash
    sudo nano /etc/nginx/sites-enabled/iri.conf
    ```

5. 以下を `iri.conf` ファイルに追加します．
  <!-- 5. Add the following to the `iri.conf` file: -->

    ```shell
    # Limit the amount of requests that'll be forwarded from a single IP address to the IRI node (5 per second)
    limit_req_zone              $binary_remote_addr zone=iri:10m rate=5r/s;

    server {

        server_name _;
        # Port that Nginx will listen on
        listen                    5000 default_server deferred;

        location / {
        # Tell Nginx to drop requests to the server if more than 5 are queued from the same IP address
        limit_req               zone=iri burst=5 nodelay;

        # IP address of your IRI node. In this case the IRI node is running on the same machine as Nginx
        proxy_pass http://127.0.0.1:14265;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        }
    }
    ```

    :::info:localhost の外から Nginx に接続したいですか？
    Nginx サーバーでポート5000を開きます．
    :::
    <!-- :::info:Want to connect to Nginx from outside localhost? -->
    <!-- Open port 5000 on your Nginx server. -->
    <!-- ::: -->

6. 変更を有効にする為に，Nginx を再起動します．
  <!-- 6. Restart Nginx to allow the changes to take effect -->

    ```bash
    sudo systemctl restart nginx
    ```

7. IRI ノードを起動し，Nginx ポートで `getNodeInfo` API エンドポイントを呼び出します．
  <!-- 7. Start your IRI node, and call the `getNodeInfo` API endpoint on the Nginx port -->

    ```bash
    sudo apt install curl jq
    curl -s http://localhost:5000 -X POST -H 'X-IOTA-API-Version: 1' -H 'Content-Type: application/json' -d '{"command": "getNodeInfo"}' | jq
    ```

:::success:おめでとうございます:tada:
Nginx は IRI ノードへのリクエストを制御しています．Nginx がリクエストを受け取ると，そのリクエストを IRI ノードに送信し，レスポンスを取得して，レスポンスをクライアントに返します．
:::
<!-- :::success:Congratulations! :tada: -->
<!-- Nginx is now controlling the requests to your IRI node. -->
<!-- When Nginx receives a request, it sends the request to your IRI node, fetches the response, and sends it back to the client. -->
<!-- ::: -->

Nginx が API リクエストのレートを制限していることをテストするには，`getNodeInfo` エンドポイントに対して20回連続してリクエストを行います．
<!-- To test that Nginx is limiting the rate of API requests, make 20 consecutive requests to the `getNodeInfo` endpoint -->

```bash
for i in {0..20}; do (curl  http://localhost:5000 -X POST -H 'X-IOTA-API-Version: 1' -H 'Content-Type: application/json' -d '{"command": "getNodeInfo"}') 2>/dev/null; done
```

同じ IP アドレスからのリクエストが多すぎると，JSON レスポンスと503エラーが混在して返されます．
<!-- You should see a mixture of JSON responses and 503 errors, which are returned when too many requests are made from the same IP address. -->

```shell
{"appName":"IRI","appVersion":"1.7.0-RELEASE","jreAvailableProcessors":8,"jreFreeMemory":1832921952,"jreVersion":"1.8.0_191","jreMaxMemory":20997734400,"jreTotalMemory":4073869600,"latestMilestone":"CUOENIPTRCNECMVOXSWKOONGZJICAPH9FIG9F9KYXF9VYXFUKTNDCCLLWRZNUHZIGLJZFWPOVCIZA9999","latestMilestoneIndex":1050373,"latestSolidSubtangleMilestone":"CUOENIPTRCNECMVOXSWKOONGZJICAPH9FIG9F9KYXF9VYXFUKTNDCCLLWRZNUHZIGLJZFWPOVCIZA9999","latestSolidSubtangleMilestoneIndex":1050373,"milestoneStartIndex":1050101,"lastSnapshottedMilestoneIndex":1050264,"neighbors":7,"packetsQueueSize":0,"time":1554971201776,"tips":7335,"transactionsToRequest":0,"features":["snapshotPruning","dnsRefresher","tipSolidification"],"coordinatorAddress":"EQSAUZXULTTYZCLNJNTXQTQHOMOFZERHTCGTXOLTVAHKSA9OGAZDEKECURBRIXIJWNPFCQIOVFVVXJVD9","duration":0}<html>
<head><title>503 Service Temporarily Unavailable</title></head>
<body bgcolor="white">
<center><h1>503 Service Temporarily Unavailable</h1></center>
<center>nginx/1.14.0 (Ubuntu)</center>
</body>
</html>
```

## 特定の IP アドレスからのリクエストをブロックする
<!-- ## Block requests from certain IP addresses -->

特定の IP アドレスからのリクエストが IRI ノードに問題を引き起こしている場合は，特定の IP アドレスをブロックすることができます．
<!-- If requests from certain IP addresses are causing issues for your IRI node, you can block them. -->

1. `iri.conf` ファイルを開きます．
  <!-- 1. Open the `iri.conf` file -->

    ```bash
    sudo nano /etc/nginx/sites-enabled/iri.conf
    ```

2. 制限したいIPアドレスを `server` ブロックディレクティブに追加します．`ipaddress` を制限したい IP アドレスに変更します．
  <!-- 2. Add the IP addresses to the `server` block directive. Change `ipaddress` to the IP address that you want to restrict. -->

    ```shell
    # Denies access from an IP address
    deny ipaddress;
    # Allows access from all other IP addresses
    allow all;
    ```

Nginx がこれらの IP アドレスからのリクエストを受信したときには，リクエストを IRI ノードに転送しません．
<!-- Now when Nginx receives requests from those IP addresses, it won't forward those requests to your IRI node. -->

## 負荷分散を追加する
<!-- ## Add load balancing -->

複数の IRI ノードがある場合は，負荷分散を追加して，複数の IRI ノード間で API リクエストを均等に分散させることができます．
<!-- If you have more than one IRI node, you can add load balancing to evenly distribute the API requests among them. -->

1. `iri.conf` ファイルを開きます．
  <!-- 1. Open the `iri.conf` file -->

    ```bash
    sudo nano /etc/nginx/sites-enabled/iri.conf
    ```

2. `http` ブロックディレクティブに，`upstream` ブロックディレクティブを追加して，`iri` のような名前を付けます．
  <!-- 2. In the `http` block directive, add an `upstream` block directive and give it a name such as iri -->

    ```shell
    upstream iri {

    }
    ```

3. `upstream` ブロックディレクティブで，[`ip_hash`](http://nginx.org/en/docs/http/load_balancing.html#nginx_load_balancing_with_ip_hash) ディレクティブを追加します．
  <!-- 4. In the `upstream` block directive, add the [`ip_hash`](http://nginx.org/en/docs/http/load_balancing.html#nginx_load_balancing_with_ip_hash) directive. -->

    ```shell
    upstream iri {
    ip_hash;
    }
    ```

4. `upstream` ブロックディレクティブで，IRI ノードの IP アドレスごとに1つの `server` シンプルディレクティブを追加します．
  <!-- 4. In the `upstream` block directive, add one `server` simple directive for each IP address of your IRI nodes -->

    ```shell
    upstream iri {
    ip_hash;
    server 127.0.0.3:8000;
    server 127.0.0.3:8001;
    server 192.168.0.1:8000;
    server 192.168.0.1:8001;
    }
    ```

5. `server` ブロックディレクティブで，`proxy_pass` シンプルディレクティブの値を `http://iri` に変更します．`iri` をあなたの `upstream` ブロックディレクティブの名前に変更します．
  <!-- 5. In the `server` block directive, change the value of the `proxy_pass` simple directive to http://iri. Change `iri` to the name of your `upstream` block directive. -->

Nginx が複数のリクエストを受信すると，`upstream` ブロックディレクティブにリストされている IRI ノード間でそれらを均等に分配します．
<!-- Now, when Nginx receives multiple requests, it evenly distributes them among your IRI nodes that are listed in the `upstream` block directive. -->

:::info:
`upstream` ディレクティブの詳細については [Nginx のドキュメント](http://nginx.org/en/docs/http/ngx_http_upstream_module.html#upstream)を参照してください．
:::
<!-- :::info: -->
<!-- See the Nginx documentation to [learn more about the `upstream` directive](http://nginx.org/en/docs/http/ngx_http_upstream_module.html#upstream). -->
<!-- ::: -->

## HTTPS をサポートする
<!-- ## Add HTTPS support -->

デフォルトでは，ノードは安全でない接続である HTTP を介して通信します．トリニティウォレットからノードに接続できるようにするには，ノードで HTTPS を使用する必要があります．
<!-- By default, nodes communicate over HTTP, which is an insecure connection. To be able to connect to your node from the Trinity wallet, your node must use HTTPS. -->

1. SSL 証明書と秘密鍵ファイルを生成します．`$YOUR_EMAIL` と `$YOUR_DOMAIN` プレースホルダーをあなたのメールアドレスとドメインに置き換えます．
  <!-- 1. Generate SSL certificates and private key files. Replace the `$YOUR_EMAIL` and `$YOUR_DOMAIN` placeholders with your email address and domain. -->

    ```bash
    cd ~ && wget https://dl.eff.org/certbot-auto && \
    chmod a+x certbot-auto && \
    sudo mv certbot-auto /usr/local/bin && \
    sudo certbot-auto --noninteractive --os-packages-only && \
    sudo certbot-auto certonly \
    --standalone \
    --agree-tos \
    --non-interactive \
    --text \
    --rsa-key-size 4096 \
    --email $YOUR_EMAIL \
    --domains '$YOUR_DOMAIN'
    ```

  :::info:
  ここでは，[Let's Encrypt](https://letsencrypt.org/how-it-works/) を使用してファイルを生成します．
  :::
  <!-- :::info: -->
  <!-- Here, we generate use [Let's Encrypt](https://letsencrypt.org/how-it-works/) to generate the files. -->
  <!-- ::: -->

2. `iri.conf` ファイルを開きます．
  <!-- 2. Open the `iri.conf` file -->

    ```bash
    sudo nano /etc/nginx/sites-enabled/iri.conf
    ```

3. 以下を `server` ブロックディレクティブに追加して，Nginx にサーバー証明書と秘密鍵ファイルの場所を提供します．`$DOMAIN_DIRECTORY` プレースホルダーを，ファイルが保存されたディレクトリに置き換えます．
  <!-- 3. Add the following to the `server` block directive to give Nginx the location of your server certificate and private key files. Replace the `$DOMAIN_DIRECTORY` placeholder with the directory in which your files were saved. -->

    ```bash
    listen                    443 ssl http2 deferred;

    ssl_certificate           /etc/letsencrypt/live/$DOMAIN_DIRECTORY/fullchain.pem;
    ssl_certificate_key       /etc/letsencrypt/live/$DOMAIN_DIRECTORY/privkey.pem;
    ssl_protocols             TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers               HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;
    ```

4. 証明書を自動的に更新するスクリプトをインストールします．
  <!-- 4. Install a script that automatically renews your certificate -->

    ```bash
    echo "0 0,12 * * * root python -c 'import random; import time; time.sleep(random.random() * 3600)' && /usr/local/bin/certbot-auto renew && /bin/systemctl reload openresty" | sudo tee /etc/cron.d/cert_renew > /dev/null
    ```

    :::info:
    このスクリプトをインストールした後は，Let's Encrypt からの有効期限通知メールを無視できます．
    :::
    <!-- :::info: -->
    <!-- After installing this script, you can ignore any expiration notification emails from Let's Encrypt. -->
    <!-- ::: -->

5. 新しい設定を Nginx にロードします．
  <!-- 5. Load your new configuration into Nginx -->

    ```bash
    sudo systemctl daemon-reload
    sudo systemctl start nginx
    ```

6. サーバーの443ポートを開きます．
<!-- 6. Open port 443 on your server -->

7. HTTPS を使用して，ノードの API にリクエストを送信します．
<!-- 7. Send a request to your node's API, using HTTPS -->

:::success:おめでとうございます:tada:
HTTPS を介してノードと通信できるようになりました．
:::
<!-- :::success: Congratulations :tada: -->
<!-- You can now communicate with your node over HTTPS. -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

[API 入門](../how-to-guides/get-started-with-the-api.md)．
<!-- [Get started with the API](../how-to-guides/get-started-with-the-api.md). -->
