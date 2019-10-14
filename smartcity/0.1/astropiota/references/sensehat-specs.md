# Sense HAT仕様
<!-- # Sense HAT Specifications -->

**このリストには、AstroPiOTAプロジェクトで使用されているSense HATハードウェアの仕様が含まれています。**
<!-- **This list contains the specifications for the Sense HAT hardware that's used in the AstroPiOTA project.** -->

## I²Cセンサー
<!-- ## I²C Sensors -->

**慣性測定センサー：**ST LSM9DS1[データシート](http://www.st.com/web/en/resource/technical/document/datasheet/DM00103319.pdf)。これは3D加速度計、3Dジャイロスコープ、3D磁力計を1チップにまとめたものです。Sense HATのピッチ、ロール、ヨーの向きを提供します。
<!-- Inertial measurement sensor: ST LSM9DS1 [data sheet](http://www.st.com/web/en/resource/technical/document/datasheet/DM00103319.pdf). This is a 3D accelerometer, 3D gyroscope and 3D magnetometer combined in one chip.   It provides the pitch, roll and yaw orientation of Sense HAT. -->

**気圧および温度センサー：**ST LPS25H[データシート](http://www.st.com/web/en/resource/technical/document/datasheet/DM00066332.pdf)。これはパスカルまたはミリバールの気圧と摂氏温度を提供します。
<!-- Barometric pressure and temperature sensor: ST LPS25H [data sheet](http://www.st.com/web/en/resource/technical/document/datasheet/DM00066332.pdf). This provides a measurement of air pressure in Pascals or Millibars, plus, temperature in centigrade. -->

**相対湿度および温度センサー：**ST HTS221[データシート](http://www.farnell.com/datasheets/1836732.pdf)。これは相対湿度のパーセンテージと摂氏温度を提供します。
<!-- Relative humidity and temperature sensor: ST HTS221 [data sheet](http://www.farnell.com/datasheets/1836732.pdf). This provides the percentage of relative humidity, as well as, the temperature in centigrade. -->

## 出力
<!-- ## Outputs -->

**最大60fpsのリフレッシュレートと15ビットの色解像度（RGB 5 5 5）を備えた8×8 RGB LEDマトリックス：** Cree CLU6AFKW/CLX6AFKB[データシート](http://www.cree.com/sitecore%20modules/web/~/media/Files/Cree/LED%20Components%20and%20Modules/HB/Data%20Sheets/CLX6AFKB.pdf)。
<!-- 8×8 RGB LED matrix with ~60fps refresh rate and 15-bit colour resolution (RGB 5 5 5): Cree CLU6AFKW/CLX6AFKB [data sheet](http://www.cree.com/sitecore%20modules/web/~/media/Files/Cree/LED%20Components%20and%20Modules/HB/Data%20Sheets/CLX6AFKB.pdf) -->

各ピクセルが16ビット（RGB 5 6 5）の場合、フレームバッファドライバ`/dev/fb1`からアクセスできます。これは、Sense HATがHDMIモニターに接続していない限り、本物の視覚的出力形式です。
<!-- Accessible via frame buffer driver /dev/fb1 where each pixel is 16 bit (RGB 5 6 5). This is the only real form of visual output that the Sense HAT has unless plugged into an HDMI monitor -->

## マイクロコントローラ
<!-- ## Micro controller -->

**小型MCUがLEDマトリックスを駆動し、ジョイスティック入力をスキャンします。：** Atmel ATTINY88[データシート](http://www.atmel.com/Images/doc8008.pdf)。
<!-- A small MCU drives the LED matrix and scans for joystick input: Atmel ATTINY88 [data sheet](http://www.atmel.com/Images/doc8008.pdf) -->


## 参考文献
<!-- ## References -->

https://projects.raspberrypi.org/en/projects/getting-started-with-the-sense-hat/9

https://github.com/richardstechnotes/RTIMULib2
