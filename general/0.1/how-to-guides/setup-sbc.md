# シングルボードコンピュータを設定する
<!-- # Set up a single-board computer -->

**シングルボードコンピュータ（SBC）は、単一の回路基板がメモリ、入力/出力ポート、マイクロプロセッサおよび他の必要な機能を含む小型のコンピュータです。SBCは、デスクトップなどのマルチボードコンピュータよりも軽量、コンパクト、信頼性が高く、電力効率もはるかに優れています。IOTAテクノロジを使用する専用の組み込みアプリケーション用にSBCを設定できます。**
<!-- **A single board computer (SBC) is a small computer in which a single circuit board includes memory, input/output ports, a microprocessor and any other necessary features. SBCs are lighter, more compact, more reliable and much more power efficient then multi-board computers such as desktops. You can set up an SBC for a purpose-built embedded application that uses IOTA technology.** -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完成するには、次のものが必要です。
<!-- To complete this guide, you need the following: -->

- SSHクライアントと構成済みネットワークがインストールされたLinuxベースのオペレーティングシステム（OS）。このガイドでは、Ubuntuを使用しますが、MacOSと同様に他のLinuxディストリビューションでも動作するはずです。
  <!-- - A Linux-based operating system (OS) with installed SSH client and a configured network. In this guide, we use Ubuntu, however other Linux distributions as well as MacOS should work. -->

    :::info:Windowsユーザー
    [仮想マシン（VM）](../how-to-guides/set-up-virtual-machine.md)または[Linuxサブシステム](https://docs.microsoft.com/en-us/windows/wsl/install-win10)を使用できます。サブシステムを使用すると、VMのオーバーヘッドなしにLinuxを実行できます。あなたが上級ユーザーであれば、LinuxツールをWindowsの同等のものと置き換えることもできます。
    :::
    <!-- :::info:Windows users -->
    <!-- You can use [a virtual machine (VM)](../how-to-guides/set-up-virtual-machine.md) or the [Linux Subsystem.](https://docs.microsoft.com/en-us/windows/wsl/install-win10).  With the subsystem, you can run Linux without the overhead of a VM.  If you are an advanced user, you can also replace the Linux tools with the Windows equivalents. -->
    <!-- ::: -->

- SBC。おすすめは`Rasperry Pi Zero W`です。
  <!-- - An SBC. Our recommendation: Rasperry Pi Zero W -->

- 可能であれば、ディスプレイとキーボードを使ってデバイスを設定します。
  <!-- - If possible, you should use a display and a keyboard to set up your device. -->

- SBCにイーサネットポートがない場合は、USB-UARTアダプタを使用します。[CP2102](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers)はよく知られている安価なアダプタですが、SBCには一体型のアダプタがあるかもしれません。調べるには、使用しているSBCのドキュメントを参照します。
  <!-- - If your SBC doesn't have an Ethernet port, use a USB-to-UART adapter. The [CP2102](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers) is a well-known and inexpensive adapter, but your SBC might have an integrated one. To find out, look at the documentation for your SBC. -->

## SDカードを準備する
<!-- ## Prepare your SD card -->

:::info:
このプロセスは、Orange Piなどの多くのSBCでも同様です。SBCのガイドが別にある場合は、そのガイドに従う必要があります。それ以外の場合は、多くの開発ボードをサポートしているので[Armbian](https://www.armbian.com/download/)を使用します。
:::
<!-- :::info: -->
<!-- This process is similar for many SBCs such as the Orange Pi. -->
<!-- If you have a separate guide for your SBC, you should follow that. Otherwise, use [Armbian](https://www.armbian.com/download/) because it supports many development boards. -->
<!-- ::: -->

シングルボードコンピュータでは、オペレーティングシステムをSDカードにフラッシュする必要があります。
<!-- In single-board computers, the operating system must be flashed onto an SD card. -->

1. [SDカードにオペレーティングシステムをフラッシュします](https://www.raspberrypi.org/documentation/installation/installing-images/)。
  <!-- 1. [Flash an operating system on your SD card](https://www.raspberrypi.org/documentation/installation/installing-images/) -->

2. SDカードを挿入してSBCの電源を入れます。
  <!-- 2. Insert your SD card and turn on your SBC -->

## 特定の機器の設定を続行する
<!-- ## Continue with the setup for your specific device -->

- デバイスでディスプレイとキーボードを使用できる場合は、["キーボードとディスプレイを使ってデバイスを設定する"](#set-up-devices-with-keyboard-and-display)ガイドに従います。
<!-- - If you can use a display and keyboard at your device, follow the ["Set up devices with keyboard and display"](#set-up-devices-with-keyboard-and-display) guide -->
- ディスプレイとキーボードがなく、USB-to-UART-Adapterがある場合は、["USB-to-UARTアダプタを介してデバイスを設定する"](#set-up-your-device-through-a-usb-to-uart-adapter)ガイドに従います。
<!-- - If you do not have display and keyboard, but an USB-to-UART-Adapter, follow the ["Set up your device through a USB-to-UART adapter"](#set-up-your-device-through-a-usb-to-uart-adapter) guide -->
- Ethernetしかない場合は、["Ethernetデバイスを設定する"](#set-up-ethernet-devices)ガイドに従います。
<!-- - If you only have Ethernet, follow the ["Set up Ethernet devices"](#set-up-ethernet-devices) guide -->

<a name="set-up-devices-with-keyboard-and-display"></a>
## キーボードとディスプレイを使ってデバイスを設定する
<!-- ## Set up devices with keyboard and display -->

1. キーボードとディスプレイをSBCに接続します。
  <!-- 1. Connect keyboard and display to your SBC -->

2. デフォルトのユーザーとパスワードでログインします。
  <!-- 2. Log in with the default user and password -->

    :::info:
    デフォルトのユーザー名またはパスワードがわからない場合は、LinuxディストリビューションのWebサイトを検索します。
    :::
    <!-- :::info: -->
    <!-- If you don't know the default username or password, search the website of your Linux distribution. -->
    <!-- ::: -->

3. ネットワークインターフェースを設定します。
  <!-- 3. Configure your network interface -->

    イーサネットがある場合は、SBCをルーターに接続するだけです。WiFiを使用したいか、またはデバイスがWiFiしか持っていない場合は、WiFiを通してルーターに接続する必要があります。以下の手順を実行して、`MY_SSID`をネットワークの名前に、`MY_PASSWORD`をネットワークのパスワードに置き換えます。
    <!-- If you have Ethernet, just connect your SBC to your router. -->
    <!-- If you want to use WiFi or your device only has WiFi, you need to connect to your router through WiFi. -->
    <!-- Do the following and replace `MY_SSID` with the name of your network and `MY_PASSWORD` with the password -->
    <!-- of your network: -->

    ```bash
    nmcli dev wifi connect MY_SSID password MY_PASSWORD
    ```

4. SBCがインターネットに接続されているか確認します。
  <!-- 4. Check if your SBC is connected to the Internet -->

    ```bash
    ping iota.org
    ```

5. IPアドレスを入手します。
  <!-- 5. Get your IP address -->

    `ifconfig`コマンドを実行します。プログラムはすべてのネットワークインタフェースと与えられたIPアドレスを返します。`eth`で始まるインターフェースはイーサネットネットワークインターフェースで、`wl`で始まるインターフェースはWiFiネットワークインターフェースです。
    <!-- Execute the `ifconfig` command. The program returns all network interfaces and the given IP addresses. -->
    <!-- The interfaces starting with `eth` are Ethernet network interfaces, -->
    <!-- and the ones starting with `wl` are the WiFi network interfaces. -->

6. SSH経由でデバイスに接続します。
  <!-- 6. Connect to your device through SSH -->

    :::info:IPv6
    IPv6を使用する場合は、SSHプログラムに`-6`コマンドライン引数を追加する必要があります。ネットワークインターフェイス名も追加する必要があります。ネットワークインターフェイス名がなければ、クライアントはSBCを見つけることができません。

    例えば：
    Host-system WiFi interface name: wlp3s0
    The SBCs' local IPv6 address: fe80::c0a2:76c6:4ed5:a44

    この例では、ホストシステムとSBCの両方がWiFi経由でルーターに接続されています。正しいルーターに接続されているインターフェースを使用する必要があります。

    結果として、以下は上記の例のために実行するべきコマンドです。
    ```bash
    ssh -6 USERNAME@fe80::c0a2:76c6:4ed5:a442%wlp3s0
    ```
    :::
    <!-- :::info:IPv6 -->
    <!-- If you use IPv6, you must add the `-6` command line argument to the SSH program. -->
    <!-- You must also add the network interface name. Without the name the client cannot find the SBC. -->
    <!--  -->
    <!-- For example: -->
    <!-- Host-system WiFi interface name: wlp3s0 -->
    <!-- The SBCs' local IPv6 address: fe80::c0a2:76c6:4ed5:a44 -->
    <!--  -->
    <!-- In this example, the host system and the SBC are both connected to the router through WiFi. -->
    <!-- You must use the interface which is connected to the correct router. -->
    <!--  -->
    <!-- As a result, this is the command to execute for the example: -->
    <!--  -->
    <!-- ```bash -->
    <!-- ssh -6 USERNAME@fe80::c0a2:76c6:4ed5:a442%wlp3s0 -->
    <!-- ``` -->
    <!-- ::: -->

    IPアドレスを見つけた場合は、SSHを通してSBCに接続します。ホストシステムで次のコマンドを使用します。
    <!-- If you found your IP address, you should connect to your SBC through SSH. -->
    <!-- Use the following command on your host-system: -->
    ```bash
    ssh USERNAME@IP_ADRESS
    ```

:::success:おめでとうございます！:tada:
SSHを通してSBCに接続しています。これでSBCでコマンドを実行できます。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You're connected to your SBC through SSH. Now you can run commands on your SBC. -->
<!-- ::: -->

<a name="set-up-your-device-through-a-usb-to-uart-adapter"></a>
## USB-UARTアダプタを介してデバイスを設定する
<!-- ## Set up your device through a USB-to-UART adapter -->

ホストシステムで以下のコマンドを実行する必要があります。
<!-- You must execute these commands on your host-system. -->

1. PlatformIOをインストールします。
  <!-- 1. Install PlatformIO -->

    シリアルポートに接続するには追加のソフトウェアが必要です。[PlatformIO](https://docs.platformio.org/en/latest/userguide/cmd_device.html?highlight=monitor#platformio-device-monitor)をお勧めします。PlatformIOはSBCと対話するための簡単なコマンドツールを提供します。
    <!-- You need additional software to connect to the serial port. -->
    <!-- We recommend [PlatformIO](https://docs.platformio.org/en/latest/userguide/cmd_device.html?highlight=monitor#platformio-device-monitor). -->
    <!-- PlatformIO provides a simple command tool to interact with your SBC. -->

2. USB-UARTアダプタを接続します。
  <!-- 2. Plug in your USB-to-UART adapter -->

3. 正しいUSBポートを探します。
  <!-- 3. Find the right USB port -->

    USBポートは`/dev/ttyUSBX`にあります。`X`がUSBポートの番号です。正しいUSBポートを見つける最も簡単な方法は、USBアダプタを接続し、`ls /dev/ttyUSB*`を使用してUSB SBCを確認し、USBアダプタを接続して再確認することです。新しく追加されたUSBポートが探しているUSBポートです。
    <!-- USB ports are available at /dev/ttyUSBX. X is the number of the USB port. -->
    <!-- The simplest way to find the right USB port is to plug out the USB-Adapter, check for the USB SBCs with -->
    <!-- ```ls /dev/ttyUSB*```, plug in the USB-Adapter and check again. The new added USB-port is the one you are looking for. -->

4. 一部のアダプターは、アクセス許可に関して予期しない動作をします。念のため、権限を変更します。
  <!-- 4. Some adapters have an unexpected behavior with their access permissions. Just in case, change the permissions -->

    ```bash
    sudo chmod 777 /dev/ttyUSBX
    ```

5. USBポートに接続します。
  <!-- 5. Connect to your USB port -->

    ボーレートを見つけるためにSBCのドキュメンテーションを参照します。Orange Pi Zeroの場合は`115200`です。
    <!-- Take a look at the documentation for your SBC to find its baud rate. In case of the Orange Pi Zero it is 115200. -->

    ```bash
    platformio SBC monitor -b BAUD_RATE -p /dev/ttyUSBX
    ```

6. SBCを再起動します。
  <!-- 6. Restart your SBC -->

7. SBCにログインします。
  <!-- 7. Log in to your SBC -->

    :::info:
    デフォルトのユーザー名またはパスワードがわからない場合は、LinuxディストリビューションのWebサイトを検索します。
    :::
    <!-- :::info: -->
    <!-- If you don't know the default username or password, search the website of your Linux distribution. -->
    <!-- ::: -->

    システムが要求したら、デフォルトのユーザー名とパスワードでログインします。rootパスワードを変更して新しいユーザーを作成する必要があります。ほとんどのシステムでは、最初のログイン後にこの変更が必要です。
    <!-- When the system asks for it, log in with the default username and password. -->
    <!-- You should change the root password and create a new user. -->
    <!-- Most systems require this change after the first login. -->

8. 利用可能かつ必要な場合は、イーサネット経由でデバイスをルーターに接続します。
  <!-- 8. If available and needed, connect your device to your router through Ethernet -->

9. 利用可能かつ必要な場合は、WiFiネットワークインターフェースを設定します。
  <!-- 9. If available and needed, configure the WiFi network interface -->

    WiFi経由でインターネットに接続する場合は、次の手順を実行して`MY_SSID`をネットワークの名前に置き換え、`MY_PASSWORD`をWiFiパスワードに置き換えます。
    <!-- If you want to connect to the Internet through WiFi, -->
    <!-- do the following and replace `MY_SSID` with the name of your network and replace `MY_PASSWORD` -->
    <!-- with your WiFi password: -->

    ```bash
    nmcli dev wifi connect MY_SSID password MY_PASSWORD
    ```

    SBCがインターネットに接続されているか確認します。
    <!-- Check if your SBC is connected to the Internet -->

    ```bash
    ping iota.org
    ```

10. IPアドレスを取得します。
  <!-- 10. Get your IP address -->

    `ifconfig`コマンドでIPアドレスを見つけることができます。`eth`で始まるインターフェースはEthernetワークインターフェースで、`wl`で始まるインターフェースはWiFiネットワークインターフェースです。
    <!-- You can now find your IP address with the  `ifconfig` command. -->
    <!-- The interfaces starting with `eth` are Ethernet network interfaces, -->
    <!-- and the ones starting with `wl` are the WiFi network interfaces. -->

11. SSH経由でSBCに接続します。
  <!-- 11. Connect to your SBC through SSH -->

    IPアドレスを見つけた場合は、SSHを通してSBCに接続します。ホストシステムで次のコマンドを使用します。
    <!-- If you found your IP address, you should connect to your SBC through SSH. -->
    <!-- Use the following command on your host-system: -->

    ```bash
    ssh USERNAME@IP_ADRESS
    ```

:::success:おめでとうございます！:tada:
SSHを通してSBCに接続しています。これでSBCでコマンドを実行できます。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You're connected to your SBC through SSH. Now you can run commands on your SBC. -->
<!-- ::: -->

<a name="set-up-ethernet-devices"></a>
## Ethernetデバイスを設定する
<!-- ## Set up Ethernet devices -->

:::warning:
このタスクはIPv4ネットワーク専用です。
:::
<!-- :::warning: -->
<!-- This task is only for IPv4 networks. -->
<!-- ::: -->

ホストシステムで以下のコマンドを実行する必要があります。
<!-- You must execute these commands on your host-system. -->

1. ローカルネットワークのIPアドレスを探します。
  <!-- 1. Find IP addresses in your local network -->

    サブネットバイトを0に設定し、ネットマスクをnmap内に設定する必要があります。故に、今回の場合は以下のようになります。

    ```bash
    内部IPアドレス：10.197.0.57
    ネットマスク：255.255.255.0
    ```
    <!-- The subnet bytes to be set to zero and the netmask must be set in nmap. -->
    <!-- So, in my case: -->
    <!-- Internal IP address: 10.197.0.57 -->
    <!-- Netmask: 255.255.255.0 -->

    IPアドレス内のすべての場所は8ビット（256ステート）を取り、ネットマスクは3バイトに設定されているため、ネットマスクは24（3×8=24）です。
    <!-- The netmask is 24, because every place in the IP address takes 8 bits (256 states) and the netmask is set on 3 bytes. 3x8=24. -->

    ```bash
    nmap -sn 10.197.0.0/24
    ```

    別の例：
    ```bash
    内部IPアドレス：10.197.3.57
    ネットマスク：255.255.0.0
    ```
    <!-- Another example: -->
    <!-- Internal IP address: 10.197.3.57 -->
    <!-- Netmask: 255.255.0.0 -->

    この例ではネットマスクは2x8=16です。したがって、24ではなく16を使用する必要があります。
    <!-- So, now it is just 2x8=16. So, you need to use 16 instead of 24. -->

    ```bash
    nmap -sn 10.197.0.0/16
    ```

    サブネットによっては、nmapがネットワーク内のすべてのIPアドレスをスキャンする必要があるため、このプロセスにはしばらく時間がかかります。小さなサブネット（netmask=24）の場合、nmapは256個のアドレスをスキャンするだけでよいので、数秒かかります。大規模なネットワークでは時間がかかる可能性があります。たとえばnetmask=16の場合、nmapは256*256アドレスをスキャンする必要があります。私のテストケースでは、これは2944.17秒かかりました。あなたが巨大なローカルネットワークにいるならば、あなたは別の変種を使うことを考えるべきです。
    <!-- Depending on the subnet, this process can take some time, since nmap needs to scan all IP addresses within the network. -->
    <!-- For a small subnet (netmask=24) is just takes some seconds, since nmap just need to scan 256 addresses. -->
    <!-- In a bigger network that can take more time. For example netmask=16: nmap needs to scan 256*256 addresses. -->
    <!-- In my test-case this took 2944.17 seconds. If you are in a huge local network, you should consider using another variant. -->

2. IPアドレスに接続します。
  <!-- 2. Connect to the IP addresses -->

    複数のIPアドレスを見つけた場合は、自分のアドレスが見つかるまですべてのIPアドレスを試します。ホストシステムで次のコマンドを使用します。
    <!-- If you found more than one IP address, just try every IP address until you found your address. -->
    <!-- Use the following command on your host-system: -->

    ```bash
    ssh USERNAME@IP_ADRESS
    ```

:::success:おめでとうございます！:tada:
SSHを通してSBCに接続しています。これでSBCでコマンドを実行できます。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You're connected to your SBC through SSH. Now you can run commands on your SBC. -->
<!-- ::: -->
