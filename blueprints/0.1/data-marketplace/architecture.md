# アプリケーションアーキテクチャ
<!-- # Application architecture -->

**データマーケットプレイスアプリケーションは、センサー、クラウドバックエンド、およびIOTAタングルを使用してデータをMAMチャネルに公開します。**
<!-- **The Data Marketplace application uses sensors, a cloud backend, and the IOTA Tangle to publish data to MAM channels.** -->

:::warning:免責事項
人間の努力のように、オープンソースプロジェクトを実行することは、不確実性とトレードオフを伴います。以下に説明するアーキテクチャが、同様のシステムを展開するのに役立つことを願っていますが、間違いが含まれている可能性があり、すべての状況に対処することはできません。あなたのプロジェクトについて何か質問があれば、IOTA財団はあなたがあなた自身の研究をし、専門家を探し、そしてIOTAコミュニティとそれらを話し合うことを奨励します。
:::
<!-- :::warning:Disclaimer -->
<!-- Running an open source project, like any human endeavor, involves uncertainty and trade-offs. We hope the architecture described below helps you to deploy similar systems, but it may include mistakes, and can’t address every situation. If you have any questions about your project, we encourage you to do your own research, seek out experts, and discuss them with the IOTA community. -->
<!-- ::: -->

PoCのアーキテクチャー（下の画像）は、センサーまたはその他のデバイス、クラウドバックエンド、および分散型台帳（DLT）が相互接続されているというパターンに従います。
<!-- The architecture of our PoC (shown in the image below) follows a pattern, where the sensor or any other device, cloud backend, and distributed ledger (DLT) are interconnected. -->

![Data Marketplace architecture](../images/data-marketplace-architecture.png)

データマーケットプレイスへのセンサーデータの送信は、組み込みデバイスで実行できる軽量の操作を目的としています。センサーデータを送信するには、デバイスはMAMチャネルの作成と使用、Web APIとの通信などのタングル操作を実行する必要があります。アプリケーションのデータ消費部分はより複雑であり、デバイスのデータストリームへのアクセスと引き換えにIOTAトークンを転送する機能が必要です。そのため、アクセス権管理アドオンが実装されています。
<!-- Submitting sensor data to the Data Marketplace is intended to be a lightweight operation that can be done by embedded devices. To submit sensor data, a device needs to perform Tangle operations, such as producing and consuming MAM channels, and communicating with web APIs. The data consuming part of the application is more complex and needs the ability to transfer IOTA tokens in exchange for access to the device's data streams. Therefore, an access-rights management add-on is implemented. -->

この表には、アプリケーションのすべての主要コンポーネントの一覧が表示されています。
<!-- This table displays a list of all the main components of the application: -->

| **コンポーネント** | **説明** |
| :----------------- | :------- |
| ユーザー認証 | GoogleアカウントでのOAuth、電子メール/パスワードなどの他の種類の認証を有効にすることができます。 API通信およびユーザー識別に使用される固有のAPIトークンおよびユーザーIDを提供します。 |
| データーベース | NoSQLクラウドデータベースは、恒久的なストレージメカニズムを提供します。 |
| クラウドファンクション | イベントまたはAPI呼び出しによってトリガーされるクラウドファンクション（AWSのLambda関数）。壁付資金、デバイスストリーム購入のためのトークン転送などのIOTAタングル関連の操作を実行します。使用済みまたはデバイスの作成、デバイスストリーム購入の管理などのデータベース関連の操作も実行します。 |
| ホスティング | Webポータルのクラウドホスティング。 |
| エラーログ | 問題の調査を簡単にするために、リクエストとレスポンスの詳細を含むエラーログへのアクセスを提供します。 |
| Webポータル | 簡単なデバイス管理とデータストリームの取得を可能にするWeb UI。 |
| API | スクリプトまたはコンソールからすべての主要機能を実行するためのAPIのセット。 |
| データ送信テンプレート | クラウドバックエンドへの事前に入力されたデバイスIDとAPIインターフェースを持つ小型NodeJSプロジェクト。さまざまな手法を使用したセンサーデータ送信の例を示します。 |

