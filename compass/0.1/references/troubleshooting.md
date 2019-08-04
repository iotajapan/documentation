# トラブルシューティング
<!-- # Troubleshooting -->

**コンパスでIOTAネットワークを作成している間に、これらの問題のいくつかを見つけるかもしれません。**
<!-- **You may find some of these issues while creating an IOTA network with Compass.** -->

## Got permission denied while trying to connect to the Docker daemon socket

`bazel run //docker`を含むコマンドを実行すると、このエラーが表示されることがあります。
<!-- You may see this error when you run any command that includes `bazel run //docker`. -->

1. 現在のユーザーをdockerグループに追加します。`$USER`変数をあなたのユーザ名に変更します。
  <!-- 1. Add your current user to the docker group. Change the `$USER` variable to your username. -->

    ```bash
    sudo usermod -a -G docker $USER
    ```

2. Linuxサーバを再起動してもう一度やり直します。
  <!-- 2. Restart your Linux server and try again -->

## Malformed snapshot state file

IRIを実行するとこのエラーが表示されることがあります。
<!-- You may see this error when you run the IRI. -->

snapshot.txtファイルで、改行文字を削除します。
<!-- In the snapshot.txt file, remove any line break characters. -->

## NumberFormatException

IRIを実行するとこのエラーが表示されることがあります。
<!-- You may see this error when you run the IRI. -->

snapshot.txtファイルで、セミコロンの後のスペースを削除します。
<!-- In the snapshot.txt file, remove any spaces after the semicolon. -->

## IllegalArgumentException

IRIを実行するとこのエラーが表示されることがあります。
<!-- You may see this error when you run the IRI. -->

snapshot.txtファイルで、セミコロンの前のスペースを削除します。
<!-- In the snapshot.txt file, remove any spaces before the semicolon. -->
