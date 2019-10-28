# トラブルシューティング
<!-- # Troubleshooting -->

**以下のガイドに従いながら、一般的な問題のいくつかを見つけることができます。**
<!-- **You may find some of these common issues while following these guides.** -->

## IPv6 over Bluetooth Low Energy境界ルーターをセットアップする
<!-- ## Set up a IPv6 over Bluetooth Low Energy border router -->

<!-- ### まだマウントされていないか、マウントポイントがビジーです -->
### None already mounted or mount point busy

このレスポンスが表示された場合、無視してください。ファイルシステムはおそらく既にマウントされています。
<!-- If you see this response, ignore it. The file system is probably already mounted. -->

```bash
mount: /sys/kernel/debug: none already mounted or mount point busy.
```

## nRF52マイクロコントローラーをセットアップする
<!-- ## Set up an nRF52 microcontroller -->

### Permission denied

マイクロコントローラーをフラッシュしようとしているときに`permission denied`エラーが表示され、DAPLinkプログラマーを使用している場合は、以下を実行してudevルールを作成します。
<!-- If you see a `permission denied` error while trying to flash your microcontroller, and you're using a DAPLink programmer, create a udev rule by doing the following: -->

1. pyOCDリポジトリをクローンします。
  <!-- 1. Clone the pyOCD repository -->

  ```bash
  git clone https://github.com/mbedmicro/pyOCD.git
  ```

2. `pyOCD/udev`ディレクトリに移動します。
  <!-- 2. Change into the `pyOCD/udev` directory -->

  ```bash
  cd pyOCD/udev
  ```

3. udevルールを作成するための[pyOCDの説明](https://github.com/mbedmicro/pyOCD/tree/master/udev)に従います。
<!-- 3. [Follow the pyOCD instructions](https://github.com/mbedmicro/pyOCD/tree/master/udev) for creating a udev rule -->

### arm-none-eabi-gcc version not supported

`arm-none-eabi-gcc version not supported`というメッセージが表示された場合は、次の手順を実行してツールチェーンの最新バージョンをインストールします。
<!-- If you see the `arm-none-eabi-gcc version not supported` message, install the latest version of the toolchain by doing the following: -->

1. 古いツールチェーンパッケージをアンインストールします。
  <!-- 1. Uninstall the old toolchain packages -->

  ```bash
  sudo apt remove binutils-arm-none-eabi gcc-arm-none-eabi libnewlib-arm-none-eabi
  ```

2. [developer.arm.comから最新のツールチェーン](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads)をダウンロードします。
<!-- 2. [Download the latest toolchain from developer.arm.com](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads) -->

3. `home`ディレクトリでツールチェーンを展開します。`$FILENAME`プレースホルダーを、ダウンロードしたファイルの名前に置き換えます。
  <!-- 3. Untar the toolchain in your `home` directory. Replace the `$FILENAME` placeholder with the name of the file that you downloaded -->

  ```bash
  tar -xjvf $FILENAME
  ```

4. ツールチェーンをパスに追加します。`$PATHTOFILE`プレースホルダーを、展開したツールチェーンへのパスに置き換えます。
  <!-- 4. Add the toolchain to your path. Replace the `$PATHTOFILE` placeholder with the path to your untarred toolchain -->

  ```bash
  echo "export PATH=$PATH:/home/$PATHTOFILE/bin/" >> ~/.bashrc
  ```

5. ターミナルウィンドウを閉じ、再び新しいターミナルウィンドウを開きます。
<!-- 5. Close the terminal window, and open a new one -->

これで、サポートされているバージョンのツールチェーンが必要です。
<!-- Now, you should have a supported version of the toolchain -->

### Recipe for target 'flash' failed

J-Link OBクローンを初めて使用すると、次のメッセージが表示される場合があります。
<!-- When you use a J-Link OB clone for the first time, you may see the following message: -->

```bash
recipe for target 'flash' failed
make: *** [flash] Error 1
```

このエラーは、J-Link OBクローンを初めて使用するときに、それ自体がアップグレードされ、フラッシュに失敗するために発生します。
<!-- This error is caused because the first time you use a J-Link OB clone, it upgrades itself and fails to flash. -->

もう一度試してフラッシュすると、成功するはずです。
<!-- Try and flash a second time and it should succeed. -->
