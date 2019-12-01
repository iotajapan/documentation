# Bluetoothスターネットワークをセットアップする
<!-- # Set up a Bluetooth star network -->

**スター型ネットワークは、センサーサーバーネットワークのセットアップに役立ちます。このガイドでは、最先端のIoT業界標準を使用して、[6LoWPAN](../concepts/ipv6-mesh-network.md)Bluetoothスターネットワークをセットアップします。**
<!-- **Star networks are useful for setting up sensor server networks. In this guide, you set up a a [6LoWPAN](../concepts/ipv6-mesh-network.md) Bluetooth star network, using state-of-the-art IoT industry standards.** -->

## 前提条件
<!-- ## Prerequisites -->

- [LinuxベースのPCを境界ルーターとして構成する](../how-to-guides/set-up-a-ble-ipv6-border-router.md)
<!-- - [Configure a Linux-based PC as a border router](../how-to-guides/set-up-a-ble-ipv6-border-router.md) -->
- [センサーサーバーノードとして使用するようにマイクロコントローラーを構成してフラッシュする](../how-to-guides/set-up-ipv6-ble-host-example.md)
<!-- - [Configure and flash the microcontroller to use as a sensor server node](../how-to-guides/set-up-ipv6-ble-host-example.md) -->

---

1. 境界ルーターでBLEデバイスをスキャンします。
  <!-- 1. Scan for BLE devices on your border router -->

    ```bash
    hcitool lescan
    ```

    また、マイクロコントローラーのシリアルコンソールで次のコマンドを実行して、アドレスを確認することもできます。
    <!-- You can also verify the address by executing the following command in the serial console of your microcontroller -->

    ```bash
    ifconfig
    ```

    Long HWaddrはBluetooth MACアドレスです。`hcitool`スキャンで返されたMACアドレスと同じでなければなりません。
    <!-- The Long HWaddr is the Bluetooth MAC address. It must be the same as the MAC address that was returned by the `hcitool` scan. -->

2. センサーサーバーを境界ルーターに接続します。`00:AA:BB:XX:YY:ZZ`プレースホルダーをセンサーサーバーのアドレスに置き換えます。
  <!-- 2. Connect your sensor server to your border router. Replace the `00:AA:BB:XX:YY:ZZ` placeholder with the address of your sensor server -->

    :::info:
    他のアドレスを追加することにより、複数のセンサーサーバーを境界ルーターに接続できます。
    :::
    <!-- :::info: -->
    <!-- You can connect multiple sensor servers to your border router by adding other addresses. -->
    <!-- ::: -->

    ```bash
    echo "connect 00:AA:BB:XX:YY:ZZ 1" >> /sys/kernel/debug/bluetooth/6lowpan_control
    ```

## 次のステップ
<!-- ## Next steps -->

Bluetoothスターネットワークができたので、サンプルアプリケーションのいずれかを試す準備ができました。
<!-- Now that you have a Bluetooth star network, you're ready to try one of our sample applications: -->

- [センサーサーバーからのデータのリクエストする](../how-to-guides/run-an-environment-sensor-and-client.md)
<!-- - [Request data from the sensor server](../how-to-guides/run-an-environment-sensor-and-client.md) -->
- [センサーデータをタングルへ添付する](../how-to-guides/run-an-environment-to-tangle-app.md)
<!-- - [Attach sensor data to the Tangle](../how-to-guides/run-an-environment-to-tangle-app.md) -->
