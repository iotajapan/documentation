# IOTA概要
<!-- # IOTA overview -->

**IOTAは、[接続されたデバイス](https://en.wikipedia.org/wiki/Connected_Devices)が無料でデータと[IOTAトークン](../clients/token.md)を相互に転送できるようにするオープンソースの分散型台帳技術です。**
<!-- **IOTA is an open-source distributed ledger technology that allows [connected devices](https://en.wikipedia.org/wiki/Connected_Devices) to transfer data and [IOTA tokens](../clients/token.md) among each other for zero fees.** -->

<iframe src="https://www.youtube.com/embed/Gr-LstcDcAw" frameborder="0" width="560" height="315" style="max-width: 100%;"></iframe>

## IOTAはどのように機能するのか？
<!-- ## How does IOTA work? -->

IOTAネットワークは[ノード](../network/nodes.md)とクライアントで構成され、ユーザーは[パブリックIOTAネットワーク](../network/iota-networks.md)の中でノードとしてもクライアントとしても自由に行動できます。
<!-- An IOTA network consists of [nodes](../network/nodes.md) and clients, and anyone is free to act as either in the [public IOTA networks](../network/iota-networks.md). -->

### ノード
<!-- ### Nodes -->

ノードは、タングルと呼ばれるトランザクションのイミュータブルな記録への読み取りおよび書き込みアクセス権を持つ唯一のデバイスであるため、IOTAネットワークのバックボーンです。
<!-- Nodes are the backbone of an IOTA network as they are the only devices that have read and write access to the immutable record of transactions called the Tangle. -->

相互接続されたノードは、同じ[ノードソフトウェア](root://node-software/0.1/introduction/overview.md)を実行することでIOTAネットワークを形成し、トランザクションを検証してタングルに接続できるようにします。
<!-- Interconnected nodes form an IOTA network by running the same [node software](root://node-software/0.1/introduction/overview.md), allowing them to validate transactions and attach them to the Tangle. -->

### クライアント
<!-- ### Clients -->

クライアントは、タングルでデータを処理または保存するためにノードに接続するデバイスです。
<!-- Clients are the devices that connect to nodes to transact or store data on the Tangle. -->

IOTAネットワーク内のすべてのクライアントには、[シード](../clients/seeds.md)と呼ばれる秘密のパスワードがあり、これがIDとして機能します。シードは、クライアントに[アドレス](../clients/addresses.md)へのアクセス権を提供します。アドレスは、IOTAトークンの残高を持つアカウントのようなものです。
<!-- All clients in an IOTA network have a secret password called a [seed](../clients/seeds.md), which acts as their identity. Seeds give clients access to [addresses](../clients/addresses.md), which are like accounts with a balance of IOTA tokens. -->

IOTAトークンを転送したり、データのみを送信したりするために、クライアントは転送命令を[トランザクション](../transactions/transactions.md)と呼ばれるオブジェクトにパッケージ化し、それらをノードに送信してタングルにアタッチします。
<!-- To transfer IOTA tokens or even to send only data, clients package the transfer instructions into objects called [transactions](../transactions/transactions.md) and send them to a node to attach to the Tangle. -->

## IOTAを使うメリット
<!-- ## Why should I use IOTA? -->

IOTAを使用することにより、次のメリットを享受するアプリケーションを構築できます。
<!-- By using IOTA, you can build applications that benefit from the following: -->

- **信頼性：**データを送信したことや、IOTAトークンの所有権を証明できます。
<!-- - **Authenticity:** Prove that you sent data and/or own IOTA tokens -->
- **整合性：**データが変更されていないことを証明できます。
<!-- - **Integrity:** Prove that your data is unchanged -->
- **機密性：**暗号化によりデータにアクセスできるユーザーを制御できます。
<!-- - **Confidentiality:** Control who has access to your data through encryption -->
- **マイクロペイメント：**手数料を支払うことなく、少量のIOTAトークンを送信できます。
<!-- - **Micropayments:** Send small amounts of IOTA tokens without paying any fees -->

IOTAネットワークには、データや価値を転送する従来の方法と比較して多くの利点があります。
<!-- IOTA networks have many benefits compared to traditional ways of transferring data or value. -->

### 信頼性
<!-- ### Trust -->

IOTAネットワーク内の各ノードは、トランザクションを検証し、同じことを行う他のノードにトランザクションを送信します。その結果、すべての有効なトランザクションはすべてのノードによって合意され、ネットワーク内の単一のトランザクションを信頼する必要がなくなります。
<!-- Each node in an IOTA network validates transactions, then sends them to other nodes that do the same. As a result, all valid transactions are agreed on by all nodes, removing the need to trust a single one in the network. -->

自分自身のノードを実行して、ネットワーク内のトランザクションの検証を開始することもできます。
<!-- You can even run your own node to start validating transaction in the network. -->

### イミュータビリティ
<!-- ### Immutability -->

タングル内のすべてのトランザクションはイミュータブルで透過的です。
<!-- All transactions in the Tangle are immutable and transparent. -->

各トランザクションは、前の2つのトランザクションハッシュを参照します。そのため、任意のトランザクションの内容が変更されると、ハッシュが無効になり、トランザクションが無効になります。
<!-- Each transaction references the transaction hashes of two previous ones. So, if the contents of any transaction were to change, the hashes would be invalid, making the transactions invalid. -->

### 安全性
<!-- ### Security -->

IOTAは、量子ロバストなワンタイム[署名](../clients/signatures.md)を使用して、攻撃者がIOTAトークンを盗むのを防ぎます。
<!-- IOTA uses quantum-robust one-time [signatures](../clients/signatures.md) to stop attackers from stealing IOTA tokens. -->

IOTAネットワークは、中央当局がタングルを制御しないP2Pネットワークです。代わりに、すべてのノードがタングルのコピーを保持し、その内容について合意に達します。
<!-- IOTA networks are peer-to-peer networks where no central authority controls the Tangle. Instead, all nodes hold a copy of it and reach a consensus on its contents. -->

### 経費節約
<!-- ### Cost saving -->

IOTAは無料で使用できます。サブスクリプションを支払う必要も、契約に署名する必要もありません。トランザクションも無料です。
<!-- IOTA is free to use. You don't need to pay a subscription, or sign a contract. Even transactions are feeless. -->

制限なしでタングルにデータを保存できます。必要なのは、トランザクションを送信できるノードだけです。
<!-- You can store data on the Tangle with no restrictions. All you need is a node to which you can send transactions. -->

### スケーラビリティ
<!-- ### Scalability -->

タングルにアタッチされる各トランザクションごとに、2つの以前のトランザクションが検証されます。このプロセスにより、IOTAが非常にスケーラブルになります。これは、より多くの新しいトランザクションが検証の高速化につながるためです。
<!-- For each transaction that's attached to the Tangle, two previous transactions are validated. This process makes IOTA incredibly scalable because more new transactions lead to faster validations. -->

## ユースケース
<!-- ## What are some example use cases? -->

IOTAは、単一の真実のソースを使用して、データを共有およびアクセスするための安全なプラットフォームです。そのため、IOTAは、効率性の向上、生産性の向上、データの整合性の確保により、多くの業界に利益をもたらします。
<!-- IOTA is a secure platform for sharing and accessing data, using a single source of truth. As such, IOTA can benefit many industries by improving efficiency, increasing production, and ensuring data integrity. -->

- [モビリティ](https://www.iota.org/verticals/mobility-automotive)
<!-- - [Mobility](https://www.iota.org/verticals/mobility-automotive) -->
- [グローバル貿易とサプライチェーン](https://www.iota.org/verticals/global-trade-supply-chains)
<!-- - [Global trade and supply chains](https://www.iota.org/verticals/global-trade-supply-chains) -->
- [工業用IoT](https://www.iota.org/verticals/industrial-iot)
<!-- - [Industrial IoT](https://www.iota.org/verticals/industrial-iot) -->
- [ヘルスケア](https://www.iota.org/verticals/ehealth)
<!-- - [Healthcare](https://www.iota.org/verticals/ehealth) -->
- [エネルギー](https://www.iota.org/verticals/smart-energy)
<!-- - [Energy](https://www.iota.org/verticals/smart-energy) -->

IOTAのいくつかのアプリケーションは次のとおりです。
<!-- These are some applications of IOTA: -->

| **会社** | **説明** | **リファレンス** |
| :------: | :------: | :--------------: |
| bIOTAsphere | bIOTAsphereはテスラ車をIOTAネットワークに接続しました。自動車はIOTAトークンで動的保険を購入します。 | [ユースケースを紹介するYouTubeビデオ](https://www.youtube.com/watch?v=2zvrA5KqeYw) |
| iampass | iampassは、手のひらを暗号化してスキャンし、IOTAネットワークのタングルでそのデータを確認することにより、ユーザーを認証するIDおよびアクセス管理システムです。 | [iampassのウェブサイト](https://iampass.io/) |
| +CityxChange consortium | CityxChangeコンソーシアムは、安全なデータ転送レイヤーとしてIOTAを使用するスマートシティを構築するプロジェクトです。 | [+CityxChangeのウェブサイト](http://cityxchange.eu/) |
| eCl@ss | eCl@ssはIOTAと協力して、ISO/IEC準拠の製品分類と製品説明の共有と保存を行っています。 | [eCl@ssのウェブサイト](https://www.eclass.eu/en/association/cooperation.html) |
| Elaadnl | ElaadnlはIOTAトークンでの支払いを受け入れる電気自動車充電器を作成しています。 | [Elaadnlのウェブサイト](https://www.elaad.nl/news/worlds-first-iota-charging-station-released/) |

## IOTAを使う出発点
<!-- ## Where do I start? -->

**非開発者：**[公式Trinityウォレット](root://wallets/0.1/trinity/introduction/overview.md)を使用して、シードの作成と保存、トランザクションの送受信などを行う。
<!-- **Non-developers:** Use the [official Trinity wallet](root://wallets/0.1/trinity/introduction/overview.md) to create and store your seed, send and receive transactions, and more. -->

**開発者：**[クライアントライブラリの1つを使用する](root://client-libraries/0.1/getting-started/quickstart.md)か[自分自身のノードを実行してIOTAネットワークに参加する](root://node-software/0.1/iri/how-to-guides/quickstart.md)。
<!-- **Developers:** [Get started with one of the client libraries](root://client-libraries/0.1/getting-started/quickstart.md) or [join an IOTA network by running your own node](root://node-software/0.1/iri/how-to-guides/quickstart.md) -->

**取引所：**[公式ハブウォレット](root://wallets/0.1/hub/introduction/overview.md)を使用して、IOTAを取引所に統合する。
<!-- **Exchanges:** Use the [official Hub wallet](root://wallets/0.1/hub/introduction/overview.md) to integrate IOTA into your exchange. -->
