# ローカルスナップショット
<!-- # Local snapshot -->

**ローカルスナップショットは、IRIノードが台帳の状態をスナップショットファイルに記録するプロセスです。 IRIノードがスナップショットの実行を開始するには、特定のマイルストーンから始めて、隣接ノードと台帳を同期させる必要があります。 IRIの各バージョンには、ノードがスナップショットを開始できるハードコードされたマイルストーンがあります。より速く同期するために、IRIノードはローカルスナップショットファイルを使用することを選択できます。**
<!-- **A local snapshot is the process in which an IRI node records the state of its ledger in snapshot files. When an IRI node starts running, it must synchronize its ledger with its neighbors, starting from a particular milestone. Each version of the IRI has a hard-coded milestone that nodes can start from. To synchronize faster, an IRI node can choose to use local snapshot files.** -->

:::info:
ローカルスナップショットは、IRIのバージョン1.6.0以降でのみ使用可能です。
:::
<!-- :::info: -->
<!-- Local snapshots are available only in version 1.6.0 and later of the IRI. -->
<!-- ::: -->

## スナップショットファイル
<!-- ## Snapshot files -->

スナップショットファイルは、定期的にIRIノードの台帳の状態を保存します。
<!-- Snapshot files store the state of an IRI node's ledger at regular intervals. -->

