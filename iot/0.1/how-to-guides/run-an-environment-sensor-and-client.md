# センサーサーバーにデータをリクエストする
<!-- # Request data from the sensor servers -->

**このガイドでは、Bluetoothスターネットワークのセンサーにデータをリクエストします**
<!-- **In this guide, you request data from the sensors in your Bluetooth star network** -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完了するには、次のものが必要です。
<!-- To complete this guide, you need the following: -->

- [6LoWPANネットワークを構成する](set-up-a-bluetooth-star-network.md)
<!-- - [Configure a 6LoWPAN network](set-up-a-bluetooth-star-network.md) -->
- [シングルボードコンピューター（SBC）またはPCにGoツールチェーンをインストールする](https://golang.org/doc/install)
<!-- - [Install the Go toolchain on a single-board computer (SBC) or a PC](https://golang.org/doc/install) -->

## アーキテクチャ
<!-- ## Architecture -->

環境センサーは、クライアントからの着信UDPリクエストを待機するサーバーアプリケーションを実行します。
<!-- The environment sensor runs a server application that waits for incoming UDP requests from a client. -->

UDPはコネクションレス型プロトコルであるため、サーバーはTCPパケットではなくUDPパケットを受け入れます。これはTCPよりも効率的で回復力があります。
<!-- The server accepts UDP packets instead of TCP ones because UDP is a connection-less protocol, which is more efficient and more resilient than TCP. -->

レスポンスは、リクエストが送信されたポートと同じポートでクライアントに送り返されます。たとえば、クライアント（SBCまたはPC）がポート90（SBCの発信ポート）でUDPパケットをポート51037（センサーの着信ポート）に送信する場合、センサーは（SBCまたはPCの）ポート90にレスポンスを送信します。
<!-- Responses are sent back to the client on the same port from which the request was sent. For example, if the client (the SBC or the PC) sends a UDP packet on port 90 (outgoing-port at the SBC) to port 51037 (incoming-port on the sensor), the sensor sends the response to port 90 (on the SBC or PC). -->

## センサーサーバーとクライアントを実行する
<!-- ## Run the sensor server and client -->

1. マイクロコントローラーでサーバーを起動します。
  <!-- 1. Start the server on your microcontroller -->

    :::info:
    マイクロコントローラーのシリアルコンソールで次のコマンドを実行する必要があります。

    マイクロコントローラーのシリアルターミナルは、`make flash term`コマンドを実行したターミナルで開かれます。
    :::
    <!-- :::info: -->
    <!-- You need to execute the following command in the serial console of your microcontroller. -->

    <!-- The serial terminal of your microcontroller is opened in the terminal where you executed the `make flash term` command. -->
    <!-- ::: -->

    ```bash
    server start
    ```

2. クライアントをSBCまたはPCにクローンします。
  <!-- 2. Clone the client onto your SBC or PC -->

    ```bash
    git clone https://github.com/iota-community/BLE-environment-sensor-client.git $GOPATH/src/github.com/citrullin/udp_client
    ```

3. `client.go`ファイルで、`"fe80::2ca:46ff:fed3:1967"`のIPv6アドレスをセンサーサーバーのIPv6アドレスに置き換えて、クライアントが接続できるようにします。
  <!-- 3. In the `client.go` file, replace the `"fe80::2ca:46ff:fed3:1967"` IPv6 address with the IPv6 address of your sensor server to allow the client to connect to it -->

    ```c
    var seedSensorConfig = SensorNode{
        Config: SensorConfig{
            Address: net.UDPAddr{
                IP: net.ParseIP("fe80::2ca:46ff:fed3:1967"), Port: 51037, Zone: interfaceName,
            },
        },
    }
    ```

    :::info:
    センサーサーバーのIPv6アドレスを見つけるには、マイクロコントローラーのシリアルインターフェースで`ifconfig`コマンドを実行します。
    :::
    <!-- :::info: -->
    <!-- To find out the IPv6 address of your sensor server, execute the `ifconfig` command in the serial interface of the microcontroller. -->
    <!-- ::: -->

4. クライアントアプリケーションを実行します。
  <!-- 4. Run the client application -->

    ```bash
    cd $GOPATH/src/github.com/citrullin/udp_client && go run client.go
    ```

## 次のステップ
<!-- ## Next steps -->

これで、[このセンサーデーターをタングルに添付する](../how-to-guides/run-an-environment-to-tangle-app.md)準備ができました。
<!-- Now, you're ready to [attach this sensor data to the Tangle](../how-to-guides/run-an-environment-to-tangle-app.md). -->
