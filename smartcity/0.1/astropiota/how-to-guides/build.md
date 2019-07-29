# 自分のAstroPiOTAをビルドする
<!-- # Build your own AstroPiOTA -->

**AstroPiOTAを実行するには、正しいハードウェアとRaspberry Pi用のオペレーティングシステムが必要です。**
<!-- **In order to run AstroPiOTA, you must have the correct hardware and operating system for a Raspberry Pi.** -->

![Photo of Sense Hat that looks like a slightly smaller circuit board with LED light panel](../RasSenseHat.png)

[Sense HAT version 1.0](https://thepihut.com/products/raspberry-pi-sense-hat-astro-pi)

![Photo of Raspberry Pi 3 B computer that looks like a ~4x3 circuit board](../RasPi.png)

## 前提条件
<!-- ## Prerequisites -->

Raspberry Piは、単独で、または[Raspberry Pi 3 Model B Starter Pack](https://www.digikey.com/catalog/en/partgroup/raspberry-pi-3-model-b-starter-pack-includes-a-raspberry-pi-3/70316?utm_adgroup=Kits&slid=&gclid=CjwKCAiAl7PgBRBWEiwAzFhmml25rcO7V-oO0hwQ4RdoVFCj-Sj2AnGcsFBi8ArlMDn74owwLJaywBoCBhUQAvD_BwE)で購入できます。このスターターパックには、RaspbianがプリインストールされたSDメモリーカードと、他の実験用の追加コンポーネントが含まれています。
<!-- Raspberry Pi can be purchased alone or in the [Raspberry Pi 3 Model B Starter Pack](https://www.digikey.com/catalog/en/partgroup/raspberry-pi-3-model-b-starter-pack-includes-a-raspberry-pi-3/70316?utm_adgroup=Kits&slid=&gclid=CjwKCAiAl7PgBRBWEiwAzFhmml25rcO7V-oO0hwQ4RdoVFCj-Sj2AnGcsFBi8ArlMDn74owwLJaywBoCBhUQAvD_BwE). This starter pack includes the SD Memory Card with Raspbian pre-installed plus some additional components for other experiments. -->

> Raspberry Pi 3 B+（B plus）には、Sense HATバージョン1.0の取り付けを妨げるピンがあります。
<!-- > Raspberry Pi 3 B+ (B plus) has pins that prevent attaching Sense HAT version 1.0. -->

Raspberry Piと直接通信する方法とSSH経由で通信する2つの方法があります。直接通信するには、USBキーボードとマウス、さらにHDMI対応モニターまたはテレビを使います。SSH経由で通信するには[PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)を使います。
<!-- There are two ways to interact with Raspberry Pi, directly and over SSH.  In order to interact directly, get a USB keyboard and mouse plus an HDMI-ready monitor or TV.  Interact over SSH using [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) -->

壁コンセントまたは携帯電話に電力を供給することができるUSBバッテリーでRaspberry Piに電力を供給することができます。
<!-- You may power Raspberry Pi with a wall plug or a USB battery capable of powering mobile phones. -->

## オペレーシングシステム
<!-- ## Operating System -->

この例では、スターターパックに付属のプレインストールカードを購入しました。スクラッチから始める場合は、[Raspberry Pi入門ガイド](https://projects.raspberrypi.org/en/projects/raspberry-pi-getting-started/2)に従います。W3Schoolsには、[入門ガイド](https://www.w3schools.com/nodejs/nodejs_raspberrypi.asp)もあります。スターターパックのバージョンは[Raspbian GNU/Linux 8 "Jessie"（2016-11-25-raspbian-jessie.img）](http://downloads.raspberrypi.org/raspbian/images/)からダウンロードできます。
<!-- For this example, I purchased the pre-installed card that comes with the Starter Pack. If you are starting from scratch, follow the [Raspberry Pi getting started guide](https://projects.raspberrypi.org/en/projects/raspberry-pi-getting-started/2).  W3Schools also has a [getting started guide](https://www.w3schools.com/nodejs/nodejs_raspberrypi.asp).  Download the Starter Pack version at: [Raspbian GNU/Linux 8 "Jessie" (2016-11-25-raspbian-jessie.img)](http://downloads.raspberrypi.org/raspbian/images/) -->

## コンポーネントを接続する
<!-- ## Connect Components -->

- [ラズベリーパイにSenseHatを慎重に取り付けます](https://docs-emea.rs-online.com/webdocs/1436/0900766b81436bef.pdf)。
<!-- - [Carefully attach SenseHat to Raspberry Pi](https://docs-emea.rs-online.com/webdocs/1436/0900766b81436bef.pdf) -->

![Screen capture of crontab file update described in text](../RasSen2Ras.png)

Raspberry Piからの使用許可を得た図
<!-- Permission to use diagram requested from Raspberry Pi -->

- キーボードとマウスをRaspberry PiのUSBポートに接続します。
<!-- - Connect keyboard and mouse to USB ports on Raspberry Pi -->
- Raspberry PiのHDMIポートにモニターまたはテレビを接続します。
<!-- - Connect monitor or TV to HDMI port on Raspberry Pi -->
- Raspberry Piの電源コードを接続します。
<!-- - Connect power cord on Raspberry Pi -->

> 電源コードを差し込むと、Raspberry Piの電源が入ります。
<!-- > When you plug in the power cord, Raspberry Pi will power on -->

[AstroPiOTAを実行する](../how-to-guides/run.md)
<!-- [Run AstroPiOTA](../how-to-guides/run.md) -->
