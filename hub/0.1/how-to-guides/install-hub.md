# ハブをインストールする
<!-- # Install Hub -->

**ハブを使用すると、gRPCフレームワークをサポートする任意のプログラミング言語を使用して、新しいユーザーを作成し、そのシードを管理し、IOTAトークンの預け入れと取り出しを行うことができます。**
<!-- **Hub allows you to create new users, manage their seeds, and action deposits and withdrawals by using any programming language that supports the gRPC framework.** -->

![IOTA Hub architecture](../iota_hub.png)

ハブを使い始めるには、以下のタスクを順番に実行してください。
<!-- To get started with Hub, complete the following tasks in order. -->

## 前提条件
<!-- ## Prerequisites -->

Linux [Ubuntu 18.04 LTS](https://www.ubuntu.com/download/server)サーバー。WindowsまたはMacオペレーティングシステムを使用している場合は、[仮想マシンにLinuxサーバーを作成します](root://general/0.1/how-to-guides/set-up-virtual-machine.md)。
<!-- A Linux [Ubuntu 18.04 LTS](https://www.ubuntu.com/download/server) server. If you are on a Windows or Mac operating system, you can [create a Linux server in a virtual machine](root://general/0.1/how-to-guides/set-up-virtual-machine.md). -->

## 依存関係をインストールする
<!-- ## Install the dependencies -->

ハブをコンパイルするには、依存関係をインストールする必要があります。
<!-- To compile Hub, you need to install the dependencies. -->

1. ローカルの`apt`リポジトリが最新であることを確認します。
  <!-- 1. Make sure that your local `apt` repository is up to date -->

    ```bash
    sudo apt update \
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
    chmod +x bazel-0.18.0-installer-linux-x86_64.sh
    ```

6. Bazelをインストールします。
  <!-- 6. Install Bazel -->

    ```bash
    ./bazel-0.18.0-installer-linux-x86_64.sh --user
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

## データベースサーバをインストールする
<!-- ## Install the database server -->

ハブには、ユーザーID、アドレス、残高などのデータを格納するためのデータベースが必要です。
<!-- Hub needs a database, in which to store data such as user IDs, addresses, and balances. -->

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

4. MariaDBサーバーをインストールします。
  <!-- 4. Install the MariaDB server -->

    ```bash
    sudo apt install mariadb-server
    ```

    インストール中に、MariaDBのrootパスワードを入力するように求められます。安全なパスワードを入力してください。後で必要になります。
    <!-- During the installation, you'll be prompted to enter a root password for MariaDB. Enter a secure password and remember it. You will need it later on. -->

    ![MariaDB password prompt](../mariapassword.png "Choose your password")

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

## ハブをビルドする
<!-- ## Build Hub -->

これらすべての依存関係を設定したら、Hubをインストールします。
<!-- After setting up all these dependencies it's time to install Hub. -->

1. GitHubリポジトリをクローンします。
  <!-- 1. Clone the GitHub repository -->

    ```bash
    git clone https://github.com/iotaledger/rpchub.git
    ```

2. `rpchub`ディレクトリに移動します。
  <!-- 2. Change into the `rpchub` directory -->

    ```bash
    cd rpchub
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
<!--  -->
<!-- ```shell -->
<!-- Target //hub:hub up-to-date: -->
<!-- 	bazel-bin/hub/hub -->
<!-- INFO: Elapsed time: 1531.342s, Critical Path: 208.27s -->
<!-- INFO: 1377 processes: 1377 linux-sandbox. -->
<!-- INFO: Build completed successfully, 1811 total actions -->
<!-- ``` -->
<!-- ::: -->

## データベースを作成する
<!-- ## Create the database -->

ハブをインストールしたら、ハブのデータを格納するデータベーステーブルを作成する必要があります。
<!-- After Hub is installed, you need to create the database tables that store Hub's data. -->

:::info:
これらのコマンドで、`myrootpassword`プレースホルダーをMariaDBのインストール時に選択したrootパスワードに置き換えてください。
:::
<!-- :::info: -->
<!-- In these commands, make sure to replace the `myrootpassword` placeholder with the root password you chose when you installed MariaDB. -->
<!-- ::: -->

1. hubという名前のデータベースを作成します。
  <!-- 1. Create a database called hub -->

    ```bash
    echo "CREATE DATABASE hub" | mysql -u root -p myrootpassword
    ```

2. ハブのソースコードからデータベースにデータベーススキーマをロードします。
  <!-- 2. Load the database schema from the Hub source code into the database -->

    ```bash
    mysql -h 127.0.0.1 -u root -p myrootpassword hub < schema/schema.sql
    ```

3. データベーストリガをインポートします。
  <!-- 3. Import the database triggers -->

    ```bash
    mysql -h 127.0.0.1 -u root -p myrootpassword hub < schema/triggers.mariadb.sql
    ```

## ハブを実行する
<!-- ## Run Hub -->

ハブを実行するには、ビルドプロセス中に作成されたバイナリファイルを実行する必要があります。このバイナリファイルは`./bazel-bin/hub/hub`ディレクトリにあります。
<!-- To run Hub, you need to execute the binary file that was created during the build process. This binary file is located in the `./bazel-bin/hub/hub` directory. -->

バイナリファイルを実行する前に、バイナリファイルを設定する必要があります。
<!-- Before you can run the binary file, you need to configure it. -->

1. `start.sh`というシェルスクリプトファイルを作成します。
  <!-- 1. Create a shell script file called `start.sh` -->

    ```bash
    nano start.sh
    ```

2. `start.sh`ファイルに、使用する[コマンドラインフラグ](../references/command-line-flags.md)を付けてハブを実行するためのコマンドを追加します。
  <!-- 2. In the start.sh file, add the command for running hub with any [command line flags](../references/command-line-flags.md) that you want to use: -->

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

    :::warning:警告
    `salt`フラグの値を少なくとも20文字の文字列に変更してください。この値はハブによってシードを作成するために使用されるので、秘密にしてください。

    ソルトを保護するために、[署名サーバーをインストールする](../how-to-guides/install-the-signing-server.md)ことをお勧めします。
    :::
    <!-- :::warning:Warning -->
    <!-- Change the value of the `salt` flag to a string of at least 20 characters. This value is used by Hub to create seeds, so keep it secret. -->
    <!--  -->
    <!-- To secure the salt, we recommend [installing a signing server](../how-to-guides/install-the-signing-server.md). -->
    <!-- ::: -->

    :::info:
    この例では、ローカルのIRIノードがポート`14265`に接続されていると想定しています。このオプションをお勧めします。信頼できるリモートノードに接続したい場合は、`apiAddress`フィールドの値を、接続したいノードのURLまたはIPアドレスに置き換えます。

    ハブはHTTPSプロトコルを使用するノードに接続できません。[利用可能なノードの一覧](https://iota.dance/)をご覧ください。

    利用可能な[コマンドラインフラグ](../references/command-line-flags.md)を表示するには、次の操作行います。
    ```bash
    ./bazel-bin/hub/hub --help
    ```
    :::
    <!-- :::info: -->
    <!-- This example assumes that you have a local IRI node connected to port `14265`. We recommend this option. If you want to connect to a trusted remote node, replace the value of the `apiAddress` field with the URL or IP address of the node that you want to connect to. -->
    <!--  -->
    <!-- Hub can't connect to nodes that use the HTTPS protocol. [View a list of available nodes](https://iota.dance/). -->
    <!--  -->
    <!-- To view the available [command line flags](../references/command-line-flags.md), do the following: -->
    <!--  -->
    <!-- ```bash -->
    <!-- ./bazel-bin/hub/hub --help -->
    <!-- ``` -->
    <!-- ::: -->

3. スクリプトを実行する権限を自分に与えます。
  <!-- 3. Give yourself permission to execute the script -->

    ```bash
    chmod a+x start.sh
    ```

4. シェルスクリプトを実行してハブを起動します。
  <!-- 4. Run the shell script to start Hub -->

    ```bash
    ./start.sh
    ```

    :::success:おめでとうございます:tada:
    ハブが稼働中です。
    :::
    <!-- :::success:Congratulations:tada: -->
    <!-- Hub is now running! -->
    <!-- ::: -->

    :::danger:json.exception.parse_error.101
    次のエラーメッセージが表示される場合は、接続先のノードがHTTPSではなくHTTPプロトコルを使用していることを確認してください。ノードのアドレスは、[コマンドラインフラグ](../references/command-line-flags.md)の`apiAddress`フィールドで確認できます。
    `E0328 10:57:54.417129 13906 beast.cc:65] [json.exception.parse_error.101] parse error at 1: syntax error - invalid literal; last read: '<'`
    :::
    <!-- :::danger:json.exception.parse_error.101 -->
    <!-- If you see the following error message, make sure that the node you're connected to uses the HTTP protocol instead of HTTPS. You can check the address of your node in the `apiAddress` field of the [command line flags](../references/command-line-flags.md). -->
    <!-- E0328 10:57:54.417129 13906 beast.cc:65] [json.exception.parse_error.101] parse error at 1: syntax error - invalid literal; last read: '<' -->
    <!-- ::: -->

    シェルセッションでハブを実行しています。このセッションを閉じると、ハブは停止します。そのため、ハブをscreen/tmuxセッション、system-wideサービス、またはスーパーバイザープロセスで実行することを検討してください。
    <!-- You're running Hub in your shell session. If you close this session, Hub will stop. Therefore, you might want to consider running Hub in a screen/tmux session, a system-wide service, or a supervised process. -->

    このチュートリアルでは、スーパーバイザープロセスを使用して、ハブが常に実行され、再起動またはクラッシュ後に自動的に再起動するようにします。
    <!-- For this tutorial, you'll use a supervisor process to make sure that Hub always runs and automatically restarts after a reboot or a crash. -->

5. `supervisor`パッケージをインストールします（`CTRL+C`を押して現在のシェルセッションを終了します）。
  <!-- 5. Install the `supervisor` package (press `CTRL+C` to exit the current shell session): -->

    ```bash
    sudo apt install -y supervisor
    ```

6. スーパーバイザープロセス用の設定ファイルを作成します。
  <!-- 6. Create a configuration file for the supervised process -->

    ```bash
    sudo nano /etc/supervisor/conf.d/hub.conf
    ```

7. hub.confファイルに次の行を追加します。`user`フィールドの値を変更し、`command`、`directory`、`stderr_logfile`、および`stdout_logfile`の各フィールドのパスがユーザーにとって正しいことを確認します。
  <!-- 7. Add the following lines to the hub.conf file. Change the value of the `user` field, and make sure that the paths in the `command`, `directory`, `stderr_logfile`, and `stdout_logfile` fields are correct for your user. -->

    ```shell
    [program:hub]
    command=/home/dave/rpchub/start.sh
    directory=/home/dave/rpchub/
    user=dave
    autostart=true
    autorestart=true
    stderr_logfile=/home/dave/rpchub/err.log
    stdout_logfile=/home/dave/rpchub/info.log
    ```

8. hub.confファイルを保存してスーパーバイザーをリロードします。
  <!-- 8. Save the hub.conf file and reload supervisor -->

    ```bash
    sudo supervisorctl reload
    ```

    ハブはバックグラウンドで実行され、サーバーの再起動後またはクラッシュ後に自動的に再起動します。
    <!-- Hub should now be running in the background and should automatically start again after a server reboot or a crash. -->

9. スーパーバイザーの状況を確認します。
  <!-- 9. Check the supervisor status -->

    ```bash
    sudo supervisorctl status
    ```

:::success:成功
出力は次のようになります。

```shell
hub RUNNING pid 9983, uptime 0:01:22
```
:::
<!-- :::success:Success -->
<!-- The output should display something like the following: -->
<!--  -->
<!-- ```shell -->
<!-- hub RUNNING pid 9983, uptime 0:01:22 -->
<!-- ``` -->
<!-- ::: -->

## ハブをテストする
<!-- ## Test Hub -->

起動時に、ハブはあなたが対話するためのgRPCサーバーを提供します。ハブは、対話するために使用できる[gRPC呼び出しのセット](../references/api-reference.md)が限られています。
<!-- On startup, Hub provides a gRPC server for you to interact with. Hub has a [limited set of gRPC calls](../references/api-reference.md) that can be used to interact with it. -->

[gRPC](https://grpc.io/)をサポートする任意のプログラミング言語を介してハブと通信できます。このガイドでは、Pythonをいくつかの事例とともに使用します。
<!-- You can communicate with Hub through any programming language that supports [gRPC](https://grpc.io/). In this guide, you'll use Python with some prebuilt examples. -->

1. GitHubからサンプルコードをダウンロードします。
  <!-- 1. Download the sample code from GitHub -->

    ```bash
    cd ~
    git clone https://github.com/fijter/rpchub-test.git \
    cd rpchub-test
    ```

2. このサンプルコードには依存関係があります。グローバルなPython環境に依存関係をインストールしないようにするには、仮想環境を作成してください。
  <!-- 2. This example code has dependencies. To avoid installing the dependencies in your global Python environment, create a virtual environment -->

    ```bash
    sudo apt-add-repository multiverse && sudo apt update
    sudo apt install -y python3-venv
    python3 -m venv env
    ```

3. シェルセッションで仮想環境をアクティブにします。
  <!-- 3. Activate the virtual environment in a shell session -->

    ```bash
    . env/bin/activate
    ```

    :::info:
    仮想環境を終了するには、`deactivate`コマンドを使用します。
    :::
    <!-- :::info: -->
    <!-- To exit the virtual environment, use the `deactivate` command. -->
    <!-- ::: -->

4. 依存関係をインストールします。
  <!-- 4. Install the dependencies -->

    ```bash
    pip install -r requirements.txt
    ```

5. ハブで新しいユーザーアカウントを作成します。
  <!-- 5. Create a new user account in Hub -->

    ```bash
    python examples/create_user.py
    ```

    出力には次のように表示されます。
    <!-- The output should display the following: -->

    ```shell
    New user with id 'user-1' created!
    ```

6. ユーザーの新しい預け入れアドレスを作成します。
  <!-- 6. Create a new deposit address for the user -->

    ```bash
    python examples/get_address.py
    ```

    出力には、user-1の新しい預け入れアドレスが表示されます。[トリニティ](root://trinity/0.1/introduction/overview.md)で試してみるために、IOTAトークンをいくつか送ってみてください。
    <!-- The output should display a new deposit address for user-1. Feel free to send it a couple of IOTA tokens to try it out with [Trinity](root://trinity/0.1/introduction/overview.md)! -->

7. ユーザーの残高と履歴を取得します。
  <!-- 7. Get the balance and history for the user -->

    ```bash
    python examples/balance.py
    ```

ステップ6でIOTAトークンを預け入れアドレスに送信した場合、出力には次のように表示されます。
<!-- If you sent IOTA tokens to the deposit address in step 6, the output should display something like the following: -->

```shell
10i available for test 'user-1'
History:
events {
    timestamp: 1540856214000
    type: DEPOSIT
    amount: 10
}
```

[thetangle.org](https://thetangle.org/)などのタングルエクスプローラで預け入れアドレスの履歴を見ると、ハブが預け入れアドレスから別のアドレス（ユーザーが取り出しを要求するまで資金が集計されるハブ所有者のアドレス）に資金を移動したことがわかります。このプロセスはスウィープと呼ばれます。
<!-- If you look at the deposit address history in a Tangle explorer such as [thetangle.org](https://thetangle.org/), you will see that Hub moved the funds away from the deposit address and into a another address (Hub owner's address where funds are aggregated until a user requests a withdrawal). This process is called a sweep. -->

## 次のステップ
<!-- ## Next steps -->

ハブのセキュリティを向上させるには、ハブを[署名サーバー](../how-to-guides/install-the-signing-server.md)に接続します。
<!-- To improve the security of your Hub, connect it to a [signing server](../how-to-guides/install-the-signing-server.md). -->
