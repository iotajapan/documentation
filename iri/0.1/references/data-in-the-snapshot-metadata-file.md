# スナップショットメタデータファイルのデータ
<!-- # Data in the snapshot metadata file -->

**この表には、[ローカルスナップショット](../concepts/local-snapshot.md)中にsnapshot.metaファイルに追加されるデータが含まれています。**
<!-- **This table contains the data that's added to the snapshot.meta file during a [local snapshot](../concepts/local-snapshot.md).** -->

| **データ** | **説明** |
| :--------: | :------: |
| ブートストラップマイルストーンハッシュ | IRIがローカルスナップショットを開始したマイルストーントランザクションのハッシュ |
| ブートストラップマイルストーンインデックス | IRIがローカルスナップショットを開始したマイルストーントランザクションのインデックス |
| Unixタイプスタンプ | スナップショットファイルが作成された時刻 |
| <a name="solid-entry-point"></a>凝固エントリポイントの総数 | スナップショット時点で、IRIがすべての承認トランザクションを台帳に持っていることを確定したトランザクション。IRIノードは凝固エントリポイントを参照すると、トランザクションの凝固を停止します。 |
| <a name="seen-milestone"></a>シーンマイルストーンの総数 | ブートストラップマイルストーンよりも新しいマイルストーン。シーンマイルストーンの数は、[`LOCAL_SNAPSHOTS_DEPTH`](../references/iri-configuration-options.md#local-snapshots-depth)設定オプションと同じです。 |
| 凝固エントリーポイントのリスト | セミコロンで区切ったトランザクションハッシュのリストとそのトランザクションハッシュを凝固にしたマイルストーンのインデックス |
| シーンマイルストーンのリスト | セミコロンで区切ったシーンマイルストーントランザクションハッシュのリスト |