アプリケーションのクラウド部分は集中管理されています。 Google Cloud Platform上で動作し、オプションでAmazon AWSまたはMicrosoft Azure上で実行できます。
<!-- The cloud part of the application is centralized. It runs on Google Cloud Platform, and can optionally run on Amazon AWS or Microsoft Azure. -->

集中管理型クラウドバックエンドは、次のコンポーネントで構成されています。
<!-- The centralized cloud backend consists of the following components: -->

- ユーザー認証（GoogleアカウントでのOAuth）
<!-- - User authentication (OAuth with Google account) -->
- ユーザー管理
<!-- - User management -->
- アクセス権管理
<!-- - Access rights management -->
- デバイス管理（作成/読み取り/削除）
<!-- - Device management (create/read/delete) -->
- ウォレット管理（ウォレット資金、トークン転送）
<!-- - Wallet management (wallet funding, tokens transfer) -->
- デバイスストリームの購入追跡
<!-- - Device stream purchase tracking -->
- エラー追跡と報告
<!-- - Error tracking and reporting -->

トランザクションがタングルに添付されたことをノードが確認した後にのみ、ウォレットの資金操作およびトークンの転送操作が完了します。
<!-- Wallet funding and token transfer operations are completed only after the node confirms that the transaction was attached to the Tangle. -->

クラウドファンクションは、以下のデフォルト値を変更するように構成できます。
<!-- Cloud functions can be configured to change the default values of the following: -->

* 通常、IOTA DevnetノードとMainnetノードでは異なる、`depth`フィールドと[`最小重量値`](root://dev-essentials/0.1/concepts/minimum-weight-magnitude.md)フィールド。
<!-- * `depth` and [`minWeightMagnitude`](root://dev-essentials/0.1/concepts/minimum-weight-magnitude.md) fields, which are typically different for IOTA Devnet and Mainnet nodes -->
* 事前定義された電子メールアドレスを持つ管理者ユーザーが、デバイスを管理することを許可しているホワイトリストページ。
<!-- * Whitelist page, where administrator users with predefined email addresses are allowed to administer devices -->

## センサーとデータ
<!-- ## Sensors and data -->

データマーケットプレイスは、センサーとそれに接続するデータを認識しません。データを送信する機能を持ち、APIなどを介して定期的にデータを読み取るための簡単な方法を備えているセンサーであれば、データマーケットプレイスで使用できます。マーケットプレイスにデータを送信するには、スクリプトを実行してデータをIOTAタングルに送信するだけです。
<!-- The Data Marketplace is agnostic to the sensors and the data that you connect to it. Any sensor that has the ability to transmit data and has an easy way to get regular data readings, such as through an API, can be used with the Data Marketplace. In order to submit the data to the Marketplace, all you have to do is execute a script to submit data to the IOTA Tangle. -->

### センサーの例
<!-- ### Example sensors -->

* [Netatmo Weather Station](https://www.netatmo.com/en-us/weather)
* [Bosch XDK](https://xdk.bosch-connectivity.com/)
* [Nordic Semiconductor Thingy:52](https://www.nordicsemi.com/Software-and-Tools/Development-Tools/Nordic-Thingy-52-App)
* [Raspberry Pi with a sensor kit](https://www.adafruit.com/product/2733)

### データフィールド
<!-- ### Data fields -->

センサーごとに正しいデータフィールドを定義する必要があります。これらのデータフィールドはタングルに保存され、購入者のデータマーケットプレイスWebポータルに表示されます。一般的に、保存し、売却したいデータを説明するだけにします。
<!-- For each of your sensors, you have to define the correct data fields, which will be stored on the Tangle, and displayed on the Data Marketplace web portal for the purchaser. In general, just be descriptive with the data that you want to store and sell. -->
