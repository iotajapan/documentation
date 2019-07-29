# AstroPiOTAを実行する
<!-- # Run AstroPiOTA -->

以下の手順は、オペレーティングシステムが設定されていることを前提としています。このインストールには複数のコンポーネントがあるため、それらを組み合わせる前に各コンポーネントが個別に機能することを確認するためのスクリプトが含まれています。
<!-- The following instructions assume the operating system is configured. Because this installation has several components, scripts are included to make sure that each component works individually before combining them. -->

## 前提条件
<!-- ## Prerequisites -->

次の前提条件がRaspbian Jessieにプレインストールされています。
<!-- The following prerequisites are pre-installed on Raspbian Jessie: -->

- Python3
- Pip3.2
- Node.js (8+)
- [AstroPiOTAパッケージ](https://github.com/NelsonPython/AstroPiOTA.git)
<!-- - [AstroPiOTA package](https://github.com/NelsonPython/AstroPiOTA.git) -->
- [Sense HAT Node.jsパッケージ](https://www.npmjs.com/package/node-sense-hat)
<!-- - [Sense HAT Node.js package](https://www.npmjs.com/package/node-sense-hat) -->
- [IOTA JavaScriptライブラリ](https://github.com/iotaledger/iota.lib.js)
<!-- - [IOTA JavaScript Library](https://github.com/iotaledger/iota.lib.js) -->

## Raspberry Piを設定する
<!-- ## Configure Raspberry Pi -->

Raspberry Piを設定する最も簡単な方法は、プラグインモニター、キーボード、マウスを使ってRaspberry Piと直接通信することです。
<!-- The easiest way to configure Raspberry Pi is by directly interacting with it. Plug-in monitor, keyboard and mouse. -->

1. デフォルトのユーザーとパスワードで自動的に起動するRaspberry Piをオンにします。
  <!-- 1. Turn on the Raspberry Pi, which will automatically boot with the default user and password -->

    ```bash
    Default username:  pi
    Default password:  raspberry
    ```

2. Raspberry PiをWiFiに接続します。
  <!-- 2. Connect the Raspberry Pi to WiFi -->

3. キーボードマッピングをアメリカ英語に設定します。それ以外の場合は、パイプ記号（|）がチルダ記号（~）にマップされているためにインストールコマンドが失敗します。
  <!-- 3. Set the keyboard mapping to USA English. Otherwise, you will be surprised when installation commands fail because the pipe symbol (|) is mapped to the tilde symbol (~). -->

4. メニューのRaspberryをクリックして、**Preferences** > **Raspberry Pi Configuration**に移動します。
  <!-- 4. Click the Raspberry in the menu, and go to **Preferences** > **Raspberry Pi Configuration** -->

    ![Raspberry Pi configuration window as described in text](../Localisation.png)

5. **Localisation** > **Set Keyboard**をクリックし、**United States -> English (US)**を選択します。ロケール、タイムゾーン、およびWiFiの国を設定することもできます。
  <!-- 5. Click **Localisation** > **Set Keyboard**, and select **United States -> English (US)**. You can also set your locale, timezone, and WiFi country. -->

    ![Window for setting localisation as described in text](../localisation2.png)

6. **Interfaces**をクリックしてSSHを有効にし、PuTTYでリモート接続できるようにします。
  <!-- 6. Click **Interfaces** and enable SSH so you can connect remotely with PuTTY -->

    ![Window for enabling SSH as described in text](../SSH.png)

## Node.jsをインストールする
<!-- ## Install Node.js -->

Raspbian JessieにはNodeRedが付属していますが、NodeRedは削除する必要があります。
<!-- Raspbian Jessie comes with NodeRed, which must be removed. -->

1. NodeRedおよび従来のnodejsモジュールを削除します。
  <!-- 1. Remove NodeRed and legacy nodejs modules -->

    ```bash
    sudo apt-get remove nodered -y
    sudo apt-get remove nodejs nodejs-legacy -y
    ```

2. Raspberry Piを再起動します。
  <!-- 2. Reboot the Raspberry Pi -->

3. Node.js 8をインストールします。
  <!-- 3. Install Node.js 8 -->

    ```bash
    curl -sL https://deb.nodesource.com/setup_8.x | sudo bash -
    sudo apt-get install nodejs -y
    node -v
    npm -v
    ```

## IOTA JavaScriptライブラリをインストールする
<!-- ## Install the IOTA JavaScript Library -->

1. IOTA JavaScriptライブラリをダウンロードしてインストールします。
  <!-- 1. Download and install the IOTA JavaScript library -->

    ```bash
    git clone https://github.com/iotaledger/iota.lib.js
    cd iota.lib.js
    sudo npm install iota.lib.js
    ```

2. IOTA開発者のタングルからノード情報を取得してインストールを確認します。テキストファイルを開き、以下のスクリプトをコピーします。
  <!-- 2. Check your installation by retrieving Node information from the IOTA Developer's Tangle.  Open a text file and copy this script: -->

  ```javascript
  var request = require('request');

  var command = {
      'command': 'getNodeInfo'
  };

  var options = {
    url: 'https://nodes.devnet.iota.org:443',
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'X-IOTA-API-Version': '1',
      'Content-Length': Buffer.byteLength(JSON.stringify(command))
    },
    json: command
  };

  request(options, function (error, response, data) {
    if (!error && response.statusCode == 200) {
      console.log(data);
    }
  });
  ```

3. 上記のファイルを`getNode.js`としてiota.lib.jsフォルダに保存してから実行します。
  <!-- 3. Save the file in the iota.lib.js folder as `getNode.js` then run it -->

    ```bash
    node getNode.js
    ```

`appName`、`appVersion`、`latestMilestone`、`neighbors`、`tips`などの、ノードに関する統計情報が表示されます。
<!-- You should see statistics about the node, including `appName`, `appVersion`, `latestMilestone`, `neighbors`, `tips`, and more -->

## Sense HATをインストールする
<!-- ## Install Sense HAT -->

1. Sense HatをAstroPiOTAパッケージにインストールします。Sense HATでは、Pythonドライバを手動でインストールする必要があります。
  <!-- 1. Install Sense Hat in the AstroPiOTA package. Sense HAT requires that Python drivers be installed manually: -->

    ```bash
    sudo apt-get update
    sudo apt-get install sense-hat
    sudo pip-3.2 install pillow
    ```

    **メモ：** 詳細については[ドライバのドキュメント](https://pythonhosted.org/sense-hat/)を参照します。
    <!-- **Note:** See the [Driver documentation](https://pythonhosted.org/sense-hat/) for more information -->

2. Sense HATが機能していることを確認するには、python3を開いて次のコマンドを実行します。
  <!-- 2. To make sure that Sense HAT is working, open python3 and do these commands: -->

    ```python
    python3
    >>>from sense_hat import SenseHat
    >>>sense = SenseHat()
    >>>sense.show_message("Hello Sense Hat")
    ```

    "Hello Sense Hat"というメッセージがSense HatのLEDパネルをスクロールします。
    <!-- The message "Hello Sense Hat" will scroll across the Sense Hat led panel. -->

3. ここでユーザフォルダに`node-sense-hat`をインストールします。
  <!-- 3. Now, install node-sense-hat in your user folder -->

    ```bash
    sudo npm install --unsafe-perm --verbose node-sense-hat
    ```

    詳しくは、以下の資料を参照します。
    <!-- For more information see the following resources: -->

    [Sense HAT node library](https://github.com/balena-io-playground/node-sense-hat)

    [sense-hat npm](https://www.npmjs.com/package/sense-hat)


## AstroPiOTAをインストールする
<!-- ## Install AstroPiOTA -->

AstroPiOTAをダウンロードしてインストールします。
<!-- Download and install AstroPiOTA -->

```bash
git clone https://github.com/NelsonPython/AstroPiOTA.git
sudo npm install
```

## AstroPiOTAを実行する
<!-- ## Run AstroPiOTA -->

1. ターミナルウィンドウを開き、`sender.js`スクリプトを実行します。
  <!-- 1. Open a terminal window and execute the sender.js script -->

    ```bash
    npm run sender
    ```

    標準出力にコマンドが表示されます。
    <!-- You should see a command in the output: -->

    ```bash
    npm run receiver ADDRESSXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    ```

2. 出力されたコマンドをコピーして別のターミナルウィンドウにペーストして、MAMストリームを監視します。
  <!-- 2. Copy the output command and paste it into another terminal window to monitor the MAM Stream -->

![Example output from sender.js](../AstroPiOTASender.png)

![Example output from receiver.js](../AstroPiOTAReceiver.png)
