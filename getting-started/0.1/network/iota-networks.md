# IOTAネットワーク
<!-- # IOTA networks -->

**IOTAには複数の[ノード](../network/nodes.md)による2つのパブリックネットワークがあり、各ネットワークにはノードが[トランザクション](../transactions/transactions.md)を接続できる独自の[タングル](../network/the-tangle.md)があります。**
<!-- **IOTA has two public networks of [nodes](../network/nodes.md), and each one has its own [Tangle](../network/the-tangle.md) to which nodes can attach [transactions](../transactions/transactions.md).** -->

IOTAには次のパブリックネットワークがあります。
<!-- IOTA has the following public networks: -->

- **メインネット：**IOTAトークン
<!-- - **Mainnet:** IOTA token -->

- **デブネット：**デブネットトークン（無料）
<!-- - **Devnet:** Devnet token (free) -->

## メインネット
<!-- ## Mainnet -->

メインネットは、暗号資産取引所で取引される[IOTAトークン](../clients/token.md)を使用するIOTAネットワークです。
<!-- The Mainnet is the IOTA network that uses the [IOTA tokens](../clients/token.md), which are traded on cryptocurrency exchanges. -->

本番アプリケーションは、[デブネット](#devnet)または[プライベートタングル](root://compass/0.1/introduction/overview.md)でIOTAをテストした後、このネットワークを使用します。
<!-- Production applications use this network after they have tested IOTA on the [Devnet](#devnet) or a [private Tangle](root://compass/0.1/introduction/overview.md). -->

:::info:
暗号資産取引所では、IOTAトークンをMega IOTA（1,000,000）単位で販売します。これはMIOTAまたはMiとも書かれています。
:::
<!-- :::info: -->
<!-- Cryptocurrency exchanges sell IOTA tokens in units of Mega IOTA (1,000,000), which is also written as MIOTA or Mi. -->
<!-- ::: -->

![Mainnet configuration](../images/mainnet-configuration.png)

### 最小重量値
<!-- ### Minimum weight magnitude -->

メインネット上のトランザクションは、有効な14の[最小重量値](root://getting-started/0.1/transactions/proof-of-work.md#minimum-weight-magnitude)（MWM）を使用する必要があります。
<!-- Transactions on the Mainnet must use a [minimum weight magnitude](root://getting-started/0.1/transactions/proof-of-work.md#minimum-weight-magnitude) (MWM) of 14 to be valid. -->

### コーディネーターアドレス
<!-- ### Coordinator address -->

メインネット上のすべてのノードには、[コーディネーター](../network/the-coordinator.md)の次のアドレスがハードコーディングされています。
<!-- Nodes on the Mainnet are all hard-coded with the following address for the [Coordinator](../network/the-coordinator.md): -->

```bash
EQSAUZXULTTYZCLNJNTXQTQHOMOFZERHTCGTXOLTVAHKSA9OGAZDEKECURBRIXIJWNPFCQIOVFVVXJVD9
```

### ノード
<!-- ### Nodes -->

サードパーティのノードに依存してトランザクションを受信するのではなく、自分自身のノードを実行してタングルに直接アクセスすることをお勧めします。
<!-- It's best practice to run your own node to have direct access to the Tangle, instead of relying on third-party nodes to receive your transactions. -->

ただし、メインネットをテストする場合は、次のようなコミュニティWebサイトでノードの一覧を見つけることができます。
<!-- However, if you want to test the Mainnet, you can find a list of nodes on community websites such as the following: -->

- [iota.dance](https://iota.dance/)

- [thetangle.org](https://thetangle.org/nodes)

<a name="devnet"></a>
## デブネット

デブネットはメインネットに似ていますが、トークンが無料であり、トランザクションの作成と送信にかかる時間と計算能力が少ない点が異なります。
<!-- The Devnet is similar to the Mainnet, except the tokens are free and it takes less time and computational power to create and send a transaction. -->

このネットワークで、アプリケーションをテストし、無料のデブネットトークンを使用する概念実証を作成できます。
<!-- On this network, you can test your applications and build proofs of concept that use free Devnet tokens. -->

![Devnet Configuration](../images/devnet-configuration.png)

### 最小重量値
<!-- ### Minimum weight magnitude -->

デブネットでのトランザクションを有効にするには、[最小重量値](root://getting-started/0.1/transactions/proof-of-work.md#minimum-weight-magnitude)（MWM）9を使用する必要があります。
<!-- Transactions on the Devnet must use a [minimum weight magnitude](root://getting-started/0.1/transactions/proof-of-work.md#minimum-weight-magnitude) (MWM) of 9 to be valid. -->

### コーディネーターアドレス
<!-- ### Coordinator address -->

デブネット上のすべてのノードには、[コーディネーター](../network/the-coordinator.md)の次のアドレスがハードコーディングされています。
<!-- Nodes on the Devnet are all hard-coded with the following address for the [Coordinator](../network/the-coordinator.md): -->

```bash
EQQFCZBIHRHWPXKMTOLMYUYPCN9XLMJPYZVFJSAY9FQHCCLWTOLLUGKKMXYFDBOOYFBLBI9WUEILGECYM
```

### ノード
<!-- ### Nodes -->

IOTA財団は、デブネットへの接続に使用できる次のノードをホストします。
<!-- The IOTA Foundation hosts the following nodes that you can use to connect to the Devnet: -->

#### ロードバランサーノード
<!-- #### Load balancer node -->

このエンドポイントを使用すると、デブネットでノードを実行している高可用性プロキシサーバーにアクセスできます。
<!-- This endpoint gives you access to a high-availability proxy server, which is running a node on the Devnet. -->

ロードバランサーを使用して、トランザクションを送信し、ノードから台帳に関する情報をリクエストします。
<!-- Use the load balancer for sending transactions and requesting information about the ledger from the node. -->

**URL:** https://nodes.devnet.iota.org:443

#### ZMQノード
<!-- #### ZMQ node -->

このエンドポイントを使用すると、デブネット上のノードのゼロメッセージキューにアクセスできます。
<!-- This endpoint gives you access to the zero message queue of a node on the Devnet. -->

ZMQノードを使用して、タングルでライブトランザクションをリッスンします。
<!-- Use the ZMQ node to listen for live transaction on the Tangle. -->

**URL:** tcp://zmq.devnet.iota.org:5556

#### PoWノード
<!-- #### PoW node -->

このエンドポイントを使用すると、リモートでプルーフオブワークを行うことができるノードにアクセスできます。
<!-- This endpoint gives you access to a node that can do remote proof of work. -->

PoWノードを使用して、小型デバイスの電力を節約します。
<!-- Use the PoW node to save power on small devices. -->

**URL:** https://powbox.devnet.iota.org

## IOTAネットワークを選択する際のアドバイス
<!-- ## Advice for choosing an IOTA network -->

最小重量値を選択するときは、次の質問を考慮する必要があります。
<!-- When choosing a minimum weight magnitude, you should consider the following questions. -->

### IOTAトークンを転送するテストトランザクションを送信したい
<!-- ### Do you want to send test transactions that transfer IOTA tokens? -->

開発環境でIOTAをテストするときは、[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)上のノードへの接続を検討する必要があります。このIOTAネットワークは、[プルーフオブワーク](root://getting-started/0.1/transactions/proof-of-work.md)が少なくて済むため、トランザクションの作成にかかる時間が短縮され、[無料のテストIOTAトークン](root://getting-started/0.1/tutorials/get-test-tokens.md)が使用されます。
<!-- When testing IOTA in a development environment, you should consider connecting to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). This IOTA network requires less [proof of work](root://getting-started/0.1/transactions/proof-of-work.md), which reduces the time it takes to create transactions, and it uses [free test IOTA tokens](root://getting-started/0.1/tutorials/get-test-tokens.md). -->

### ネットワークのパフォーマンスを制御したい
<!-- ### Do you want to control the performance of the network? -->

IOTAをテストする場合、IOTAネットワークのパフォーマンスやタングルにアクセスできるユーザーをより詳細に制御したい場合があります。この場合、[プライベートタングル](root://compass/0.1/introduction/overview.md)を実行できます。
<!-- When testing IOTA, you may want more control over the performance of an IOTA network and/or who has access to the Tangle. In this case, you can run a [private Tangle](root://compass/0.1/introduction/overview.md). -->

### 価値の有るIOTAトークンを使用したい
<!-- ### Do you want to use the valuable IOTA token? -->

実稼働環境にアプリケーションをデプロイするときは、[メインネット](root://getting-started/0.1/network/iota-networks.md#mainnet)上のノードに接続する必要があります。このネットワークは、暗号資産取引所で取引されるIOTAトークンを使用します。
<!-- When deploying your application in a production environment, you should connect to a node on the [Mainnet](root://getting-started/0.1/network/iota-networks.md#mainnet). This network uses the IOTA token that's traded on cryptocurrency exchanges. -->

### サードパーティのノードを使用する必要があるかどうか
<!-- ### Should you use a third-party node? -->

サードパーティのノードへの接続は便利ですが、信頼できるサービスが必要な場合は不利になります。例えば：
<!-- Connecting to third-party nodes is convenient, but comes at a disadvantage if you need a reliable service. For example: -->

- トランザクションは、ノードが受信する他のトランザクションと競合し、ノードが決定する優先度で処理されます。
<!-- - Your transactions will compete with other transactions that the node receives and will be processed with a priority that the node decides -->
- 高速PoW計算の支払い、またはPoWを含むトランザクションの提供をリクエストされる場合があります。
<!-- - You might be requested to pay for fast PoW computation or to provide a transaction that includes PoW -->
- 台帳へのトランザクションのコピーは、ノードによって決定された限られた時間だけ保持される場合があります。
<!-- - A copy of your transactions might be kept only for a limited time that's decided by the node -->
- パーマノードオプション（トランザクションの永続的なストレージ）には手数料が必要な場合があります。
<!-- - A permanode option (permanent storage of your transactions) might require a fee -->

これらの欠点を克服するには、自分自身のノードを実行し、アプリケーションを接続してタングルに直接アクセスすることをお勧めします。自分自身のノードを使用すると、トランザクションがタングルにアタッチされる速度をより詳細に制御でき、トランザクションを永続的に保存できます。
<!-- To overcome these disadvantages, we recommend that you run your own node and connect your application to it for direct access to the Tangle. Your own node gives you more control on how fast your transactions are attached to the Tangle and allows you to store them permanently. -->

パブリックIOTAネットワークは、誰でも参加して使用できるネットワークです。パブリックIOTAネットワーク内のすべてのトランザクションは透過的であり、ノードに接続することで誰でもそれらとすべてのアドレスの残高を見ることができます。
<!-- A public IOTA network is one that anyone can join and use. All transactions in a public IOTA network are transparent, and anyone can see them and the balances of all addresses by connecting to a node. -->

## 関連ガイド
<!-- ## Related guides -->

[JavaScriptでノードに接続する](root://client-libraries/0.1/getting-started/js-quickstart.md)
<!-- [Connect to a node in JavaScript](root://client-libraries/0.1/getting-started/js-quickstart.md). -->
