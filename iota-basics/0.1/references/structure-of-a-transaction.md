# トランザクションの構造
<!-- # Structure of a transaction -->

**これらの表はトランザクションを形成するフィールドの一覧表です。**
<!-- **This table displays a list of fields that form a transaction.** -->

トランザクションは、トライトにエンコードされた2673文字の文字列で構成されています。デコードされると、トランザクションオブジェクトは以下のフィールドを含みます。
<!-- A transaction consists of 2673 tryte-encoded characters. When decoded, the transaction object contains the following fields. -->

| フィールド | タイプ | 説明 | 長さ (トライト) |
| :--- | :--- | :--- | :--- |
| `signatureMessageFragment` | string | 署名またはメッセージ。どちらもバンドル内の複数のトランザクションにわたって細分化される可能性があります。トランザクションがIOTAトークンを取り出す場合、このフィールドには署名が入ります。それ以外の場合、このフィールドには、トライトにエンコードされたメッセージか、メッセージが定義されていない場合は、全てが9の文字列が含まれます。 | 2187 |
| `address` | string | 送信者か受信者のアドレス。 This field contains a recipient's address if the transaction is an output. Otherwise, this field contains an address from which IOTA tokens are being withdrawn (transaction with a negative `value` field). | 81 |
| `value` | integer | アドレスから取り出しか送金するIOTAトークンの量。Amount of IOTA tokens to deposit to or withdraw from the address | 27 |
| `obsoleteTag` | string | ユーザーが決められるタグ。（削除予定）User-defined tag (soon to be removed) | 27 |
| `timestamp` | integer | Unix epoch: Seconds since Jan 1, 1970 (not-enforced and can be arbitrary) | 9 |
| `currentIndex` | integer | バンドル中のトランザクションインデックスIndex of a transaction in the bundle | 9 |
| `lastIndex` | integer | バンドル中のラストトランザクションのインデックスIndex of the last transaction in the bundle | 9 |
| `bundle` | string | バンドルのハッシュ値Hash of the bundle | 81 |
| `trunkTransaction` | string | 親トランザクションのトランザクションハッシュ値Transaction hash of a parent transaction | 81 |
| `branchTransaction` | string | 親トランザクションのトランザクションハッシュ値Transaction hash of a parent transaction | 81 |
| `attachmentTag` | string | ユーザーが決められるタグUser-defined tag | 27 |
| `attachmentTimestamp` | integer | Unix epoch: Milliseconds since Jan 1, 1970 (after POW) | 9 |
| `attachmentTimestampLowerBound` | integer | Lower limit of the `attachmentTimestamp` field (not currently used) | 9 |
| `attachmentTimestampUpperBound` | integer | Upper limit of the `attachmentTimestamp` field (not currently used) | 9 |
| `nonce` | string | Trytes that represent the amount of times a transaction must be hashed to check the [proof of work](root://the-tangle/0.1/concepts/proof-of-work.md). | 27 |
<!-- | Field                         | Type   | Description                                                                                                                                                                                                                   | Length (trytes) | -->
<!-- | :----------------------------- | :------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------ | -->
<!-- | `signatureMessageFragment`      | string | A signature or a message, both of which may be _fragmented_ over multiple transactions in the bundle. This field contains a signature if the transaction withdraws IOTA tokens. Otherwise, this field can contain a tryte-encoded message or all 9's where no message is defined. | 2187   | -->
<!-- | `address`                       | string | Contains either the sender or recipient's address. This field contains a recipient's address if the transaction is an output. Otherwise, this field contains an address from which IOTA tokens are being withdrawn (transaction with a negative `value` field).   | 81     | -->
<!-- | `value`                    | integer    | Amount of IOTA tokens to deposit to or withdraw from the address                                                                                                                                                                                            | 27     | -->
<!-- | `obsoleteTag`                   | string | User-defined tag (soon to be removed)                                                                                                                                                                                               | 27     | -->
<!-- | `timestamp`                     | integer    | Unix epoch: Seconds since Jan 1, 1970 (not-enforced and can be arbitrary)                                                                                                                                                                                    | 9      | -->
<!-- | `currentIndex`                  | integer  | Index of a transaction in the bundle                                                                                                                                                                                                   | 9      | -->
<!-- | `lastIndex`                     | integer    | Index of the last transaction in the bundle                                                                                                                                                                                           | 9      | -->
<!-- | `bundle`                        | string | Hash of the bundle                                 | 81     | -->
<!-- | `trunkTransaction`              | string |  Transaction hash of a parent transaction                                                                                                                                                                                 | 81     | -->
<!-- | `branchTransaction`             | string | Transaction hash of a parent transaction                                                                                                                                                                   | 81     | -->
<!-- | `attachmentTag`                | string | User-defined tag                                                                                                                                                                                                              | 27     | -->
<!-- | `attachmentTimestamp`          | integer   | Unix epoch: Milliseconds since Jan 1, 1970 (after POW)                                                                                                                                                                                                           | 9      | -->
<!-- | `attachmentTimestampLowerBound` | integer   | Lower limit of the `attachmentTimestamp` field (not currently used)                                                                                                                                                                                                      | 9      | -->
<!-- | `attachmentTimestampUpperBound` | integer   | Upper limit of the `attachmentTimestamp` field (not currently used)                                                                                                                                                                                                         | 9      | -->
<!-- | `nonce`                         | string | Trytes that represent the amount of times a transaction must be hashed to check the [proof of work](root://the-tangle/0.1/concepts/proof-of-work.md).                                      | 27     | -->
