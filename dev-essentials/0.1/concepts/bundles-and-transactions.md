# バンドルとトランザクション
<!-- # Bundles and transactions -->

**IOTAネットワークに何か送信したい場合は、トランザクションの形でノードに送信する必要があります。トランザクションは、入力トランザクション（アドレスからの取り出し）または出力トランザクション（アドレスへの預け入れまたはゼロトークントランザクション）のいずれかになる単一の操作です。ノードに1つ以上のトランザクションを送信するには、トランザクションをバンドルにグループ化する必要があります。**
<!-- **If you want to send anything to an IOTA network, you must send it to a node in the form of a transaction. A transaction is a single operation that can be either an input (withdraw from an address) or an output (deposit into an address or have zero-value). To send a node one or more transactions, you must group them in a bundle.** -->

バンドル内の各トランザクションは、ノードが各トランザクションを検証してタングルに添付することを可能にする以下の構造を持っています。
<!-- Each transaction in a bundle has the following structure that allows nodes to validate them and attach them to the Tangle. -->

| **フィールド** | **説明** |
| :------------- | :------- |
| `signatureMessageFragment` | 署名またはメッセージ。どちらもバンドル内の複数のトランザクションにわたって_細分化_される可能性があります。トランザクションがIOTAトークンを取り出す場合、このフィールドには署名が入ります。それ以外の場合、このフィールドには、トライトがエンコードされたメッセージか、メッセージが定義されていない場合は、全てが9の文字列が含まれます。 |
| `address` | 送信者か受信者のアドレス。出力トランザクションの場合、このフィールドには受信者のアドレスが入ります。入力トランザクション（負の`value`フィールドを持つトランザクション）の場合、このフィールドには、IOTAトークンが取り出されるアドレスが入ります。 |
| `value` | アドレスから取り出すか預け入れるIOTAトークンの量 |
| `obsoleteTag` | ユーザーが決められるタグ |
| `timestamp` | Unix時間：協定世界時（UTC）での1970年1月1日からの経過秒数（強制されていないため任意） |
| `currentIndex` | バンドル中のトランザクションインデックス |
| `lastIndex` | バンドル中の最後のトランザクションのインデックス |
| `bundle` | バンドルのハッシュ。バンドル内では、各トランザクションの`address`、`value`、`obsoleteTag`、`currentIndex`、`lastIndex`、そして`timestamp`フィールドの値が暗号化的スポンジ関数によって吸収され圧縮されてこのバンドルハッシュが生成されます。 |
| `trunkTransaction` | [親トランザクション](../concepts/the-tangle.md#parent-and-children)のトランザクションハッシュ。このトランザクションハッシュは、タングル内の既存のトランザクション、または[同じバンドル内の次のトランザクションインデックス](../references/structure-of-a-bundle.md)のいずれかです。 |
| `branchTransaction` | [親トランザクション](../concepts/the-tangle.md#parent-and-children)のトランザクションハッシュ |
| `attachmentTag` | ユーザーが決められるタグ |
| `attachmentTimestamp` | UNIX時間：POW後の協定世界時（UTC）での1970年1月1日からのミリ秒 |
| `attachmentTimestampLowerBound` | `attachmentTimestamp`フィールドの下限（現在使用されていません） |
| `attachmentTimestampUpperBound` | `attachmentTimestamp`フィールドの上限（現在使用されていません） |
| `nonce` | [プルーフオブワーク](root://dev-essentials/0.1/concepts/minimum-weight-magnitude.md)を確認するためにトランザクションをハッシュする必要がある回数を表すトライト |

<!-- | **Field** | **Description** | -->
<!-- | :-------- | :-------------- | -->
<!-- | `signatureMessageFragment`      | A signature or a message, both of which may be _fragmented_ over multiple transactions in the bundle. This field contains a signature if the transaction withdraws IOTA tokens. Otherwise, this field can contain a tryte-encoded message or all 9's where no message is defined. | -->
<!-- | `address`                       | Contains either the sender or recipient's address. This field contains a recipient's address if the transaction is an output. Otherwise, this field contains an address from which IOTA tokens are being withdrawn (transaction with a negative `value` field). | -->
<!-- | `value`                         | Amount of IOTA tokens to deposit to or withdraw from the address | -->
<!-- | `obsoleteTag`                   | User-defined tag | -->
<!-- | `timestamp`                     | Unix epoch: Seconds since Jan 1, 1970 (not-enforced and can be arbitrary) | -->
<!-- | `currentIndex`                  | Index of the current transaction in the bundle | -->
<!-- | `lastIndex`                     |Index of the last transaction in the bundle | -->
<!-- | `bundle`                        | Hash of the bundle | -->
<!-- | `trunkTransaction`              |Transaction hash of a [parent transaction](../concepts/the-tangle.md#parent-and-children). This transaction hash can either be of an existing transaction in the Tangle or of [the next transaction index in the same bundle](../references/structure-of-a-bundle.md). | -->
<!-- | `branchTransaction`             |Transaction hash of a [parent transaction](../concepts/the-tangle.md#parents-and-children) | -->
<!-- | `attachmentTag`                 |User-defined tag | -->
<!-- | `attachmentTimestamp`           |Unix epoch: Milliseconds since Jan 1, 1970 (after POW) | -->
<!-- | `attachmentTimestampLowerBound` |Lower limit of the `attachmentTimestamp` field (not currently used) | -->
<!-- | `attachmentTimestampUpperBound` |Upper limit of the `attachmentTimestamp` field (not currently used) | -->
<!-- | `nonce`                         |Trytes that represent the amount of times a transaction must be hashed to check the [proof of work](root://dev-essentials/0.1/concepts/minimum-weight-magnitude.md). | -->

トランザクションがバンドル内でグループ化されると、バンドル内の場所を定義する`currentIndex`フィールドとバンドル内の最後のトランザクションを定義する`lastIndex`フィールドの両方が与えられます。
<!-- When a transaction is grouped in a bundle, it's given both a `currentIndex` field, which defines its place in the bundle, and a `lastIndex` field, which defines the last transaction in a bundle. -->

:::info:
バンドルの構造は、先頭トランザクション、ボディトランザクション、および末尾トランザクションから構成されます。末尾トランザクションが最初の（インデックス0）、先頭トランザクションがバンドル内の最後のトランザクションです。
:::
<!-- :::info: -->
<!-- The structure of a bundle consists of a head, a body, and a tail, where the tail is first (index 0) and the head is the last transaction in the bundle. -->
<!-- ::: -->

同じバンドル内のすべてのトランザクションは`bundle`フィールドの中で同じ値を持ちます。`bundle`フィールドにはバンドルハッシュが含まれています。これは**バンドルエッセンス**から導出されたもので、次のトランザクションフィールドの値のハッシュ値で構成されています。
<!-- All transactions in the same bundle have the same value in the `bundle` field. This field contains the bundle hash, which is derived from the **bundle essence**, which consists of a hash of the values of the following transaction fields: -->

* `address`
* `value`
* `obsoleteTag`
* `currentIndex`
* `lastIndex`
* `timestamp`

したがって、これらのフィールドのいずれかの値が変更されると、ノードはバンドル内のすべてのトランザクションを無効にします。
<!-- So, if the values of any of these fields were to change, the nodes would invalidate all transactions in the bundle. -->

その結果、バンドルは不可分です。バンドル内のすべてのトランザクションが有効かつ確定済みであるか、またはどれも有効ではありません。
<!-- As a result, bundles are atomic: Either all transactions in the bundle are valid and confirmed or none of them are. -->

バンドルを不可分にする必要がある理由を説明するために、次の例を挙げます。
<!-- To explain why bundles need to be atomic, take this example. -->

> オンラインで精算をするとして、支払うべき合計が10Miとします。あなたのシードは2つのアドレス（インデックス0と1）を持ち、両方とも5Miを含みます。したがって、3つのトランザクションを作成します。アドレス0から5Miを取り出す入力トランザクション、アドレス1から5Miを取り出す入力トランザクション、およびベンダーのアドレスに10Miを支払う出力トランザクションです。（入力トランザクションの両方のアドレスがセキュリティレベル1の秘密鍵から作成されたものとします。そのため、署名は各入力トランザクションに含まれています。）
<!-- > You're at an online checkout and the total to pay is 10 Mi. Your seed has 2 addresses (index 0 and 1), which both contain 5 Mi. So, you create three transactions: One input transaction to withdraw 5 Mi from address 0, another input transaction to withdraw 5 Mi from address 1, and one output transaction to deposit 10 Mi to the vendor's address. (We'll assume that both addresses in the input transactions were created from a private key with security level 1, so the signatures can fit in each transaction.) -->

> ベンダーが10Miを受け取るには、これら3つのトランザクションすべてが有効でなければなりません。各トランザクションは、IOTAトークンを転送するという目標を達成するために、互いの有効性に依存する連続的な命令です。
> For the vendor to receive 10 Mi, all three of those transactions must be valid. They're sequential instructions that rely on each other's validity to achieve the goal of transferring IOTA tokens.

:::info:
バンドルにパッケージ化する必要があるのは、複数のトランザクションだけではありません。
1つのトランザクションでも、パッケージ化する必要があります。
:::
<!-- :::info: -->
<!-- It's not just multiple transactions that need to be packaged in a bundle, even individual ones do. -->
<!-- ::: -->

## トランザクションの種類
<!-- ## Types of transaction -->

トランザクションは、以下の2つの種類のうちの1つになります。
<!-- Transactions can be one of two types: -->

* 入力トランザクション
<!-- * Input transaction -->
* 出力トランザクション
<!-- * Output transaction -->

### 入力トランザクション
<!-- ### Input transactions -->

入力トランザクションはアドレスからIOTAトークンを取り出します。
<!-- Input transactions withdraw IOTA tokens from addresses. -->

バンドルには複数の入力トランザクションを含めることができ、それぞれに有効な[署名](../concepts/addresses-and-signatures.md)を含める必要があります。署名の長さはアドレスの[セキュリティレベル](../references/security-levels.md)によって異なります。アドレスのセキュリティレベルが1より大きい場合、署名は1つのトランザクションに収まるには大きすぎるため、ゼロトークン出力トランザクションに断片して収める必要があります。
<!-- Bundles can contain multiple input transactions, and each one must include a valid [signature](../concepts/addresses-and-signatures.md). The length of the signature depends on the [security level](../references/security-levels.md) of the address. If the security level of the address is greater than 1, the signature is too large to fit in one transaction and must be fragmented across zero-value output transactions. -->

:::danger:
[同じアドレスからは2回以上トークンを取り出してはいけません](../concepts/addresses-and-signatures.md#address-reuse)。その結果、送信側が受信側にすべてのトークンを転送しない場合でも、入力トランザクションはアドレスからすべてのIOTAトークンを取り出す必要があります。残りのIOTAトークンは、出力トランザクションの残り用アドレス（通常は送信者のアドレス）に預け入れることになります。
:::
<!-- :::danger: -->
<!-- [Addresses must not be withdrawn from more than once](../concepts/addresses-and-signatures.md#address-reuse). As a result, input transactions must withdraw all IOTA tokens from an address even if the sender does not want to transfer all of them to the recipient. The remaining IOTA tokens can be deposited into a remainder address (usually the sender's address) in an output transaction. -->
<!-- ::: -->

### 出力トランザクション
<!-- ### Output transactions -->

出力トランザクションは、次のいずれかになります。
<!-- Output transactions can be one of the following: -->

* `signatureMessageFragment`フィールドにメッセージのみを含むゼロトークントランザクション
<!-- * A zero-value transactions that contains only a message in the `signatureMessageFragment` field -->
* IOTAトークンを目的のアドレスに預け入れる正の値を持つトランザクション
<!-- * A transaction with a positive value that deposits IOTA tokens into an address -->

バンドルには複数の出力トランザクションを含めることができます。出力トランザクション内のメッセージが`signatureMessageFragment`フィールド（2187トライト）よりも大きい場合、そのメッセージは他のゼロトークン出力トランザクションにまたがって分断化される可能性があります。
<!-- Bundles can contain multiple output transactions. If a message in an output transaction is larger than the `signatureMessageFragment` field, the message can be fragmented across other zero-value output transactions. -->

:::info:
IOTAトークンを預け入れるトランザクションは、IOTAトークンを取り出さないので、署名を含まないため、メッセージを含めることもできます。
:::
<!-- :::info: -->
<!-- Transactions that deposit IOTA tokens can also contain a message because they don't withdraw IOTA tokens, and therefore don't contain a signature. -->
<!-- ::: -->

## トランザクションを送信するためのオプション
<!-- ## Options for sending a transaction -->

トランザクションを作成したら、以下の2つの引数と共にトランザクションをノードに送信する必要があります。
<!-- When you've created your transaction, you need to send it to a node along with two other arguments: -->

* 深さ
<!-- * Depth -->
* 最小重量値
<!-- * Minimum weight magnitude -->

深さは、ノードがタングルのどのくらい前まで戻って[チップ選択アルゴリズム](../concepts/the-tangle.md#tip-selection)を開始するかに影響します。ノードはチップ選択アルゴリズムを使用して、新しいトランザクションが`branchTransaction`と`trunkTransaction`フィールドで参照できるタングル内の2つの既存の末尾トランザクションを見つけます。
<!-- The depth affects how far back in the Tangle the node starts the [tip selection algorithm](../concepts/the-tangle.md#tip-selection). Nodes use this algorithm to find two existing tail transactions in the Tangle that a new transaction can reference in the `branchTransaction` and `trunkTransaction` fields. -->

:::info:
深さが大きいほど、ノードはタングルの奥に戻ります。そのため、深さが大きいほど、チップ選択に時間がかかり、ノードの計算能力が高くなります。
:::
<!-- :::info: -->
<!-- The greater the depth, the farther back in the Tangle the node starts, so a greater depth makes tip selection take longer and makes the node use more computational power. -->
<!-- ::: -->

[最小重量値](../concepts/minimum-weight-magnitude.md)（MWM）は、プルーフオブワーク中に行われる作業量を定義する変数です。ノードにトランザクションを送信するときは、そのノードのネットワークに対して正しいMWMを使用する必要があります。そうでなければ、トランザクションは有効にならず、すべてのノードがトランザクションを拒否します。
<!-- The [minimum weight magnitude](../concepts/minimum-weight-magnitude.md) (MWM) is a variable that defines how much work is done during proof of work. When you send a transaction to a node, you must use the correct MWM for that node's network. Otherwise, your transaction won't be valid and all nodes will reject it. -->

## トランザクションとバンドルの間の参照
<!-- ## References among transactions and bundles -->

先頭を除くバンドル内の各トランザクションは`trunkTransaction`フィールドを通して[前のトランザクションを参照します](../references/structure-of-a-bundle.md)。この接続により、ノードはタングル内のバンドルを再構築し、バンドル内のすべてのトランザクションの内容を検証できます。
<!-- Each transaction in a bundle, except the head, [references the proceeding one](../references/structure-of-a-bundle.md) through the `trunkTransaction` field. These connections allow nodes to reconstruct bundles in the Tangle and validate the contents of all its transactions. -->

他の`branchTransaction`と`trunkTransaction`フィールドは、チップ選択後にノードが返したタングル内の2つの既存のバンドルの末尾トランザクションを参照します。
<!-- The other `branchTransaction` and `trunkTransaction` fields reference the tail transactions of two existing bundles in the Tangle that the node returned after tip selection. -->

:::info:
これらの参照を見るためには[トランザクションのバンドルを送信します](../how-to-guides/send-bundle.md)。
:::
<!-- :::info: -->
<!-- [Send a bundle of transactions](../how-to-guides/send-bundle.md) to see these references. -->
<!-- ::: -->

## ノードがバンドルを検証する方法
<!-- ## How nodes validate bundles -->

バンドルを[ノード](root://node-software/0.1/iri/introduction/overview.md)に送信すると、トランザクションは検証され、台帳に追加されます。
<!-- After you send a bundle to a [node](root://node-software/0.1/iri/introduction/overview.md), it validates the transactions and appends each one to its ledger. -->

[チップ選択](root://node-software/0.1/iri/concepts/tip-selection.md)の間、ノードは`trunkTransaction`フィールドを辿ることによって、[バンドル内の各トランザクションを見つけて検証します](root://node-software/0.1/iri/concepts/transaction-validation.md#bundle-validator)。ノードが先頭トランザクションまでのすべてのトランザクションを検証したら、バンドルは有効と見なされます。
<!-- During [tip selection](root://node-software/0.1/iri/concepts/tip-selection.md), a node finds and [validates each transaction in your bundle](root://node-software/0.1/iri/concepts/transaction-validation.md#bundle-validator) by traversing its `trunkTransaction` field. When the node has validated all transactions up to the head, your bundle is considered valid. -->

![Example of a bundle of 4 transactions](../images/bundle.png)

## バンドル例
<!-- ## Example bundles -->

### セキュリティレベル1のアドレスからの取り出し
<!-- ### Withdraw from address with security level 1 -->

次のバンドルは、セキュリティレベル1のアドレスから80iを受信者に転送します。
<!-- This bundle transfers 80 i to a recipient from an address with a security level of 1. -->

| **トランザクションインデックス** | **トランザクションの内容** | **トークン量** |
| --- | --- | --- |
| 0 | 受信者のアドレス | 80（受信者への送信量）|
| 1 | 送信者のアドレスと署名 | -100（送信者のアドレスのトークン合計量 マイナス表示される）|
| 2 | 残りのIOTAトークンを転送するためのアドレス（通常は送信者のアドレスの1つ）| 20（トランザクション1の送信者のアドレスの残りのトークン量）|

### セキュリティレベル2のアドレスからの取り出し
<!-- ### Withdraw from address with security level 2 -->

次のバンドルは、セキュリティレベル2のアドレスから80iを受信者に転送します。
<!-- This bundle transfers 80 i to a recipient from an address with a security level of 2. -->

| **トランザクションインデックス** | **トランザクションの内容** | **トークン量** |
| --- | --- | --- |
| 0 | 受信者のアドレス | 80（受信者への送信量）|
| 1 | 送信者のアドレスと署名の1番目の断片 | -100（送信者のアドレスのトークン合計量 マイナス表示される）|
| 2 | 送信者のアドレスと署名の最後の断片 | 0 |
| 3 | 残りのIOTAトークンを転送するためのアドレス（通常は送信者のアドレスの1つ）| 20（トランザクション1の送信者のアドレスの残りのトークン量）|

### セキュリティレベル3のアドレスからの取り出し
<!-- ### Withdraw from an address with security level 3 -->

次のバンドルは、セキュリティレベル3のアドレスから80iを受信者に転送します。
<!-- This bundle transfers 80 i to a recipient from an address with a security level of 3. -->

| **トランザクションインデックス** | **トランザクションの内容** | **トークン量** |
| --- | --- | --- |
| 0 | 受信者のアドレス | 80（受信者への送信量）|
| 1 | 送信者のアドレスと署名の1番目の断片 | -100（送信者のアドレスのトークン合計量 マイナス表示される）|
| 2 | 送信者のアドレスと署名の2番目の断片 | 0 |
| 3 | 送信者のアドレスと署名の最後の断片 | 0 |
| 4 | 残りのIOTAトークンを転送するためのアドレス（通常は送信者のアドレスの1つ）| 20（トランザクション1の送信者のアドレスの残りのトークン量）|
