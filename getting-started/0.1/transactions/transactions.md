# トランザクション
<!-- # Transactions -->

**トランザクションは，[IOTA トークン](../clients/token.md)を[アドレス](../clients/addresses.md)から取り出したり，アドレスにデポジットしたり，ゼロトークン（データ，メッセージ，または署名を含む）にすることができる単一の転送命令です．IOTA ネットワークに何かを送信する場合は，トランザクションとして[ノード](../network/nodes.md)に送信する必要があります．**
<!-- **A transaction is a single transfer instruction that can either withdraw [IOTA tokens](../clients/token.md) from an [address](../clients/seeds.md), deposit them into an address, or have zero-value (contain data, a message, or a signature). If you want to send anything to an IOTA network, you must send it to a [node](../network/nodes.md) as a transaction.** -->

## トランザクションの構造
<!-- ## Structure of a transaction -->

トランザクションは，2,673文字の[トライトにエンコードされた](../introduction/ternary.md#tryte-encoding)文字で構成されています．デコードされると，トランザクションオブジェクトには次のフィールドが含まれます．
<!-- A transaction consists of 2,673 [tryte-encoded](../introduction/ternary.md#tryte-encoding) characters. When decoded, the transaction object contains the following fields. -->

| **フィールド**                                                    | **型**  | **説明**                                                                                                                                                                                                                       | **長さ(トライト)** |
| :-----------------------------                                    | :------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------            |
| `hash`                                                            | string  | トランザクションハッシュ．すべてのトランザクションフィールドの値から導出し，[プルーフオブワーク]の一部が含まれます．                                                                                                           | 81                 |
| <a name="signatureMessageFragment"></a>`signatureMessageFragment` | string  | [署名](../clients/signatures.md)またはメッセージ．両方とも[バンドル](../transactions/bundles.md)内の多くのトランザクションで_断片化_される可能性があります．このフィールドには，メッセージが定義されていない場合は9でパディングされます．                                            | 2,187              |
| <a name="address"></a> `address`                                  | string  | 送信者または受信者のアドレスのいずれかが含まれます．トランザクションが[出力トランザクション](#output-transactions)の場合，このフィールドには受信者のアドレスが含まれます．                                                     | 81                 |
| `value`                                                           | integer | アドレスへのデポジット（正の値）またはアドレスからの取り出し（負の値）のIOTAトークンの量                                                                                                                                       | 27                 |
| `obsoleteTag`                                                     | string  | ユーザー定義のタグ（すぐに削除されます）                                                                                                                                                                                       | 27                 |
| `timestamp`                                                       | integer | Unix時間（1970年1月1日からの秒数）．このフィールドは強制されておらず，この値は任意です．                                                                                                                             | 9                  |
| `currentIndex`                                                    | integer | バンドル内の現在のトランザクションのインデックス                                                                                                                                                                               | 9                  |
| `lastIndex`                                                       | integer | バンドル内の最後のトランザクションのインデックス                                                                                                                                                                               | 9                  |
| `bundle`                                                          | string  | バンドルハッシュ．次のトランザクションフィールドの値のハッシュから導出します．`address`，`value`，`obsoleteTag`，`currentIndex`, `lastIndex`，及び `timestamp`．これらのフィールドはバンドルエッセンスと呼ばれます．          | 81                 |
| <a name="trunkTransaction"></a> `trunkTransaction`                | string  | [タングル](../network/the-tangle.md)内の既存のトランザクション，またはバンドル内の次のインデックスを持つトランザクションのトランザクションハッシュ．                                                                           | 81                 |
| <a name="branchTransaction"></a> `branchTransaction`              | string  | タングル内の既存のトランザクションのトランザクションハッシュ                                                                                                                                                                   | 81                 |
| <a name="tag"></a> `attachmentTag`                                | string  | ユーザー定義のタグ                                                                                                                                                                                                             | 27                 |
| `attachmentTimestamp`                                             | integer | Unixエポック（[プルーフオブワーク](../transactions/proof-of-work.md)が行われた直後の1970年1月1日からのミリ秒）                                                                                                                                                   | 9                  |
| `attachmentTimestampLowerBound`                                   | integer | `attachmentTimestamp` フィールドの下限（現在は使用されていません）                                                                                                                                                             | 9                  |
| `attachmentTimestampUpperBound`                                   | integer | `attachmentTimestamp` フィールドの上限（現在は使用されていません）                                                                                                                                                             | 9                  |
| `nonce`                                                           | string  | プルーフオブワークの証拠を表すトライト                                                                                                                                                                                         | 27                 |

<!-- | **Field**                         | **Type**   | **Description**                                                                                                                                                                                                                   | **Length (trytes)** | -->
<!-- | :----------------------------- | :------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------ | -->
<!-- |`hash`|string|Transaction hash, which is derived from the values of every transaction field and contains part of the [proof of work](../transactions/proof-of-work.md).|81| -->
<!-- | <a name="signatureMessageFragment"></a>`signatureMessageFragment`      | string | A [signature](../clients/signatures.md) or a message, both of which may be _fragmented_ over many transactions in a [bundle](../transactions/bundles.md). This field contains all 9's where no message is defined. | 2,187   | -->
<!-- |<a name="address"></a> `address`                       | string | Contains either the sender's or recipient's address. This field contains a recipient's address if the transaction is an [output transaction](#output-transactions).   | 81     | -->
<!-- | `value`                    | integer    | Amount of IOTA tokens to either deposit (positive value) into an address or withdraw (negative value) from it                                                                                                                                                                                        | 27     | -->
<!-- | `obsoleteTag`                   | string | User-defined tag (soon to be removed)                                                                                                                                                                                               | 27     | -->
<!-- | `timestamp`                     | integer    | Unix epoch (seconds since Jan 1, 1970). This field is not-enforced and its value can be arbitrary.                                                                                                                                                                                   | 9      | -->
<!-- | `currentIndex`                  | integer  | Index of the current transaction in the bundle                                                                                                                                                                                                   | 9      | -->
<!-- | `lastIndex`                     | integer    | Index of the last transaction in the bundle                                                                                                                                                                                           | 9      | -->
<!-- | `bundle`                        | string | Bundle hash, which is derived from a hash of the values of the following transaction fields: `address`, `value`, `obsoleteTag`, `currentIndex`, `lastIndex`, and `timestamp`. These fields are called the bundle essence.                               | 81     | -->
<!-- | <a name="trunkTransaction"></a> `trunkTransaction`              | string |  Transaction hash of either an existing transaction in the [Tangle](../network/the-tangle.md) or of the transaction with the next index in the bundle.                                                                                                                                 | 81     | -->
<!-- |<a name="branchTransaction"></a> `branchTransaction`             | string | Transaction hash of an existing transaction in the Tangle                                                                                                                                                                | 81     | -->
<!-- | <a name="tag"></a> `attachmentTag`                | string | User-defined tag                                                                                                                                                                                                              | 27     | -->
<!-- | `attachmentTimestamp`          | integer   | Unix epoch (milliseconds since Jan 1, 1970 after [proof of work](../transactions/proof-of-work.md) was done)                                                                                                                                                                                                           | 9      | -->
<!-- | `attachmentTimestampLowerBound` | integer   | Lower limit of the `attachmentTimestamp` field (not currently used)                                                                                                                                                                                                      | 9      | -->
<!-- | `attachmentTimestampUpperBound` | integer   | Upper limit of the `attachmentTimestamp` field (not currently used)                                                                                                                                                                                                         | 9      | -->
<!-- | `nonce`                         | string | Trytes that represent the proof of work                                      | 27     | -->


## トランザクションハッシュ
<!-- ## Transaction hash -->

バンドルハッシュとともに，トランザクションハッシュはタングルをイミュータブルにするものの一部です．トランザクションフィールドの値のいずれかが変更された場合，トランザクションハッシュは無効になり，トランザクションの子と，[タングル](../network/the-tangle.md)上で直接または間接的にそれらを参照するトランザクションも無効になります．
<!-- Along with the bundle hash, the transaction hash is part of what makes the Tangle immutable. If any of the values in the transaction fields were to change, the transaction hash would be invalid, which would also invalidate the transaction's children and whichever transactions directly or indirectly reference them in [the Tangle](../network/the-tangle.md). -->

## トランザクションタイプ
<!-- ## Transaction types -->

トランザクションは，次のタイプのいずれかです．
<!-- Transactions can be one of the following types: -->

- 入力トランザクション
<!-- - Input transaction -->
- 出力トランザクション
<!-- - Output transaction -->
- ゼロトークントランザクション
<!-- - Zero-value transaction -->

### 入力トランザクション
<!-- ### Input transactions -->

入力トランザクションには，アドレスから IOTA トークンを取り出す命令が含まれています．
<!-- Input transactions contain an instruction to withdraw IOTA tokens from an address. -->

有効な入力トランザクションには，常に次のものが含まれている必要があります．
<!-- A valid input transaction must always contain the following: -->

- `value` フィールドの負の値
<!-- - A negative value in the `value` field -->
- 少なくとも `value` フィールドの金額を含むアドレス
<!-- - An address that contains at least the amount in the `value` field -->
- `signatureMessageFragment` フィールドの有効な署名の少なくとも最初のフラグメント
<!-- - At least the first fragment of a valid signature in the `signatureMessageFragment` field -->
- 有効なナンス
<!-- - A valid nonce -->

たとえば，以下の入力トランザクションには，アドレスから100 Mi を取り出す命令が含まれています．
<!-- For example, this input transaction contains an instruction to withdraw 100 Mi from an address: -->

```json
{
 "hash": "GOYYXODDTVTHSLMRECBXJROIBHRNJ9ZPDATIUTQAJSWYD9GKVVERXWNHFFDOMQHQYDGRUGWR9W9R99999",
 "signatureMessageFragment": "U9ZRUHALC9WOSNXBQNIDVWAOWTZRYKDXCGYYRWRQJLGIXXIIMDETANIFZMR9XTSLFHXSTVJGIIKUIQQNYKRHSZJIYSQBKHUVZWRTNQMHKWAATRPIFNNEBWJ9XNKLIRKTJOKIJCK9OBQMSUDS9RKBRXLGKYMCPFS9I9POSX9VTGPZHABUFNFLGXB9LJMCLOCMB9NLTCLMTLFWNQRWRGQMCUOUFLLXZHATCLSYMAPFITDDWPOQHE9NPCTWPBSVFMNDHYULXXFSBOFGPVLKDFTSJBTGPDKLYFPRML9BSQQQKEJGIHEZGKTYD9RU9AEIRUWEJ9NXQTAFEMPECESOQFLFWHJJKD9APEEWPYKYAKANQJNRKQFSEOHLR9APVHAAVWWX9LZSEZCVCWGDODVVXTHQZPEZ9XCFJJZIV9DQLNRLZMXWMBOZYXUNDFZOSY9REOGQTQBTBYVE9BQ99UJKUOIWJLXKHHPZBKHXREORRWPHOIBABVMDMWNIODHUOSIXWRVDTBVJBJHXHHXYDGEXSLQVEANTGXFVEDDWIRFEFNOYHSHEGELWZZRJJJXA9PLHH9MKJCSAGYFKNGFCTERQWSVFNITIHEYYCQSETRYEPZQFHYGLIGCMTFCKAULHCQCCLXKGNJTNLK9AEPYSDDQSODSTWR9FFARCYPPFUCYDGKLSJMIRKFUAVIREWJMEJODDCYQWUQP9PMYVSWELO9TRYZZGMG9IBBVSNHXR9TKHQHBXAUNFBEUSJUPFSXDTZI9DBELZXHIQUD9DKIZXWPVJQHH9VHVEVQTRWH9HGWAUKWTIFYLJOJPMSAMBQMDYSYE9ZSREGBYZIYZHMUHKSHBXWJHBZNTWYXGPYQSZBS9USGPIRNNN9QMHLGGEW9UMDTYMBIRTCEALBJPQTWVVMCTNWYMZHWPTMRKFMMZVCIOQH9XKADU9JQGJAKMDGOEOMGTGCBAKIAUKN9KTWTACFDNKXFYJVJMPWITBDDMHUDYESDAI9YQLHJCFVPIYJSXOTHUJCRJ9FZUJ9F9HFSSKNYWOGIK9DSMJGAON9GXSTVCUJCRJCNOPBAMG9IMBLUEYKALPUNIIMNVKIYNMUZPYRABFVBKSPIATZSLZUARXMWOGQEJDBFR9YXUWMNBMOTOT9VJQGHBSGKWXYKFKSBAEKUQYKTKCTMNKSSHIBODQOEJ9YNEGHDXZ9R9MZDLJPDFUZPINIACZCRILKVVXRQUWVHJTTAIJ9DENNKEXQCMDVMPBWKAVQHRXYZVXWJVLV9NULZBDK9MTVGPSAI9ZUIYF9BBXQXQXJW9SDJL9BJQLBQTS9FRRGOLJICRVGCPGGTALQTKBGKJMOQANSKISX9NKYUGOOLHGD9WDDSIIYA9QDUWDEVZEELKKE9ZGERSUFORDIZ9BKJSNTEIKIAZUMGYFHPHVWYVPY9EKEDOTKMKIOHXAJWYAKTEWHMTZ99MSHNOTKB9PIFTRIRZBGXZMAHPZIPKMNDSEHEMDJSUXDGYCBJZWEFNWIEBNBJSFTTF99LS9SDDYWTVAVZZDVLWHRXYEKZXVQPFZUIKCVRTZVOZMVUWXKCH9CCQSUIPNWOUTTPKMYCUEYKZNBBHIYOGZJUJEILJTNCRWHHCZNGWWIDNMBNDSHPDDDMECCHMBEJPGGPHBSQUDVIOUPIXKQMIQRTPRGJXMWCCCQENFTORFAGCYWTMBDGNUQWAEX9IVFINCSSNHPULJWYOEWUEYEWGXBCIEBAKBSCLFCPOCCDMZCQWPWJFFQJTAZXROATTLTOPWQYSZLBMAA9YGDBWCXMOTDCYPJHAMAZBNUXLTLFUKKXCSKXBVMOUKXHSAQWVLEACXRRWCJVKZLKIJALANL9WRKVIETSUYXHVRGJNCDYUUOSDFJRRNWPOEAFA9BPJAMSXASUFKPKFFENTFVRWYTPPHNZSAEAPUUFWKNKKDKQDOJ9MBWVKPDBTQNAZDV9ZMUXFJKJHPIOLWBSMM9SVMMPWZJREYFCWL9HYMQLNIJVNRZZJFSFRZDTSHWXXTUATUGCICWNWUIHTCZOOXXVEGSLFLTOYDHVXABTEFOPJHDVRCSLPNPYHFNCW9KDVCNFTKVJWRP9KWSWKDAMMTQKDCMPRARKSXSLYCB9ZENFYSJIASMJDLXURPRVEBHIFRLPYTICKOKAVZVLJRTIPBZDKNQKPBD9B",
 "address": "LEYNSIMADMXAUYRGXKKEXPHDMZLRISZBSRZXUMCIKP9JQDOXSCIUGKYFFNPPVPGCHEJAWWSDHCKGOORPC",
 "value": -100000000,
 "obsoleteTag": "ANDROID9WALLET9TRANSFER9999",
 "timestamp": 1507558559,
 "currentIndex": 1,
 "lastIndex": 3,
 "bundle": "VAJOHANFEOTRSIPCLG9MIPENDFPLQQUGSBLBHMKZ9XVCUSWIKJOOHSPWJAXVLPTAKMPURYAYD9ONODVOW",
 "trunkTransaction": "BSQTZZSXBAGNCBLEUFMWDOITACAK9DKVRTTVYEDQENTEWWCAAFKXWP9Q9MHRS9ZQPXTSRFGZJNEGZ9999",
 "branchTransaction": "9QEIQDDDMLMTMYDDURPYKJJZ9MOLGYBRI9DSOWGRIEKTKPJKUKZPGOYJDDZOOSZAIJVZWBQGVPJCA9999",
 "tag": "ANDROID9WALLET9TRANSFER9999",
 "attachmentTimestamp": 1572010100517,
 "attachmentTimestampLowerBound": 0,
 "attachmentTimestampUpperBound": 3812798742493,
 "nonce": "9ADKOSMXLFDYTED9NXYMAWABEAS"
}
```

### 出力トランザクション
<!-- ### Output transactions -->

出力トランザクションは IOTA トークンをアドレスにデポジットします．
<!-- Output transactions deposit IOTA tokens into an address. -->

有効な出力トランザクションには，常に以下が含まれている必要があります．
<!-- A valid output transaction must always contain the following: -->

- `value` フィールドの正の値
<!-- - A positive value in the `value` field -->
- 有効なアドレス
<!-- - A valid address -->
- 有効なナンス
<!-- - A valid nonce -->

たとえば，以下の出力トランザクションには，99.999998 Mi をアドレスにデポジットする命令が含まれています．
<!-- For example, this output transaction contains an instruction to deposit 99.999998 Mi into an address: -->

```json
{
 "hash": "9UNGZBAIGVLRHPLKYTGSPZRQWNLG9DVIZEVJOYLVMJUHLWHMIIF9IAXTYTZHQTBQBUBBSRMBXHEC99999",
 "signatureMessageFragment": "999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999",
 "address": "LEYNSIMADMXAUYRGXKKEXPHDMZLRISZBSRZXUMCIKP9JQDOXSCIUGKYFFNPPVPGCHEJAWWSDHCKGOORPC",
 "value": 99999998,
 "obsoleteTag": "ANDROID9WALLET9TRANSFER9999",
 "timestamp": 1507558559,
 "currentIndex": 3,
 "lastIndex": 3,
 "bundle": "VAJOHANFEOTRSIPCLG9MIPENDFPLQQUGSBLBHMKZ9XVCUSWIKJOOHSPWJAXVLPTAKMPURYAYD9ONODVOW",
 "trunkTransaction": "ZWHYWAXHUGRQMRBCDYJGZMCYFJRQEUX9NAAKCHUKQDGIIJMFDAAWTRZZZXTFCOUANXK9GWTUDKEB99999",
 "branchTransaction": "PRJCVOAEAQXOHSTGLFVUYRKGBCXSKOEYU9DEYGSTMAKPGFNEKVBSAUSKVCFDRGHWCKIHPGSXGHTZZ9999",
 "tag": "ANDROID9WALLET9TRANSFER9999",
 "attachmentTimestamp": 1572076153742,
 "attachmentTimestampLowerBound": 0,
 "attachmentTimestampUpperBound": 3812798742493,
 "nonce": "XOOWILZJ9GWTROWMMWBFWZUCFXL"
}
```

### ゼロトークントランザクション
<!-- ### Zero-value transactions -->

ゼロトークントランザクションは，`value` フィールドの値が0です．ゼロトークントランザクションは，IOTA トークンなしでメッセージを送信するのに役立ちます．
<!-- A zero-value transaction has a value of 0 in the `value` field. These transactions are useful for sending messages without IOTA tokens. -->

:::info:
IOTA トークンは転送されないため，アドレスは誰のものである必要はありません．
:::
<!-- :::info: -->
<!-- The address does not need to belong to anyone because no value is being transferred. -->
<!-- ::: -->

たとえば，以下のゼロトークントランザクションにはトライトの `Hello world` メッセージが含まれています．
<!-- For example, this zero-value transaction contains a `Hello world` message (in trytes): -->

```json
 {
  "hash": "ZFLRESUHTVAAVUBTTOEY9KGXQAZHDKLYAGEJZ9GPUXVZNPVGEDSNARVMUZYWIJJPFQTMNHOPVZSGK9999",
  "signatureMessageFragment": "RBTC9D9DCDEAFCCDFD9DSCFA999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999",
  "address": "JJRJVMQQAKNRKKHKPKCLHPHACJFJTNHRKJTHJNCJAJVJ9U9KKFJDOIQITDWIHLPRORXCHUOGDUPCIJXMY",
  "value": 0,
  "obsoleteTag": "UA9999999999999999999999999",
  "timestamp": 1572276504,
  "currentIndex": 0,
  "lastIndex": 0,
  "bundle": "VQRKLYGVLLCSEPZFQEZJZJIPPRAUGZDCXTURAKGEXT9LUR9J9BSHCQTZEJPFBEIAHJEYYQ9IBWZUIOW9Y",
  "trunkTransaction": "WCOJRHDCODZAFM9HTTLQNGQFJLDB9SVQXBTAM9XMPUNBSW9BUQWSYAVLQJFHOOZR9UOJXDGSVETGYM999",
  "branchTransaction": "JMCVKXBXPWIEXBIOFCDHIKMMLJKUDWPMPXADSQYXHPYRSNUPK9KROCYFHTETALUBGTDQCLVC9CRVJN999",
  "tag": "UA9999999999999999999999999",
  "attachmentTimestamp": 1572276505684,
  "attachmentTimestampLowerBound": 0,
  "attachmentTimestampUpperBound": 3812798742493,
  "nonce": "OVMNHAVIBLEDUCRUSLKVGGBCVTM"
 }
```

## ユーティリティ
<!-- ## Utilities -->

以下の IOTA タングルユーティリティは，トランザクションの操作に役立ちます．
<!-- These IOTA Tangle utilities are useful for working with transactions: -->

- [トランザクションをデコードするユーティリティ](https://utils.iota.org/transaction-decoder)
<!-- - [Decode a transaction](https://utils.iota.org/transaction-decoder) -->

- [ゼロトークントランザクションを送信するユーティリティ](https://utils.iota.org/simple-transaction)
<!-- - [Send a zero-value transaction](https://utils.iota.org/simple-transaction) -->

- [トランザクションを検索するユーティリティ](https://utils.iota.org/)
<!-- - [Search for transactions](https://utils.iota.org/) -->

## 関連ガイド
<!-- ## Related guides -->

[JavaScript で "hello world" トランザクションを送信する](root://client-libraries/0.1/how-to-guides/js/send-your-first-bundle.md)．
<!-- [Send a "hello world" transaction in JavaScript](root://client-libraries/0.1/how-to-guides/js/send-your-first-bundle.md). -->

[JavaScript で IOTA トークンを転送する](root://client-libraries/0.1/how-to-guides/js/transfer-iota-tokens.md)．
<!-- [Transfer IOTA tokens in JavaScript](root://client-libraries/0.1/how-to-guides/js/transfer-iota-tokens.md). -->

[トリニティでトランザクションを送信する](root://wallets/0.1/trinity/how-to-guides/send-a-transaction.md)．
<!-- [Send a transaction in Trinity](root://wallets/0.1/trinity/how-to-guides/send-a-transaction.md). -->
