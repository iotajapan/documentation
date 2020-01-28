# スナップショットメタデータファイルのデータ構造
<!-- # Data in the snapshot metadata file -->

**この表には、[ローカルスナップショット](root://getting-started/0.1/network/nodes.md#local-snapshots)中に `snapshot.meta` ファイルに追加されるデータが含まれています。**
<!-- **This table contains the data that's added to the snapshot.meta file during a [local snapshot](root://getting-started/0.1/network/nodes.md#local-snapshots).** -->

| **データ** | **説明** |
| :--------: | :------: |
| ブートストラップマイルストーンハッシュ | IRI がローカルスナップショットを開始したマイルストーントランザクションのハッシュ |
| ブートストラップマイルストーンインデックス | IRI がローカルスナップショットを開始したマイルストーントランザクションのインデックス |
| Unix タイプスタンプ | スナップショットファイルが作成された時点のUnixエポック |
| <a name="solid-entry-point"></a>凝固エントリポイントの総数 | スナップショット中にIRIが自身の台帳に全履歴があると確認したトランザクション。IRI ノードは凝固エントリポイントを参照すると、トランザクションの凝固を停止します。 |
| <a name="seen-milestone"></a>シーンマイルストーンの総数 | ブートストラップマイルストーンよりも新しいマイルストーン。シーンマイルストーンの数は、[`LOCAL_SNAPSHOTS_DEPTH`](../references/iri-configuration-options.md#local-snapshots-depth) 構成オプションと同じです。 |
| 凝固エントリーポイントのリスト | セミコロンで区切ったトランザクションハッシュのリストとそのトランザクションハッシュを凝固にしたマイルストーンのインデックス |
| シーンマイルストーンのリスト | セミコロンで区切ったシーンマイルストーントランザクションハッシュのリスト |
