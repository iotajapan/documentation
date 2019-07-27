# シングルボードコンピュータにGoをインストールする
<!-- # Install Go on a single-board computer -->

**Goプログラミング言語を使用するアプリケーションを作成または実行するには、Goプログラミング言語をインストールする必要があります。**
<!-- **To create or run applications that use the Go programming language, you need to install it.** -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完成するには、次のものが必要です。
<!-- To complete this guide, you must have the following -->

- ARMシステムアーキテクチャ（32ビットまたは64ビット）を使用するシングルボードコンピュータ
<!-- - A single board computer that uses an ARM system architecture (32-bit or 64-bit) -->
- Linuxベースのオペレーティングシステム
<!-- - Linux-based operating system -->

1. システムアーキテクチャを確認します。
  <!-- 1. Check your system architecture -->

    ```bash
    uname -i
    ```

2. システムアーキテクチャ用の[Goソースとバイナリファイル](https://golang.org/dl/)をダウンロードします。
  <!-- 2. Download the [Go source and binary files](https://golang.org/dl/) for your system architecture -->

3. Goのソースファイルとバイナリファイルを移動させます。
  <!-- 3. Move the Go source and binary files -->

    ```bash
    sudo mv go /usr/local/
    ```

4. `go`ディレクトリを作成します。`$USERNAME`プレースホルダーを現在のLinuxユーザー名に置き換えます。
  <!-- 4. Create the `go` directories. Replace the `$USERNAME` placeholder with your current Linux username. -->

    ```bash
    sudo mkdir -p ~/$USERNAME/go/src
    sudo mkdir ~/$USERNAME/go/bin
    sudo mkdir ~/$USERNAME/go/pkg
    ```

5. 環境変数ファイルを開きます。
  <!-- 5. Open your environment variables file -->

    ```bash
    sudo nano /etc/environment
    ```

6. Goインストールディレクトリを環境変数ファイルに追加します。 `$USERNAME`プレースホルダーを現在のLinuxユーザー名に置き換えます。
  <!-- 6. Add the Go install directory to your environment variables file. Replace the `$USERNAME` placeholder with your current Linux username. -->

    ```bash
    GOROOT=/usr/local/go
    GOPATH=~/[USERNAME]/go
    ```

7. Goバイナリファイルへのソフトリンクを作成します。
  <!-- 7. Create soft links to the Go binary files -->

    ```bash
    ln -s /usr/bin/go /usr/local/go/bin/go
    ln -s /usr/local/go/bin/gofmt /usr/bin/gofmt
    ```

8. Goがインストールされていることを確認します。
  <!-- 8. Check that Go is installed -->

    ```bash
    go version
    ```

    バージョン番号を確認します。確認できない場合は、Goをもう一度インストールします。
    <!-- You should see a version number. If not, try installing Go again. -->

:::success:おめでとうございます:tada:
SBCにGoをインストールしました。これで、Goプログラミング言語で書かれたアプリケーションの作成または使用を開始できます。
:::
<!-- :::success:Congratulation :tada: -->
<!-- You've installed Go on your SBC. Now, you can start creating or using applications written in the Go programming language. -->
<!-- ::: -->
