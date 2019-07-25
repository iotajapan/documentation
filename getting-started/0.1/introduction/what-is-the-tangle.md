# タングルとは？
<!-- # What is the Tangle? -->

**バンドルを作成するときは、ネットワーク内の2つの既存のトランザクションを参照する必要があります。ノードがトランザクションを受け取ると、ノードは受け取ったトランザクションを既存のトランザクションに添付します。すべてのトランザクション間の添付関係は、タングルと呼ばれるデータ構造を形成します。**
<!-- **When you create a bundle, you must reference it to two existing transactions in the network. When a node receives your transactions, it attach them to these existing ones. The attachments among all the transactions form a data structure that's called the Tangle.** -->

タングルは、各トランザクションが1つの頂点を表す一種の[有向非巡回グラフ](https://en.wikipedia.org/wiki/Directed_acyclic_graph)（DAG）を形成します。
<!-- This model forms a type of [directed acyclic graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph) (DAG), in which each transaction represents a vertex. -->

![A directed acyclic graph](../images/dag.png)

この図では、トランザクション5はトランザクション2と3に関連付けられています。したがって、トランザクション5はトランザクション2と3を**直接**参照しています。
<!-- In this diagram, transaction 5 is attached to transactions 2 and 3. So, transaction 5 **directly** references transactions 2 and 3. -->

トランザクション6はトランザクション5（および表示されていない別のトランザクション）に接続されています。そのため、トランザクション6はトランザクション5を介して**間接的**にトランザクション3を参照します。
<!-- Transaction 6 is attached to transaction 5 (and another transaction that's not shown). So, transaction 6 **indirectly** references transaction 3 (through transaction 5). -->

## トランザクションの確定方法
<!-- ## How a transaction becomes confirmed -->

転送バンドルをノードに送信しても、影響を受けるアドレスの残高はすぐには更新されません。
<!-- When you send a transfer bundle to a node, it doesn't update the balances of the affected addresses straight away. -->

全ノードは、バンドル内のすべてのトランザクションが確定されるまでIOTAトークンを転送しません。
<!-- Nodes do not transfer IOTA tokens until all transactions in the bundle are confirmed. -->

ペンディング状態から確定済み状態に移行するには、全ノードがトランザクションの状態について合意に達する必要があります。
<!-- To go from a pending state to a confirmed state, nodes must reach consensus on the state of a transaction. -->

現時点では、全ノードは**マイルストーン（コーディネータによって作成および送信されたトランザクション）によって直接的または間接的に参照された**トランザクションについてのみ合意に達します。
<!-- At the moment, nodes reach a consensus on transactions that are **directly or indirectly referenced by a milestone** (transaction that's created and sent by the Coordinator). -->

:::info:
トランザクション6がマイルストーンの場合、トランザクション5、3、2、および1が確定され、最終的なものと見なされます。
:::
<!-- :::info: -->
<!-- If transaction 6 were a milestone, then transaction 5, 3, 2, and 1 would all be confirmed and considered final. -->
<!-- ::: -->

[コーディネータ](root://the-tangle/0.1/concepts/the-coordinator.md)と[チップ選択](root://the-tangle/0.1/concepts/tip-selection.md)について学ぶ。
<!-- Learn more about [the Coordinator](root://the-tangle/0.1/concepts/the-coordinator.md) and [tip selection](root://the-tangle/0.1/concepts/tip-selection.md). -->