IRIノードが同期している場合、[`LOCAL_SNAPSHOTS_INTERVAL_SYNCED`設定オプション](../references/iri-configuration-options.md#local-snapshots-interval-synced)で定義されているマイルストーン間隔でスナップショットファイルが作成されます。
<!-- If an IRI node is synchronized, it creates snapshot files at the milestone intervals that are defined in the [`LOCAL_SNAPSHOTS_INTERVAL_SYNCED`](../references/iri-configuration-options.md#local-snapshots-interval-synced) configuration option. -->

IRIノードが同期していない場合は、[`LOCAL_SNAPSHOTS_INTERVAL_UNSYNCED`設定オプション](../references/iri-configuration-options.md#local-snapshots-interval-unsynced)で定義されているマイルストーン間隔でスナップショットファイルが作成されます。
<!-- If an IRI node isn't synchronized, it creates snapshot files at the milestone intervals that are defined in the [`LOCAL_SNAPSHOTS_INTERVAL_UNSYNCED`](../references/iri-configuration-options.md#local-snapshots-interval-unsynced) configuration option. -->

:::info:
各間隔で、スナップショットファイルは上書きされます。
:::
<!-- :::info: -->
<!-- At each interval, the snapshot file is overwritten. -->
<!-- ::: -->

ローカルスナップショットにより、次のスナップショットファイルが作成されます。
<!-- Local snapshots result in the following snapshot files: -->
- **snapshot.meta：** [IRIが自身の台帳と隣接IRIノードとの同期を開始するために使用するトランザクションデータ](../references/data-in-the-snapshot-metadata-file.md)。
<!-- - **snapshot.meta:** [Transaction data that the IRI uses to start synchronizing its ledger with neighbor IRI nodes](../references/data-in-the-snapshot-metadata-file.md) -->
- **snapshot.state：** ローカルスナップショット時に0より大きい残高を持つすべてのアドレスの一覧。
<!-- - **snapshot.state:** A list of all addresses that have a balance greater than 0 at the time of the local snapshot. -->

:::info:
これらのファイルは [`LOCAL_SNAPSHOTS_BASE_PATH`](../references/iri-configuration-options.md#local-snapshots-base-path)設定オプションのパスにあります。
:::
<!-- :::info: -->
<!-- These files are located in the path of the [`LOCAL_SNAPSHOTS_BASE_PATH`](../references/iri-configuration-options.md#local-snapshots-base-path) configuration option. -->
<!-- ::: -->

起動時に、IRIノードはスナップショットファイルをエントリポイントとして使用して、自身の台帳を隣接ノードと同期させることができます。
<!-- On startup, IRI nodes can use the snapshot files as an entry point to synchronize their ledgers with their neighbors. -->

## ローカルスナップショットのシナリオ例
<!-- ## Example scenario of a local snapshot -->

**設定パラメータ：**
<!-- **Configuration parameter:** -->

- `LOCAL_SNAPSHOTS_DEPTH` = 100
- `LOCAL_SNAPSHOTS_INTERVAL_SYNCED` = 10

**現在のマイルストーンインデックス：**
<!-- **Current milestone index:** -->

- 990, 100

このシナリオでは、IRIノードは同期されています。したがって、マイルストーン990,110では、IRIノードは次のことを行います。
<!-- In this scenario, the IRI node is synchronized. So, at milestone 990, 110, the node will do the following: -->

- 前の100個のマイルストーンを取得し、それらを[seen milestones](../references/data-in-the-snapshot-metadata-file.md#seen-milestone)としてsnapshot.metaファイルに追加する。
<!-- - Take the previous 100 milestones and add them to the snapshot.meta file as [seen milestones](../references/data-in-the-snapshot-metadata-file.md#seen-milestone) -->
- 凝固トランザクションを見つけて、それらを[solid entry points](../references/data-in-the-snapshot-metadata-file.md#solid-entry-point)としてsnapshot.metaファイルに追加する。
<!-- - Find the solid transactions and add them as [solid entry points](../references/data-in-the-snapshot-metadata-file.md#solid-entry-point) -->
- すべてのアドレスとその残高のリストをsnapshot.stateファイルに追加する。
<!-- - In the snapshot.state file, add a list of all addresses and their balances -->

IRIノードを再起動すると、スナップショットファイルをエントリポイントとして使用して台帳を同期できます。
<!-- When the IRI node restarts, it can use the snapshot files as the entry point to synchronize its ledger. -->

## トランザクションの刈り取り
<!-- ## Transaction pruning -->

ローカルスナップショットの間、IRIノードは、古いマイルストーンによって確定されたトランザクションを台帳から刈り取ることができます。
<!-- During a local snapshot, an IRI node can prune transactions from its ledger if they were confirmed by an old milestone. -->

古いマイルストーンは、[`LOCAL_SNAPSHOTS_DEPTH`](../references/iri-configuration-options.md#local-snapshots-depth)および[`LOCAL_SNAPSHOTS_PRUNING_DELAY`](../references/iri-configuration-options.md#local-snapshots-pruning-delay)設定オプションを合計した値より大きいインデックスを持ちます。
<!-- An old milestone is one that has an index greater than the combined value of the [`LOCAL_SNAPSHOTS_DEPTH`](../references/iri-configuration-options.md#local-snapshots-depth) and [`LOCAL_SNAPSHOTS_PRUNING_DELAY`](../references/iri-configuration-options.md#local-snapshots-pruning-delay) configuration options. -->

## トランザクションの刈り取りのシナリオ例
<!-- ## Example scenario of transaction pruning -->

**設定パラメータ：**
<!-- **Configuration parameters:** -->

- `LOCAL_SNAPSHOTS_PRUNING_DELAY` = 50,000
- `LOCAL_SNAPSHOTS_DEPTH` = 100

**現在のマイルストーンインデックス：**
<!-- **Current milestone index:** -->

- 990, 100

このシナリオでは、`LOCAL_SNAPSHOTS_PRUNING_DELAY` + `LOCAL_SNAPSHOTS_DEPTH`の合計は50,100です。したがって、IRIノードは940,000（990,100 - 50,100）より小さいインデックスを持つマイルストーンによって確定されたトランザクションを刈り取ります。その結果、マイルストーン940,000と990,100の間のすべてのトランザクションが台帳に保持されます。
<!-- In this scenario, the sum of `LOCAL_SNAPSHOTS_PRUNING_DELAY` + `LOCAL_SNAPSHOTS_DEPTH` is 50, 100. Therefore, an IRI node will prune transactions that were confirmed by any milestone with an index lower than 940, 000 (990, 100 - 50,100). As a result all transactions between milestones 940, 000 and 990, 100 will be kept in the ledger. -->
