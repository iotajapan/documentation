# 台帳
<!-- # The ledger -->

**ノードは、自分が受け取った有効なすべてのトランザクションを、組み込みの追加専用のRocksDBデータベースである台帳に格納することによって記録します。 IOTAネットワーク内の全ノードの集合データベースがタングルを構成します。**
<!-- **Nodes keep a record of all valid transactions that they receive by storing them in a ledger, which is an embedded, append-only RocksDB database. The collective database of all nodes in an IOTA network makes up the Tangle.** -->

台帳はノードの主なデータソースです。台帳のデータは、ノードが受信したすべての有効なトランザクションの完全な履歴です。
<!-- The ledger is the primary data source for a node. The data in the ledger is a complete history of all the valid transactions that a node has received. -->

## 台帳凝固
<!-- ## Ledger solidification -->

台帳凝固は、ノードが[タングル](root://the-tangle/0.1/introduction/overview.md)のすべての[マイルストーン](root://the-tangle/0.1/concepts/the-coordinator.md)の履歴を受け取るプロセスです。
<!-- Ledger solidification is the process by which a node receives the history of all [milestones](root://the-tangle/0.1/concepts/the-coordinator.md) in the [Tangle](root://the-tangle/0.1/introduction/overview.md). -->

ノードが実行を開始すると、**エントリポイントのマイルストーン**から始めて最新のマイルストーンまでの各マイルストーンが参照するトランザクション（マイルストーンの履歴）のリクエストを開始します。
<!-- When a node starts running, it starts to request the transactions that each milestone references (its history), starting from an **entry point milestone** and ending at the latest one. -->

:::info:
参照はトランザクションの[`branchTransaction`と`trunkTransaction`フィールド](root://iota-basics/0.1/references/structure-of-a-transaction.md)で定義されています。
:::
<!-- :::info: -->
<!-- References are defined in a transaction's [`branchTransaction` and `trunkTransaction` fields](root://iota-basics/0.1/references/structure-of-a-transaction.md). -->
<!-- ::: -->

ノードにマイルストーンのブランチトランザクションとトランクトランザクションがある場合、ノードはマイルストーンのブランチトランザクションとトランクトランザクションが参照するすべてのトランザクションをリクエストし始めます。このプロセスは、ノードがエントリポイントのマイルストーンに達するまで各トランザクションに対して継続します。この時点で、ノードはそのマイルストーンを**凝固**なものとしてマークし、次のマイルストーンからプロセスを再開します。
<!-- When a node has the milestone's branch and trunk, it starts to request all the transactions that those transactions reference. This process continues for each transaction until the node reaches the entry point milestone. At this point, the node marks that milestone as **solid**, and starts the process again from the next one. -->

エントリポイントのマイルストーンが古いほど、凝固にかかる時間が長くなります。
<!-- The older the entry point milestone, the longer solidification takes. -->

## 台帳はいつ同期するか？
<!-- ## When is a ledger synchronized? -->

IRIノードは、最新のものまですべてのマイルストーンを凝固なものにした時点で同期したと見なされます。
<!-- An IRI node is considered synchronized when it has solidified all the milestones up to the latest one. -->

`latestMilestoneIndex`フィールドが`latestSolidSubtangleMilestoneIndex`フィールドと等しいことを確認することで、ノードが同期しているかどうかを調べることができます。
<!-- You can find out if a node is synchronzed by checking that its `latestMilestoneIndex` field is equal to the `latestSolidSubtangleMilestoneIndex` field. -->

`latestMilestoneIndex`フィールドは、IRIが近隣IRIノードから受け取った最新のマイルストーンのインデックスです。
<!-- The `latestMilestoneIndex` field is the index of the latest milestone that the IRI has received from its neighbors. -->

`latestSolidSubtangleMilestoneIndex`フィールドは、IRIノードがマイルストーンを凝固（マイルストーンが直接および間接的に参照するすべてのトランザクションをIRIノードが持った状態）にした最新のマイルストーンのインデックスです。
<!-- The `latestSolidSubtangleMilestoneIndex` field is the index of the latest milestone for which the IRI node has all the transactions that the milestone directly and indirectly references. -->

:::info:
`getNodeInfo` APIエンドポイントは`latestMilestoneIndex`と`latestSolidSubtangleMilestoneIndex`の情報を返します。この情報を見るために[IRIノードと対話](../how-to-guides/interact-with-an-iri-node.md)してみてください。
:::
<!-- :::info: -->
<!-- The `getNodeInfo` API endpoint returns this information. Try [interacting with an IRI node](../how-to-guides/interact-with-an-iri-node.md) to see this information. -->
<!-- ::: -->
