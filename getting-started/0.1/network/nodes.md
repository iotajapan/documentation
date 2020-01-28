# ノード
<!-- # Nodes -->

**ノードは IOTA ネットワークの中核です。[タングル](../network/the-tangle.md)への読み取りおよび書き込みアクセスを許可する[ノードソフトウェア](root://node-software/0.1/introduction/overview.md)を実行します。他の[分散システム](https://en.wikipedia.org/wiki/Distributed_computing)と同様に、ノードは IOTA ネットワークを形成するためにネイバーと呼ばれる他のノードに接続されます。1つのノードは、世界のどこにいても[トランザクション]を受信すると、受信したトランザクションをすべての隣接ノード(ネイバー)に転送しようとします。このようにして、すべてのノードは最終的にすべてのトランザクションを検証し、検証したトランザクションをレッジャーと呼ばれる[タングル](../network/the-tangle.md)のローカルコピーに保存します。**
<!-- **Nodes are the core of an IOTA network. They run the [node software](root://node-software/0.1/introduction/overview.md) that gives them read and write access to the [Tangle](../network/the-tangle.md). Like any [distributed system](https://en.wikipedia.org/wiki/Distributed_computing), nodes are connected to others called neighbors to form an IOTA network. When one node, no matter where it is in the world, receives a [transaction](../transactions/transactions.md), it will try to forward it to all its neighbors. This way, all nodes eventually validate all transactions and store them in their local copy of the [Tangle](../network/the-tangle.md) called a ledger.** -->

## ノードの仕組み
<!-- ## How nodes work -->

ノードは IOTA ネットワークのコアであり、次の主要な機能を担当します。
<!-- Nodes are the core of an IOTA network, and are responsible for the following key functions: -->

- 残高が0より大きいアドレスの記録を保持する
<!-- - Keeping a record of the addresses with a balance greater than 0 -->
- トランザクションを検証する
<!-- - Validating transactions -->
- 有効なトランザクションをタングルにアタッチする
<!-- - Attaching valid transactions to the Tangle -->

:::info:
[トリニティ](root://wallets/0.1/trinity/introduction/overview.md)などの多くのアプリケーションは、[クライアントライブラリ](root://client-libraries/0.1/introduction/overview.md)を介してノードの API と対話します。
:::
<!-- :::info: -->
<!-- Many applications such as [Trinity](root://wallets/0.1/trinity/introduction/overview.md) interact with a node's API through the [client libraries](root://client-libraries/0.1/introduction/overview.md). -->
<!-- ::: -->

## 検証
<!-- ## Validation -->

ノードは、[タングル](../network/the-tangle.md)内のトランザクションを検証して、履歴がコンフリクトせず、偽造トランザクションが決して確定されないようにする責任があります。トランザクションを検証するために、ノードは次ことを確認します。
<!-- Nodes are responsible for validating transactions in the [Tangle](../network/the-tangle.md) to make sure that their histories do not conflict and that counterfeit transactions are never confirmed. To validate a transaction, nodes check the following: -->

- [プルーフオブワーク](../transactions/proof-of-work.md)が[最小重量値](../network/minimum-weight-magnitude.md)に従っている
<!-- - [Proof of work](../transactions/proof-of-work.md) is done according to the [minimum weight magnitude](../network/minimum-weight-magnitude.md) -->
- すべてのトランザクションのトークン量が[全総供給量](../clients/token.md)を超えない
<!-- - The value of any transaction doesn’t exceed the [total global supply](../clients/token.md) -->
- [バンドル]内のすべてのトランザクションを合計した値が0である（取り出されるすべてのIOTAトークンは他のアドレスにもデポジットされます）
<!-- - The total value of all transactions in a [bundle] is 0 (all IOTA tokens that are withdrawn are also deposited into other addresses) -->
- トークンを有するトランザクションの[署名](../clients/signatures.md)がすべて有効である
<!-- - Any [signatures](../clients/signatures.md) in value transactions are valid -->

ノードはクライアントのシードを持っていないため、どのクライアントがどのアドレスを所有しているかを知らないため、暗号化を使用して[トランザクション内の署名を検証します](../clients/signatures.md#how-nodes-validate-signatures)。
<!-- Nodes don't know which client owns an address because they don't have the clients' seeds, so they use cryptography to [validate the signature in a transaction](../clients/signatures.md#how-nodes-validate-signatures). -->

## 隣接ノード
<!-- ## Neighbor nodes -->

隣接ノードは、相互に接続され、同じ IOTA ネットワーク上で互いに直接通信するノードです。自身の台帳をネットワークの残りの部分と同期させるために、すべてのノードは近隣ノード間でトランザクションを送受信します。
<!-- Neighbors are nodes that are mutually connected and that communicate directly with each other on the same IOTA network. To synchronize their ledgers with the rest of the network, all nodes send and receive transactions among their neighbors. -->

ノードは、新しいトランザクションを受信した後、台帳にトランザクションの履歴があることを確認します。ノードにトランザクションが欠落している場合、ノードはネットワークの残りの部分と同期するように近隣ノードに要求し始めます。
<!-- After receiving a new transaction, nodes check that they have the transaction's history in its ledger. If a node is missing any transactions, it starts to ask its neighbors for them to become synchronized with the rest of the network. -->

## 同期済みノード
<!-- ## Synchronized nodes -->

ノードは世界中に分散しているため、いつでも台帳に異なるトランザクションがある場合があります。
<!-- Because nodes are distributed throughout the world, they may have different transactions in their ledgers at any time. -->

任意のノードの台帳のトランザクション群は、**タングルの概観**を構成します。
<!-- The transactions in any node's ledger make up its **view of the Tangle**. -->

最終的にすべてのノードがタングルの同じ概観を持つようにするには、すべてのノードがネットワークの他の部分と同期する必要があります。
<!-- To make sure that all nodes eventually have the same view of the Tangle, they must synchronize with the rest of the network. -->

ノードは、最新のマイルストーンまですべてのマイルストーンが凝固したときに同期済みになります。
<!-- A node is synchronized when it has solidified all milestones up to the latest one. -->

メインネットを実行する IRI および cIRI [ノードソフトウェア](root://node-software/0.1/introduction/overview.md)では、`latestMilestoneIndex` フィールドと `latestSolidSubtangleMilestoneIndex` フィールドの値が等しい場合にノードが同期済みになります。
<!-- In the IRI and cIRI [node software](root://node-software/0.1/introduction/overview.md), which runs the Mainnet, a node is synchronized when the values of the  `latestMilestoneIndex` and `latestSolidSubtangleMilestoneIndex` fields are equal. -->

:::ifno:
`latestMilestoneIndex` フィールドは、ノードが同期済みの隣接ノードに接続されている場合にのみ正確です。
:::
<!-- :::info: -->
<!-- The `latestMilestoneIndex` field is accurate only when the node is connected to synchronized neighbors. -->
<!-- ::: -->

### 凝固
<!-- ### Solidification -->

凝固とは、ノードがタングル上のすべての[マイルストーン](../network/the-coordinator.md#milestones)の履歴をノードに要求するプロセスです。**エントリポイントマイルストーン**から始まり、最新のマイルストーンで終了します。
<!-- Solidification is the process in which a node asks its neighbors for the history of all [milestones](../network/the-coordinator.md#milestones) in the Tangle, starting from an **entry point milestone** and ending at the latest one. -->

ノードにエントリポイントマイルストーンまでのマイルストーンの履歴がある場合、ノードはそのマイルストーンを**凝固済み**としてマークし、次のマイルストーンからプロセスを再開します。
<!-- When a node has a milestone's history up to the entry point milestone, it marks that milestone as **solid**, and starts the process again from the next milestone. -->

その結果、エントリポイントのマイルストーンが古いほど、凝固に時間がかかります。
<!-- As a result, the older the entry point milestone, the longer solidification takes. -->

## コンセンサス
<!-- ## Consensus -->

コンセンサスは、どのトランザクションが信頼でき、確定のために考慮する必要があるかについてノードがどのように合意するかを説明します。
<!-- Consensus describes how nodes agree on which transactions are trustworthy and should be considered for confirmation. -->

トランザクションを確定済みと見なすには、ノードがアドレスの残高を更新する前に、いつ最終的なトランザクションと見なすかについて、全ノードが合意に達する必要があります。
<!-- For a transaction to be considered confirmed, nodes must reach a consensus on when to consider it final before they can update the balances of addresses. -->

トランザクションが、凝固済みであり、[コーディネーター](../network/the-coordinator.md)によって署名されたトランザクションによって直接または間接的に参照されている場合、確定済みと見なされます。
<!-- A transaction is considered confirmed when it's solid and it's directly or indirectly referenced by a transaction that's signed by the [Coordinator](../network/the-coordinator.md). -->

## ノードクォーラム
<!-- ## Node quorums -->

ノードクォーラムは、同じリクセストを送信し、一貫性を保つためにレスポンスを比較するノードのグループです。
<!-- A node quorum is a group of nodes to which you send the same request and compare the responses for consistency. -->

タングルからの情報のソースとして1つのノードのみに依存している場合、そのノードが正しいと確信することはできません。たとえば、そのノードは、利用可能な残高に関する誤った情報を送信する可能性があります。ノードの信頼性を高めるために、ノードクォーラムから情報をリクエストできます。
<!-- When you rely only on one node as a source of information from the Tangle, you can't be confident that it's correct. For example, that node could send you the wrong information about your available balance. To increase your confidence in a node, you can request information from a node quorum. -->

## ローカルスナップショット
<!-- ## Local snapshots -->

IOTA はネットワークへの参加に許可を必要としない（パーミッションレス）ネットワークです。1日24時間、週7日、誰でもタングルに任意の量のデータを無料で保存できます（トランザクションごとに少量の[プルーフオブワーク](root://getting-started/0.1/transactions/proof-of-work.md)のみ必要です）。
<!-- IOTA is a permissionless network. For 24 hours a day, 7 days a week, anyone can store any amount of data on the Tangle for free (just a small amount of [proof of work](root://getting-started/0.1/transactions/proof-of-work.md) per transaction) -->

その結果、タングルを含むデータベースのサイズは常に増加しており、このストレージスペースには多くの費用がかかります。台帳が大きくなりすぎるのを防ぐために、ノードは多くの場合、ローカルスナップショットを作成します。
<!-- As a result, the size of the databases that contain the Tangle are always growing, and this storage space costs a lot of money. To stop their ledgers from becoming too large, nodes often do local snapshots. -->

ローカルスナップショットは、ノードが自身の台帳の状態をスナップショットファイルに記録するプロセスです。スナップショットファイルを使用すると、タングルに含まれるトランザクションの数が少なくなるため、ノードはネイバーノードとの同期を大幅に高速化できます。
<!-- A local snapshot is the process in which a node records the state of its ledger in snapshot files. Using these files, nodes can synchronizing with their neighbors a lot faster because the Tangle contains fewer transactions. -->

## パーマノード
<!-- ## Permanodes -->

多くのビジネスユースケースでは、IOTA タングルのデータを長期間保存する必要があります。たとえば、財務データは場合によっては10年間保存する必要があり、ID データは ID の存続期間中保持する必要があります。
<!-- For many business use cases, data in the IOTA Tangle needs to be stored for long periods of time. For example, financial data must be stored for 10 years in some cases, and identity data needs to be kept for the lifetime of the identity. -->

ローカルスナップショットを作成するのではなく、パーマノードはタングルの完全な履歴を保存し、アプリケーションが拡張 API を介してデータを検索できるようにします。
<!-- Rather than doing local snapshots, a permanode stores the full history of the Tangle and enables applications to search the data through an extended API. -->

## 関連ガイド
<!-- ## Related guides -->

[IRI](root://node-software/0.1/iri/introduction/overview.md)を実行する。IRI は、パブリック IOTA ネットワークで実行されるノードソフトウェアです。
<!-- Run [IRI](root://node-software/0.1/iri/introduction/overview.md), the node software that runs on the public IOTA networks. -->

Coordicide につながるプロトタイプノードソフトウェアである [GoShimmer](root://node-software/0.1/goshimmer/introduction/overview.md) を実行する。
<!-- Run [GoShimmer](root://node-software/0.1/goshimmer/introduction/overview.md), the prototype node software that will lead to Coordicide. -->

IOTA 財団のパーマノードソフトウェアである [Chronicle](root://node-software/0.1/chronicle/introduction/overview.md) を実行する。
<!-- Run [Chronicle](root://node-software/0.1/chronicle/introduction/overview.md), the IOTA Foundation's permanode software. -->
