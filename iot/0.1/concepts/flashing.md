# フラッシュ
<!-- # Flashing -->

**マイクロコントローラーでアプリケーションを実行できるようにするには、プログラマーを使用してバイナリコードをフラッシュメモリにフラッシュする必要があります。**
<!-- **To allow a microcontroller to run your application, you need to flash the binary code onto its flash memory, using a programmer.** -->

通常のPCまたはシングルボードコンピューターでは、オペレーティングシステムはHDD、SDD、SDカード、eMMCなどのストレージシステム上にあります。システムでオペレーティングシステムを実行する前に、[ファームウェア](https://en.wikipedia.org/wiki/Firmware#Personal_computers)を実行してストレージシステムと通信する必要があります。
<!-- On a normal PC or single-board computer, the operating system is on a storage system such as HDD, SDD, SD Card, or eMMC. -->
<!-- Before your system can run the operating system, it must run [firmware](https://en.wikipedia.org/wiki/Firmware#Personal_computers) to communicate with the storage system. -->

一方、マイクロコントローラーは、組み込みの[リアルタイムオペレーティングシステム](https://en.wikipedia.org/wiki/Real-time_operating_system)（RTOS）を含むファームウェアのみを実行します。
<!-- Microcontrollers on the other hand run only the firmware, which includes the embedded [real-time operating system](https://en.wikipedia.org/wiki/Real-time_operating_system)(RTOS). -->

RTOS用のアプリケーションを作成するときは、すべてを1つのバイナリにコンパイルし、それをマイクロコントローラーのフラッシュメモリにフラッシュします。次に、マイクロコントローラーはコンパイルされたコードを実行します。
<!-- When you write an application for an RTOS, you compile everything into one binary and flash it onto the flash memory of the microcontroller. Then, the microcontroller runs the compiled code. -->

:::info:
Arduino Unoなどの一部のマイクロコントローラーは、PCまたはSBCのファームウェアと同様のジョブを実行するブートローダーを使用します。ブートローダーはマイクロコントローラにロジックを追加するため、USBを介してマイクロコントローラーをプログラムできます。ブートローダーは、マイクロコントローラーのプログラミングを開始するために追加のハードウェアを必要としないため、より使いやすいです。ただし、ブートローダーはフラッシュメモリ上の不要なスペースを使用します。これはアプリケーションで使用できます。
:::
<!-- :::info: -->
<!-- Some microcontrollers, such as the Arduino Uno, use a bootloader, which does a similar job as the firmware on a PC or SBC. -->
<!-- The bootloader adds some logic to the microcontroller, so that you are able to program the microcontroller through USB. -->
<!-- A bootloader is more use-friendly because you don't need additional hardware to start programming the microcontroller. But, the bootloader uses unnecessary space on the flash memory, which could be used by your application. -->
<!-- ::: -->

## フラッシュの仕組み
<!-- ## How flashing works -->

マイクロコントローラーには、フラッシュに使用される以下のインターフェースがあります。
<!-- Microcontrollers have the following interfaces that are used for flashing: -->

- [Serial Wire Debug](http://www.ti.com/lit/wp/spmy004/spmy004.pdf)
- [JTAG](https://en.wikipedia.org/wiki/JTAG)

これらのインターフェースは両方とも同じプロトコルを使用しますが、ピン数が異なります。
<!-- Both of these interfaces use the same protocol, but have different pin counts. -->

バイナリコードをマイクロコントローラーにフラッシュするには、プログラマーと呼ばれるデバイスを使用する必要があります。
<!-- To flash binary code onto the microcontroller, you need to use a device called programmer. -->

J-Linkは最も有名な汎用プログラマーの一つです。J-Linkでほとんどのマイクロコントローラーをプログラムできます。
<!-- J-Link is one of the most famous general programmers. You can program most microcontrollers with J-Link. -->

特定の会社によって作られたマイクロコントローラー用のプログラマーもあります。たとえば、[STLinkV2](https://www.st.com/en/development-tools/st-link-v2.html)は、[STMicroelectronics](https://de.wikipedia.org/wiki/STMicroelectronics)によって作成されたマイクロコントローラーのプログラミングに使用されます。
<!-- Other programmers exist for microcontrollers that are made by specific companies. For example, the [STLinkV2](https://www.st.com/en/development-tools/st-link-v2.html) is used to program microcontrollers made by [STMicroelectronics](https://de.wikipedia.org/wiki/STMicroelectronics). -->
