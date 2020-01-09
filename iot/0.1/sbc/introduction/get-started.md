# Get started with single-board computers

**A single-board computer (SBC) is a small computer in which a single circuit board includes memory, input/output ports, a microprocessor and any other necessary features. SBCs are lighter, more compact, more reliable, and more power efficient than multi-board computers such as desktops. In this guide, you learn how to get started with SBCs. When you've completed this guide, you'll be able to connect to your SBC through the SSH protocol.**

## Hardware

このガイドを完了するには、次のものが必要です。
<!-- To complete this guide, you need the following: -->

- SD card
- An Internet connection
- A Linux PC with an SSH client. In this guide, we use Ubuntu, but you can use other Linux distributions or macOS.

:::info:Windows users
If your device has a Windows operating system, you can use [a virtual machine (VM)](root://general/0.1/how-to-guides/set-up-virtual-machine.md) or the [Linux Subsystem.](https://docs.microsoft.com/en-us/windows/wsl/install-win10).
:::

- A keyboard and a monitor (optional)

## Step 1. Choose an SBC

When choosing an SBC, you should consider its features and decide whether it will meet your needs.

For example, if you plan to use your SBC to run an IRI node, you need at least 4 GB of RAM.

Wikipedia have a [list of different SBCs](https://en.wikipedia.org/wiki/Comparison_of_single-board_computers) and a comparison of their features.

:::info:
Some of the cheapest SBCs include the Raspberry Pi Zero or Orange Pi Zero.
We recommend a device with Wi-Fi and Bluetooth LE (version >= 4.0) so that you can easily connect to it.
:::

## 手順1. SDカードを準備する
<!-- ## Step 1. Prepare your SD card -->

:::info:
このプロセスは、Orange Piなどの多くのSBCで同様です。SBCの個別のガイドがある場合は、それに従う必要があります。それ以外の場合は、[Armbian](https://www.armbian.com/download/)を使用してください。これは多くの開発ボードをサポートしています。
:::
<!-- :::info: -->
<!-- This process is similar for many SBCs such as the Orange Pi. -->
<!-- If you have a separate guide for your SBC, you should follow that. Otherwise, use [Armbian](https://www.armbian.com/download/), which supports many development boards. -->
<!-- ::: -->

シングルボードコンピューターでは、オペレーティングシステムをSDカードにフラッシュする必要があります。
<!-- In single-board computers, the operating system must be flashed onto an SD card. -->

1. [SDカードのオペレーティングシステムをフラッシュします](https://www.raspberrypi.org/documentation/installation/installing-images/)。
<!-- 1. [Flash an operating system on your SD card](https://www.raspberrypi.org/documentation/installation/installing-images/) -->

2. SDカードを挿入し、SBCの電源を入れます。
<!-- 2. Insert your SD card and turn on your SBC -->

## 手順2. SBCをセットアップする
<!-- ## Step 2. Set up your SBC -->

SBCをセットアップするには、次のオプションがあります。
<!-- You have the following options for setting up your SBC: -->

- [モニターとキーボードで設定する](#use-a-display-and-keyboard)（IPv4またはIPv6ネットワークの場合）
<!-- - [Set it up with a monitor and a keyboard](#use-a-display-and-keyboard) (for IPv4 or IPv6 networks) -->
- [USB-to-UARTコネクターを使用してセットアップする](#set-up-your-device-through-a-usb-to-uart-adapter)（IPv4またはIPv6ネットワークの場合）
<!-- - [Set it up with a USB-to-UART connector](#set-up-your-device-through-a-usb-to-uart-adapter) (for IPv4 or IPv6 networks) -->
- [イーサネットポートで設定する](#set-up-ethernet-devices)（IPv4ネットワークのみ）
<!-- - [Set it up with an Ethernet port](#set-up-ethernet-devices) (for IPv4 networks only) -->

<a name="use-a-display-and-keyboard"></a>
### モニターとキーボードを使用する
<!-- ### Use a monitor and a keyboard -->

1. モニターとキーボードをSBCに接続します。
<!-- 1. Connect the monitor and the keyboard to your SBC -->

2. デフォルトのユーザー名とパスワードでログインします。
  <!-- 2. Log in with the default username and password -->

    :::info:
    デフォルトのユーザー名またはパスワードがわからない場合は、LinuxディストリビューションのWebサイトを検索してください。
    :::
    <!-- :::info: -->
    <!-- If you don't know the default username or password, search the website of your Linux distribution. -->
    <!-- ::: -->

3. ネットワークインターフェイスを設定します。
  <!-- 3. Configure your network interface -->

    イーサネットがある場合は、イーサネットポートを介してSBCをルーターに接続します。WiFi経由でルーターに接続する場合は、次の手順を実行して、`MY_SSID`をネットワークの名前に、`MY_PASSWORD`をネットワークのパスワードに置き換えます
    <!-- If you have Ethernet, connect your SBC to your router through the Ethernet port. -->
    <!-- If you want to connect to your router through WiFi, do the following and replace `MY_SSID` with the name of your network and `MY_PASSWORD` with the password of your network -->

    ```bash
    nmcli dev wifi connect MY_SSID password MY_PASSWORD
    ```

4. SBCがインターネットに接続されているかどうかを確認します。
  <!-- 4. Check if your SBC is connected to the Internet -->

    ```bash
    ping iota.org
    ```

5. IPアドレスを取得します。
  <!-- 5. Get your IP address -->

    `ifconfig`コマンドを実行します。プログラムは、すべてのネットワークインターフェイスとそれらの指定されたIPアドレスを返します。`eth`で始まるインターフェースはイーサネットネットワークインターフェースであり、`wl`で始まるインターフェースはWiFiネットワークインターフェースです。
    <!-- Execute the `ifconfig` command. The program returns all network interfaces and their given IP addresses. The interfaces starting with `eth` are Ethernet network interfaces, and the ones starting with `wl` are the WiFi network interfaces. -->

6. SSHを介してデバイスに接続します。
  <!-- 6. Connect to your device through SSH -->

--------------------
### IPv4
`USERNAME`プレースホルダーをユーザー名に、`IP_ADDRESS`プレースホルダーをSBCのIPv4アドレスに置き換えます。

```bash
ssh USERNAME@IP_ADDRESS
```
---
### IPv6
IPv6を使用する場合は、`-6`コマンドライン引数とネットワークインターフェイス名をSSHコマンドに追加する必要があります。

例：

```bash
WiFi interface name: wlp3s0
The SBCs' local IPv6 address: fe80::c0a2:76c6:4ed5:a44
```

この例では、ホストシステムとSBCの両方がWiFiを介してルーターに接続されています。 結果として、これはSSH経由でSBCに接続するコマンドです。

```bash
ssh -6 USERNAME@fe80::c0a2:76c6:4ed5:a442%wlp3s0
```
--------------------

:::success:おめでとうございます！:tada:
SSHを介してSBCに接続しています。これで、SBCでコマンドを実行できます。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You're connected to your SBC through SSH. Now you can run commands on your SBC. -->
<!-- ::: -->

<a name="set-up-your-device-through-a-usb-to-uart-adapter"></a>
### USB-UARTコネクターを使用する
<!-- ### Use a USB-to-UART connector -->

ホストシステムでこれらのコマンドを実行する必要があります。
<!-- You must execute these commands on your host system. -->

1. PlatformIOをインストールします。
  <!-- 1. Install PlatformIO -->

    シリアルポートに接続するには、追加のソフトウェアが必要です。[PlatformIO](https://docs.platformio.org/en/latest/userguide/cmd_device.html?highlight=monitor#platformio-device-monitor)をお勧めします。PlatformIOは、SBCと対話するためのシンプルなコマンドラインツールを提供します。
    <!-- You need additional software to connect to the serial port. -->
    <!-- We recommend [PlatformIO](https://docs.platformio.org/en/latest/userguide/cmd_device.html?highlight=monitor#platformio-device-monitor). -->
    <!-- PlatformIO provides a simple command-line tool to interact with your SBC. -->

2. USB-to-UARTコネクターを差し込みます。
<!-- 2. Plug in your USB-to-UART connector -->

3. USB-to-UARTコネクタを削除し、`ls /dev/ttyUSB*`コマンドを実行し、USB-to-UARTコネクタをPCに再び接続して、`ls /dev/ttyUSB*`コマンドを再度実行することにより、USB-to-UARTコネクタへのパスを見つけます。新しいエントリはコネクタです。
<!-- 3. Find the path to your USB-to-UART connector by removing it, executing the `ls /dev/ttyUSB*` command, plugging the USB-to-UART connector back into your PC, then executing the `ls /dev/ttyUSB*` command again. The new entry is your connector. -->

4. USB-UARTコネクターのアクセス許可を変更します。`/USB/PORT`プレースホルダーを、`/dev/ttyUSB0`などのUSB-UARTコネクターへのパスに置き換えます。
  <!-- 4. Change the permissions for your USB-to-UART connector. Replace the `$USB_PORT` placeholder with the path to your USB-to-UART connector such as `/dev/ttyUSB0`. -->

    ```bash
    sudo chmod 777 $USB_PORT
    ```

5. USBポートに接続します。`/USB/PORT`と`$BAUD_RATE`プレースホルダーを、`/dev/ ttyUSB0`などのUSB-to-UARTコネクターへのパスとSBCのボーレートに置き換えます。
  <!-- 5. Connect to your USB port. Replace the `$USB_PORT` and `$BAUD_RATE` placeholders with the path to your USB-to-UART connector such as `/dev/ttyUSB0` and the baud rate of your SBC. -->

    ```bash
    platformio SBC monitor -b $BAUD_RATE -p $USB_PORT
    ```

    :::info:
    ボーレートを確認するには、SBCのドキュメントを参照してください。Orange Pi Zeroの場合、ボーレートは115200です。
    :::
    <!-- :::info: -->
    <!-- See the documentation for your SBC to find its baud rate. For the Orange Pi Zero, the baud rate is 115200. -->
    <!-- ::: -->

6. SBCを再起動します。
<!-- 6. Restart your SBC -->

7. システムから要求されたら、デフォルトのユーザー名とパスワードでログインします。ルートパスワードを変更して、新しいユーザーを作成する必要があります。ほとんどのシステムでは、最初のログイン後にこの変更が必要です。
  <!-- 7. When the system asks for it, log in with the default username and password. You should change the root password and create a new user. Most systems require this change after the first login. -->

    :::info:
    デフォルトのユーザー名またはパスワードがわからない場合は、LinuxディストリビューションのWebサイトを検索してください。
    :::
    <!-- :::info: -->
    <!-- If you don't know the default username or password, search the website of your Linux distribution. -->
    <!-- ::: -->

8. ネットワークインターフェイスを設定します。
  <!-- 8. Configure your network interface -->

    イーサネットがある場合は、イーサネットポートを介してSBCをルーターに接続します。WiFi経由でルーターに接続する場合は、次の手順を実行して、`MY_SSID`をネットワークの名前に、`MY_PASSWORD`をネットワークのパスワードに置き換えます。
    <!-- If you have Ethernet, connect your SBC to your router through the Ethernet port. -->
    <!-- If you want to connect to your router through WiFi, do the following and replace `MY_SSID` with the name of your network and `MY_PASSWORD` with the password of your network -->

    ```bash
    nmcli dev wifi connect MY_SSID password MY_PASSWORD
    ```

9. SBCがインターネットに接続されているかどうかを確認します。
  <!-- 9. Check if your SBC is connected to the Internet -->

    ```bash
    ping iota.org
    ```

10. IPアドレスを見つけます。
  <!-- 10. Find your IP address -->

    ```bash
    ifconfig
    ```

    :::info:
    `eth`で始まるインターフェースはイーサネットネットワークインターフェースであり、`wl`で始まるインターフェースはWiFiネットワークインターフェースです。
    :::
    <!-- :::info: -->
    <!-- The interfaces that start with `eth` are Ethernet network interfaces, and the ones that start with `wl` are the WiFi network interfaces. -->
    <!-- ::: -->

11. SSHを介してSBCに接続します。`USERNAME`および`IP_ADDRESS`プレースホルダーをユーザー名とIPアドレスに置き換えます。
  <!-- 11. Connect to your SBC through SSH. Replace the `USERNAME` and `IP_ADDRESS` placeholders with your username and IP address. -->

    ```bash
    ssh USERNAME@IP_ADDRESS
    ```

:::success:おめでとうございます！:tada:
SSHを介してSBCに接続しています。これで、SBCでコマンドを実行できます。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You're connected to your SBC through SSH. Now you can run commands on your SBC. -->
<!-- ::: -->

<a name="set-up-ethernet-devices"></a>
### イーサネット接続を使用する
<!-- ### Use an Ethernet connection -->

:::warning:
このタスクを完了するには、IPv4ネットワーク上にいる必要があります。
:::
<!-- :::warning: -->
<!-- You must be on an IPv4 network to complete this task. -->
<!-- ::: -->

1. ローカルネットワークでIPアドレスを見つけます。
  <!-- 1. Find IP addresses in your local network -->

    サブネットバイトをゼロに設定し、ネットマスクをnmapで設定する必要があります。

    例：

    内部IPアドレス：`10.197.0.57`

    ネットマスク：`255.255.255.0`
    <!-- The subnet bytes must be set to zero and the netmask must be set in nmap. -->
    <!-- For example: -->
    <!-- Internal IP address: 10.197.0.57 -->
    <!-- Netmask: 255.255.255.0 -->

    ここでは、IPアドレスのすべての場所が8ビット（256ステート）を取り、ネットマスクが3バイトに設定されているため、ネットマスクは24です。`3x8 = 24`。
    <!-- Here, the netmask is 24 because every place in the IP address takes 8 bits (256 states) and the netmask is set on 3 bytes. 3x8=24. -->

    ```bash
    nmap -sn 10.197.0.0/24
    ```

    別の例：

    内部IPアドレス：`10.197.3.57`

    ネットマスク：`255.255.0.0`
    <!-- Another example: -->
    <!-- Internal IP address: 10.197.3.57 -->
    <!-- Netmask: 255.255.0.0 -->

    したがって、現在は`2x8 = 16`になっています。したがって、24ではなく16を使用する必要があります。
    <!-- So, now it is just 2x8=16. So, you need to use 16 instead of 24. -->

    ```bash
    nmap -sn 10.197.0.0/16
    ```

    nmapはネットワーク内のすべてのIPアドレスをスキャンする必要があるため、サブネットによっては、このプロセスに時間がかかる場合があります。nmapは256個のアドレスをスキャンするだけなので、小さなサブネット（`netmask = 24`）には数秒しかかかりません。より大きなネットワークでは、より時間がかかる可能性があります。たとえば、`netmask = 16`：nmapは`256 * 256`アドレスをスキャンする必要があります。私のテストケースでは、これには2944.17秒かかりました。巨大なローカルネットワークにいる場合は、別のバリアントの使用を検討する必要があります。
    <!-- Depending on the subnet, this process can take some time, since nmap needs to scan all IP addresses within the network. -->
    <!-- For a small subnet (netmask=24) is just takes some seconds, since nmap just need to scan 256 addresses. -->
    <!-- In a bigger network that can take more time. For example netmask=16: nmap needs to scan 256*256 addresses. -->
    <!-- In my test-case this took 2944.17 seconds. If you are in a huge local network, you should consider using another variant. -->

2. IPアドレスに接続します。複数のIPアドレスが見つかった場合は、SBCのアドレスが見つかるまですべてのIPアドレスを試してください。
  <!-- 2. Connect to the IP addresses. If you found more than one IP address, try every IP address until you find the address of your SBC. -->

    ```bash
    ssh USERNAME@IP_ADDRESS
    ```

:::success:おめでとうございます！:tada:
SSHを介してSBCに接続しています。これで、SBCでコマンドを実行できます。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You're connected to your SBC through SSH. Now you can run commands on your SBC. -->
<!-- ::: -->
