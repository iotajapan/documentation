# タングル
<!-- # The Tangle -->

**タングルは、[トランザクション](../transactions/transactions.md)の最新の履歴を含むイミュータブルのデータ構造です。IOTA ネットワーク内のすべての[ノード](../network/nodes.md)はタングルのコピーを保存し、その内容について合意に達します。**
<!-- **The Tangle is the immutable data structure that contains an up-to-date history of [transactions](../transactions/transactions.md). All [nodes](../network/nodes.md) in an IOTA network store a copy of the Tangle and reach a consensus on its contents.** -->

タングルをイミュータブルにするために、タングル中の各トランザクションは、その[ブランチトランザクションフィールドとトランクトランザクションフィールド](../transactions/transactions.md#trunkTransaction)の[トランザクションハッシュ](../transactions/transactions.md#transaction-hash)によって前の2つのトランザクションにアタッチされます。
<!-- To make the Tangle immutable, each transaction in it is attached to two previous transactions by the [transaction hashes](../transactions/transactions.md#transaction-hash) in its [branch and trunk transaction fields](../transactions/transactions.md#trunkTransaction). -->

これらの参照は[有向非巡回グラフ](https://en.wikipedia.org/wiki/Directed_acyclic_graph)（DAG）と呼ばれるデータ構造を形成し、左側のトランザクションがシーケンスの最初に来て、右側のトランザクションが後に来ます。
<!-- These references form a data structure called a [directed acyclic graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph) (DAG), where transactions on the left come first in the sequence, and the transactions on the right come after. -->

![A directed acyclic graph](../images/dag.png)

    矢印（エッジとも呼ばれる）は、トランザクション間の参照を表します。
    <!-- The arrows (also called edges) represent the references among transactions. -->
    番号付きのボックス（頂点とも呼ばれる）はトランザクションを表します。
    <!-- The numbered boxes (also called a vertices) represent transactions -->

<a name="reference-types"></a>
## 参照タイプ
<!-- ## Reference types -->

タングル内の参照は、次の2つのタイプのいずれかです。
<!-- References in the Tangle can be one of two types: -->

- **直接参照：**トランザクションをブランチトランザクションフィールドおよびトランクトランザクションフィールドのトランザクションとの繋がり。たとえば、トランザクション5は**直接**トランザクション2およびトランザクション3を参照します。
<!-- - **Direct:** Connects a transaction with those in its branch and trunk transaction fields. For example, transaction 5 **directly** references transactions 2 and 3. -->

- **間接参照：**トランザクションをブランチトランザクションフィールドおよびトランクトランザクションフィールドのトランザクションの前にあるトランザクションとの繋がり。たとえば、トランザクション6は**間接的に**（トランザクション5を介して）トランザクション3を参照します。
<!-- - **Indirect:** Connects a transaction with those that come before the ones in its branch and trunk transaction fields. For example, transaction 6 **indirectly** references transaction 3 (through transaction 5). -->

これらの参照はトランザクションの履歴を形成します。これにより、トランザクションが**子**の場合、その直接参照は**親**になり、間接参照は**祖父母**になります。
<!-- These references form a transaction's history, whereby if a transaction is a **child**, its direct references are its **parents** and its indirect references are its **grandparents**, and so on. -->

<a name="transaction-validity"></a>
## トランザクションの有効性
<!-- ## Transaction validity -->

トランザクションは、履歴が競合しない他の2つのトランザクションを参照する場合にのみ有効です。
<!-- A transaction can be valid only if it references two other transaction's whose history does not conflict with it. -->

たとえば、トランザクション6がある[アドレス](root://getting-started/0.1/clients/addresses.md) A から [IOTA トークン](root://getting-started/0.1/clients/token.md)を10 Mi 取り出すようにノードに指示した場合、トランザクション6の親の履歴は、アドレス A へ少なくとも10 Mi が送信されているポイントに繋がっている必要があります。
<!-- For example, if transaction 6 instructs a node to withdraw 10 Mi of [IOTA tokens](root://getting-started/0.1/clients/token.md) from an [address](root://getting-started/0.1/clients/addresses.md), the history of that transaction's parents must lead to a point where that address is sent at least 10 Mi. -->

<a name="transaction-states"></a>
## トランザクションの状態
<!-- ## Transaction states -->

トランザクションはタングルの任意の部分（サブタングル）にアタッチできるため、さまざまな方向に成長できます。これらの方向のいくつかは、有効な履歴につながる可能性がありますが、他の方向は、二重支払い（同じ IOTA トークンが2つの異なるトランザクションで費やされる）などの不整合につながる可能性があります。
<!-- Because transactions can be attached to any part (subtangle) of the Tangle, it can grow in many different directions. Some of these directions may lead to a valid history, while others may lead to inconsistencies such as double spends (where the same IOTA tokens are spent in two different transactions). -->

その結果、すべてのトランザクションはペンディング状態で開始され、ノードがトランザクションが確定されたかどうかのコンセンサスに達するまでペンディング状態のままになります。
<!-- As a result, all transactions start in a pending state and stay that way until the nodes reach a consensus on whether a transaction is confirmed. -->

現時点では、ノードは、**[コーディネーター](../network/the-coordinator.md)によって作成および送信されたトランザクション（マイルストーン）によって直接または間接的に参照されている場合、トランザクションが確定されたと見なします**。
<!-- At the moment, nodes consider a transaction confirmed if it is **directly or indirectly referenced by a transaction that's created and sent by the [Coordinator](../network/the-coordinator.md)** (milestone). -->

:::info:
トランザクション6がマイルストーンの場合、トランザクション5、3、2、および1がすべて確定されます。
:::
<!-- :::info: -->
<!-- If transaction 6 were a milestone, then transaction 5, 3, 2, and 1 would all be confirmed. -->
<!-- ::: -->

<a name="tip-transactions"></a>
## チップトランザクション
<!-- ## Tip transactions -->

チップトランザクションは、他のトランザクションによってまだ参照されていないトランザクションです。チップトランザクションは、チップ選択中にノードによって選択されるのを待ち、チップ選択中にチップトランザクションの履歴が検証され、新しいチップトランザクションによって参照されます。
<!-- Tip transactions are those that are not yet referenced by others. These transactions wait to be chosen by nodes during tip selection, at which point their history will be validated and they will referenced by a new tip transaction. -->

<a name="inconsistent-subtangle"></a>
## 一貫性のないサブタングル
<!-- ## Inconsistent subtangle -->

チップトランザクションが有効でない場合、ノードはチップトランザクションを選択しません。その結果、有効でないチップトランザクションとその履歴は確定されません。これらの無効なチップトランザクションは、一貫性のないサブタングルと呼ばれることもあります。
<!-- If a tip transaction is not valid, the nodes will not select it. As a result, the transaction and its history will never be confirmed. Those invalid transaction are sometimes called an inconsistent subtangle. -->
