# トランザクションの構造
<!-- # Structure of a transaction -->

**以下の表はトランザクションを形成するフィールドの一覧表です。**
<!-- **This table displays a list of fields that form a transaction.** -->

トランザクションは、トライトがエンコードされた2673文字の文字列で構成されています。デコードされると、トランザクションオブジェクトは以下のフィールドを含みます。
<!-- A transaction consists of 2673 tryte-encoded characters. When decoded, the transaction object contains the following fields. -->

| **フィールド** | **タイプ** | **説明** | **長さ (トライト)** |
| :--- | :--- | :--- | :--- |
| `signatureMessageFragment` | string | 署名またはメッセージ。どちらもバンドル内の複数のトランザクションにわたって細分化される可能性があります。トランザクションがIOTAトークンを取り出す場合、このフィールドには署名が入ります。それ以外の場合、このフィールドには、トライトがエンコードされたメッセージか、メッセージが定義されていない場合は、全てが9の文字列が含まれます。 | 2187 |
| `address` | string | 送信者か受信者のアドレス。出力トランザクションの場合、このフィールドには受信者のアドレスが入ります。入力トランザクション（負の`value`フィールドを持つトランザクション）の場合、このフィールドには、IOTAトークンが取り出されるアドレスが入ります。 | 81 |
| `value` | integer | アドレスから取り出すか預け入れるIOTAトークンの量 | 27 |
| `obsoleteTag` | string | ユーザーが決められるタグ（削除予定） | 27 |
| `timestamp` | integer | Unix時間：協定世界時（UTC）での1970年1月1日からの経過秒数（強制されていないため任意） | 9 |
| `currentIndex` | integer | バンドル中のトランザクションインデックス | 9 |
| `lastIndex` | integer | バンドル中の最後のトランザクションのインデックス | 9 |
| `bundle` | string | バンドルのハッシュ。バンドル内では、各トランザクションの`address`、`value`、`obsoleteTag`、`currentIndex`、`lastIndex`、そして`timestamp`フィールドの値が暗号化的スポンジ関数によって吸収され圧縮されてこのバンドルハッシュが生成されます。 | 81 |
| `trunkTransaction` | string | [親トランザクション](../concepts/the-tangle.md#parent-and-children)のトランザクションハッシュ。このトランザクションハッシュは、タングル内の既存のトランザクション、または[同じバンドル内の次のトランザクションインデックス](../references/structure-of-a-bundle.md)のいずれかです。 | 81 |
| `branchTransaction` | string | [親トランザクション](../concepts/the-tangle.md#parent-and-children)のトランザクションハッシュ | 81 |
| `attachmentTag` | string | ユーザーが決められるタグ | 27 |
| `attachmentTimestamp` | integer | UNIX時間：POW後の協定世界時（UTC）での1970年1月1日からのミリ秒 | 9 |
| `attachmentTimestampLowerBound` | integer | `attachmentTimestamp`フィールドの下限（現在使用されていません） | 9 |
| `attachmentTimestampUpperBound` | integer | `attachmentTimestamp`フィールドの上限（現在使用されていません） | 9 |
| `nonce` | string | [プルーフオブワーク](root://dev-essentials/0.1/concepts/minimum-weight-magnitude.md)を確認するためにトランザクションをハッシュする必要がある回数を表すトライト | 27 |
