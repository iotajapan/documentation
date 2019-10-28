# I2Cセンサーに接続する
<!-- # Connect to an I2C sensor -->

**I2Cは、1つのデバイスがシングルデータラインとクロック信号を使用して、1つ以上の接続されたデバイスとデータを交換できるようにするプロトコルです。このガイドでは、I2CセンサーをnRF52マイクロコントローラーに接続して、相互に通信できるようにします。**
<!-- **I2C is a protocol that allows one device to exchange data with one or more connected devices through the use of a single data line and clock signal. In this guide, you connect an I2C sensor to an nRF52 microcontroller so that they can communicate with each other.** -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完了するには、次のものが必要です。
<!-- To complete this guide, you need the following: -->

- nRF52シリーズマイクロコントローラー
<!-- - An nRF52 series microcontroller -->
- I2Cセンサー開発ボード
<!-- - An I2C sensor development board -->

## シングルセンサーをマイクロコントローラーに接続する
<!-- ## Connect a single sensor to your microcontroller -->

1. SCLピンとSDAピンをマイクロコントローラーに接続します。
  <!-- 1. Connect the SCL and SDA pins to the microcontroller -->

    :::info:
    RIOT OSは、ピンP0.26およびP0.27をI2Cプロトコルに割り当てます。
    :::
    <!-- :::info: -->
    <!-- RIOT OS assigns pins P0.26 and P0.27 to the I2C protocol. -->
    <!-- ::: -->

    | **マイクロコントローラー** | **I2Cセンサー** |
    | :------------------------- | :-------------- |
    |      P0.26      |     SDA    |
    |      P0.27      |     SCL    |

    <!-- | **Microcontroller** | **I2C sensor** | -->
    <!-- |-----------------|------------| -->
    <!-- |      P0.26      |     SDA    | -->
    <!-- |      P0.27      |     SCL    | -->

:::info:
nRF52には[EasyDMA](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fcom.nordic.infocenter.nrf52832.ps.v1.1%2Feasydma.html&cp=3_1_0_9&anchor=easydma)という機能があり、[ワイヤープロトコルを任意のピンに割り当てる](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fcom.nordic.infocenter.nrf52832.ps.v1.1%2Ftwim.html&cp=3_1_0_32&anchor=concept_scx_f5p_xr)ことができます。
:::
<!-- :::info: -->
<!-- The nRF52 has a feature called [EasyDMA](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fcom.nordic.infocenter.nrf52832.ps.v1.1%2Feasydma.html&cp=3_1_0_9&anchor=easydma), which allows you to [assign wire protocols to any pin](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fcom.nordic.infocenter.nrf52832.ps.v1.1%2Ftwim.html&cp=3_1_0_32&anchor=concept_scx_f5p_xr). -->
<!-- ::: -->

## 複数のセンサーをマイクロコントローラーに接続する
<!-- ## Connect multiple sensors to your microcontroller -->

複数のセンサーが同じSDAラインを使用できるため、マイクロコントローラーはそれらを区別し、一度に1つのセンサーと通信する方法が必要です。I2Cプロトコルは、デバイスのアドレス指定の概念を使用して、データ回線上のトラフィックを調整します。
<!-- Because multiple sensors can use the same SDA line, the microcontroller needs a way to distinguish them and talk to a single sensor at a time. The I2C protocol uses the concept of device addressing to coordinate traffic on the data line. -->

複数のI2Cセンサーをマイクロコントローラーに接続している場合、各センサーが一意の7ビットアドレスを持っていることを確認する必要があります。
<!-- If you're connecting more than one I2C sensor to your microcontroller, you need to make sure that each one has a unique 7-bit address. -->

ほとんどのI2C集積回路では、I2Cアドレスを選択して、他のデバイスとのアドレスの競合を回避できます。
<!-- Most I2C integrated circuits allow you to select I2C addresses to avoid address conflicts with other devices. -->

センサーを使用する前に、常にセンサーのデータシートを確認する必要があります。一般的に、センサーにはPINがあり、これをプルアップまたはプルダウンしてI2Cアドレスを選択する必要があります。
<!-- You should always check the datasheet of your sensor before using it. Generally, sensors have a PIN that you need to pull up or down to select the I2C address. -->
