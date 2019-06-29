# リバースプロキシサーバーを設定する
<!-- # Set up a reverse proxy server -->

**クライアントは、APIリクエストをスパムすることによって、IRIノードのオープンAPIポートを悪用することができます。IPアドレスでAPIリクエストを制限したり、APIリクエストの数を制限したりするには、IRIノードをリバースプロキシサーバーに接続します。**
<!-- **Clients can abuse the open API port of an IRI node by spamming API requests to it. To restrict API requests by IP address or to limit the number of API requests, you can connect your IRI node to a reverse proxy server.** -->

多くの[リバースプロキシサーバー](https://en.wikipedia.org/wiki/Reverse_proxy) exist. Iが存在します。このガイドでは、[Nginx](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/)をIRIノードと同じLinuxサーバーにインストールします。
<!-- Many [reverse proxy servers](https://en.wikipedia.org/wiki/Reverse_proxy) exist. In this guide, you'll install [Nginx](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/) on the same Linux server as your IRI node. -->

1. Linuxサーバーで、ターミナルウィンドウを開いて、Nginxをインストールします。
  <!-- 1. On your Linux server, open a terminal window, and install Nginx -->

    ```bash
    sudo apt-get update
    sudo apt-get install nginx
    ```
    :::info:
    インストールプロセスの最後に、UbuntuはNginxを起動します。再起動のたびにNginxを自動的に起動させたくない場合は、次のコマンドを実行します。

    ```bash
    sudo systemctl disable nginx
    ```

    設定を元に戻すには、次のコマンドを実行します。

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

2. Nginxサーバーが稼働していることを確認します。
  <!-- 2. Check that the Nginx server is running -->

    ```bash
    systemctl status nginx
    ```
    標準出力に次のようなものが表示されるはずです。
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

3. Webブラウザを開き、アドレスバーにLinuxサーバのIPアドレスを入力します。
  <!-- 3. Go to a web browser and enter the IP address of your Linux server in the address bar -->

    NginxのWebページが表示されます。このページはNginxに含まれており、サーバーが稼働していることを示しています。次に、IRIノードのリバースプロキシとしてNginxを設定する必要があります。
    <!-- You'll see the Nginx webpage. This page is included with Nginx to show you that the server is running. Now, you need to configure Nginx as a reverse proxy for your IRI node. -->

4. `iri.conf`というカスタム設定ファイルを作成します。
  <!-- 4. Create a custom configuration file called iri.conf -->

    ```bash
    sudo nano /etc/nginx/sites-enabled/iri.conf
    ```

5. 以下を`iri.conf`ファイルに追加します。
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

    :::info:localhostの外からNginxに接続したいですか？
    Nginxサーバーでポート5000を開きます。
    :::
    <!-- :::info:Want to connect to Nginx from outside localhost? -->
    <!-- Open port 5000 on your Nginx server. -->
    <!-- ::: -->

6. 変更を有効にする為に、Nginxを再起動します。
  <!-- 6. Restart Nginx to allow the changes to take effect -->

    ```bash
    sudo systemctl restart nginx
    ```

7. IRIノードを起動し、Nginxポートで`getNodeInfo`APIエンドポイントを呼び出します。
  <!-- 7. Start your IRI node, and call the `getNodeInfo` API endpoint on the Nginx port -->

    ```bash
    sudo apt install curl jq
    curl -s http://localhost:5000 -X POST -H 'X-IOTA-API-Version: 1' -H 'Content-Type: application/json' -d '{"command": "getNodeInfo"}' | jq
    ```

:::success:おめでとうございます！:tada:
NginxはIRIノードへのリクエストを制御しています。Nginxがリクエストを受け取ると、そのリクエストをIRIノードに送信し、レスポンスを取得して、レスポンスをクライアントに送り返します。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- Nginx is now controlling the requests to your IRI node. -->
<!-- When Nginx receives a request, it sends the request to your IRI node, fetches the response, and sends it back to the client. -->
<!-- ::: -->

NginxがAPIリクエストのレートを制限していることをテストするには、`getNodeInfo`エンドポイントに対して20回連続してリクエストを行います。
<!-- To test that Nginx is limiting the rate of API requests, make 20 consecutive requests to the `getNodeInfo` endpoint -->

```bash
for i in {0..20}; do (curl  http://localhost:5000 -X POST -H 'X-IOTA-API-Version: 1' -H 'Content-Type: application/json' -d '{"command": "getNodeInfo"}') 2>/dev/null; done
```

同じIPアドレスからの要求が多すぎると、JSONレスポンスと503エラーが混在して返されます。
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

## 特定のIPアドレスからの要求をブロックする
<!-- ## Block requests from certain IP addresses -->

特定のIPアドレスからのリクエストがIRIノードに問題を引き起こしている場合は、特定のIPアドレスをブロックすることができます。
<!-- If requests from certain IP addresses are causing issues for your IRI node, you can block them. -->

1. `iri.conf`ファイルを開きます。
  <!-- 1. Open the `iri.conf` file -->

    ```bash
    sudo nano /etc/nginx/sites-enabled/iri.conf
    ```

2. IPアドレスを`server`ブロックディレクティブに追加します。`ipaddress`を制限したいIPアドレスに変更します。
  <!-- 2. Add the IP addresses to the `server` block directive. Change `ipaddress` to the IP address that you want to restrict. -->


    ```shell
    # Denies access from an IP address
    deny ipaddress;
    # Allows access from all other IP addresses
    allow all;
    ```

NginxがこれらのIPアドレスからのリクエストを受信したときには、リクエストをIRIノードに転送しません。
<!-- Now when Nginx receives requests from those IP addresses, it won't forward those requests to your IRI node. -->

## 負荷分散を追加する
<!-- ## Add load balancing -->

複数のIRIノードがある場合は、負荷分散を追加して、複数のIRIノード間でAPIリクエストを均等に分散させることができます。
<!-- If you have more than one IRI node, you can add load balancing to evenly distribute the API requests among them. -->

1. `iri.conf`ファイルを開きます。
  <!-- 1. Open the `iri.conf` file -->

    ```bash
    sudo nano /etc/nginx/sites-enabled/iri.conf
    ```

2. `http`ブロックディレクティブに、`upstream`ブロックディレクティブを追加して、iriのような名前を付けます。
  <!-- 2. In the `http` block directive, add an `upstream` block directive and give it a name such as iri -->

    ```shell
    upstream iri {

    }
    ```

3. `upstream`ブロックディレクティブに、IRIノードのIPアドレスごとに1つの`server`シンプルディレクティブを追加します。
  <!-- 3. In the `upstream` block directive, add one `server` simple directives for each IP address of your IRI nodes -->

    ```shell
    upstream iri {
    server 127.0.0.3:8000;
    server 127.0.0.3:8001;
    server 192.168.0.1:8000;
    server 192.168.0.1:8001;
    }
    ```

4. `server`ブロックディレクティブで、`proxy_pass`シンプルディレクティブの値を`http://iri`に変更します。`iri`をあなたの`upstream`ブロックディレクティブの名前に変更します。
  <!-- 4. In the `server` block directive, change the value of the `proxy_pass` simple directive to http://iri. Change `iri` to the name of your `upstream` block directive. -->

Nginxが複数のリクエストを受信すると、`upstream`ブロックディレクティブにリストされているIRIノード間でそれらを均等に分配します。
<!-- Now, when Nginx receives multiple requests, it evenly distributes them among your IRI nodes that are listed in the `upstream` block directive. -->

:::info:
`upstream`ディレクティブの詳細については[Nginxのドキュメント](http://nginx.org/en/docs/http/ngx_http_upstream_module.html#upstream)を参照してください。
:::
<!-- :::info: -->
<!-- See the Nginx documentation to [learn more about the `upstream` directive](http://nginx.org/en/docs/http/ngx_http_upstream_module.html#upstream). -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

[HTTPSプロキシサーバーを設定](https://nginx.org/en/docs/http/configuring_https_servers.html)したい場合は、Nginxにサーバー証明書と秘密鍵ファイルの場所を指定する必要があります。
<!-- If you want to [configure an HTTPS proxy server](https://nginx.org/en/docs/http/configuring_https_servers.html), you need to give Nginx the location of your server certificate and private key files. -->
