# AstroPiOTA

**AstroPiOTAは[AstroPi](https://www.nasa.gov/mission_pages/station/research/experiments/2429.html)のクローンです。AstroPiは、ISS（国際宇宙ステーション）でRaspberry Piを使用して、宇宙飛行士が自分の環境について最新の情報を入手できるようにしたプロジェクトです。 AstroPiは、温度、湿度、その他のデータを検知するためのRaspberry PiコンピュータとSense HATセンサーを使って作られています。**
<!-- **AstroPiOTA is a clone of [AstroPi](https://www.nasa.gov/mission_pages/station/research/experiments/2429.html), which is a project that used Raspberry Pis on the International Space Station (ISS) to keep astronauts up to date about their environment.  AstroPi was made with a Raspberry Pi computer and a Sense HAT sensor for sensing temperature, humidity, and other data.** -->

AstroPiOTAはタングル上のローカル環境データを追跡するためにマスク認証メッセージング（MAM）を使用します。地球上で、AstroPiOTAはローカルの天気予報を理解し報告するのを助け、地震予知に役立つかもしれません。
<!--  AstroPiOTA uses masked authenticated messaging (MAM) for keeping track of local environment data on the Tangle. Here on earth, AstroPiOTA helps us understand and report local weather and may aid in earthquake prediction. -->

MAMは購読サービスです。最新のデータを受信するためにAstroPiOTAチャンネルを購読します。
<!-- MAM is a subscription service. Subscribe to the AstroPiOTA channel to receive the latest data. -->

ビデオをご覧ください。（Coming soon）
<!-- Watch the video:  (Coming soon) -->

## 環境データ
<!-- ## Environment Data -->

Sense Hatには慣性計測装置（IMU）があり、以下が含まれています。
<!-- Sense Hat has an inertial measurement unit (IMU) and includes the following: -->

- 温度および湿度センサー
<!-- - Temperature and humidity sensors -->
- 気圧センサー
<!-- - Barometric pressure sensor -->
- 加速力を測定する加速度計
<!-- - Accelerometer that measures acceleration forces -->
- 運動量と回転を測定するジャイロスコープ
<!-- - Gyroscope that measures momentum and rotation -->
- 地球の磁場を測定する磁力計（コンパスのようなもの）
<!-- - Magnetometer that measures the Earth’s own magnetic field (a bit like a compass) -->

一部のIMUデータは、[直交座標](https://en.wikipedia.org/wiki/Cartesian_coordinate_system)を使用して測定されます。
  <!-- Some IMU data is measured using [Cartesian coordinates](https://en.wikipedia.org/wiki/Cartesian_coordinate_system) where: -->

    ```bash
    xはx軸を中心としたロールまたは回転
    yはy軸を中心としたピッチまたは回転
    zはヨーまたはz軸を中心とした回転
    ```

    <!-- ```bash -->
    <!-- x is roll or rotation about the x-axis -->
    <!-- y is pitch or rotation about the y-axis -->
    <!-- z is yaw or rotation about the z-axis -->
    <!-- ``` -->

## プログラミング言語
<!-- ## Programming Language -->

AstroPiOTAはJavaScriptで書かれており、Sense HATデータを以下のjsonフォーマットでタングルに送信します。
<!-- AstroPiOTA is written in JavaScript and sends Sense HAT data to the Tangle in this json format: -->

```json
{"AstroPiData":
{"timestamp":"2018-12 09T01:41:09.752Z",
"accel":{"x":0.01195599976927042,"y":0.0029279999434947968,"z":0.9884439706802368},
"gyro":{"x":0.0393543504178524,"y":0.02155846171081066,"z":-0.02419554442167282},
"compass":{"x":-33.50177764892578,"y":-1.302448034286499,"z":3.9364640712738037},
"fusionPose":{"x":2.86590838432312,"y":1.2279855012893677,"z":-2.3845863342285156},
"tiltHeading":3.102440595626831,
"pressure":1011.86083984375,
"temperature":34.03499984741211,
"humidity":32.178466796875},
"location":"Los Angeles,CA,USA"}
```

## 自分のAstroPiOTAをビルドする
<!-- ## Build Your Own AstroPiOTA -->

[AstroPiOTAをビルドする](../how-to-guides/build.md)
<!-- [Build AstroPiOTA](../how-to-guides/build.md) -->

[ソフトウェアをインストールして実行する](../how-to-guides/run.md)
<!-- [Install the software and run it](../how-to-guides/run.md) -->

[ヘッドレスモードを使用して実行する](../how-to-guides/connect.md)
<!-- [Run using Headless mode](../how-to-guides/connect.md) -->

[AstroPiOTAをカスタマイズする](../how-to-guides/customize.md) - AstroPiOTAサンプルコードは、[Dave de FijterのHigh Mobility MAMの例](https://github.com/iotaledger/high-mobility-blueprints/tree/master/mam)からインスピレーションを受けています。
<!-- [Customize AstroPiOTA](../how-to-guides/customize.md) - AstroPiOTA sample code was inspired by [Dave de Fijter's High Mobility MAM example](https://github.com/iotaledger/high-mobility-blueprints/tree/master/mam). -->

[Sense HAT仕様](../references/sensehat-specs.md)
<!-- [Sense Hat specifications](../references/sensehat-specs.md) -->
