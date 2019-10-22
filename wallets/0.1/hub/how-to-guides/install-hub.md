# ハブを実行する
<!-- # Run Hub -->

**Hubは、新しいユーザーを作成し、IOTAトークンの預け入れと取り出しを管理するために使用できるウォレット管理システムです。ハブを独自のアプリケーションまたは取引所に統合するために使用できるgRPC APIまたはRESTful APIを公開するオプションがあります。**
<!-- **Hub is a wallet management system that you can use to create new users, and manage their deposits and withdrawals of IOTA tokens. You have the option to expose either a gRPC API or a RESTful API that you can use to integrate Hub into your own applications or exchanges.** -->

![IOTA Hub architecture](../images/iota_hub.png)

## 前提条件
<!-- ## Prerequisites -->

Linux [Ubuntu 18.04 LTS](https://www.ubuntu.com/download/server)サーバー。WindowsまたはMacオペレーティングシステムを使用している場合は、[仮想マシンにLinuxサーバーを作成します](root://general/0.1/how-to-guides/set-up-virtual-machine.md)。
<!-- A Linux [Ubuntu 18.04 LTS](https://www.ubuntu.com/download/server) server. If you are on a Windows or Mac operating system, you can [create a Linux server in a virtual machine](root://general/0.1/how-to-guides/set-up-virtual-machine.md). -->

## 手順1. 依存関係をインストールする
<!-- ## Step 1. Install the dependencies -->

ハブをコンパイルするには、依存関係をインストールする必要があります。
<!-- To compile Hub, you need to install the dependencies. -->

1. ローカルの`apt`リポジトリが最新であることを確認します。
  <!-- 1. Make sure that your local `apt` repository is up to date -->

    ```bash
    sudo apt update
    sudo apt upgrade
    ```

2. GCC、Clang、または[@iota_toolchains](https://github.com/iotaledger/toolchains)のツールチェーンなどのコンパイラをインストールします。
  <!-- 2. Install a compiler, such as GCC, Clang, or a toolchain from [@iota_toolchains](https://github.com/iotaledger/toolchains) -->

    ```bash
    sudo apt install -y gcc-7
    ```

3. Bazelバイナリインストーラ用の依存関係をインストールします。
  <!-- 3. Install the dependencies for the Bazel binary installer -->

    ```bash
    sudo apt install -y pkg-config zip g++ zlib1g-dev unzip python
    ```

4. [最新バージョンのBazel](https://github.com/bazelbuild/bazel/releases)用のバイナリインストーラをダウンロードします。
  <!-- 4. Download the binary installer for the [latest version of Bazel](https://github.com/bazelbuild/bazel/releases) -->

    ```bash
    wget https://github.com/bazelbuild/bazel/releases/download/0.18.0/bazel-0.18.0-installer-linux-x86_64.sh
    ```
    ダウンロードにはしばらく時間がかかります。
    <!-- The download may take some time. -->

    すべてうまくいけば、標準出力に次のように表示されます。
    <!-- If everything went well, you should see the following in the output: -->

    ```bash
    HTTP request sent, awaiting response ... 200 OK
    ```

5. スクリプトを実行する権限を自分に与えます。
  <!-- 5. Give yourself permission to execute the script -->

    ```bash
    chmod +x bazel-0.29.1-installer-linux-x86_64.sh
    ```

6. Bazelをインストールします。
  <!-- 6. Install Bazel -->

    ```bash
    ./bazel-0.29.1-installer-linux-x86_64.sh --user
    ```

    `--user`フラグは、Bazelをシステムの`$HOME/bin`ディレクトリにインストールします。
    <!-- The `--user` flag installs Bazel to the `$HOME/bin` directory on your system. -->

7. `$HOME/bin`ディレクトリを`$PATH`変数に追加します。
  <!-- 7. Add the `$HOME/bin` directory to your `$PATH` variable -->

    ```BASH
    PATH="$PATH:$HOME/bin"
    ```

8. Python用の`pyparsing`パッケージをインストールします。
  <!-- 8. Install the `pyparsing` package for Python -->

    ```bash
    sudo apt install -y python-pyparsing
    ```

9. Gitをインストールします。
  <!-- 9. Install Git -->

    ```bash
    sudo apt install -y git
    ```

## 手順2. データベースサーバーをインストールする
<!-- ## Step 2. Install the database server -->

ハブは、ユーザーID、アドレス、残高などのデータをデータベースに保存します。
<!-- Hub stores data such as user IDs, addresses, and balances in a database. -->

CHECK制約をサポートしているため、デフォルトではハブは[MariaDB 10.2.1](https://mariadb.com/)を使用します。CHECK制約はテーブルに追加できるデータを制限します。無効なデータを列に挿入しようとすると、MariaDBはエラーを投げます。
<!-- By default, Hub uses [MariaDB 10.2.1+](https://mariadb.com/) because it supports CHECK constraints. A CHECK constraint restricts the data you can add to the table. If you attempt to insert invalid data in a column, MariaDB throws an error. -->

Ubuntu 18.04 LTS用のデフォルトリポジトリはデータベースに使用できるパッケージを提供していません。代わりに、公式のMariaDBリポジトリ用にカスタムのPersonal Package Archive（PPA）をインストールすることができます。
<!-- The default repositories for Ubuntu 18.04 LTS don't provide a package that can be used for the database. Instead, you can install a custom Personal Package Archive (PPA) for the official MariaDB repository. -->

1. PPAをダウンロードするためにGNU Privacy Guard（GPG）キーをリクエストします。
  <!-- 1. Request a GNU Privacy Guard (GPG) key to download the PPA -->

    ```bash
    sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
    ```

2. MariaDBリポジトリを追加します。
  <!-- 2. Add the MariaDB repository -->

    ```bash
    sudo add-apt-repository 'deb [arch=amd64,arm64,ppc64el] http://ftp.utexas.edu/mariadb/repo/10.3/ubuntu bionic main'
    ```

3. ローカルの`apt`リポジトリが最新であることを確認します。
  <!-- 3. Make sure that your local `apt` repository is up to date -->

    ```bash
    sudo apt update
    ```

4. MariaDBサーバをインストールします。
  <!-- 4. Install the MariaDB server -->

    ```bash
    sudo apt install mariadb-server
    ```

    インストール中に、MariaDBのrootパスワードを入力するように求められます。安全なパスワードを入力してください。後で必要になります。
    <!-- During the installation, you'll be prompted to enter a root password for MariaDB. Enter a secure password and remember it. You will need it later on. -->

    ![MariaDB password prompt](../images/mariapassword.png)

5. MySQLがインストールされていることを確認します。
  <!-- 5. Make sure that MySQL is installed -->

    ```bash
    mysql --version
    ```

出力は次のようになります。
<!-- The output should display something like the following: -->

```shell
mysql  Ver 15.1 Distrib 10.3.10-MariaDB, for debian-linux-gnu (x86_64) using readline 5.2
```

これで、MariaDB 10.3.10がインストールされていることがわかります。これは、最小の10.2.1より新しいバージョンです。
<!-- Here, you can see that MariaDB 10.3.10 is installed, which is a later version than the minimum of 10.2.1. -->

## 手順3. ハブをビルドする
<!-- ## Step 3. Build Hub -->

これらすべての依存関係を設定したら、Hubをインストールします。
<!-- After setting up all these dependencies it's time to install Hub. -->

1. GitHubリポジトリをクローンします。
  <!-- 1. Clone the GitHub repository -->

    ```bash
    git clone https://github.com/iotaledger/hub.git
    ```

2. `hub`ディレクトリに移動します。
  <!-- 2. Change into the `hub` directory -->

    ```bash
    cd hub
    ```

3. ソースコードからHubをビルドします。
  <!-- 3. Build Hub from the source code: -->

    ```bash
    bazel build -c opt //hub:hub
    ```

ハードウェアまたは仮想マシンによっては、このプロセスにはしばらく時間がかかります。
<!-- This process can take a while, depending on the hardware or virtual machine. -->

:::success:成功
ビルドが完了すると、標準出力に次のように表示されます。
:::

```shell
Target //hub:hub up-to-date:
    bazel-bin/hub/hub
INFO: Elapsed time: 1531.342s, Critical Path: 208.27s
INFO: 1377 processes: 1377 linux-sandbox.
INFO: Build completed successfully, 1811 total actions
```
<!-- :::success:Success -->
<!-- After the build is complete, the output should display something like the following: -->

<!-- ```shell -->
<!-- Target //hub:hub up-to-date: -->
<!--    bazel-bin/hub/hub -->
<!-- INFO: Elapsed time: 1531.342s, Critical Path: 208.27s -->
<!-- INFO: 1377 processes: 1377 linux-sandbox. -->
<!-- INFO: Build completed successfully, 1811 total actions -->
<!-- ``` -->
<!-- ::: -->

## 手順4. データベースを作成する
<!-- ## Step 4. Create the database -->

ハブをインストールしたら、ハブのデータを格納するデータベーステーブルを作成する必要があります。
<!-- After Hub is installed, you need to create the database tables that store Hub's data. -->

:::info:
これらのコマンドで、`myrootpassword`プレースホルダーをMariaDBのインストール時に選択したrootパスワードに置き換えてください。
:::
<!-- :::info: -->
<!-- In these commands, make sure to replace the `myrootpassword` placeholder with the root password you chose when you installed MariaDB. -->
<!-- ::: -->

1. `hub`と言う名のデータベースを作成します。
  <!-- 1. Create a database called hub -->

    ```bash
    echo "CREATE DATABASE hub" | mysql -uroot -pmyrootpassword
    ```

2. ハブのソースコードからデータベースにデータベーススキーマをロードします。
  <!-- 2. Load the database schema from the Hub source code into the database -->

    ```bash
    mysql -h127.0.0.1 -uroot -pmyrootpassword hub < schema/schema.sql
    ```

3. データベーストリガをインポートします。
  <!-- 3. Import the database triggers -->

    ```bash
    mysql -h127.0.0.1 -uroot -pmyrootpassword hub < schema/triggers.mariadb.sql
    ```

## 手順5. ハブを実行する
<!-- ## Step 5. Run Hub -->

ハブを実行するには、ビルドプロセス中に作成されたバイナリファイルを実行する必要があります。このバイナリファイルは`./bazel-bin/hub/hub`ディレクトリにあります。
<!-- To run Hub, you need to execute the binary file that was created during the build process. This binary file is located in the `./bazel-bin/hub/hub` directory. -->

バイナリファイルを実行する前に、バイナリファイルを設定する必要があります。
<!-- Before you can run the binary file, you need to configure it. -->

1\. `start.sh`というシェルスクリプトファイルを作成します。
  <!-- 1\. Create a shell script file called `start.sh` -->

```bash
nano start.sh
```

2\. `start.sh`ファイルに、使用する[コマンドラインフラグ](../references/command-line-flags.md)を付けてハブを実行するためのコマンドを追加します。
  <!-- 2\. In the start.sh file, add the command for running hub with the [command line flags](../references/command-line-flags.md) that you want to use -->

### ベストプラクティス
<!-- ### Best practice -->

ハブを制御するローカルMainnetノードに接続することをお勧めします。ローカルMainnetノードがない場合は、[IRIノードソフトウェアについて](root://node-software/0.1/iri/introduction/overview.md)を参照してセットアップします。
<!-- We recommend connecting Hub to a local Mainnet node that you control. If you don't have a local Mainnet node, [read about the IRI node software](root://node-software/0.1/iri/introduction/overview.md) for guides on setting one up. -->

--------------------
### gRPC API

このコマンドは、ポート14265でローカルMainnetノードに接続し、localhostのポート50051でgRPC APIサーバーを公開します。

```shell
#!/bin/bash

./bazel-bin/hub/hub \
	--salt CHANGETHIS \
	--db hub \
	--dbUser root \
	--dbPassword myrootpassword \
	--apiAddress 127.0.0.1:14265 \
	--minWeightMagnitude 14 \
	--listenAddress 127.0.0.1:50051
```
---

### RESTful API

このコマンドは、ポート14265でローカルMainnetノードに接続し、ローカルホストのポート50051でRESTful APIサーバーを公開します。

```shell
#!/bin/bash

./bazel-bin/hub/hub \
	--salt CHANGETHIS \
	--db hub \
	--dbUser root \
	--dbPassword myrootpassword \
	--apiAddress 127.0.0.1:14265 \
	--minWeightMagnitude 14 \
	--listenAddress 127.0.0.1:50051
	--serverType http
```
---

### HTTPSのDevnetノード

テストの目的で、リモートの[Devnet](root://getting-started/0.1/references/iota-networks.md#devnet)ノードに接続できます。ほとんどのリモートノードはHTTPS接続を使用するため、このコマンドには[`--useHttpsIRI`フラグ](../references/command-line-flags.md#useHttpsIRI)が`true`に設定されています。

```shell
#!/bin/bash

./bazel-bin/hub/hub \
	--salt CHANGETHIS \
	--db hub \
	--dbUser root \
	--dbPassword myrootpassword \
	--apiAddress nodes.devnet.iota.org:443 \
	--minWeightMagnitude 9 \
	--listenAddress 127.0.0.1:50051 \
	--useHttpsIRI true
```
--------------------

:::warning:警告！
`salt`フラグの値を少なくとも20文字の文字列で置き換えます。この値は、ハブがシードを作成するために使用するため、秘密にしてください。

`salt`を保護するには、[署名サーバーのインストール](../how-to-guides/install-the-signing-server.md)をお勧めします。
:::
<!-- :::warning:Warning -->
<!-- Replace the value of the `salt` flag with a string of at least 20 characters. This value is used by Hub to create seeds, so keep it secret. -->

<!-- To secure the salt, we recommend [installing a signing server](../how-to-guides/install-the-signing-server.md). -->
<!-- ::: -->

:::info:
ハブで使用可能な[コマンドラインフラグ](../references/command-line-flags.md)を表示するには、次の操作も実行できます。

```bash
./bazel-bin/hub/hub --help
```
:::
<!-- :::info: -->
<!-- To view the available [command line flags](../references/command-line-flags.md) in Hub, you can also do the following: -->

<!-- ```bash -->
<!-- ./bazel-bin/hub/hub --help -->
<!-- ./bazel-bin/hub/hub --help -->
<!-- ``` -->
<!-- ::: -->

3\. スクリプトを実行する許可を自分に与えます。
<!-- 3\. Give yourself permission to execute the script -->

```bash
chmod a+x start.sh
```

4\. シェルスクリプトを実行してハブを起動します。
<!-- 4\. Run the shell script to start Hub -->

```bash
./start.sh
```

:::success:おめでとうございます:tada:
ハブが稼働中です。
:::
<!-- :::success:Congratulations -->
<!-- :tada: Hub is now running! -->
<!-- ::: -->

シェルセッションでハブを実行しています。このセッションを閉じると、ハブは停止します。そのため、ハブをscreen/tmuxセッション、system-wideサービス、またはスーパーバイザープロセスで実行することを検討してください。
<!-- You're running Hub in your shell session. If you close this session, Hub will stop. Therefore, you might want to consider running Hub in a screen/tmux session, a system-wide service, or a supervised process. -->

このチュートリアルでは、スーパーバイザープロセスを使用して、ハブが常に実行され、再起動またはクラッシュ後に自動的に再起動するようにします。
<!-- For this tutorial, you'll use a supervisor process to make sure that Hub always runs and automatically restarts after a reboot or a crash.  -->

5\. `supervisor`パッケージをインストールします（`CTRL+C`を押して現在のシェルセッションを終了します）。
  <!-- 5\. Install the `supervisor` package (press `CTRL+C` to exit the current shell session): -->

```bash
sudo apt install -y supervisor
```

6\. スーパーバイザープロセス用の設定ファイルを作成します。
  <!-- 6\. Create a configuration file for the supervised process -->

```bash
sudo nano /etc/supervisor/conf.d/hub.conf
```

7\. hub.confファイルに次の行を追加します。 `user`フィールドの値をユーザー名に置き換え、`command`、`directory`、`stderr_logfile`、`stdout_logfile`フィールドのパスがユーザーに対して正しいことを確認します。
<!-- 7\. Add the following lines to the hub.conf file. Replace the value of the `user` field with your username, and make sure that the paths in the `command`, `directory`, `stderr_logfile`, and `stdout_logfile` fields are correct for your user. -->

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

8\. hub.confファイルを保存してスーパーバイザーをリロードします。
  <!-- 8\. Save the hub.conf file and reload supervisor -->

```bash
sudo supervisorctl reload
```

ハブはバックグラウンドで実行され、サーバの再起動後またはクラッシュ後に自動的に再起動します。
<!-- Hub should now be running in the background and should automatically start again after a server reboot or a crash. -->

9\. スーパーバイザーステータスを確認します。
  <!-- 9\. Check the supervisor status -->

```bash
sudo supervisorctl status
```

:::success:成功です！
出力には、次のようなものが表示されるはずです。

```shell
hub RUNNING pid 9983, uptime 0:01:22
```
:::
<!-- :::success:Success -->
<!-- The output should display something like the following: -->

<!-- ```shell -->
<!-- hub RUNNING pid 9983, uptime 0:01:22 -->
<!-- ``` -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

ハブの起動方法に応じて、gRPC APIサーバーまたはRESTful APIサーバーのいずれかを公開して、ユーザーと対話できるようにします。
<!-- Depending on how you started Hub, it exposes either a gRPC API server or a RESTful API server for you to interact with: -->

* [gRPC API入門](../how-to-guides/get-started-with-the-grpc-api.md)
<!-- * [Get started with the gRPC API](../how-to-guides/get-started-with-the-grpc-api.md) -->
* [RESTful API入門](../how-to-guides/get-started-with-the-grpc-api.md)
<!-- * [Get started with the RESTful API](../how-to-guides/get-started-with-the-grpc-api.md) -->

ハブのセキュリティを向上させるには、ハブを[署名サーバー](../how-to-guides/install-the-signing-server.md)に接続する。
<!-- To improve the security of your Hub, connect it to a [signing server](../how-to-guides/install-the-signing-server.md). -->
