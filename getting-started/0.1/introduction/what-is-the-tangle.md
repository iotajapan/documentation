# タングルとは？
<!-- # What is the Tangle? -->

** タングルは、IOTAトランザクションの履歴を含むイミュータブルなデータ構造です。IOTAネットワーク内のすべてのノードは、台帳にタングルのコピーを保存し、そこから読み取って新しいトランザクションをタングルに添付することができます。**
<!-- **The Tangle is the immutable data structure that contains a history of IOTA transactions. All nodes in an IOTA network store a copy of the Tangle in their ledgers and can read from it and attach new transactions to it.** -->

タングル内のトランザクションは、トランザクション内容が他の2つのトランザクションの履歴を暗号学的に参照しているためイミュータブルです。したがって、その履歴の中で何らかのトランザクションが変更されると、すべての参照が壊れてしまいます。
<!-- Transactions in the Tangle are immutable because their contents are cryptographically referenced to the history of two other transactions. So, if any transaction were to change in that history, all the references would be broken. -->

![A directed acyclic graph](../images/dag.png)

この図では、番号が付けられた各ボックスがトランザクションです。トランザクション5はトランザクション2と3を**直接**参照し、トランザクション6はトランザクション3を介して**間接的に**トランザクション5を参照します。
<!-- In this diagram, each numbered box is a transaction. Transaction 5 **directly** references transactions 2 and 3, and transaction 6 **indirectly** references transaction 3 (through transaction 5). -->

## トランザクションの確定方法
<!-- ## How a transaction becomes confirmed -->

タングル内のトランザクションは、ペンディングまたは確定済みの2つの状態のいずれかになります。
<!-- Transactions in the Tangle can be in one of two states: Pending or confirmed. -->

バンドルをノードに送信すると、そのバンドルはペンディング状態になり、ノードは確定されるまで影響を受けるアドレスの残高を更新しません。
<!-- When you send a bundle to a node, that bundle is pending and the node doesn't update the balances of the affected addresses until it's confirmed. -->

ペンディング状態から確定済み状態に移行するには、全ノードがトランザクションの状態について合意に達する必要があります。
<!-- To go from a pending state to a confirmed state, nodes must reach consensus on the state of a transaction. -->

現時点では、全ノードは**マイルストーン（コーディネータによって作成および送信されたトランザクション）によって直接的または間接的に参照された**トランザクションについてのみ合意に達します。
<!-- At the moment, nodes reach a consensus on transactions that are **directly or indirectly referenced by a milestone** (transaction that's created and sent by the Coordinator). -->

:::info:
トランザクション6がマイルストーンの場合、トランザクション5、3、2、および1が確定されます。
:::
<!-- :::info: -->
<!-- If transaction 6 were a milestone, then transaction 5, 3, 2, and 1 would all be confirmed. -->
<!-- ::: -->

[タングル](root://dev-essentialsiota-basics/0.1/concepts/the-tangle.md)についてさらに学びます。
<!-- Learn more about [the Tangle](root://dev-essentials/0.1/concepts/the-tangle.md). -->
