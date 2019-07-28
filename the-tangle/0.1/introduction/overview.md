# タングル概要
<!-- # The Tangle overview -->

**バンドルを送信する前に、トランザクションをネットワーク内の2つの既存のトランザクションに添付する必要があります。これを行うと、ノードは接続を辿って各トランザクションを見つけて検証できます。これらの接続は、タングルと呼ばれるデータ構造を形成します。 タングル内のトランザクションは、ペンディングまたは確定済みの2つの状態のいずれかになります。**
<!-- **Before you send a bundle, you must attach your transactions to two existing ones in the network. You do this so that the node can walk on the connections to find and validate each transaction. These connections form a data structure that's called the Tangle. Transactions in the Tangle can be in one of two states: Pending or confirmed.** -->

タングルを構成するデータ構造は一種の[有向非巡回グラフ](https://en.wikipedia.org/wiki/Directed_acyclic_graph)（DAG）であり、2015年にSerguei Popov教授によってIOTAホワイトペーパーで正式に導入されました。
<!-- The data structure that forms the Tangle is a type of [directed acyclic graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph) (DAG), and it was formally introduced in the IOTA whitepaper by Professor Serguei Popov in 2015. -->

タングルでは、トランザクションはそれぞれの[`branchTransaction`フィールドと`trunkTransaction`フィールド](root://iota-basics/0.1/references/structure-of-a-transaction.md)を介した参照によって互いに接続されています。これらのフィールドには、同じバンドル内のトランザクション、または[チップ選択](../concepts/tip-selection.md)中に返されたチップトランザクションのいずれかのトランザクションハッシュが含まれます。
<!-- In the Tangle, transactions are connected to each other by reference through their [`branchTransaction` and `trunkTransaction` fields](root://iota-basics/0.1/references/structure-of-a-transaction.md). These fields contain the transaction hash of either a transaction in the same bundle or a tip transaction that was returned during [tip selection](../concepts/tip-selection.md). -->

この参照は家系図を形成します。これにより、トランザクションが**子**の場合、ブランチトランザクションとトランクトランザクションがその**親**になります。
<!-- References form a family tree, whereby if a transaction is a **child**, the branch and trunk transactions are its **parents**. -->

![A directed acyclic graph](../images/dag.png)

上図では、トランザクション6はトランザクション5を直接参照しているため、トランザクション5はトランザクション6の**親**です。一方、トランザクション6はトランザクション3を間接的に参照しているため、トランザクション3はトランザクション6の**祖父母**です。
<!-- In this diagram, transaction 6 directly references transaction 5, so transaction 5 is a **parent** of transaction 6. On the other hand, transaction 6 indirectly references transaction 3, so, transaction 3 is a **grandparent** of transaction 6. -->

チップ選択を行うとノードがバンドルを検証するため、あるトランザクションを直接または間接的に参照するすべてのトランザクションは、あるトランザクションとそれまでの全履歴を承認します。
<!-- Because tip selection causes nodes to validate bundles, any transaction that directly or indirectly references other transactions approves them and their entire history. -->

:::info:
上図のトランザクション0はジェネシストランザクションで、これはタングルの最初のトランザクションです。
:::
<!-- :::info: -->
<!-- Transaction 0 is the genesis transaction, which is the very first transaction in the Tangle. -->
<!-- ::: -->

## コンセンサス
<!-- ## Consensus -->

IOTAでは、ノードはアドレスの残高を更新する前に、トランザクションがいつ確定済みと見なされるかについて、合意に達する必要があります。
<!-- In IOTA, the nodes must reach a consensus about when a transaction can be considered confirmed before they can update the balances of addresses. -->

現在のIOTAでは、トランザクションが[コーディネーター](../concepts/the-coordinator.md)発行のマイルストーンによって直接または間接的に参照されている場合、そのトランザクションは確定済みと見なされます。
<!-- A transaction is considered confirmed when it's directly or indirectly referenced by a [Coordinator](../concepts/the-coordinator.md)-issued milestone. -->

## 更なる研究
<!-- ## Further research -->

IOTA財団はタングルとそれに関連するプロトコルの開発に焦点を当てている活発な研究部を持っています。
<!-- We have an active research department that focuses on developing the Tangle and its related protocols. -->

:::info:Coordicide
現時点で、IOTA財団は[Coordicide](https://coordicide.iota.org/)と呼ばれるプロジェクトに焦点を当てています。これが起こるとき、ノードはマイルストーンなしで合意に達することができるでしょう。
:::
<!-- :::info:Coordicide -->
<!-- At the moment, we are focused on a project called [Coordicide](https://coordicide.iota.org/), which is our proposal for the removal of the Coordinator. When this happens, nodes will be able to reach a consensus without milestones. -->
<!-- ::: -->

* [学術論文](https://www.iota.org/research/academic-papers)
<!-- * [Academic Papers](https://www.iota.org/research/academic-papers) -->
* [ロードマップ](https://www.iota.org/research/roadmap)
<!-- * [Roadmap](https://www.iota.org/research/roadmap) -->
