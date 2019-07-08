# データマーケットプレイスアプリをデプロイしてテストする
<!-- # Deploy and test the Data Marketplace app -->

**データマーケットプレイスでデータストリームの売買を開始するには、自身のアプリケーションをデプロイするか、デモアプリケーションでテストします。**
<!-- **To start buying and selling data streams on the Data Marketplace, deploy your own application, or test it in our demo app.** -->

## 前提条件
<!-- ## Prerequisites -->

センサーデータをデータマーケットプレイスに送信するには、以下が必要です。
<!-- To submit sensor data to the Data Marketplace, you must have the following: -->

### ハードウェア
<!-- ### Hardware -->

* 次のようなデータを生成するセンサーまたはデバイス
    <!-- * A sensor or device that generates data such as the following: -->
    * [Netatmo Weather Station](https://www.netatmo.com/en-us/weather)
    * [Bosch XDK](https://xdk.bosch-connectivity.com/)
    * [Nordic Semiconductor Thingy:52](https://www.nordicsemi.com/Software-and-Tools/Development-Kits/Nordic-Thingy-52)
    * [Raspberry Pi with a sensor kit](https://www.adafruit.com/product/2733)

### ソフトウェア
<!-- ### Software -->

* [データマーケットプレイス](https://github.com/iotaledger/data-marketplace)
<!-- * [Data Marketplace](https://github.com/iotaledger/data-marketplace) -->

* [Node.js](https://nodejs.org/)

* [MAM（マスク認証メッセージング）](https://github.com/iotaledger/mam.client.js)
<!-- * [MAM (masked authenticated messaging)](https://github.com/iotaledger/mam.client.js) -->

次のクラウドサービスのいずれかを選択してください。
<!-- Choose from one of the following cloud services: -->

* Google Cloud
    * [Web server](https://firebase.google.com/docs/hosting/)
    * [Authentication service](https://firebase.google.com/docs/auth/)
    * [NoSQL database](https://firebase.google.com/docs/firestore/)
    * [Background tasks and API server](https://firebase.google.com/docs/functions)

* Amazon
    * [Web server](https://aws.amazon.com/s3/)
    * [API server](https://aws.amazon.com/api-gateway/)
    * [NoSQL database](https://aws.amazon.com/dynamodb/)
    * [Background tasks](https://aws.amazon.com/lambda/)

* Azure
    * [Web server](https://azure.microsoft.com/en-us/services/storage/)
    * [API server](https://azure.microsoft.com/en-us/services/app-service/)
    * [NoSQL database](https://azure.microsoft.com/en-us/services/cosmos-db/)
    * [Background tasks](https://azure.microsoft.com/en-us/services/functions/)

### プログラミング言語
<!-- ### Programming knowledge -->

* JavaScript/TypeScript
* HTML/CSS
* [Reactフレームワーク](https://github.com/facebook/create-react-app)
<!-- * [React framework](https://github.com/facebook/create-react-app) -->
* 必要なサードパーティライセンス
<!-- * Required third-party licenses -->
* クラウドサービス
<!-- * Cloud service -->

### IOTA知識
<!-- ### IOTA knowledge -->

MAMチャンネルについての理解
<!-- An understanding of MAM channels. -->

## データマーケットプレイスアプリをデプロイする
<!-- ## Deploy the Data Marketplace app -->

自身のデータマーケットプレイスを展開するには、次のブログ投稿の手順に従ってください。
<!-- To deploy your own data marketplace, follow the instructions in these blog posts: -->

1. https://medium.com/@lexerr/47b608c527c9
2. https://medium.com/@lexerr/b33d9856c852

## データマーケットプレイスアプリをテストする
<!-- ## Test the Data Marketplace app -->

自身のデータマーケットプレイスを展開する代わりに、自身のデバイスを追加して当社のデモアプリケーションをテストすることができます。
<!-- Instead of deploying your own data marketplace, you can test our demo app by adding your own device to it. -->

1. [ダッシュボードページ](https://data.iota.org/#/dashboard)にアクセスしてログインします。Google OAuthログイン画面が表示されます。**Googleでサインイン**をクリックします。
  <!-- 1. Go to the [Dashboard page](https://data.iota.org/#/dashboard) to log in. Here you will see a Google OAuth login screen. Click **Sign in with Google** -->

2. 新しいデバイスを作成します。
  <!-- 2. Create a new device -->

    :::info:
    デフォルト設定では、アカウントごとに最大5つのデバイスを作成できます。この数はリクエストに応じて調整することができます。
    :::
    <!-- :::info: -->
    <!-- You can create up to 5 devices per account with the default settings. This number can be adjusted upon request. -->
    <!-- ::: -->

3. お使いのデバイスに関する基本情報を入力します。収集したいデータストリームに関連するフィールドを必ず入力してください。
<!-- 3. Enter some basic information about your device. Make sure you complete the fields relating to the data streams you would like to collect. -->

    * **デバイスID：** このIDは既存のすべてのセンサー間で一意である必要があり、データストリームを購入するときのアクセスキーとして機能します。IDは必ず英字（a-z）で始まり、その後に任意の数の英字、数字（0-9）、ハイフン（"-"）、および下線（"_"）を続けることができます。最大許容長は40文字に設定されています。
    <!-- * **Device ID**: This ID should be unique among all existing sensors and will serve as access key when purchasing a data stream. Please note that the ID must begin with a letter ([a-z]) and may be followed by any number of letters, digits ([0–9]), hyphens (“-”), and underscores (“_”). Max allowed length is set to 40 characters. -->
    * **デバイスタイプ：** プレーンテキストタイプの説明。これは、他のデータマーケットプレイス参加者がセンサーのデータストリームの使用の可能性を識別するのに役立ちます。典型的な例：「気象ステーション」、追跡装置」、充電ステーション」。
    <!-- * **Device Type**: Plain text type description, that helps other Data Marketplace participants identify potential usage of the sensors’ data stream. Typical examples: “weather station”, “tracking device”, “charging station”. -->
    * **カンパニー：** センサーを所有および管理している会社の名前を入力します。この情報は、センサーの所有者とデータストリームの購入に関心のあるデータマーケットプレイスユーザーとの間の信頼関係を高めます。
    <!-- * **Company**: Provide the name of the company that owns and maintains the sensor. This information creates more trust between sensor owners and Data Marketplace users who are interested in purchasing the data stream. -->
    * **場所（市/国）：** この情報は、センサーデータが購入に値するかどうかを識別するのに役立ちます。たとえば、一部のユーザーは、特定の地域にある気象センサーや環境センサーからデータストリームを購入することに関心があるかもしれません。
    <!-- * **Location (city / country)**: This information helps to identify if the sensor data is relevant for purchasing. For example, some users might be interested in purchasing data streams from weather or environmental sensors located in a specific region. -->
    * **GPS座標（経度/緯度）：** https://www.gps-coordinates.net/ などのサービスを使用して、センサーに適したGPSデータを取得できます。負の値の場合はハイフン（"-"）と小数点記号（"."）を先頭に付けることができます。文字や特殊文字を入力しないでください。
    <!-- * **GPS Coordinates (latitude / longitude)**: You can use a service like https://www.gps-coordinates.net/ to get the right GPS data for your sensor. We accept coordinates as a number of digits ([0–9]), that could be prepended by a hyphen (“-”) for negative value, and a decimal separator (“.”). Please do not enter any letters or special characters. -->
    * **データストリームの値段：** ここでセンサーデータのコストを定義できます。価値の定義を制限するわけではありませんが、プレビューや払い戻しのオプションがなければ、ごく少数の顧客だけがデータストリームのためにお金を払う準備ができていることに注意してください。一方、非常に低い価格を設定すると、合計1000分の1セント未満の支払総額が発生する可能性があります。1Miの[現在の価格](https://coinmarketcap.com/currencies/iota/)である1,000,000（100万）IOTAトークンを確認することをお勧めします。センサーのコストと保守作業に応じて、1,000iから50,000iの範囲内で価格を設定することをお勧めします。
    <!-- * **Price of the data stream**: Here you can define the cost of the sensor data. We do not restrict you with defining the value, but please keep in mind that without a preview and refund options, only a few customers will be ready to pay a fortune for a data stream. On the other hand, setting a very low price might result in a total collected payment under 1/1000 of a cent. We encourage you to check the [current price](https://coinmarketcap.com/currencies/iota/) of 1Mi, which is 1000000 (one million) IOTA tokens. We suggest that you set the price within the range of 1000i to 50000i, depending on the cost and maintenance efforts of the sensor. -->
    * **データフィールド：** センサー構成の最も重要な部分。センサーによってキャプチャされ、タングルに保存されるすべてのパラメータに関する情報を入力してください。右側の**+**と**X**ボタンを押すと、フィールドを追加または削除できます。
    <!-- * **Data Fields**: The most essential part of the sensor configuration. Please provide information for every parameter that will be captured by the sensor and stored on the Tangle. You can add or remove fields by pressing the **+** and **X** buttons on the right. -->

    パラメータ情報は3つのフィールドで構成されています。
    <!-- Parameter information consist of 3 fields: -->

    * **フィールドID：** このIDは特定のセンサーのために他のすべてのパラメータの中で一意であるべきです。IDは必ず英字（[a-z]）で始まり、その後に任意の数の英字、数字（[0-9]）、ハイフン（"-"）、および下線（"_"）を続けることができます。最大許容長は40文字に設定されています。
    <!-- * **Field ID**: This ID should be unique among all other parameters for the specific sensor. Please note that the ID must begin with a letter ([a-z]) and may be followed by any number of letters, digits ([0–9]), hyphens (“-”), and underscores (“_”). Max allowed length is set to 40 characters. -->
    * **フィールド名：** フィールドの目的を示すプレーンテキストのパラメータの説明。最大許容長は40文字に設定されています。
    <!-- * **Field Name**: Plain text parameter description that indicates the purpose of the field. Max allowed length is set to 40 characters. -->
    * **フィールド単位：** 主に[国際単位系](https://whatis.techtarget.com/reference/Table-of-Physical-Units)（SI）における[物理量](https://en.wikipedia.org/wiki/List_of_physical_quantities)および[単位](https://whatis.techtarget.com/reference/Table-of-Physical-Units)の略語。湿度、大気質、降水量、風速、風向などの[環境測定](https://www.fondriest.com/environmental-measurements/parameters/)単位でもあります。このフィールドの長さは20文字以内にしてください。
    <!-- * **Field Unit**: [Physical quantities](https://en.wikipedia.org/wiki/List_of_physical_quantities) and [units](https://whatis.techtarget.com/reference/Table-of-Physical-Units) abbreviation, primarily in the International [System of Units](https://whatis.techtarget.com/reference/Table-of-Physical-Units) (SI). It can also be a unit of [environmental measurements](https://www.fondriest.com/environmental-measurements/parameters/), like humidity, air quality, precipitation, wind speed or direction. Please keep the length of this field within 20 characters. -->

4. **Download Publish Script**をクリックして、タングルにデバイスデータを保存する方法の例とドキュメントが含まれている事前設定された.zipアーカイブをダウンロードします。
  <!-- 4. Click **Download Publish Script** to download of a preconfigured .zip archive that contains examples and documentation on how to store data in the Tangle for your device -->

5. アーカイブの内容を抽出し、[Visual Studio Code](https://code.visualstudio.com/Download)などのコードエディタでフォルダを開いてスクリプトの操作を開始します。
  <!-- 5. Extract the content of the archive, and open the folder in a code editor such as [Visual Studio Code](https://code.visualstudio.com/Download) to start working with the script -->

    :::info:
    スクリプトを使い始める前にREADME.mdファイルを読んでください。
    :::
    <!-- :::info: -->
    <!-- Read the README.md file before you start using the script. -->
    <!-- ::: -->

    スクリプトは、選択したデバイスのデータを公開するように事前設定されています。 config.jsonファイルにセンサーIDとその秘密鍵があります。
    <!-- The script is pre-configured to publish data for the selected device. You’ll find the sensor ID and it’s secret key in the config.json file. -->

    複数のセンサーに同じスクリプトを使用する場合は、秘密鍵も変更する必要があります。そうしないと、公開したデータを復号化することができません。
    <!-- If you decide to use the same script for multiple sensors, please note that the secret key should be changed as well. Otherwise you won’t be able to decrypt your published data. -->

    デフォルトでは、スクリプトはデバッグモードで実行されます。つまり、データは公開されません。キャプチャされたデータはすべてコンソールに出力されるので、確認して調整することができます。ペイロードが良好に見えたら、config.jsonファイルでdebug変数を`false`に設定してデバッグモードを無効にし（下記のスクリーンショットを参照）、データを公開することができます。
    <!-- By default the script runs in debug mode, which means that no data is published. All captured data is printed out in the console, so you can verify and adjust it. Once the payload looks good, you can disable debug mode by setting the debug variable to false in the config.json file (see screenshot below), and let data be published. -->

    Proof-of-Work操作はすべてのデータパッケージに対して実行されることに注意してください。これはハードウェアによっては最大60秒かかる場合があります。これを考慮に入れて、この操作の実行中にスクリプトを中断しないでください。
    <!-- Please note that the Proof-of-Work operation is conducted for every data package, which might take up to 60 seconds depending on your hardware. Please take this into account and do not interrupt the script while running this operation. -->

6. IOTAタングルへのセンサーデータの公開についての詳細は、[ブログの投稿](https://blog.iota.org/the-iota-data-marketplace-a-tech-intro-part-3-eea5cbcd1eb7)を参照してください。
  <!-- 6. See the [blog post](https://blog.iota.org/the-iota-data-marketplace-a-tech-intro-part-3-eea5cbcd1eb7) to learn more about publishing sensor data to IOTA Tangle -->

### 新しいウォレット作成して資金を供給する
<!-- ### Create and fund a new wallet -->

無料のIOTAトークンをデバイスのウォレットに供給することができます。通常、100,000iから1,000,000iのDevnetトークンを無料で新しいデバイスのウォレットに転送します。
<!-- You can fund a device's wallet with free IOTA tokens. We usually transfer between 100000i and 1000000i of Devnet tokens for free to your new device's wallet. -->

:::info:
DevnetトークンをMainnetで使用したり、暗号通貨の取引所で交換したりすることはできません。
:::
<!-- :::info: -->
<!-- Devnet tokens can't be used on the Mainnet or exchanged on any cryptocurrency exchange. -->
<!-- ::: -->

1. データストリームを購入したいセンサーまたはデバイスを選択します。
  <!-- 1. Select a sensor or device for which you want to purchase a data stream -->

2. センサーページの右上に、自分のウォレットに資金を供給するためのボタンがあります。
  <!-- 2. Go to the Sensor page, in the top right corner you will find a button to fund your wallet -->

ページを更新したり、アプリケーションの別のページに移動したりして、この操作を中断しないでください。IOTAトークンの転送が完了すると、ウォレットの残高は自動的に更新されます。
<!-- Please do not interrupt this operation by refreshing the page or navigating to another page of the application. Once IOTA token transfer is completed, the balance of the wallet is automatically updated. -->

あるいは、[ウォレットに資金を供給するためにAPIを使う](https://data.iota.org/static/docs#create-and-fund-wallet)ことができます。
<!-- Alternatively, you can [use the API to fund a wallet](https://data.iota.org/static/docs#create-and-fund-wallet). -->

### 既存のウォレットに資金を供給する
<!-- ### Fund an existing wallet -->

ウォレットの残高が少ない場合は、IOTA蛇口を使用してより多くのDevnetトークンをウォレットに供給することができます。
<!-- If your wallet balance is low, you can fund it with more Devnet tokens by using the IOTA faucet. -->

:::info:
Devnetトークンは、Mainnetで使用したり、暗号通貨の取引所で交換したりすることはできません。
:::
<!-- :::info: -->
<!-- Devnet tokens can't be used on the Mainnet or exchanged on any cryptocurrency exchange platform. -->
<!-- ::: -->

IOTA蛇口ページに移動し、ウォレットアドレスを入力します。これは、[`getUser`](https://data.iota.org/static/docs#get-user)エンドポイントを呼び出すことで取得できます。またキャプチャを解決する必要があります。
<!-- Go to the Faucet page, enter your wallet address, that you can get by calling the [`getUser`](https://data.iota.org/static/docs#get-user) endpoint. You also need to solve the captcha. -->

### データストリームをクエリーする
<!-- ### Query a data stream -->

データストリームを購入すると、タングルのMAMチャンネルに公開されているデータをクエリできます。
<!-- When you buy a data stream you can query the data that's published to the MAM channel on the Tangle. -->

1. センサーページに行きます。
  <!-- 1. Go to the Sensor page -->

    すでにストリームを購入したことがある場合、データは自動的にまとまって読み込まれます（リクエストごとに20パケット）。
    <!-- If you've already purchased the stream, data will be automatically loaded in chunks (20 packets per request). -->

2. ページの一番下までスクロールすると、次の20個のパケットが自動的にクエリーされます。
  <!-- 2. Scroll to the bottom of the page to automatically query the following 20 packets. -->

あるいは、[APIを使用してデータストリームをクエリする](https://data.iota.org/static/docs#query-stream)こともできます。
<!-- Alternatively, you can [use the API to query a data stream](https://data.iota.org/static/docs#query-stream). -->

### データストリームを購入する
<!-- ### Buy a data stream -->

ウォレットの残高を使ってデバイスのデータストリームを購入することができます。データストリームを購入すると、MAMチャンネルのルートへのアクセス権が付与されるので、タングルでデータを見つけることができます。
<!-- You can buy a device's data stream using the balance of the wallet. When you buy a data stream, you're given access to the MAM channel's root so that you can find the data on the Tangle. -->

1. センサーページに行きます。
<!-- 1. Go to the Sensor page -->

    ストリームを購入していない場合は、デバイスの所有者が設定した金額でデータストリームを購入するかどうかを尋ねるメッセージが表示されます。 Webポータルからデバイスデータストリームを購入すると、ウォレットの残高は自動的に更新されます。
    <!-- If you have not purchased the stream, a message appears asking whether you would like to purchase the data stream for an amount, that was set by the device owner. When you purchase device data stream from the web portal, your wallet balance is automatically updated. -->

あるいは、[APIを使用してデータストリームを購入する](https://data.iota.org/static/docs#query-stream)こともできます。
<!-- Alternatively, you can [use the API to buy a data stream](https://data.iota.org/static/docs#purchase-stream). -->

### デバイスを削除する
<!-- ### Remove a device -->

デバイスをデータマーケットプレイスに表示したくない場合は、削除できます。
<!-- If you no longer want your device to be visible on the Data Marketplace, you can remove it. -->

1. [ダッシュボード](https://data.iota.org/#/dashboard)ページに行きます。
  <!-- 1. Go to the [Dashboard](https://data.iota.org/#/dashboard) page -->

2. デバイスカードの右上隅にある**X**を押します。
  <!-- 2. Press **X** in the top-right corner of the device card -->

    デバイスに関連付けられているシードも削除されることに注意してください。つまり、デバイスのシードに転送された資金を取り出すことができなくなります。
    <!-- Be aware that the seed associated with the device is also removed. That means that you can no longer withdraw funds that were transferred to the device's seed. -->

あるいは、[APIを使用してデバイスを削除する](https://data.iota.org/static/docs#query-stream)こともできます。
<!-- Alternatively, you can [use the API to remove a device](https://data.iota.org/static/docs#remove-device). -->
