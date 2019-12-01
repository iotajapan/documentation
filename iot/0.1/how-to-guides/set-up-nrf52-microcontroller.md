# nRF52マイクロコントローラーをセットアップする
<!-- # Set up an nRF52 microcontroller -->

**マイクロコントローラーの使用を開始する前に、適切なハードウェア要件とソフトウェア要件でセットアップする必要があります。**
<!-- **Before you can start using your microcontroller, you need to set it up with the correct hardware and software requirements.** -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完了するには、次のものが必要です。
<!-- To complete this guide, you need the following: -->

- nRF52シリーズマイクロコントローラー
<!-- - An nRF52 series microcontroller -->
- LinuxベースのPC
<!-- - A Linux-based PC -->
- J-Link、J-Linkオンボードクローン、またはDAPLink [プログラマー](https://www.engineersgarage.com/how-to-guides/microcontroller-programmer-burner)
<!-- - A J-Link, J-Link on-board clone, or DAPLink [programmer](https://www.engineersgarage.com/how-to-guides/microcontroller-programmer-burner)  -->
- USB-UARTコネクター
<!-- - A USB-to-UART connector -->

:::info:
一部の開発ボードには、統合されたプログラマーおよび/または統合されたUSB-to-UARTコネクターがあります。これらのボードのいずれかをお持ちの場合、追加のプログラマーやUSB-to-UARTコネクターは必要ありません。

ボードにプログラマーやUSB-to-UARTコネクターが統合されているかどうかを確認するには、ボードのデータシートを参照してください。
:::
<!-- :::info: -->
<!-- Some development boards have an integrated programmer and/or an integrated USB-to-UART connector. If you have one of these boards, you don't need an additional programmer and/or USB-to-UART connector. -->

<!-- To find out if your board has an integrated programmer and/or USB-to-UART connector, see its datasheet. -->
<!-- ::: -->

## 手順1. マイクロコントローラーのハードウェアをセットアップする
<!-- ## Step 1. Set up the hardware for your microcontroller -->

1. マイクロコントローラーのピンがボードにはんだ付けされていない場合は、はんだ付けします。
  <!-- 1. If the pins on your microcontroller aren't soldered onto the board, solder them. -->

    :::info:
    こちらの[役に立つハンダ付けガイド](https://learn.pimoroni.com/tutorial/sandyj/the-ultimate-guide-to-soldering)を参照してください。
    :::
    <!-- :::info: -->
    <!-- This is a [useful soldering guide](https://learn.pimoroni.com/tutorial/sandyj/the-ultimate-guide-to-soldering). -->
    <!-- ::: -->

2. マイクロコントローラーに統合プログラマーがない場合は、[外部プログラマーを接続します](../how-to-guides/connect-programmer.md)。
<!-- 2. If your microcontroller doesn't have an integrated programmer, [connect your external one to it](../how-to-guides/connect-programmer.md) -->

3. マイクロコントローラーに統合USB-to-UARTコネクターがない場合、[外部コネクターを接続します](../how-to-guides/connect-to-serial-interface.md)。
<!-- 3. If your microcontroller doesn't have an integrated USB-to-UART connector, [connect your external one to it](../how-to-guides/connect-to-serial-interface.md) -->

## 手順2. PCでソフトウェアをセットアップする
<!-- ## Step 2. Set up the software on your PC -->

マイクロコントローラーをプログラムおよびフラッシュするには、必要なツール（ツールチェーンと呼ばれる）を備えたLinuxベースのPCが必要です。
<!-- To program and flash a microcontroller, you need a Linux-based PC that has the necessary tools (known as a toolchain). -->

1. [Gitをインストールします](https://help.github.com/en/articles/set-up-git)。
<!-- 1. [Install Git](https://help.github.com/en/articles/set-up-git) -->

2. [ARMツールチェーンをインストールします](../how-to-guides/install-arm-gcc-toolchain.md)。ARMツールチェーンにより、マイクロコントローラーが実行できるバイナリにコードをコンパイルできます。
<!-- 2. [Install the ARM toolchain](../how-to-guides/install-arm-gcc-toolchain.md), which allows you to compile code into binary that your microcontroller can run -->

3. [OpenOCDをインストールします](https://github.com/RIOT-OS/RIOT/wiki/OpenOCD)。OpenOCDには、マイクロコントローラーにバイナリを簡単にフラッシュできるスクリプトが含まれています。
<!-- 3. [Install OpenOCD](https://github.com/RIOT-OS/RIOT/wiki/OpenOCD), which includes scripts to make it easy to flash the binary onto your microcontroller -->

4. Python 3と`pip`インストーラーをインストールします。
  <!-- 4. Install Python 3 and the `pip` installer -->

    ```bash
    sudo apt-get install -y python3-all
    sudo apt-get install -y python-pip3
    ```

5. LinuxディストリビューションにPython 2がプリインストールされている場合は、デフォルトでPython 3を使用していることを確認してください。
  <!-- 5. If your Linux distribution has Python 2 pre-installed, make sure that it uses Python 3 by default -->

    ```bash
    echo "alias python=python3" >> ~/.bashrc
    ```

6. 使用しているプログラマーに応じて、以下のプログラマーツールチェーンのいずれかをインストールします。
  <!-- 6. Install one of these programmer toolchains, depending on the programmer that you're using -->

    **オプション1：** [J-LinkまたはJ-Link OBツールチェーンをインストールします](https://gnu-mcu-eclipse.github.io/debug/jlink/install/)。
   <!-- **Option 1:** [Install the J-Link or J-Link OB toolchain](https://gnu-mcu-eclipse.github.io/debug/jlink/install/) -->

    **オプション2：** [DAPLinkツールチェーンをインストールします](https://github.com/mbedmicro/pyOCD#installing)。`pip3`コマンドを使用して、このツールチェーンをインストールしてください。
   <!-- **Option 2:** [Install the DAPLink toolchain](https://github.com/mbedmicro/pyOCD#installing). Make sure that you install this toolchain, using the `pip3` command. -->

7. フォークしたRIOT OSリポジトリをクローンし、`BLE-environment-sensor/examples/hello-world`ディレクトリに移動します。
  <!-- 7. Clone our forked RIOT OS repository and change into the `BLE-environment-sensor/examples/hello-world` directory -->

    ```bash
    git clone https://github.com/iota-community/BLE-environment-sensor.git
    cd BLE-environment-sensor/examples/hello-world
    ```

    :::info:
    RIOT OSはモジュラー[マイクロカーネルオペレーティングシステム](https://wiki.osdev.org/Microkernel)です。モジュール性により、アプリケーションに不可欠なモジュールのみをコンパイルできるため、オペレーティングシステムを可能な限り小さく保つことができます。この機能は、多くの場合マイクロコントローラーは使用可能なメモリ領域が少ないため、マイクロコントローラーにとって有益です。
    :::
    <!-- :::info: -->
    <!-- RIOT OS is a modular [microkernel operating system](https://wiki.osdev.org/Microkernel). -->
    <!-- The modularity helps keep the operating system as small as possible by allowing you to compile only the modules that are essential for your application. This feature is beneficial for microcontrollers because they often have a small amount of available memory space. -->
    <!-- ::: -->

8. USB-to-UARTコネクターへのパスを削除し、`ls/dev/ttyUSB*`コマンドを実行し、USB-to-UARTコネクターをPCに再び接続して、`ls/dev/ttyUSB*`コマンドをもう一度実行します。新しいエントリはコネクタです。
8. USB-to-UARTコネクタを削除し、`ls /dev/ttyUSB*`コマンドを実行し、USB-to-UARTコネクタをPCに接続して、`ls /dev/ttyUSB*`コマンドを再度実行することにより、USB-to-UARTコネクタへのパスを見つけます。新しいエントリはコネクタです。
<!-- 8. Find the path to your USB-to-UART connector by removing it, executing the `ls /dev/ttyUSB*` command, plugging the USB-to-UART connector back into your PC, then executing the `ls /dev/ttyUSB*` command again. The new entry is your connector. -->

9. USB-to-UARTコネクターの許可を変更します。`/USB/PORT`プレースホルダーを、`/dev/ttyUSB0`などのUSB-UARTコネクターへのパスに置き換えます。
  <!-- 9. Change the permissions for your USB-to-UART connector. Replace the `$USB_PORT` placeholder with the path to your USB-to-UART connector such as `/dev/ttyUSB0`. -->

    ```bash
    sudo chmod 777 $USB_PORT
    ```

10. `hello world`サンプルをマイクロコントローラーにフラッシュします。`$BOARD`および`$USB_PORT`プレースホルダーを、ボードの名前と`/dev/ttyUSB0`などのUSB-to-UARTコネクターへのパスに置き換えます。
  <!-- 10. Flash the 'hello world' example onto your microcontroller. Replace the `$BOARD` AND `$USB_PORT` placeholders with the name of your board and the path to your USB-to-UART connector such as `/dev/ttyUSB0` -->

    :::info:
    [ボードの名前を見つける方法](https://api.riot-os.org/group__boards.html)については、RIOTのドキュメントを参照してください。
    :::
    <!-- :::info: -->
    <!-- To [find the name of your board](https://api.riot-os.org/group__boards.html), see the RIOT documentation. -->
    <!-- ::: -->

    ```bash
    BOARD=BOARD PORT=/dev/ttyUSB0 make flash term
    ```

    :::info:
    `permission denied`または`arm-none-eabi-gcc version not supported`というメッセージが表示された場合は、[トラブルシューティングガイド](../references/troubleshooting.md)を参照してください。
    :::
    <!-- :::info: -->
    <!-- If you see  a `permission denied` or `arm-none-eabi-gcc version not supported` message, [see our troubleshooting guide](../references/troubleshooting.md). -->
    <!-- ::: -->

    シェルに次のようなものが表示されるはずです。
    <!-- You should see something like the following in the shell: -->

    ```bash
    2019-08-27 09:17:09,359 - INFO # main(): This is RIOT! (Version: 2019.10-devel-488-g1b1c9)
    2019-08-27 09:17:09,359 - INFO # Hello World!
    2019-08-27 09:17:09,361 - INFO # You are running RIOT on a(n) nrf52832-mdk board.
    ```

:::success: おめでとうございます:tada:
コードをコンパイルしてマイクロコントローラーにフラッシュできるようになったので、実際のアプリケーションを作成する準備が整いました。
:::
<!-- :::success: Congratulations :tada: -->
<!-- Now that you can compile code and flash it to your microcontroller, you're ready to build some real applications. -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

マイクロコントローラーガイドのいずれかを参照してください。
<!-- Follow one of the our microcontroller guides: -->

- [マイクロコントローラーのセンサーからデータを読み取る](../how-to-guides/read-sensor-data.md)
<!-- - [Read data from a sensor on your microcontroller](../how-to-guides/read-sensor-data.md) -->
- [環境センサーサーバーとクライアントを実行する](../how-to-guides/run-an-environment-sensor-and-client.md)
<!-- - [Run an environment sensor server and client](../how-to-guides/run-an-environment-sensor-and-client.md) -->
- [センサーデータをタングルに添付する](../how-to-guides/run-an-environment-to-tangle-app.md)
<!-- - [Attach sensor data to the Tangle](../how-to-guides/run-an-environment-to-tangle-app.md) -->
