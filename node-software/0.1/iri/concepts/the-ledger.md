# 台帳
<!-- # The ledger -->

**ノードは、トランザクションを台帳と呼ばれるローカルの追加専用の RocksDB データベースに追加することで、トランザクションをタングルに添付します。**
<!-- **Nodes attach transactions to the Tangle by appending them to their local append-only RocksDB database called the ledger.** -->

ノードが新しいトランザクションを受信すると、その新しいトランザクションの子トランザクション（履歴）を持っているかどうかを確認します。ノードが子トランザクションを持っていない場合は、ノードは凝固と呼ばれるプロセスを通して、新しく受け取ったトランザクションのために隣接ノードに尋ね始めます。
<!-- When a node receives a new transaction, it checks that it has the history its children. If the node is missing any transactions, it starts to ask its neighbors for them in a process called solidification. -->

## 凝固
<!-- ## Solidification -->

凝固は、ノードが[タングル](root://dev-essentials/0.1/concepts/the-tangle.md#milestones)内のすべての[マイルストーン](root://dev-essentials/0.1/concepts/the-tangle.md)の履歴を受信するプロセスです。
<!-- Solidification is the process by which a node receives the history of all [milestones](root://dev-essentials/0.1/concepts/the-tangle.md#milestones) in the [Tangle](root://dev-essentials/0.1/concepts/the-tangle.md). -->

ノードが実行を開始すると、**エントリポイントのマイルストーン**から始めて最新のマイルストーンまでの各マイルストーンが参照するトランザクション（マイルストーンの履歴）のリクエストを開始します。
<!-- When a node starts running, it starts to request the transactions that each milestone references (its history), starting from an **entry point milestone** and ending at the latest one. -->

:::info:
参照はトランザクションの [`branchTransaction` と `trunkTransaction` フィールド](root://dev-essentials/0.1/references/structure-of-a-transaction.md)で定義されています。
:::
<!-- :::info: -->
<!-- References are defined in a transaction's [`branchTransaction` and `trunkTransaction` fields](root://dev-essentials/0.1/references/structure-of-a-transaction.md). -->
<!-- ::: -->

ノードにマイルストーンのブランチトランザクションとトランクトランザクションがある場合、ノードはマイルストーンのブランチトランザクションとトランクトランザクションが参照するすべてのトランザクションをリクエストし始めます。このプロセスは、ノードがエントリポイントのマイルストーンに達するまで各トランザクションに対して継続します。この時点で、ノードはそのマイルストーンを**凝固**したとしてマークし、次のマイルストーンからプロセスを再開します。
<!-- When a node has the milestone's branch and trunk, it starts to request all the transactions that those transactions reference. This process continues for each transaction until the node reaches the entry point milestone. At this point, the node marks that milestone as **solid**, and starts the process again from the next one. -->

エントリポイントのマイルストーンが古いほど、凝固にかかる時間が長くなります。
<!-- The older the entry point milestone, the longer solidification takes. -->

## ノードはいつ同期するか？
<!-- ## When is a node synchronized? -->

IRI ノードは、最新のマイルストーンまですべてのマイルストーンを凝固にした時点で同期したと見なされます。
<!-- An IRI node is considered synchronized when it has solidified all the milestones up to the latest one. -->

`latestMilestoneIndex` フィールドが `latestSolidSubtangleMilestoneIndex` フィールドと等しいことを確認することで、ノードが同期しているかどうかを調べることができます。
<!-- You can find out if a node is synchronized by checking that its `latestMilestoneIndex` field is equal to the `latestSolidSubtangleMilestoneIndex` field. -->

`latestMilestoneIndex` フィールドは、IRI が近隣 IRI ノードから受け取った最新のマイルストーンのインデックスです。
<!-- The `latestMilestoneIndex` field is the index of the latest milestone that the IRI has received from its neighbors. -->

`latestSolidSubtangleMilestoneIndex` フィールドは、IRI ノードがマイルストーンを凝固（マイルストーンが直接および間接的に参照するすべてのトランザクションを IRI ノードが持った状態）にした最新のマイルストーンのインデックスです。
<!-- The `latestSolidSubtangleMilestoneIndex` field is the index of the latest milestone for which the IRI node has all the transactions that the milestone directly and indirectly references. -->

:::info:
`getNodeInfo` API エンドポイントは `latestMilestoneIndex` と `latestSolidSubtangleMilestoneIndex` の情報を返します。この情報を見るために [IRI ノードと対話](../how-to-guides/interact-with-an-iri-node.md)してみてください。
:::
<!-- :::info: -->
<!-- The `getNodeInfo` API endpoint returns this information. Try [interacting with an IRI node](../how-to-guides/interact-with-an-iri-node.md) to see this information. -->
<!-- ::: -->
