# 台帳からトランザクションを刈り取る
<!-- # Prune transactions from the ledger -->

**時間が経つにつれて，IRI ノードの台帳には多くのトランザクションが蓄積され，多くの場合，IRI ノードの使用可能メモリよりも大きくなります．台帳が大きくなり過ぎないようにするために，定期的に台帳から古いトランザクションを削除することを選択できます．**
<!-- **Over time, the ledger of an IRI node accumulates many transactions, which often cause it to become larger than the IRI node's available memory. To stop the ledger from becoming too large, you can choose to delete old transactions from the ledger at regular intervals.** -->

次の計算の結果よりも古いマイルストーントランザクションによってトランザクションが確定された場合にのみ，トランザクションは刈り取られます．
<!-- Transactions are pruned only if they were confirmed by a milestone transaction that is older than the result of the following calculation: -->

現在のマイルストーンインデックス - ([`LOCAL_SNAPSHOTS_DEPTH`](../references/iri-configuration-options.md#local-snapshots-depth) +
[`LOCAL_SNAPSHOTS_PRUNING_DELAY`](../references/iri-configuration-options.md#local-snapshots-pruning-delay))
<!-- current milestone index - ([`LOCAL_SNAPSHOTS_DEPTH`](../references/iri-configuration-options.md#local-snapshots-depth) + -->
<!-- [`LOCAL_SNAPSHOTS_PRUNING_DELAY`](../references/iri-configuration-options.md#local-snapshots-pruning-delay)) -->

## ノードを設定する
<!-- ## Configure your node -->

設定オプションを変更する前に，IRI を停止する必要があります．
<!-- You must stop the IRI before making changes to the configuration options. -->

1. [`LOCAL_SNAPSHOTS_ENABLED` および `LOCAL_SNAPSHOTS_PRUNING_ENABLED` 構成オプション](../references/iri-configuration-options.md#local-snapshots)が `true` に設定されていることを確認します．
  <!-- 1. Make sure that the [`LOCAL_SNAPSHOTS_ENABLED` and the `LOCAL_SNAPSHOTS_PRUNING_ENABLED` configuration options](../references/iri-configuration-options.md#local-snapshots) are set to `true` -->

2. `LOCAL_SNAPSHOTS_PRUNING_DELAY` および `LOCAL_SNAPSHOTS_DEPTH` 構成オプションの値を変更します．
  <!-- 2. Change the value of the `LOCAL_SNAPSHOTS_PRUNING_DELAY` and the `LOCAL_SNAPSHOTS_DEPTH` configuration options -->

マイルストーンは約2分ごとに送信されます．したがって，次の公式を使用して，新しいトランザクションが台帳に残る日数を計算します．
  <!-- Milestones are sent approximately every two minute. So, use the following formula to calculate the number of days that new transactions will remain in the ledger: -->

- **総マイルストーンインデックス：**`LOCAL_SNAPSHOTS_PRUNING_DELAY` + `LOCAL_SNAPSHOTS_DEPTH`
<!-- - **Total milestone index:** `LOCAL_SNAPSHOTS_PRUNING_DELAY` + `LOCAL_SNAPSHOTS_DEPTH` -->

- **分単位の総時間：**総マイルストーンインデックス / 120
<!-- - **Total amount of time in minutes:** Total milestone index / 120 -->

- **日単位の総時間：** 分単位の合計時間 / 24
<!-- - **Total amount of time in days:** Total amount of time in minutes / 24 -->
