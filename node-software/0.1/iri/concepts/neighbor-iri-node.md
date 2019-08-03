# 隣接IRIノード
<!-- # Neighbor IRI node -->

** 隣接ノードは、相互に接続され、同じIOTAネットワーク上で互いに直接通信するIRIノードです。IRIの台帳をネットワークの他の部分と同期させるには、すべてのIRIノードが隣接ノード間でトランザクションを送受信する必要があります。**
<!-- **Neighbors are IRI nodes that are mutually connected and that communicate directly with each other on the same IOTA network. To synchronize their ledgers with the rest of the network, all IRI nodes must send and receive transactions among their neighbors.** -->

IRIノードが隣接IRIノードに接続する主な理由は、互いのトランザクションを検証することです。すべてのクライアントが、どの近隣ノードにも接続されていない1つのIRIノードだけに依存している場合、そのIRIノードは無効なトランザクションの検証を開始できます。
<!-- The primary reason that IRI nodes connect to neighbor IRI nodes is to validate each other's transactions. If all clients relied on only one IRI node, which wasn't connected to any neighbors, that IRI node could start validating invalid transactions. -->

したがって、IRIノードは、受信したすべてのトランザクションを隣接IRIノードに送信して、それらの隣接ノードが同じトランザクションを検証および保存できるようにします。
<!-- Therefore, IRI nodes send all transactions that they receive to their neighbor IRI nodes so that those neighbors can validate and store the same transactions. -->

すべてのIRIノードがそれぞれの台帳に同じトランザクションを持っていると、合意に達しています。この合意は、クライアントが任意のIRIノードに接続した時に、同じ台帳にアクセスできるようにする分散型元帳を形成します。
<!-- When all IRI nodes have the same transactions in their ledgers, they have reached a consensus. This consensus forms the distributed ledger that allows clients to connect to any IRI node and have access to the same ledger. -->

## ゴシッププロトコル
<!-- ## Gossip protocol -->

IRIノードはゴシッププロトコルを介して隣接ノードと通信します。
<!-- IRI nodes communicate with their neighbors through a gossip protocol. -->

<dl><dt>ゴシッププロトコル</dt><dd>同じネットワーク内のコンピュータがデータを共有できるようにするピアツーピア通信プロトコル。</dd></dl>
<!-- <dl><dt>gossip protocol</dt><dd>A peer-to-peer communication protocol that allows computers in the same network to share data.</dd></dl> -->

## 非凝固トランザクション
<!-- ## Non-solid transactions -->

IRIノードは、隣接ノードにトランザクションを送信するだけでなく、同期をとるために、隣接ノードからの非凝固トランザクションをリクエストします。
<!-- As well as sending transactions to neighbors, IRI nodes request non-solid transactions from its neighbors in order to become synchronized. -->

非凝固トランザクションは、IRIノードの台帳にあるトランザクションによって参照されているトランザクションですが、IRIはまだ検証していません。
<!-- A non-solid transaction is one that is referenced by a transaction in an IRI node's ledger, but that the IRI has not yet validated. -->

[検証](../concepts/transaction-validation.md)中または[チップ選択](root://node-software/0.1/iri/concepts/tip-selection.md)中にIRIノードが非凝固トランザクションを検出した場合、IRIノードは非凝固トランザクションを隣接ノードにリクエストします。隣接ノードにその情報が無い場合、隣接ノードは隣接ノードの隣接ノードに問い合わせ、それをどんどん続けていきます。
<!-- If an IRI node sees a non-solid transaction during [validation](../concepts/transaction-validation.md) or [tip selection](root://node-software/0.1/iri/concepts/tip-selection.md), the IRI node asks its neighbors for it, and if its neighbors are missing the information, those neighbors will ask their neighbors, and so on. -->
