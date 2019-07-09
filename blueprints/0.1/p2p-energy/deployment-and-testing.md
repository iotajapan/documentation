# P2Pエネルギーグリッドアプリをデプロイする
<!-- # Deploy the P2P energy grid app -->

**P2Pエネルギーグリッドアプリケーションの使用を開始するには、自身のアプリケーションをデプロイしてください。**
<!-- **To start using the P2P energy grid app, deploy your own application.** -->

## 前提条件
<!-- ## Prerequisites -->

P2Pエネルギーグリッドアプリケーションをデプロイするには、次のものが必要です。
<!-- To deploy the P2P energy grid app, you must have the following: -->

### ハードウェア
<!-- ### Hardware -->

* 電力源、生産者、および消費者はRaspberry Pis（ラズパイ）で実行できます
<!-- * The sources, producers, and consumers can run on Raspberry Pis (RPi) -->
* グリッドは、Web API、データベースストレージ、およびバックグラウンドタスクが可能なサーバーで実行する必要があります。
<!-- * The grid must be run on a server that's capable of web APIs, database storage and background tasks -->

### ソフトウェア
<!-- ### Software -->

* [Node.js](https://nodejs.org/)

* [MAM（マスク認証メッセージング）](https://github.com/iotaledger/mam.client.js)
<!-- * [MAM (masked authenticated messaging)](https://github.com/iotaledger/mam.client.js) -->

次のいずれかのクラウドサービスまたはローカルサーバーから選択します。
<!-- Choose from one of the following cloud services or a local server: -->

* Amazon
    * [Webサーバー](https://aws.amazon.com/s3/)
    <!-- * [Web server](https://aws.amazon.com/s3/) -->
    * [APIサーバー](https://aws.amazon.com/api-gateway/)
    <!-- * [API server](https://aws.amazon.com/api-gateway/) -->
    * [NoSQLデーターベース](https://aws.amazon.com/dynamodb/)
    <!-- * [NoSQL database](https://aws.amazon.com/dynamodb/) -->
    * [バックグラウンドタスク](https://aws.amazon.com/lambda/)
    <!-- * [Background tasks](https://aws.amazon.com/lambda/) -->

* ローカルサーバー
<!-- * Local server -->
    * ストレージ：ローカルファイルシステム
    <!-- * Storage: local file system -->
    * Webサーバー：Nginx
    <!-- * Web server: Nginx -->
    * APIサーバー：Node.js（Express）
    <!-- * API server: Node.js with Express -->
    * NoSQLデーターベース：MongoDB
    <!-- * NoSQL database: MongoDB -->
    * バックグラウンドタスク：Node.jsタスクを実行するタスクスケジューラ
    <!-- * Background tasks: Task scheduler running Node.js tasks -->

### プログラミング言語
<!-- ### Programming knowledge -->

* JavaScript/TypeScript
* HTML/CSS
* [Reactフレームワーク](https://github.com/facebook/create-react-app)
<!-- * [React framework](https://github.com/facebook/create-react-app) -->
* 必要なサードパーティライセンス
<!-- * Required third-party licenses -->
* クラウドサービス
<!-- * Cloud services -->

### IOTA知識
<!-- ### IOTA knowledge -->

MAMチャネルの理解
<!-- An understanding of MAM channels. -->

## P2Pエネルギーグリッドアプリをデプロイする
<!-- ## Deploy the P2P energy grid app -->

デプロイの手順は[GitHubリポジトリ](https://github.com/iotaledger/poc-p2p-energy)に文書化されています。
<!-- The deployment instructions are documented in the [GitHub repository](https://github.com/iotaledger/poc-p2p-energy). -->

:::info:
展開手順は5月に公開される予定です。
:::
<!-- :::info: -->
<!-- The deployment instructions will be available in May. -->
<!-- ::: -->

ラズパイ上で実行されている電力源には以下が必要です。
<!-- Sources running on RPi need the following: -->
* グリッドへのローカルエリアネットワーク（LAN）またはワイドエリアネットワーク（WAN）接続
<!-- * Local area network (LAN) or wide area network (WAN) connection to the grid -->
* IOTAノードへの接続（内蔵することもできます）
<!-- * Connection to an IOTA node (can be internal) -->
* [Node.js](https://github.com/audstanley/NodeJs-Raspberry-Pi)

ラズパイ上で実行されている生産者には以下が必要です。
<!-- Producers running on RPi need the following: -->
* グリッドへのローカルエリアネットワーク（LAN）またはワイドエリアネットワーク（WAN）接続
<!-- * Local area network (LAN) or wide area network (WAN) connection to the grid -->
* IOTAノードへの接続（内蔵することもできます）
<!-- * Connection to an IOTA node (can be internal) -->
* [Node.js](https://github.com/audstanley/NodeJs-Raspberry-Pi)

ラズパイ上で実行されている消費者には以下が必要です。
<!-- Consumers running on RPi need the following: -->
* グリッドへのローカルエリアネットワーク（LAN）またはワイドエリアネットワーク（WAN）接続
<!-- * Local area network (LAN) or wide area network (WAN) connection to the grid -->
* IOTAノードへの接続（内蔵することもできます）
<!-- * Connection to an IOTA node (can be internal) -->
* [Node.js](https://github.com/audstanley/NodeJs-Raspberry-Pi)

クラウドまたはローカルサーバー上で実行されているグリッドには、次のものが必要です。
<!-- Grids running in the cloud or on a local server need the following: -->
* WAN接続
<!-- * WAN connection -->
* IOTAノードへの接続
<!-- * Connection to an IOTA node -->
