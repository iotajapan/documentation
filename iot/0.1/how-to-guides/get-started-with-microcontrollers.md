# マイクロコントローラー開発入門
<!-- # Get started with microcontroller development -->

**このガイドでは、nRF52シリーズのマイクロコントローラーの使用方法を学びます。このガイドを完了すると、ここで説明されているモノのインターネットでIOTAを使用するための一連のチュートリアルを行うことができます。**
<!-- **In this guide, you learn how to get started with the nRF52 series of microcontrollers. When you've completed this guide, you'll be able to follow our series of tutorials for using IOTA on the Internet of Things.** -->

nRF52シリーズのマイクロコントローラーは、ウェアラブル、玩具、スマートホームデバイスと家電、ワイヤレス充電に最適なシングルチップソリューションです。
<!-- The nRF52 series of microcontrollers are an ideal single-chip solution for wearables, toys, smart home devices and appliances, and wireless charging. -->

これらのチュートリアルにはBluetoothが含まれており、通常は安価なので、このシリーズのマイクロコントローラーを使用します。
<!-- We use this series of microcontrollers in these tutorials because they include Bluetooth and are usually cheap. -->

1\. 専門の電子小売業者から次のnRF52マイクロコントローラー（nRF52のとてもよい[説明記事](https://shizuk.sakura.ne.jp/bluetooth/nrf52/overview.html)）のいずれかを購入します。
<!-- 1\. Buy one of the following nRF52 microcontrollers from a specialized electronic retailer -->

- SparkFunによるnRF52832ブレイクアウトボード
<!-- - nRF52832 breakout board by SparkFun -->
- nRF52開発キット
<!-- - nRF52 development kit -->
- nRF52832マイクロ開発キット（これらのチュートリアルでテスト済み）
<!-- - nRF52832 micro development kit (tested on these tutorials) -->
- nRF52832モジュール + nRF52832最小テストボード（これらのチュートリアルでテスト済み）
<!-- - nRF52832 module + nRF52832 minimum test board (tested on these tutorials) -->

マイクロコントローラーを購入するときは、次のことに注意してください。
<!-- When you buy a microcontroller, keep the following in mind: -->

- マイクロコントローラーに統合されたプログラマー（デバッガーと呼ばれることもある）がない場合は、別のプログラマーを購入する必要があります。
<!-- - If your microcontroller does not have an integrated programmer (sometimetimes called a debugger), you'll need to buy a separate one -->
- マイクロコントローラーに統合されたUSB-to-UARTコネクター（USB-TTLシリアルコネクターとも呼ばれる）がない場合は、別途購入する必要があります。
<!-- - If your microcontroller does not have an integrated USB-to-UART connector (sometimes called a USB to TTL serial connector), you'll need to buy a separate one -->

2\. 統合されたプログラマー（デバッガーとも呼ばれる）を持っていないマイクロコントローラーを購入する場合は、統合されたプログラマーを購入して[マイクロコントローラーに接続](../how-to-guides/connect-programmer.md)してください。
<!-- 2\. If you buy a microcontroller that doesn't have an integrated programmer (sometimes called a debugger), buy one and [connect it](../how-to-guides/connect-programmer.md). -->

プログラマーを使用して、コードをマイクロコントローラーにフラッシュします。プログラマーの詳細については[こちら](https://www.engineersgarage.com/tutorials/microcontroller-programmer-burner)を参照してください。
<!-- A programmer is used to flash the code onto the microcontroller. -->
<!-- [Read more about what a programmer is.](https://www.engineersgarage.com/tutorials/microcontroller-programmer-burner) -->

:::info:
nRF52シリーズマイクロコントローラーをフラッシュするための安価なオプションが必要な場合は、J-Linkオンボード（OB）クローンの購入を検討してください。

標準のJ-linkプログラマーは、マイクロプロセッサーでも使用できるため、より高価です。
:::
<!-- :::info: -->
<!-- If you want a cheap option for flashing an nRF52 series microcontroller, consider buying a J-Link on-board (OB) clone. -->

<!-- The standard J-link programmers are more expensive because they can also be used on microprocessors. -->
<!-- ::: -->

3\. USB-to-UARTコネクターが統合されていないマイクロコントローラーを購入する場合は、USB-to-UARTコネクターを購入して、[USB-to-UARTコネクターをマイクロコントローラーのシリアルインターフェースに接続します](../how-to-guides/connect-to-serial-interface.md)。
<!-- 3\. If you buy a microcontroller that doesn't have an integrated USB-to-UART connector, buy one and [use it to connect to your microcontroller's serial interface](../how-to-guides/connect-to-serial-interface.md). -->

:::info:
ご使用のオペレーティングシステムと互換性のあるコネクタを必ず購入してください。
:::
<!-- :::info: -->
<!-- Be sure to buy a connector that's compatible with your operating system. -->
<!-- ::: -->

4\. **オプション：**ブレッドボード、デュポンケーブル、ピンヘッダーを購入します。
<!-- 4\. **Optional:** Buy a breadboard, DuPont cables, and pin headers -->

これらのコンポーネントは、ハードウェアプロジェクトを構築する場合に役立ちます。たとえば、I2Cポートが1つしかないマイクロコントローラーに複数のI2Cセンサーを接続する場合などです。
<!-- These components are often useful when you're building hardware projects. -->
<!-- For example, if you want to connect more than one I2C sensor to a microcontoller that has only one I2C port. -->

**ブレッドボード**
<!-- **Breadboard** -->

ブレッドボードを使用すると、ハードウェアコンポーネントをはんだ付けせずに接続するための一時的な回路を作成できます。
<!-- A breadboard allows you to make temporary circuits for connecting hardware components without needing to solder them. -->

![breadboard](../images/breadboards.png)

**デュポンケーブル**
<!-- **DuPont cables** -->

デュポンケーブルは、ブレッドボードに接続するのに便利です。
<!-- DuPont cables are useful for plugging into a breadboard. -->

通常、デュポンケーブルは10の倍数で注文できます。ケーブルには、メスからメス、オスからメス、オスからオスなど、さまざまなバリエーションがあります。バリエーションを組み合わせて購入することをお勧めします。
<!-- You can usually order DuPont cables in multiples of 10, and they come in different variations such as female-to-female, male-to-female, and male-to-male. We recommended buying a mixture of variations. -->

![DuPoint cables](../images/dupont_cable.png)

左から右：メスからメス、オスからメス、オスからオス
<!-- From left to right: female-to-female, male-to-female, male-to-male -->

**ピンヘッダー**
<!-- **Pin headers** -->

ピンヘッダーを使用すると、さまざまなハードウェアコンポーネントを接続できます。
<!-- Pin headers allow you to connect your different hardware components. -->

最も一般的に使用されるヘッダーは、オスからオスへの接続です。
<!-- The most commonly used header has a male-to-male connection. -->

6\. **オプション：**シングルボードコンピューターを購入します。
<!-- 6\. **Optional:** Buy a single-board computer -->

多くの場合、シングルボードコンピューター（SBC）は、マイクロコントローラーよりも強力であり、より多くのメモリを搭載しています。その結果、SBCを使用してマイクロコントローラーに接続したり、[IOTAノード](root://ciri/0.1/how-to-guides/run-a-ciri-node-on-an-sbc.md)を実行することもできます。
<!-- A single-board computer (SBC) is often more powerful and has more memory than a microcontroller. As a result, you can use one to connect to your microcontroller or even to run an [IOTA node](root://ciri/0.1/how-to-guides/run-a-ciri-node-on-an-sbc.md). -->

:::info:
最も安価なSBCには、Raspberry Pi ZeroまたはOrange Pi Zeroが含まれます。簡単に接続できるように、Wi-FiおよびBluetooth LE（バージョン >= 4.0）を搭載したデバイスをお勧めします。
:::
<!-- :::info: -->
<!-- Some of the cheapest SBCs include the Raspberry Pi Zero or Orange Pi Zero. -->
<!-- We recommend a device with Wi-Fi and Bluetooth LE (version >= 4.0) so that you can easily connect to it. -->
<!-- ::: -->

7\. Bosch BME/BMP 280センサーなどのI2C環境センサーを購入します。
<!-- 7\. Buy an I2C environment sensor such as the Bosch BME/BMP 280 sensor -->

:::info:
チュートリアルでは、RIOT OSというオペレーティングシステムを使用します。

他のセンサーを購入する前に、[RIOT OSセンサードライバーリスト](http://riot-os.org/api/group__drivers__sensors.html)を確認して、RIOT OSがサポートされていることを確認してください。
:::
<!-- :::info: -->
<!-- We use an operating system called RIOT OS in our tutorials. -->

<!-- Before buying any other sensors, check the [RIOT OS sensor driver list](http://riot-os.org/api/group__drivers__sensors.html) to make sure that they are supported. -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

[マイクロコントローラーをセットアップする](../how-to-guides/set-up-nrf52-microcontroller.md)
<!-- [Set up your microcontroller](../how-to-guides/set-up-nrf52-microcontroller.md). -->
