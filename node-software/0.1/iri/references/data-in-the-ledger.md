# 台帳内のデータ構造
<!-- # Data in the ledger -->

**この表には，有効なトランザクションごとに IRI ノードが自身の台帳に追加するデータが含まれています．**
<!-- **This table contains the data that an IRI node appends to its ledger for each valid transaction.** -->

| **カラム** | **データ** | **説明** |
| :--------: | :--------: | :------- |
| `transaction` | トランザクションハッシュ | トランザクションのトランザクションバイト |
| `transactionMetadata` | トランザクションハッシュ | トランザクションのデータとIRIによって追加された追加のメタデータ |
| `milestone` | マイルストーンインデックス | トランザクションを承認するマイルストーンバンドル内の末尾トランザクションハッシュとトランザクションのインデックスの両方 |
| `stateDiffs` | マイルストーンハッシュ | マイルストーンが台帳で確定したアドレスの残高の変更 |
| `address` | アドレスハッシュ | アドレスの残高を変更したトランザクションハッシュのリスト |
| `approvees` | トランザクションハッシュ | トランザクションを直接参照するトランザクションのトランザクションハッシュ |
| `bundle` | バンドルハッシュ | トランザクションのバンドルに属するトランザクションハッシュのリスト |
| `obsoleteTag` | 痕跡タグ | バンドルエッセンス（署名される部分）の一部である，同じ廃止タグを持つトランザクションハッシュのリスト |
| `tag` | タグ | 同じタグを持つトランザクションハッシュのリスト．このタグはバンドルエッセンスの一部ではありません（このタグは署名されない部分）． |

## トランザクションメタデータ
<!-- ## Transaction metadata -->

次の表に，IRI ノードが `transactionMetadata` カラムに追加するメタデータを示します．
<!-- The following table contains the metadata that an IRI node appends to the `transactionMetadata` column. -->

| **フィールド** | **データ** | **説明** |
| :------------- | :--------- | :------- |
| `validity` | -1か0か1 | この値は[バンドルバリデータ](../concepts/transaction-validation.md#bundle-validator)によって設定されます．値の意味は-1（無効），1（有効），0（不明）です． |
| `type` | `PREFILLED_SLOT` か `FILLED_SLOT` | `PREFILLED_SLOT` はトランザクションハッシュが存在することを意味しますが，ノードは自身の台帳にそのトランザクションハッシュを持っていません．`FILLED_SLOT` は，ノードの台帳にトランザクションハッシュがあることを意味します． |
| `arrivalTime` | integer | この値は，IRI ノードがトランザクションを受信した時間（ミリ秒）です． |
| `parsed` | boolean | この値は，メタデータがパースされたかどうかを確認するために内部的に使用されます． |
| `solid` | boolean | このトランザクションとこのトランザクションが参照するすべてのトランザクションの履歴が台帳にある場合にのみ，この値は `true` に設定されます． |
| `sender` | URL か IP アドレス | トランザクションを送信したノードの URL または IP アドレス |
| `snapshot` | マイルストーンインデックス | トランザクションを確定した最初のマイルストーントランザクションのマイルストーンインデックス |

<!-- |**Field**|**Data** |**Description**| -->
<!-- |:--------|:---------------|:-------| -->
<!-- |`validity`|-1, 0, or 1|This value is set by the [bundle validator](../concepts/transaction-validation.md#bundle-validator). The value can be -1 (invalid), 1 (valid), 0 (unknown)| -->
<!-- |`type`|`PREFILLED_SLOT` or `FILLED SLOT` |`PREFILLED_SLOT` means that the transaction hash exists, but the node doesn't have it in its ledger. `FILLED_SLOT` means that the node has the transaction hash in its ledger.| -->
<!-- |`arrivalTime`|integer |This value is the time in milliseconds that the node received the transaction| -->
<!-- |`parsed`|boolean |This value is used internally to check whether the metadata has been parsed| -->
<!-- |`solid`|boolean|This value is set to `true` only if this transaction and the history of all transactions that it references are in the ledger| -->
<!-- |`sender`|URL or IP address|URL or the IP address of the node that sent the transaction| -->
<!-- |`snapshot`|Milestone index|Milestone index of the first milestone transaction that confirmed this transaction| -->
