# センサーデーターをセットアップする
<!-- # Set up a sensor server -->

**センサーをサーバーとして設定すると、他のデバイスがインターネット経由でセンサーからデータをリクエストできるようになります。これは、このデータにアクセスして計算を実行する必要があるアプリケーションに役立ちます。**
<!-- **By setting up a sensor as a server, you can allow other devices to request data from it over the internet. This is useful for applications that need to access this data and perform calculations on it.** -->

:::info:
RIOT OSのバグにより、nRF52832が現在サポートされている唯一のマイクロコントローラーです。nRF52840などの最先端のハードウェアの実装が進行中です。
:::
<!-- :::info: -->
<!-- Due to a bug in RIOT OS, the nRF52832 is the only supported microcontroller at the moment. An implementation for state-of-the-art hardware such as the nRF52840 is in progress. -->
<!-- ::: -->

## 前提条件
<!-- ## Prerequisites -->

- [nRF52832マイクロコントローラーをセットアップする](../how-to-guides/set-up-nrf52-microcontroller.md)
<!-- - [Set up an nRF52832 microcontroller](../how-to-guides/set-up-nrf52-microcontroller.md) -->
- [BME/BMP 280センサーをマイクロコントローラーに接続する](../how-to-guides/connect-bosch-bme-280-bmp-280.md)
<!-- - [Connect a BME/BMP 280 sensor to the microcontroller](../how-to-guides/connect-bosch-bme-280-bmp-280.md) -->
- マイクロコントローラーに接続されているLinuxベースのPC
<!-- - Linux-based PC that's connected to your microcontroller -->

## サーバーを起動する
<!-- ## Start the server -->

1. `BLE-environment-sensor/examples/env_sensor_network`ディレクトリに移動します。
  <!-- 1. Change into the `BLE-environment-sensor/examples/env_sensor_network` directory -->

    ```bash
    cd BLE-environment-sensor/examples/env_sensor_network/
    ```

2. サンプルをマイクロコントローラーにフラッシュします。`$BOARD`と`$USB_PORT`プレースホルダーをボードの名前と`/dev/ttyUSB0`などのUSB-to-UARTコネクターへのパスに置き換えます。
  <!-- 2. Flash the example onto the microcontroller. Replace the `$BOARD` AND `$USB_PORT` placeholders with the name of your board AND the path to your USB-to-UART connector such as `/dev/ttyUSB0` -->

    ```bash
    BOARD=BOARD PORT=/dev/ttyUSB0 make flash term
    ```

    :::info:
    [RIOTのドキュメント](https://api.riot-os.org/group__boards.html)を参照して、ボードの名前を見つけてください。
    :::
    <!-- :::info: -->
    <!-- See the RIOT documentation to [find the name of your board](https://api.riot-os.org/group__boards.html). -->
    <!-- ::: -->

    :::info:
    これはサンプルアプリケーションです。`BLE-environment-sensor/examples/env_sensor_network/server.c`および`BLE-environment-sensor/examples/env_sensor_network/sensor.c`ファイルを編集して、BME/BMP 280に対して異なるセンサーをサポートできます。
    :::
    <!-- :::info: -->
    <!-- This is an example application. You can edit the `BLE-environment-sensor/examples/env_sensor_network/server.c` and `BLE-environment-sensor/examples/env_sensor_network/sensor.c` files to support different sensors to the BME/BMP 280. -->
    <!-- ::: -->

すべてがうまくいけば、エラーは表示されず、`help`を実行すると、利用可能なコマンドのリストが表示されます。`server`は利用可能なオプションの1つでなければなりません。
<!-- If everything went well, you should see no errors, and if you do `help`, you should see a list of available commands. `server` must be one of the available options. -->

実行中のすべてのプロセスのリストを表示するには、`ps`を実行します。
<!-- To see a list of all running processes, do `ps`. -->

## トラブルシューティング
<!-- ## Troubleshooting -->

次のエラーが表示される場合は、Linuxディストリビューションのpython-protobufをインストールしてください。
<!-- If you see the following error, install python-protobuf for your Linux distribution: -->

```bash
*************************************************************
*** Could not import the Google protobuf Python libraries ***
*** Try installing package 'python-protobuf' or similar.  ***
*************************************************************
```

たとえば、Ubuntuの場合、次の手順を実行します。
<!-- For example, for Ubuntu, do the following: -->

```bash
sudo apt-get install python-protobuf
```

## 次のステップ
<!-- ## Next steps -->

境界ルーターをまだ設定していない場合は、[境界ルーターをセットアップします](../how-to-guides/set-up-a-ble-ipv6-border-router.md)。
<!-- If you haven't already set up a border router, [do it now](../how-to-guides/set-up-a-ble-ipv6-border-router.md). -->

既に境界ルーターを設定している場合は、完全なBluetoothスターネットワークがあり、サンプルアプリケーションのいずれかを試す準備ができています。
<!-- If you've already set up a border router, then you have a complete Bluetooth star network, and you're ready to try one of our sample applications: -->

* [センサーサーバーにデータをリクエストする](../how-to-guides/run-an-environment-sensor-and-client.md)
<!-- * [Request data from the sensor server](../how-to-guides/run-an-environment-sensor-and-client.md) -->
* [センサーデーターをタングルに添付する](../how-to-guides/run-an-environment-to-tangle-app.md)
<!-- * [Attach sensor data to the Tangle](../how-to-guides/run-an-environment-to-tangle-app.md) -->
