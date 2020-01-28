# 署名サーバーをインストールする
<!-- # Install the signing server -->

**ハブのセキュリティを向上させるために，バンドル署名操作と `salt`（シードの生成に使用）をハブだけが接続できる署名サーバーに移動できます．このガイドでは，SSL 暗号化接続を介してハブに接続する署名サーバーをインストールして実行します．**
<!-- **To improve the security of Hub, you can move the bundle signing operation and the salt (used to generate seeds) to a signing server that only Hub can connect to. In this guide, you'll install and run a signing server that connects to Hub over an SSL-encrypted connection.** -->

:::info:
セキュリティを最大限に高めるには，署名サーバーをリモートの場所で実行するのがベストプラクティスです．このように，ハブが侵害された場合，攻撃者は署名サーバーにアクセスせずに IOTA トークンを盗むことはできません．
:::
<!-- :::info: -->
<!-- For maximum security, it's best practice to run the signing server in a remote location. This way, if Hub is compromised, attackers can't steal IOTA tokens without access to the signing server. -->
<!-- ::: -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完了するには，次のものが必要です．
<!-- To complete this guide, you need the following: -->

- [ハブのインスタンス](../how-to-guides/install-hub.md)
<!-- - An [instance of Hub](../how-to-guides/install-hub.md) -->
- Linux [Ubuntu 18.04 LTS](https://www.ubuntu.com/download/server) サーバー．Windows または Mac オペレーティングシステムを使用している場合，[Ubuntu 18.04 LTS](https://www.ubuntu.com/download/server) の新規インストールで[仮想マシン上に Linux サーバーを作成できます](root://general/0.1/how-to-guides/set-up-virtual-machine.md)．
<!-- - A Linux [Ubuntu 18.04 LTS](https://www.ubuntu.com/download/server) server. If you are on a Windows or Mac operating system, you can [create a Linux server in a virtual machine](root://general/0.1/how-to-guides/set-up-virtual-machine.md).a new installation of [Ubuntu 18.04 LTS](https://www.ubuntu.com/download/server). -->

## 手順1. 依存関係をインストールする
<!-- ## Step 1. Install the dependencies -->

署名サーバーを構築して実行するには，コンパイラ，Python，および Git をインストールする必要があります．
<!-- To build and run the signing server, you need to install a compiler, Python, and Git. -->

1. ローカルの `apt` リポジトリが最新でマルチバースリポジトリを含んでいることを確認します．
  <!-- 1. Make sure that the local apt repository is up to date and contains the multiverse repository -->

    ```bash
    sudo apt update
    sudo apt upgrade
    ```

2. GCC，Clang，または[@iota_toolchains](https://github.com/iotaledger/toolchains) のツールチェーンなどのコンパイラをインストールします．
  <!-- 2. Install a compiler, such as GCC, Clang, or a toolchain from [@iota_toolchains](https://github.com/iotaledger/toolchains) -->

    ```bash
    sudo apt install -y gcc-7
    ```

3. Bazel バイナリインストーラ用の依存関係をインストールします．
  <!-- 3. Install the dependencies for the Bazel binary installer -->

    ```bash
    sudo apt install -y pkg-config zip g++ zlib1g-dev unzip python
    ```

4. [最新バージョンの Bazel](https://github.com/bazelbuild/bazel/releases) 用のバイナリインストーラをダウンロードします．
  <!-- 4. Download the binary installer for the [latest version of Bazel](https://github.com/bazelbuild/bazel/releases) -->

    ```bash
    wget https://github.com/bazelbuild/bazel/releases/download/0.29.0/bazel-0.29.0-installer-linux-x86_64.sh
    ```

5. インストーラスクリプトを実行できることを確認します．
  <!-- 5. Make sure that you can execute the installer script -->

    ```bash
    chmod +x bazel-0.29.1-installer-linux-x86_64.sh
    ```

6. Bazel をインストールします．
  <!-- 6. Install Bazel -->

    ```bash
    ./bazel-0.29.1-installer-linux-x86_64.sh --user
    ```

    `--user` フラグは Bazel を `$HOME/bin` ディレクトリにインストールします．
    <!-- The `--user` flag installs Bazel in the `$HOME/bin` directory. -->

7. `$HOME/bin` ディレクトリを `$PATH` 変数に追加します．
  <!-- 7. Add the `$HOME/bin` directory to your `$PATH` variable -->

    ```BASH
    PATH="$PATH:$HOME/bin"
    ```

8. Python 用の `pyparsing` パッケージをインストールします．
  <!-- 8. Install the `pyparsing` package for Python -->

    ```bash
    sudo apt install -y python-pyparsing
    ```

9. Git をインストールします．
  <!-- 9. Install Git -->

    ```bash
    sudo apt install -y git
    ```

## 手順2. 署名サーバーをビルドする
<!-- ## Step 2. Build the signing server -->

1. GitHub リポジトリをクローンします．
  <!-- 1. Clone the GitHub repository -->

    ```bash
    git clone https://github.com/iotaledger/hub.git
    ```

2. `hub` ディレクトリに移動します．
  <!-- 2. Change into the `hub` directory -->

    ```bash
    cd hub
    ```

3. ソースコードから署名サーバーをビルドします．
  <!-- 3. Build the signing server from the source code -->

    ```bash
    bazel build -c opt //signing_server
    ```

ハードウェアまたは仮想マシンによっては，このプロセスにはしばらく時間がかかります．
<!-- This process can take a while, depending on your hardware or virtual machine. -->

ビルドが完了すると，標準出力に次のように表示されます．
<!-- After the build is complete, the output should display something like the following: -->

```shell
Target //signing_server:signing_server up-to-date:
    bazel-bin/signing_server/signing_server
INFO: Elapsed time: 1250.848s, Critical Path: 19.29s
INFO: 1283 processes: 1283 linux-sandbox.
INFO: Build completed successfully, 1412 total actions
```

## 手順3. 自己署名 SSL 証明書を生成する
<!-- ## Step 3. Generate self-signed SSL certificates -->

SSL 証明書は，ハブと署名サーバー間の通信を保護するために使用されます．ハブリポジトリには，テスト目的で自己署名証明書を生成するスクリプトが含まれています．
<!-- SSL certificates are used to secure the communication between Hub and the signing server. The Hub repository includes scripts that generate self-signed certificates for testing purposes. -->

:::info:
実稼働環境で SSL 証明書を使用する場合は，専用の認証局（CA）を実行する必要があります．
:::
<!-- :::info: -->
<!-- If you want to use SSL certificates in a production environment, you should run a dedicated Certificate authority (CA). -->
<!-- ::: -->

1. `01_generate_ca.sh` ファイルを開きます．
  <!-- 1. Open the `01_generate_ca.sh` file -->

    ```bash
    nano docs/ssl/grpc/01_generate_ca.sh
    ```

2. 証明書の有効期限を9999日に増やすには，`-days 365` を `-days 9999` に置き換えます．
  <!-- 2. To increase the expiry date of the certificate to 9999 days, replace `-days 365` with `-days 9999` -->

3. `-subj` スイッチの共通名をデバイスのホスト名に置き換え，ファイルを保存します．たとえば，ホスト名が `signer` の場合，`CN=localhost` を `CN=signer` に置き換えます．
  <!-- 3. Replace the common name in the `-subj` switch with the hostname of your device and save the file. For example, if your hostname is `signer`, replace `CN=localhost` with `CN=signer`. -->

    :::info:
    ホスト名を確認するには，コマンドラインで `hostname` コマンドを入力します．
    :::
    <!-- :::info: -->
    <!-- To find out your hostname, enter the `hostname` command in the command line. -->
    <!-- ::: -->

4. `02_generate-server.sh` ファイルを開きます．
  <!-- 4. Open the `02_generate-server.sh` file -->

    ```bash
    nano docs/ssl/grpc/02_generate_server.sh
    ```

5. `-days 365` を `-days 9999` に置き換えます．
  <!-- 5. Replace `-days 365` with `-days 9999` -->

6. `-subj` スイッチの共通名をデバイスのホスト名に置き換え，ファイルを保存します．
  <!-- 6. Replace the common name in the `-subj` switch with the hostname of your device and save the file -->

7. `03_generate_client.sh` ファイルを開きます．
  <!-- 7. Open the `03_generate_client.sh` file -->

    ```bash
    nano docs/ssl/grpc/03_generate_client.sh
    ```

8. `-days 365` を `-days 9999` に置き換えます．
  <!-- 8. Replace `-days 365` with `-days 9999` -->

9. `-subj` スイッチの共通名をデバイスのホスト名に置き換え，ファイルを保存します．
<!-- 9. Replace the common name in the `-subj` switch with the hostname of your device and save the file -->

10. 3つすべてのスクリプトを実行します．
  <!-- 10. Execute all three scripts -->

    ```bash
    ./docs/ssl/grpc/01_generate_ca.sh
    ./docs/ssl/grpc/02_generate_server.sh
    ./docs/ssl/grpc/03_generate_client.sh
    ```

これで，SSL サーバーとクライアントの証明書ファイルがいくつか作成されました．
<!-- You should now have some SSL server and client certificate files. -->

## 手順4. 署名サーバーを実行する
<!-- ## Step 4. Run the signing server -->

署名サーバーを実行するには，ビルドプロセス中に作成されたバイナリファイルを実行する必要があります．
<!-- To run the signing server, you need to execute the binary file that was created during the build process. -->

1. `start.sh` というシェルスクリプトを作成します．
  <!-- 1. Create a shell script called start.sh -->

    ```bash
    nano start.sh
    ```

2. `start.sh` ファイルで，使用する[コマンドラインフラグ](../references/command-line-options.md)を使用して署名サーバーを実行するためのコマンドを追加します．パスを SSL ファイルへの絶対パスに置き換えます．
<!-- 2. In the start.sh file, add the command for running the signing server with any [command line flags](../references/command-line-options.md) that you want to use. Replace the paths with the absolute paths to your SSL files. -->

    ```bash
    #!/bin/bash

    ./bazel-bin/signing_server/signing_server \
    --salt CHANGETHISTOSOMETHINGMORESECURE \
    --authMode ssl \
    --sslKey /home/jake/hub/docs/ssl/grpc/server.key \
    --sslCert /home/jake/hub/docs/ssl/grpc/server.crt \
    --sslCA /home/jake/hub/docs/ssl/grpc/ca.crt \
    ```

    :::warning:警告！
    ハブ設定で使用したものと同じ `salt` を使用してください．
    :::
    <!-- :::warning:Warning -->
    <!-- Use the same salt as the one you used in the Hub configuration. -->
    <!-- ::: -->

3. 現在のユーザーに `start.sh` ファイルを実行する許可を与えます．
  <!-- 3. Give the current user permission to execute the `start.sh` file -->

    ```bash
    chmod a+x start.sh
    ```

4. 署名サーバーを起動します．
  <!-- 4. Start the signing server -->

    ```bash
    ./start.sh
    ```

    :::success:おめでとうございます:tada:
    現在署名サーバーは実行中です．ハブがバンドルを作成するたびに，署名サーバーに署名して署名を返すように要求します．
    :::
    <!-- :::success:Congratulations :tada: -->
    <!-- The signing server is now running. -->
    <!-- Whenever Hub creates a bundle, it will ask the signing server to sign it and to return the signature. -->
    <!-- ::: -->

    シェルセッションで署名サーバーを実行しています．このセッションを閉じると，署名サーバーは停止します．したがって，署名サーバーを screen/tmux セッション，system-wide サービス，またはスーパーバイザープロセスで実行することを検討する必要があります．
    <!-- You're running the signing server in your shell session. If you close this session, the server will stop. Therefore, you might want to consider running the signing server in a screen/tmux session, a system-wide service, or a supervised process. -->

    このガイドでは，スーパーバイザーを使用して，署名サーバーが常に実行され，再起動またはクラッシュ後に自動的に再起動するようにします．
	<!-- For this guide, you'll use supervisor to make sure the signing server always runs and automatically restarts after a reboot or a crash. -->

5. スーパーバイザーをインストールします（`CTRL+C` を押して現在のシェルセッションを終了します）．
  <!-- 5. Install supervisor (press `CTRL+C` to exit the current shell session): -->

    ```bash
    sudo apt install -y supervisor
    ```

6. スーパーバイザー用の構成ファイルを作成します．
  <!-- 6. Create a configuration file for supervisor -->

    ```bash
    sudo nano /etc/supervisor/conf.d/signing.conf
    ```

7. 次の行を `signing.conf` ファイルに追加します．`user` フィールドの値をユーザー名に置き換え，`command`，`directory`，`stderr_logfile`，`stdout_logfile` フィールドのパスが正しいことを確認します．
  <!-- 7. Add the following lines to the `signing.conf` file. Replace the value of the `user` field with your username, and make sure that the paths in the `command`, `directory`, `stderr_logfile`, and `stdout_logfile` field are correct. -->

    ```shell
    [program:hub]
    command=/home/dave/hub/start.sh
    directory=/home/dave/hub/
    user=dave
    autostart=true
    autorestart=true
    stderr_logfile=/home/dave/hub/err.log
    stdout_logfile=/home/dave/hub/info.log
    ```

8. `signing.conf` ファイルを保存してスーパーバイザーをリロードします．
  <!-- 8. Save the `signing.conf` file and reload supervisor -->

    ```bash
    sudo supervisorctl reload
    ```

    これで署名サーバーはバックグラウンドで実行され，サーバーの再起動後またはクラッシュ後に自動的に再起動するはずです．
    <!-- The signing server should now be running in the background and should automatically start again after a server reboot or a crash. -->

9. スーパーバイザーの状況を確認します．
  <!-- 9. Check the supervisor status -->

    ```bash
    sudo supervisorctl status
    ```

標準出力は次のようになります．
<!-- The output should display something like this: -->

```shell
signing RUNNING pid 11740, uptime 0:00:02
```

次に，ハブを署名サーバーに接続する必要があります．
<!-- Now, you need to connect Hub to the signing server. -->

### 手順5. ハブを署名サーバーに接続する
<!-- ### Step 5. Connect Hub to the signing server -->

ハブサーバーで，SSL 証明書をインポートし，それらを使用するように `start.sh` スクリプトを編集する必要があります．
<!-- On the Hub server, you need to import the SSL certificates and edit the `start.sh` script to use them. -->

1. 証明書ファイル（`client.crt`，`client.key`，および `ca.crt`）をハブサーバーにコピーします．これは任意の方法で実行できます．この例では，`scp` コマンドを使用して SSH で送信します．`192.168.2.212` IP アドレスを，ハブサーバーの URL または IP アドレスに置き換えます．次に，`/home/dave/hub/` ディレクトリを `hub` ディレクトリへの絶対パスに置き換えます．
  <!-- 1. Copy the certificate files ( `client.crt`, `client.key`, and `ca.crt`) to the Hub server. You can do this in any way you prefer. In this example, we send them over SSH, using the `scp` command. Replace the 192.168.2.212 IP address with the URL or IP address of your Hub server. Then, replace the `/home/dave/hub/` directory with the absolute path to your `hub` directory. -->

    ```bash
    scp client.crt client.key ca.crt 192.168.2.212:/home/dave/hub/
    ```

    出力は次のようになります．
    <!-- The output should display something like the following: -->

    ```shell
    client.crt      100% 1887     1.6MB/s   00:00
    client.key      100% 3243     3.0MB/s   00:00
    ca.crt          100% 2029     1.9MB/s   00:00
    ```

2. 新しいファイルを作成します．
  <!-- 2. Create a new file -->

    ```bash
    sudo nano /etc/hosts
    ```

3. 署名サーバーのホスト名をその IP アドレスにマップします．`192.168.2.210` IP アドレスを署名サーバーの IP アドレスに置き換えます．次に，`signer` を署名サーバーのホスト名に置き換えます．
  <!-- 3. Map the hostname of the signing server to its IP address. Replace the 192.168.2.210 IP address with the IP address of your signing server. Then, replace `signer` with the hostname of your signing server. -->

    ```shell
    192.168.2.210   signer
    ```

4. `start.sh` ファイルを開きます．
  <!-- 4. Open the  `start.sh` file -->

    ```bash
    nano start.sh
    ```

5. `--salt` パラメーターを削除します．署名サーバーが `salt` を持っているので，`--salt` パラメーターはもう必要ありません．次に，`--signingMode`，`--signingProviderAddress`，`--signingServerChainCert`，`--signingServerKeyCert`，および `--signingServerSslCert` オプションを設定します．
  <!-- 5. Remove the `--salt` parameter. This parameter is not needed here anymore because the signing server has it. Then, configure the `--signingMode`, `--signingProviderAddress`, `--signingServerChainCert`, `--signingServerKeyCert`, and `--signingServerSslCert` options. -->

    :::info:
    `salt` とは別に，既存のコマンドラインオプションをすべて保持してください．
    :::
    <!-- :::info: -->
    <!-- Apart from the salt, make sure you keep any existing command-line options. -->
    <!-- ::: -->

    ```shell
    #!/bin/bash

    ./bazel-bin/hub/hub \
    # Keep any existing command-line options
    --signingMode remote \
    --signingProviderAddress signer:50051 \
    --signingServerChainCert client.crt \
    --signingServerKeyCert client.key \
    --signingServerSslCert ca.crt
    ```

6. `start.sh` ファイルを保存してハブを再起動します．
  <!-- 6. Save the `start.sh` file and restart Hub -->

    ```bash
    sudo supervisorctl restart hub
    ```

:::success:成功です！
すべてがうまくいけば，ハブは署名サーバーに接続されます．`salt` はハブと同じサーバー上にないため，署名プロセスがより安全になります．
:::
<!-- :::success:Success -->
<!-- If everything went well, Hub will be connected to your signing server. The salt is no longer on the same server as Hub, making the signing process more secure. -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

[署名サーバーが安全であることを確認してください](https://hostadvice.com/how-to/how-to-harden-your-ubuntu-18-04-server/)．
<!-- [Make sure that your signing server is secure](https://hostadvice.com/how-to/how-to-harden-your-ubuntu-18-04-server/). -->

ハブの起動方法に応じて，gRPC API サーバーまたは RESTful API サーバーのいずれかを公開して，ユーザーと対話できるようにします．
<!-- Depending on how you started Hub, it exposes either a gRPC API server or a RESTful API server for you to interact with: -->

- [gRPC API 入門](../how-to-guides/get-started-with-the-grpc-api.md)
<!-- - [Get started with the gRPC API](../how-to-guides/get-started-with-the-grpc-api.md) -->
- [RESTful API 入門](../how-to-guides/get-started-with-the-grpc-api.md)
<!-- - [Get started with the RESTful API](../how-to-guides/get-started-with-the-grpc-api.md) -->
