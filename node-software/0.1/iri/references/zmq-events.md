# ZMQ イベント
<!-- # ZMQ events -->

**この表には、IRI ノードが発行できるゼロメッセージキュー（ZMQ）イベントが含まれています。**
<!-- **This table contains the zero message queue (ZMQ) events that an IRI node can publish.** -->

すべてのイベントは、スペースで区切られたデータを含む1つ以上のバッファーオブジェクトを返します。バッファの最初の項目は常にイベントの名前です。返されたデータ列の情報は、バッファが文字列に変換され、スペース文字で配列に分割されたかのように表示されます。
<!-- All events return one or more buffer objects, which contain space-separated data. The first item in the buffer is always the name of the event. The information in the Returned data column is displayed as though the buffer had been converted to a string and split on a space character into an array. -->

| **イベントと説明** | **返ってくるデータ** |
| :----------------- | :------------------- |
| `mctn` |
| チップ選択中に通過したトランザクションの数 | <ul><li>**インデックス 1：**チップ選択中に辿ったトランザクションの総数</li></ul> |
| `lmi` |
| 最新のマイルストーンインデックス |<ul><li>**インデックス 1：**前のマイルストーンインデックス</li><li>**インデックス 2：**最新のマイルストーンインデックス</li></ul> |
| `lmsi` |
| 最新の凝固サブタングルマイルストーンインデックス | <ul><li>**インデックス 1：**前の凝固サブタングルマイルストーンのインデックス</li><li>**インデックス 2：**最新の凝固サブタングルマイルストーンのインデックス</li></ul> |
| `lmhs` |
| 最新の凝固サブタングルマイルストーンのトランザクションハッシュ | <ul><li>**インデックス 1：**マイルストーントランザクションハッシュ</li></ul> |
| `sn` |
| 最近確定したトランザクション | <ul><li>**インデックス 1：**トランザクションを確定させたマイルストーンのインデックス</li><li>**インデックス 2：**トランザクションハッシュ</li><li>**インデックス 3：**アドレス</li><li>**インデックス 4：**トランクトランザクションハッシュ</li><li>**インデックス 5：**ブランチトランザクションハッシュ</li><li>**インデックス 6：**バンドルハッシュ</li></ul> |
|`sn_trytes`|
|最近確定されたトランザクションのトランザクショントライト| <ul><li>**インデックス 1：**トランザクショントライト</li><li>**インデックス 2：**トランザクションハッシュ</li><li>**インデックス 3：**トランザクションを確定したマイルストーンのインデックス</li></ul>
| `tx_trytes` |
| 全ての新しいトランザクションのトランザクショントライト | <ul><li>**インデックス 1：**トランザクショントライト</li><li>**インデックス 2：**トランザクションハッシュ</li></ul> |
| <a name="tx"></a> `tx` |
| IRI ノードが最近台帳に追加したトランザクション| <ul><li>**インデックス 1：**トランザクションハッシュ</li><li>**インデックス 2：**アドレス</li><li>**インデックス 3：**IOTA トークンの量</li><li>**インデックス 4：**痕跡タグ</li><li>**インデックス 5：**トランザクションの`タイムスタンプ`フィールドの値</li><li>**インデックス 6：**バンドル内でのトランザクションのインデックス</li><li>**インデックス 7：**バンドル内の最後のトランザクションインデックス</li><li>**インデックス 8：**バンドルハッシュ</li><li>**インデックス 9：**トランクトランザクションハッシュ</li><li>**インデックス 10：**ブランチトランザクションハッシュ</li><li>**インデックス 11：**IRI がトランザクションを受信したときの Unix エポック</li><li>**インデックス 12：**タグ</li></ul> |
| <a name="address"></a> 81トライトのアドレス（大文字）|
| 確定済みトランザクションのために特定のアドレスを監視する | <ul><li>**インデックス 1：**アドレス</li><li>**インデックス 2：**アドレスが表示された確定済みトランザクションのトランザクションハッシュ</li><li>**インデックス 3：**トランザクションを確定させたマイルストーンのインデックス </li></ul> |

<!-- |  **Event and description** | **Returned data** -->
<!-- | :----------| :----------| -->
<!-- |`mctn`| -->
<!-- |Number of transactions that IRI traversed during tip selection| <ul><li>**Index 1:** Total number of transactions that were traversed during tip selection</li></ul> -->
<!-- |`lmi` | -->
<!-- |The latest milestone index|<ul><li>**Index 1:** Index of the previous solid subtangle milestone</li><li>**Index 2:** Index of the latest solid subtangle milestone</li></ul> -->
<!-- |`lmsi` | -->
<!-- |The latest solid subtangle milestone| <ul><li>**Index 1:** Index of the previous solid subtangle milestone</li><li>**Index 2:** Index of the latest solid subtangle milestone</li></ul> -->
<!-- |`lmhs`| -->
<!-- | The latest solid subtangle milestone transaction hash| <ul><li>**Index 1:** Milestone transaction hash</li></ul> -->
<!-- |`sn`| -->
<!-- | Transaction that has recently been confirmed| <ul><li>**Index 1:** Index of the milestone that confirmed the transaction</li><li>**Index 2:** Transaction hash</li><li>**Index 3:** Address</li><li>**Index 4:** Trunk transaction hash</li><li>**Index 5:** Branch transaction hash</li><li>**Index 6:** Bundle hash</li></ul> -->
<!-- |`sn_trytes`| -->
<!-- | Transaction trytes of recently confirmed transactions| <ul><li>**Index 1:** Transaction trytes</li><li>**Index 2:** Transaction hash</li><li>**Index 3:** Index of the milestone that confirmed the transaction</li></ul> -->
<!-- |`tx_trytes`| -->
<!-- | Transaction trytes of any new transactions| <ul><li>**Index 1:** Transaction trytes</li><li>**Index 2:** Transaction hash</li></ul> -->
<!-- |<a name="tx"></a> `tx` | -->
<!-- |Transaction that the IRI node has recently appended to the ledger| <ul><li>**Index 1:** Transaction hash</li><li>**Index 2:** Address</li><li>**Index 3:** Value</li><li>**Index 4:** Obsolete tag</li><li>**Index 5:** Value of the transaction's `timestamp` field</li><li>**Index 6:** Index of the transaction in the bundle</li><li>**Index 7:** Last transaction index of the bundle</li><li>**Index 8:** Bundle hash</li><li>**Index 9:** Trunk transaction hash</li><li>**Index 10:** Branch transaction hash</li><li>**Index 11:** Unix epoch of when the IRI received the transaction</li><li>**Index 12:** Tag</li></ul> -->
<!-- |<a name="address"></a>81-tryte address (uppercase characters)| -->
<!-- |Monitor a given address for a confirmed transaction| <ul><li>**Index 1:** Address</li><li>**Index 2:** Transaction hash of a confirmed transaction that the address appeared in</li><li>**Index 3:** Index of the milestone that confirmed the transaction </li></ul> -->
