# 用語集
<!-- # Glossary -->

**この用語集には、IOTA独自の定義が含まれています。**
<!-- **This glossary contains definitions specific to IOTA.** -->

用語はカテゴリ別にアルファベット順に記載されています。
<!-- Terms are listed in alphabetical order by category. -->

### 頭文字語
<!-- ### Acronyms -->

<dl><dt>CTPS</dt><dd>1秒あたりの確定済みトランザクション数。</dd></dl>
<!-- <dl><dt>CTPS</dt><dd>Number of Confirmed Transactions Per Second</dd></dl> -->

<a name="TPS"></a><dl><dt>TPS</dt><dd>[IRIノード](#iota-terms)を介した1秒あたりのトランザクション処理数。</dd></dl>
<!-- <dl><dt>TPS</dt><dd>Number of Transactions Per Second processing through an [IRI node](#iota-terms)</dd></dl> -->

### アカウント
<!-- ### Accounts -->

<dl><dt>アドレス</dt><dd>IOTAトークンを所有するアカウント。アドレスを使ってデータとIOTAトークンを送受信できます。</dd></dl>
<!-- <dl><dt>address</dt><dd>Account that owns IOTA tokens. Addresses can send and receive data and IOTA tokens</dd></dl> -->

<dl><dt>複数署名アドレス（マルチシグアドレス）</dt><dd>IOTAトークンを取り出せるようにするには、複数の当事者がトランザクションに署名する必要があるアドレス。</dd></dl>
<!-- <dl><dt>multi-signature address (multi-sig address)</dt><dd>Address that needs multiple parties to sign a transaction before IOTA tokens can be withdrawn from it</dd></dl> -->

<dl><dt>セキュリティレベル</dt><dd>署名の長さに影響する整数（1、2、または3）。セキュリティレベルが高いほど、署名が長くなり、アドレスのセキュリティが強くなります。</dd></dl>
<!-- <dl><dt>security level</dt><dd>An integer (1, 2, or 3) that affects the length of a signature. The higher the security level, the longer the signature, and the stronger the security of the address.</dd></dl> -->

<dl><dt>シード</dt><dd>アドレスと署名の生成に使用される81トライトのパスワード。</dd></dl>
<!-- <dl><dt>seed</dt><dd>81-tryte password used to generate addresses and signatures</dd></dl> -->

<dl><dt>署名</dt><dd>アドレスの所有権の暗号法による証明。有効な署名がないと、IOTAトークンをアドレスから取り出すことはできません。</dd></dl>
<!-- <dl><dt>signature</dt><dd>Cryptographic proof of ownership of an address. IOTA tokens can't be withdrawn from an address without a valid signature.</dd></dl> -->

### 暗号法
<!-- ### Cryptography -->

<dl><dt>チェックサム</dt><dd>エラーを検出し、正しいデータを入力したことを確認できるようにするためにデータをハッシュしたもの。例えば、トリニティでは、9文字のチェックサムが81文字のアドレスの後ろに追加されるので、ユーザーは自分のシード（81文字）とアドレス（90文字）を簡単に区別できます。</dd></dl>
<!-- <dl><dt>checksum</dt><dd>Hashed data that allows you to detect errors and verify that you entered the correct data.  For example, in Trinity, a 9-character checksum is appended to the 81-tryte address so users can easily differentiate between their seed (81-trytes) and their address (90-characters) </dd></dl> -->

<dl><dt>Curl</dt><dd>IOTAで使用されている主なハッシュ関数。Curlは[Keccakクリエイター](https://en.wikipedia.org/wiki/SHA-3)（SHA-3）によって発明されたスポンジ構造に基づいています。 Curlはモノのインターネット（IoT）用に設計されています。</dd></dl>
<!-- <dl><dt>Curl</dt><dd>Main hash function that's used in IOTA. Curl is based on a sponge construction, which was invented by the [Keccak creators](https://en.wikipedia.org/wiki/SHA-3) (SHA-3). Curl is designed for the Internet of things (IoT). </dd></dl> -->

<dl><dt>Kerl</dt><dd>[2の補数](https://en.wikipedia.org/wiki/Two%27s_complement)を使用して、243トリットから48バイトへ、48バイトから243トリットへの入出力の追加変換を含むKeccek-384ハッシュ関数。</dd></dl>
<!-- <dl><dt>Kerl</dt><dd>Keccek-384 hash function that includes the additional conversion of its input and output from/to 243 trits to 48 bytes, using [two's complement](https://en.wikipedia.org/wiki/Two%27s_complement).</dd></dl> -->

<dl><dt>Winternitzワンタイム署名（W-OTS）</dt><dd>IOTAトークンを取り出す入力トランザクションに署名するために使用されるポスト量子耐性署名スキーム。</dd></dl>
<!-- <dl><dt>Winternitz one-time signature (W-OTS)</dt><dd>Post-quantum signature scheme that's used to sign input transactions that withdraw IOTA tokens.</dd></dl> -->

## 一般用語
<!-- ## General terms -->

<dl><dt>有向非巡回グラフ（DAG）</dt><dd>有向で循環のないグラフに基づくデータ構造。 IOTAでは、頂点はトランザクションを表し、辺は承認を表します。</dd></dl>
<!-- <dl><dt>directed acyclic graph (DAG)</dt><dd>Data structure that's based on a graph without any directed cycles. In IOTA, vertices represent transactions, and edges represent approvals </dd></dl> -->

<dl><dt>ピアツーピアネットワーク</dt><dd>互いにデータを共有するノードで構成される分散ネットワーク。</dd></dl>
<!-- <dl><dt>peer-to-peer network</dt><dd>Decentralized network that consists of nodes, which share data among each other</dd></dl> -->

<dl><dt>プルーフオブワーク（PoW）</dt><dd>解くのが難しく、検証が簡単な計算パズル。 IOTAはHashcashベースのパズルを使用してネットワークへのDoS攻撃とスパム攻撃の両方を防止します。</dd></dl>
<!-- <dl><dt>proof of work (PoW)</dt><dd>Computational puzzle that's hard to solve and easy to verify. IOTA uses a Hashcash-based puzzle to prevent both denial of service (DoS) attacks and spam attacks on the network</dd></dl> -->

## IOTA用語
<!-- ## IOTA terms -->

<dl><dt>タンポポ</dt><dd>多数のチップトランザクションに囲まれた1つの中央トランザクションからなるタングル構造。</dd></dl>
<!-- <dl><dt>blowball</dt><dd>Tangle formation that consists of one central transaction, which is surrounded by a large number of tip transactions.</dd></dl> -->

<dl><dt>クライアント</dt><dd>IOTAネットワークと対話して、トランザクションを送信するためにIRIノードに接続するソフトウェアを実行しているデバイス。</dd></dl>
<!-- <dl><dt>client</dt><dd>Device running software that connects to an IRI node to interact with an IOTA network and send transactions.</dd></dl> -->

<dl><dt>分散型台帳</dt><dd>IOTAネットワーク内のすべてのIRIノードによって保持されている共通台帳。IRIノードは、同じ有効なトランザクションが繋がっているすべてのノードの台帳に含まれるまで、台帳の内容を互いに共有します。</dd></dl>
<!-- <dl><dt>distributed ledger</dt><dd>Common ledger that is held by all IRI nodes in an IOTA network. IRI nodes share their ledgers with each other until all of them contain the same valid transactions.</dd></dl> -->

<dl><dt>IRIノード</dt><dd>トランザクションを検証し、トランザクションのコピーを台帳に保存することを担当するソフトウェア。</dd></dl>
<!-- <dl><dt>IRI node</dt><dd>Software that's responsible for validating transactions and storing a copy of them in a ledger</dd></dl> -->

<a name="ledger"></a><dl><dt>台帳</dt><dd>IRIノードによって検証されたすべてのトランザクションを格納するデータベース。</dd></dl>
<!-- <a name="ledger"></a><dl><dt>ledger</dt><dd>Database storing all transactions validated by an IRI node</dd></dl> -->

<a name="subgraph"></a><dl><dt>部分グラフ</dt><dd>マイルストーントランザクションとチップトランザクションの間のすべてのトランザクションが含まれる台帳の一部。</dd></dl>
<!-- <a name="subgraph"></a><dl><dt>subgraph</dt><dd>Section of the ledger that contains all transactions between a milestone transaction and tip transactions</dd></dl> -->

<dl><dt>タングル</dt><dd>分散型台帳内でIOTAトランザクションが参照するトランザクションに接続することによって形成される有向非巡回グラフ（DAG）の名称。</dd></dl>
<!-- <dl><dt>Tangle</dt><dd>Name of the directed acyclic graph (DAG) that is formed by connecting IOTA transactions in the distributed ledger to the transactions that they reference</dd></dl> -->

### トランザクション
<!-- ### Transactions -->

<dl><dt>承認者</dt><dd>別のトランザクションをトランクトランザクションまたはブランチトランザクションとして直接参照するトランザクション。</dd></dl>
<!-- <dl><dt>approver</dt><dd>Transaction that directly references another transaction as its trunk transaction or branch transaction</dd></dl> -->

<a name="branch"></a><dl><dt>ブランチトランザクション</dt><dd>別のトランザクションによって参照されているトランザクション。</dd></dl>
<!-- <a name="branch"></a><dl><dt>branch transaction</dt><dd>Transaction that is referenced by another transaction.</dd></dl> -->

<dl><dt>バンドル</dt><dd>IRIノードにまとめて送信されるトランザクションのグループ。バンドルは原子です。バンドル内のすべてのトランザクションが受け入れられるか、または受け入れられないかです。</dd></dl>
<!-- <dl><dt>bundle</dt><dd>Group of transactions that are sent together to an IRI node.  Bundles are atomic. Either all transactions inside the bundle are accepted or none of them are.</dd></dl> -->

<dl><dt>確定</dt><dd>マイルストーンによって承認されたときのトランザクションの状態。</dd></dl>
<!-- <dl><dt>confirmed</dt><dd>State of a transaction when it's been approved by a milestone</dd></dl> -->

<dl><dt>累積荷重</dt><dd>[node](#iota-terms)が[transaction](#transactions)に与える評価。</dd></dl>
<!-- <dl><dt>cumulative weight</dt><dd>Rating that a [node](#iota-terms) gives to a [transaction](#transactions).</dd></dl> -->

<dl><dt>深さ</dt><dd>エントリポイント[マイルストーン](#milestone)は、[部分グラフ](#subgraph)を通る重み付きランダムウォークを開始するために使用されます。値が高いほど、部分グラフの奥に戻って、重み付きランダムウォークが開始されます。ウォレットが使用する典型的な深さは3で、これは重み付きランダムウォークが過去の3マイルストーンから開始する原因となります。</dd></dl>
<!-- <dl><dt>depth</dt><dd>Entry point [milestone](#milestone) that's used to start a weighted random walk through a [subgraph](#subgraph). The higher the value, the farther back in the subgraph the weighted random walk starts. A typical depth that wallets use is 3, which causes the weighted random walk to start 3 milestones in the past.</dd></dl> -->

<dl><dt>未来集合</dt><dd>同じトランザクションを承認するトランザクションのグループ。</dd></dl>
<!-- <dl><dt>future set</dt><dd>Group of transactions that approve the same transaction</dd></dl> -->

<dl><dt>矛盾している</dt><dd>利用不可能な資金の取り出しにつながるトランザクションの状態。</dd></dl>
<!-- <dl><dt>inconsistent</dt><dd>State of a transaction when it leads to withdrawing non-available funds</dd></dl> -->

<dl><dt>包含ステート</dt><dd>[IRIノード](#iota-terms)がトランザクションの受け入れまたはトランザクションの確定のいずれかをチェックするために実行するプロセス。トランザクションとチップトランザクションの一覧が与えられた場合、チップトランザクションがそのトランザクションを参照していれば、包含ステートは真です。</dd></dl>
<!-- <dl><dt>inclusion state</dt><dd>Process that an [IRI node](#iota-terms) performs to check either the acceptance of a transaction or the confirmation of a transaction. Given a transaction and a list of tip transactions, the inclusion state is true if the tip transactions reference that transaction.</dd></dl> -->

<dl><dt>無効</dt><dd>トランザクションの情報が足りない、矛盾している、古すぎるトランザクションを参照しているときのトランザクションの状態。</dd></dl>
<!-- <dl><dt>invalid</dt><dd>State of a transaction when it's either non-solid, inconsistent, or it references a transaction that's too old.</dd></dl> -->

<a name="milestone"></a><dl><dt>マイルストーン</dt><dd>コーディネーターによって作成された有効なトランザクション。[ノード](#iota-terms)はマイルストーンを使用して、他のトランザクションが確定済み状態にあるかどうかを判断します。</dd></dl>
<!-- <a name="milestone"></a><dl><dt>milestone</dt><dd>Valid transaction that is created by either the Coordinator. [Nodes](#iota-terms) use milestones to determine if other transactions are in a confirmed state.</dd></dl> -->

<dl><dt>[最小重量値（MWM）](root://iota-basics/0.1/concepts/minimum-weight-magnitude.md)</dt><dd>[PoW](#general-terms)中に実行される作業量を設定する整数。</dd></dl>
<!-- <dl><dt>[minimum weight magnitude (MWM)](root://iota-basics/0.1/concepts/minimum-weight-magnitude.md)</dt><dd>Integer that sets the amount of work that will be carried out during [PoW](#general-terms).</dd></dl> -->

<dl><dt>ペンディング</dt><dd>ネットワークから見た時にまだ確定されていないトランザクションの状態。</dd></dl>
<!-- <dl><dt>pending</dt><dd>State of a transaction when it's been seen by the network, but not yet confirmed.</dd></dl> -->

<dl><dt>再添付</dt><dd>新しいチップトランザクションを要求し、再度プルーフオブワークを行い、トランザクションをIRIノードに送信します。</dd></dl>
<!-- <dl><dt>reattach</dt><dd>Request new tip transactions, do the proof of work again, and send the transaction to an IRI node</dd></dl> -->

<dl><dt>再ブロードキャスト</dt><dd>トランザクションをIRIノードにもう一度送信します。</dd></dl>
<!-- <dl><dt>rebroadcast</dt><dd>Send a transaction to an IRI node again</dd></dl> -->

<dl><dt>ソリッド</dt><dd>IRIノードが自分の台帳に、対象となるトランザクションのすべての履歴（直接的および間接的に参照するすべてのトランザクション）を持っているときのトランザクションの状態。</dd></dl>
<!-- <dl><dt>solid</dt><dd>State of a transaction when an IRI node has its entire history (all directly and indirectly referenced transactions) in its ledger</dd></dl> -->

<dl><dt>末尾トランザクション</dt><dd>バンドル内のトランザクション0（インデックスが0のトランザクション）。[IRIノード](#iota-terms)は、各末尾トランザクションのトランクトランザクションを辿ることでバンドルを再構築し検証します。</dd></dl>
<!-- <dl><dt>tail transaction</dt><dd>Transaction 0 in a bundle. [IRI nodes](#iota-terms) reconstruct and validate bundles by traversing the trunk transaction of each tail transaction.</dd></dl> -->

<dl><dt>チップ選択</dt><dd>[IRIノード](#iota-terms)が2つの[チップトランザクション](#tip-transaction)を選択してクライアントに返し、新規トランザクションの[ブランチトランザクション](#branch)と[トランクトランザクション](#trunk)として使用する処理。</dd></dl>
<!-- <dl><dt>tip selection</dt><dd>Process whereby an [IRI node](#iota-terms) selects two [tip transactions](#tip-transaction) and returns them to a client for use as a new transaction's [branch transaction](#branch) and [trunk transaction](#trunk)</dd></dl> -->

<a name="tip-transaction"></a><dl><dt>チップトランザクション</dt><dd>承認者がいないトランザクション。</dd></dl>
<!-- <a name="tip-transaction"></a><dl><dt>tip transaction</dt><dd>Transaction with no approvers</dd></dl> -->

<dl><dt>トランザクション</dt><dd>IOTAトークンまたはデータを転送するために[IRIノード](#iota-terms)に送信される命令。トランザクションは、IOTAプロトコルに準拠した方法で構造化されている必要があります。</dd></dl>
<!-- <dl><dt>transaction</dt><dd>Instruction sent to an [IRI node](#iota-terms) to transfer value or data. Transactions must be structured in a way that conforms to the IOTA protocol.</dd></dl> -->

<a name="trunk"></a><dl><dt>トランクトランザクション</dt><dd>別のトランザクションによって参照されているトランザクション。バンドル内のトランザクションは、トランクトランザクションを介して互いに接続されています。この接続により、IRIノードがバンドル内のトランザクションを検証するのが速くなります。</dd></dl>
<!-- <a name="trunk"></a><dl><dt>trunk transaction</dt><dd>Transaction that is referenced by another transaction. Transactions in a bundle are connected to each other through the trunk transaction. This connection makes it faster for IRI nodes to validate transactions in a bundle.</dd></dl> -->

<dl><dt>重み付きランダムウォーク</dt><dd>[ノード](#iota-terms)が台帳のチップトランザクションへのパスを見つけるためにチップ選択時に使用するアルゴリズム。</dd></dl>
<!-- <dl><dt>weighted random walk</dt><dd>Algorithm that a [node](#iota-terms) uses during tip selection to find a path to a tip transaction in the ledger.</dd></dl> -->

## 3進法
<!-- ## Trinary -->

<dl><dt>3進法</dt><dd>3を底とし、底の冪の和で数を表現する方法。</dd></dl>
<!-- <dl><dt>trinary</dt><dd>Base-3 numeral system</dd></dl> -->

<dl><dt>トリット</dt><dd>3進コンピューターの処理の基本単位。トリットはしばしば1、0、-1で表されます。</dd></dl>
<!-- <dl><dt>trit</dt><dd>Digits in a trinary number system. Trits are often represented by 1, 0, -1</dd></dl> -->

<dl><dt>トライト</dt><dd>1トライトは27の値を表すことができる3トリットです。したがって、27文字が必要で、IOTAでは'ABCDEFGHIJKLMNOPQRSTUVWXYZ9'を使います。</dd></dl>
<!-- <dl><dt>tryte</dt><dd>A tryte is 3 trits which can represent 27 values. Thus, 27 characters are needed: 'ABCDEFGHIJKLMNOPQRSTUVWXYZ9'</dd></dl> -->
