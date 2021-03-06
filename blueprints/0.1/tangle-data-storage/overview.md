# タングルデータストレージ概要
<!-- # Tangle data storage overview -->

**データは，官民組織のイノベーションを促進し，新たな収益源を生み出すために使用することができます．しかし，ベンダーに依存しないデータ検証方法がなければ，データを信頼することはできません．この設計図では，IOTA タングルをイミュータブルなデータ構造として使用し，サードパーティのストレージソリューション内のファイルの内容を検証しています．**
<!-- **Data boosts innovation for public and private sector organisations, and can be used to create new revenue streams. However, without a vendor-neutral way of verifying data, it can't be trusted. This blueprint uses the IOTA Tangle as an immutable data structure to verify the contents of a file in a third-party storage solution.** -->

## ビジネスケース
<!-- ## Business case -->

データの保存，共有，取引は，接続された世界のビジネスの基盤であり，公共および民間部門の組織の革新を後押しします．ただし，データが変更されていないことを信頼できない場合，データはほとんど役に立たなくなります．IOTA タングルを使用することにより，企業と接続されたマシンは，共有するデータのセキュリティとプライバシーを信頼できます．
<!-- Storing, sharing and trading data is a cornerstone of business in the connected world and boosts innovation for public and private sector organisations. However, if you can't trust that the data has not been altered, then the data becomes nearly useless. By using the IOTA Tangle, businesses and connected machines can trust in the security and privacy of the data they share. -->

### 課題
<!-- ### Challenges -->

ほとんどすべてのユースケースまたはアプリケーションに共通の要件は，後で使用するためにデータを保存する必要があることです．次に例を示します．
<!-- A common requirement for nearly all use cases or applications is that data must be stored for later use, for example: -->

- **サプライチェーン：**配送文書または画像は，イベントの永久記録を形成するために保存する必要があります．
<!-- - **Supply chain:** Shipping documents or images must be stored to form a permanent record of events -->
- **車両のデジタルツイン：**走行距離計の読み取り値，メンテナンス記録，所有権の変更などのすべてのデータを保存して，車両のライフサイクル全体の恒久的な記録を作成する必要があります．
<!-- - **Digital twin of a vehicle:** All data such as the odometer readings, maintenance record, and ownership change should be stored to form a permanent record of the entire lifecycle of the vehicle -->

しかし，タングルはデータベースではありません． タングルはイミュータブルな分散型台帳で，データのハッシュ値とサードパーティのデータストレージに保存したデータへのポインタを保存できます．
<!-- However, the Tangle is not a database. The Tangle is an immutable, distributed ledger which allows you to store a hash of data and a pointer to its location in third-party data storage. -->

:::info:
タングルに永続的に保存することは現在不可能ですが，IOTA 財団は[クロニクル](root://node-software/0.1/chronicle/introduction/overview.md)と呼ばれるソリューションを開発しています．
:::
<!-- :::info: -->
Although permanent storage on the Tangle isn't currently possible, the IOTA Foundation are developing a solution for it called [Chronicle](root://node-software/0.1/chronicle/introduction/overview.md).
<!-- ::: -->

### 解決策
<!-- ### Solution -->

生成源でドキュメントの暗号化ハッシュ値を作成し，それをタングルに保存することで，データが変更されていないことを証明することができます．さらに，データを非公開にして，許可されたユーザーだけが閲覧できるようにしたり，必要に応じてこの許可を取り消したりすることもできます．標準的な暗号化方法を使用することで，データを保護し，対応する復号化キーを持つ人だけが見ることができます．
<!-- By creating a cryptographic hash of the document at the source and storing the hash in the Tangle, you have a method of proving that the data is unchanged. In addition, you may also want the data to remain private and allow only authorized users to view it as well as be able to revoke this authorization when needed. By using any standard encryption method, the data can be secured and only visible to those with the corresponding decryption key. -->

この設計図で説明しているように，タングルとサードパーティのデータストレージソリューションを一緒に使用することで，データをすべてタングルに添付せずに，データの不変の安全性とタイムスタンプを得ることができます．
<!-- When you use the Tangle with a third-party data storage solution as described in this blueprint, your data is immutably secured and timestamped, without having to attach all of it to the Tangle. -->

## デモ
<!-- ## Demo -->

アプリケーションのデモについては，[この Web サイト](https://ipfs.iota.org/)を参照してください．
<!-- See this website for a [demonstration of the application](https://ipfs.iota.org/). -->

## 追加のリソース
<!-- ## Additional Resources -->

---------------
#### GitHub リポジトリ ####
[Link](https://github.com/iotaledger/poc-ipfs)

この設計図をテストするには，コードとクイックスタートの手順を読んでください．
<!-- Read the code and some quickstart instructions to test this blueprint. -->
---
#### iota.js クライアントライブラリ ####
[Link](root://client-libraries/0.1/getting-started/js-quickstart.md)

iota.js クライアントライブラリを使ってトランザクションを作成，送信，受信する方法を学びます．
<!-- Learn how to use the iota.js client library to create, send, and receive transactions. -->
---
#### IPFS ####
[Link](https://ipfs.io/)

この設計図は，IPFS にファイルを保存してから，イミュータブルで永続的な IPFS リンクをタングルにアタッチします．これにより，ファイル全体をタングルに載せなくてもコンテンツを保護できます．
<!-- This blueprint stores files on IPFS, then attaches the immutable, permanent IPFS links to the Tangle. This secures your content, without having to put the whole file on the Tangle. -->
---------------
