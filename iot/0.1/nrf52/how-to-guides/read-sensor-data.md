# マイクロコントローラーからセンサーデータを読み取る
<!-- # Read sensor data from a microcontroller -->

**このガイドでは、nRF52シリーズマイクロコントローラーに接続されているセンサーからデータを読み取ることができる低予算のアプリケーションを作成します。**
<!-- **In this guide, you create a low-budget application that allows you to read data from a sensor that's connected to an nRF52-series microcontroller.** -->

## ハードウェア
<!-- ## Hardware -->

このガイドを完了するには、次のものが必要です。
<!-- To complete this guide, you need the following: -->

- [準備されたnRF52マイクロコントローラー](../introduction/get-started.md)
<!-- - [A prepared nRF52 microcontroller](../introduction/get-started.md) -->
- [接続されたBME / BMP 280センサー](../setup-guides/connect-bosch-sensor.md)
<!-- - [A connected BME/BMP 280 sensor](../setup-guides/connect-bosch-sensor.md) -->

## 手順1. センサードライバーを構成する
<!-- ## Step 1. Configure the sensor drivers -->

センサーからデータを読み取るには、センサーのドライバーがコンパイルされてマイクロコントローラーにフラッシュされるようにコードを構成する必要があります。
<!-- To be able to read data from a sensor, you need to configure the code so that the sensor's drivers are compiled and flashed onto the microcontroller. -->

1. `BLE-environment-sensor/examples/saul`ディレクトリに移動します。
  <!-- 1. Change into the `BLE-environment-sensor/examples/saul` directory -->

    ```bash
    cd BLE-environment-sensor/examples/saul
    ```

2. `Makefile`ファイルで、`USEMODULE + = ps`の下に`USEMODULE + = bmx280`を追加して、BME/BMP 280センサーのドライバーをアプリケーションにコンパイルできるようにします。
  <!-- 2. In the `Makefile` file, add `USEMODULE += bmx280` under `USEMODULE += ps` so that the driver for the BME/BMP 280 sensor can be compiled into your application -->

## 手順2. アプリケーションをコンパイルしてフラッシュする
<!-- ## Step 2. Compile and flash the application -->

正しいセンサードライバーを使用するようにコードを構成したら、コンパイルしてマイクロコントローラーにフラッシュできます。
<!-- After configuring the code to use the correct sensor drivers, you can compile it and flash it onto your microcontroller. -->

1. オペレーティングシステムとアプリケーションをコンパイルしてフラッシュします。`$BOARD`および`$USB_PORT`プレースホルダーをボードの名前と、USB/UARTコネクタへのパス（`/dev/ttyUSB0`など）に置き換えます。
  <!-- 1. Compile and flash the operating system and the application. Replace the `$BOARD` AND `$USB_PORT` placeholders with the name of your board and the path to your USB-to-UART connector such as `/dev/ttyUSB0`. -->

    ```bash
    BOARD=BOARD PORT=USB_PORT make flash term
    ```

    :::info:
    RIOTのドキュメントを参照して、[ボードの名前を見つけてください](https://api.riot-os.org/group__boards.html)。
    :::
    <!-- :::info: -->
    <!-- Search the RIOT documentation to [find the name of your board](https://api.riot-os.org/group__boards.html). -->
    <!-- ::: -->

2. RIOT OS [ハードウェアアブストラクションレイヤー](https://en.wikipedia.org/wiki/Hardware_abstraction)（[SAUL](https://riot-os.org/api/group__drivers__saul.html)）を使用して利用可能なすべてのセンサーのリストを検索します。
  <!-- 2. Use the RIOT OS [hardware abstraction layer](https://en.wikipedia.org/wiki/Hardware_abstraction) ([SAUL](https://riot-os.org/api/group__drivers__saul.html)) to find a list of all available sensors -->

    :::info:
    [SAULデバイスクラス](https://riot-os.org/api/group__drivers__saul.html#ga425be5f49e9c31d8d13d53190a3e7bc2)からセンサーにアクセスすることもできます
    :::
    <!-- :::info: -->
    <!-- You can also access the sensor by its [SAUL device class](https://riot-os.org/api/group__drivers__saul.html#ga425be5f49e9c31d8d13d53190a3e7bc2) -->
    <!-- ::: -->

    ```bash
    saul
    ```

    使用可能なすべてのセンサーのリストが表示されます。例えば：
    <!-- You should see a list of all available sensors. For example: -->

    ```bash
    2019-09-02 16:54:12,881 - INFO #  saul
    2019-09-02 16:54:12,883 - INFO # ID     Class           Name
    2019-09-02 16:54:12,887 - INFO # #0     ACT_SWITCH      Led Red
    2019-09-02 16:54:12,888 - INFO # #1     ACT_SWITCH      Led Green
    2019-09-02 16:54:12,891 - INFO # #2     ACT_SWITCH      Led Blue
    2019-09-02 16:54:12,893 - INFO # #3     SENSE_TEMP      NRF_TEMP
    2019-09-02 16:54:12,894 - INFO # #4     SENSE_TEMP      bme280
    2019-09-02 16:54:12,896 - INFO # #5     SENSE_PRESS     bme280
    2019-09-02 16:54:12,897 - INFO # #6     SENSE_HUM       bme280
    ```

3. 特定のセンサーからデータを読み取るには、`saul read`コマンドに続けてIDを実行します。すべてのセンサーからデータを読み取るには、`saul read all`コマンドを実行します。
  <!-- 3. To read the data from a particular sensor, execute the `saul read` command followed by an ID. To read the data from all sensors, execute the `saul read all` command. -->

    ```bash
    saul read 5
    ```

    次のようなものが表示されるはずです。
    <!-- You should see something like the following: -->

    ```bash
    2019-09-02 16:54:30,904 - INFO #  saul read 5
    2019-09-02 16:54:30,907 - INFO # Reading from #5 (bme280|SENSE_PRESS)
    2019-09-02 16:54:30,909 - INFO # Data:         1631 hPa
    ```

## 次のステップ
<!-- ## Next steps -->

このようなシェルセッションからセンサーデータを読み取ることは、アプリケーションのデバッグ中にのみ役立ちます。
<!-- Reading sensor data from a shell session like this is useful only while you debug an application. -->

実稼働アプリケーションの場合、同じローカルネットワーク上のクライアントがセンサーに接続してデータを読み取ることを許可する[Bluetoothスターネットワークを設定する](../how-to-guides/set-up-a-bluetooth-star-network.md)ことができます。
<!-- For a production application, you can [set up a Bluetooth star network](../how-to-guides/set-up-a-bluetooth-star-network.md) that allows clients on the same local network to connect to the sensor and read its data. -->
