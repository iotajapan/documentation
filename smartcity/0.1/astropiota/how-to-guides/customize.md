# AstroPiOTAをカスタマイズする
<!-- # Customize AstroPiOTA -->

**AstroPiOTA送信者はSense HATからセンサーデータを収集し、センサーデータをスクロールマーキーに公開し、センサーデータをJSON形式でタングルのMAMチャネルに送信します。AstroPiOTA受信者はMAMチャネルを監視してセンサーデータをリッスンします。これらのファイルの両方をカスタマイズして、AstroPiOTAによるデータの公開方法とレポート方法を変更できます。**
<!-- **The AstroPiOTA sender gathers sensor data from Sense HAT, publishes it to the scrolling marquee, and sends it in JSON format to a MAM channel on the Tangle. The ASTROPiOTA receiver listens to the MAM channel and reports the sensor data. You can customize both of these files to change how ASTROPiOTA publishes and reports data.** -->

このサンプルコードは、[Dave de FijterのHigh Mobility MAMの例](https://github.com/iotaledger/high-mobility-blueprints/tree/master/mam)にインスパイアされています。
<!-- This sample code was inspired by [Dave de Fijter's High Mobility MAM example](https://github.com/iotaledger/high-mobility-blueprints/tree/master/mam). -->

## AstroPiOTA送信者
<!-- ## AstroPiOTA sender -->

まず、`sender.js`ファイルにMAMクライアントとIOTA trytesコンバータをインポートします。次に、スクロールマーキーを有効にするためにsense-hat-ledパッケージをインポートします。最後に、IMUデータを検出するためにIMUパッケージをインポートします。
<!-- First, the sender.js file imports the MAM client and the IOTA trytes converter.  Next, it imports the sense-hat-led package to enable the scrolling marquee.  Finally, it imports the IMU package used to sense IMU data. -->

```javascript
const Mam = require('../external/mam.client.js');
const { asciiToTrytes } = require('@iota/converter');
const sense = require("sense-hat-led");
const imu = require("node-sense-hat").Imu;
const IMU = new imu.IMU();
```

`sender.js`はMAMメッセージストリームを初期化します。初期化中に、メッセージを安全に送信するために使用されるシードとアドレスが生成されます。この例では、メッセージはMainnetネットワークではなくDevnetネットワークに保存されています。
<!-- Sender.js initializes the MAM message stream.  During initialization, seeds and addresses are generated to be used for securely posting messages.  In this example, messages are stored on the Devnet network rather than the Mainnet network. -->

```javascript
let mamState = Mam.init('https://nodes.devnet.iota.org:443');
```

モードは`public`に設定されています。パブリックモードでは、誰でもAstroPiOTAによって公開された環境データを見ることができます。プレースホルダ、mamSecretは、`private`または`restricted`メッセージタイプでも使用できます。
<!-- Mode is set to "public".  Public mode allows anyone to view environment data published by AstroPiOTA. A placeholder, mamSecret, is available for use with a "private" or "restricted" message type. -->

```javascript
const mamType = 'public';
const mamSecret = 'DONTSHARETHIS';
mamState = Mam.changeMode(mamState, mamType, mamSecret);
```

`sender.js`は、湿度と温度のデータを摂氏または摂氏と華氏で表示するようにSense Hatのスクロールマーキーを設定しています。マーキーをスピードアップするために、データは丸められています。
<!-- Sender.js configures the Sense Hat scrolling marquee to post humidity and temperature data in centigrade or Celsius and Fahrenheit. To speed up the marquee, this data was rounded. -->

> 生の温度データが表示されます。ただし、Sense Hatの温度センサーはRaspberry Piの近くにあり、使用中は加熱されます。キャリブレーションテストによると、Sense Hatの温度は現地の気温より約13度高くなっています。Sense Hatの気温と現地の気温を比較して、自分の位置に合わせて調整できます。MAMメッセージで報告されているデータを変更したい場合があります。
<!-- > Raw temperature data is shown.  However, the Sense Hat temperature sensor is near Raspberry Pi which heats up during use. Calibration tests indicate that the Sense Hat temperature is about 13 degrees above local temperature. You can calibrate to your location by comparing Sense Hat temperature to local temperature. You may want to change the data being reported in your MAM message. -->

スクロール速度は0.2で、スクロールテキストは読むのに十分遅くなりますが、遅くなりすぎません。`backColour`と`textColour`は、0から255までのRGB値を使用して設定されます。この例では、背景色はオレンジ、テキストは青です。
<!-- Scroll speed is 0.2, making the scrolling text slow enough to read, but not too slow.  The `backColour` and `textColour` are set using RGB values between 0 and 255.  In this example, the background color is orange and the text is blue. -->

```javascript
async function publish (data) {
  sense.showMessage("AstroPi Temp " + String(Math.round(data.temperature)) +
      "C " + String(Math.round(data.temperature*1.8 + 32.00)) + "F  Humidity " +
      String(Math.round(data.humidity)) + " IOTA     " ,
      scrollSpeed=0.2, backColour=[100,100,0], textColour=[0,0,100]);
```

MAMメッセージはjsonにフォーマットされています。ロケーションはハードコーディングされています。
<!-- The MAM message is formatted into json.  Location is hard-coded. -->

```javascript
  let toSend = JSON.stringify({ 'AstroPiData': data, 'location': 'Los Angeles,CA ,USA' });
  console.log('Data to send to tangle:', toSend);
```

MAMメッセージは、IOTAタングルで要求されている形式であるトライトに変換されます。
<!-- The MAM message is converted to trytes, the format required by the IOTA Tangle -->

```javascript
  const trytes = asciiToTrytes(toSend);
  const message = Mam.create(mamState, trytes);

  mamState = message.state;
```

センサーデータは、MAMチャネルの初期化中に作成されたMAMメッセージアドレスに送信されます。
<!-- Sensor data is sent to the MAM message address created during initialization of the MAM channel -->

```javascript
  try {
    await Mam.attach(message.payload, message.address);
    console.log('Attached to Tangle!');
  } catch (e) {
    console.log(e);
  }
```

`receiver.js`がAstroPiOTAの現在のMAMチャンネルを見つけるためには、メッセージルートが必要です。したがって、`sender.js`はコンソールにメッセージルートを次の形式で出力します。
<!-- In order for Receiver.js to find AstroPiOTA's current MAM channel, it must have the message root.  Thus, sender.js prints instructions on the console in this format: -->

```bash
ADDRESSXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

AstroPiOTAメッセージを表示するには、コンソールからコマンドをコピーして別のターミナルウィンドウにペーストします。
<!-- Copy the command from your console and paste it into another terminal window to view the AstroPiOTA messages. -->

```javascript
  if (mamState.channel.start === 1) {
    console.log('\r\nListen to this stream with\n\r\n\r   >  npm run receiver', message.root, '\r\n\r\n');
  } else {
    console.log('\r\nUpdated root: ', message.root, '\r\n');
  }
}
```

Sense Hat IMUからのエラーはすべて報告されます。センサーデータが公開されています。
<!-- Any errors from Sense Hat IMU are reported. Sensor data is published. -->

```javascript
async function updateLocation () {
  IMU.getValue((err, data) => {
    if (err !== null) {
      console.error("Hmmm...Sensehat data: ", err);
      return;
    }
    try {
      publish(data);
    } catch (e) {
      console.log('IMU Error', e);
    }
  });
}

console.log('\r\n\r\nUpdating MAM stream with SenseHat data\r\n');
```

アップデータサービスは約30秒ごとに新しいセンサーデータをチェックします。
<!-- The updater service checks for new sensor data about every 30 seconds -->

```javascript
updateLocation();
setInterval(updateLocation, 30000);
```

以下でスクロールマーキーがリセットされます。
<!-- The scrolling marquee is reset -->

```bash
sense.clear(0,100,0)
```

## AstroPiOTA受信者
<!-- ## AstroPiOTA receiver -->

送信側と同様に、`receiver.js`はMAMクライアントとtrytesコンバータをインポートします。モードは `public`に設定されています。プレースホルダ、mamSecretは、`private`または`restricted`メッセージタイプでも使用できます。
<!-- Like the sender, receiver.js imports the MAM client and the trytes convertor.  Mode is set to "public".  A placeholder, mamSecret, is available for use with a "private" or "restricted" message type. -->

```javascript
const Mam = require('../external/mam.client.js');
const { trytesToAscii } = require('@iota/converter');
const mamType = 'public';
const mamSecret = 'DONTSHARETHIS';
```

`receiver.js`は現在のルートと次のルートを追跡するため、何をリッスンし、いつデータを画面に出力するかがわかります。`receiver.js`がSense Hatのスクロールマーキーにデータを公開することはありません。
<!-- Receiver.js keeps track of the current and next root so it knows what to listen to and when to output data to the screen.  Receiver.js never publishes data on the Sense Hat scrolling marquee. -->

```javascript
let root = null;
let nextRoot = process.argv[2];

function showData (raw) {
  const data = JSON.parse(trytesToAscii(raw));
  console.log(" ");
  console.log(data.ts, '-', data.AstroPiData);
}
```

タングルのアドレスは、`sender.js`で使用されているアドレスと同一である必要があります。そうしないと、受信者はメッセージを見つけることができません。
<!-- The address of the Tangle must be identical to one used in sender.js or the receiver cannot find the messages -->

``` javascript
async function initMam () {
  console.log('\r\n\r\n');
  console.log('Listening for MAM sensor data from AstroPi...');
  console.log('\r\n');
  await Mam.init('https://nodes.devnet.iota.org:443');
}
```

`receiver.js`は、現在のルート上の新しいデータについて5秒ごとにMAMストリームをチェックします。新しいルートが見つかった場合は、新しいルートを監視します。
<!-- Receiver.js checks the MAM stream every 5 seconds for new data on the current root.  If a new root is found, then it monitors the new root. -->

```javascript
async function checkMam () {
  if (root !== nextRoot) {
    root = nextRoot
  }
```

`showData`コールバックは、見つかったメッセージごとに順番に呼び出されます。したがって、メッセージはタングルに投稿された日時に基づいて時系列に表示されます。
<!-- The showData callback will be called, in order, for each message found.  Thus, the messages are shown in chronological order based on the date and time posted to the Tangle. -->

```javascript
  const data = await Mam.fetch(root, mamType, mamSecret, showData)
  nextRoot = data.nextRoot

  // Check again in 5 seconds
  setTimeout(checkMam, 5000);
}
```

`receiver.js`が初期化され、AstroPiOTAからのMAMメッセージのためにタングルの監視を開始します。
<!-- Receiver.js initializes and starts monitoring the Tangle for MAM messages from AstroPiOTA -->

```javascript
initMam();
checkMam();
```
