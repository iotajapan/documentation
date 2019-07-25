# バンドルとトランザクション
<!-- # Bundles and transactions -->

**トランザクションは、ノードに送信できる単一の操作です。トランザクションはIOTAトークンの取り出し/預け入れや、データを送信することができます。ノードに1つ以上のトランザクションを送信するには、トランザクションをバンドルにパッケージ化する必要があります。**
<!-- **A transaction is a single operation that you can send to a node. Transactions can withdraw/deposit IOTA tokens or send data. To send a node one or more transactions, you must package them in a bundle.** -->

バンドル内の各トランザクションは、IOTAプロトコルに従って[構造化](../references/structure-of-a-transaction.md)され、すべてのトランザクションフィールドに対して有効な値を含んでいる必要があります。
<!-- Each transaction in a bundle must be [structured](../references/structure-of-a-transaction.md) according to the IOTA protocol and contain valid values for all the transaction fields. -->

トランザクションがバンドルにパッケージ化されると、バンドル内での位置を定義する`currentIndex`フィールドと、バンドルの終わり（先頭トランザクション）を定義する`lastIndex`フィールドの両方が与えられます。次に、先頭トランザクションを除くバンドル内の各トランザクションは、`trunkTransaction`フィールドを介して互いに接続されます。次に、各トランザクションの`address`、`value`、`obsoleteTag`、`currentIndex`、`lastIndex`、および`timestamp`の各フィールドの値が、スポンジ関数によって吸収され圧搾され、81トライトのバンドルハッシュ値が生成されます。このバンドルハッシュ値は、パッケージを一纏めにするために各トランザクションのバンドルフィールドに含まれています。
<!-- When a transaction is packaged in a bundle, it's given both a `currentIndex` field, which defines its place in the bundle, and a `lastIndex` field, which defines the end of the bundle (head). Next, each transaction in the bundle, except the head, is [connected to each other](../references/structure-of-a-bundle.md) through the `trunkTransaction` field. Then, the values of each transaction's `address`, `value`, `obsoleteTag`, `currentIndex`, `lastIndex` and `timestamp` fields are absorbed and squeezed by a cryptographic sponge function to produce an 81-tryte bundle hash. This bundle hash is included in each transaction's `bundle` field to seal the package. -->

バンドルは不可分です。つまり、バンドル内のトランザクションのいずれかが変更された場合、各トランザクションのバンドルハッシュ値は無効になります。
<!-- Bundles are atomic, meaning that if any of the transactions in the bundle change, the bundle hash of each transaction would be invalid. -->

バンドルを不可分にする必要がある理由を説明するために、次の例を挙げます。
<!-- To explain why bundles need to be atomic, take this example. -->

オンラインで精算をするとして、支払うべき合計が10Miとします。あなたのシードは2つのアドレス（インデックス0と1）を持ち、両方とも5Miを含みます。したがって、3つのトランザクションを作成します。アドレス0から5Miを取り出す入力トランザクション、アドレス1から5Miを取り出す入力トランザクション、およびベンダーのアドレスに10Miを支払う出力トランザクションです。（入力トランザクションの両方のアドレスがセキュリティレベル1の秘密鍵から作成されたものとします。そのため、署名は各入力トランザクションに含まれています。）
<!-- You're at an online checkout and the total to pay is 10Mi. Your seed has 2 addresses (index 0 and 1), which both contain 5Mi. So, you create three transactions: One input transaction to withdraw 5Mi from address 0, another input transaction to withdraw 5Mi from address 1, and one output transaction to deposit 10Mi to the vendor's address. (We'll assume that both addresses in the input transactions were created from a private key with security level 1, so the signatures can fit in each transaction.) -->

ベンダーが10Miを受け取るには、これら3つのトランザクションすべてが有効でなければなりません。各トランザクションは、IOTAトークンを転送するという全体的な目標を達成するために、互いの有効性に依存する連続的な命令です。
<!-- For the vendor to receive 10Mi, all three of those transactions must be valid. They're sequential instructions that rely on each other's validity to achieve the overall goal of transferring IOTA tokens. -->

:::info:
バンドルにパッケージ化する必要があるのは、複数のトランザクションだけではありません。
1つのトランザクションでも、パッケージ化する必要があります。
:::
<!-- :::info: -->
<!-- It's not just multiple transactions that need to be packaged in a bundle, even individual ones do. -->
<!-- ::: -->

## 取り出しと預け入れ
<!-- ## Withdrawals and deposits -->

バンドルは、任意の数の取り出しと預け入れのトランザクションで構成できます。ただし、[プルーフオブワーク](root://the-tangle/0.1/concepts/proof-of-work.md)には時間とリソースが関係しているため、バンドル内で最大30個のトランザクションを推奨します。
<!-- A bundle can consist of any number of withdrawals and deposits. However because of the time and resources that are involved during [proof of work](root://the-tangle/0.1/concepts/proof-of-work.md), we recommend a maximum of 30 transactions in a bundle. -->

### 入力トランザクション
<!-- ### Input transaction -->

入力トランザクションはアドレスからIOTAトークンを取り出します。
<!-- Input transactions withdraw IOTA tokens from addresses. -->

バンドルには複数の入力トランザクションを含めることができ、それぞれに有効な署名を含める必要があります。署名の長さはアドレスの[セキュリティレベル](../references/security-levels.md)によって異なります。アドレスのセキュリティレベルが1より大きい場合、署名は1つのトランザクションに収まるには大きすぎるため、ゼロトークン出力トランザクションに断片して収める必要があります。
<!-- Bundles can contain multiple input transactions, and each one must include a valid signature. The length of the signature depends on the [security level](../references/security-levels.md) of the address. If the security level of the address is greater than 1, the signature is too large to fit in one transaction and must be fragmented across zero-value output transactions. -->

:::danger:
[同じアドレスからは2回以上トークンを取り出してはいけません](../concepts/addresses-and-signatures.md#address-reuse)。したがって、送信側が受信側にすべてのトークンを転送しない場合でも、入力トランザクションはアドレスからすべてのIOTAトークンを取り出す必要があります。残りのIOTAトークンは、出力トランザクションの残り用アドレス（通常は送信者のアドレス）に預け入れることになります。
:::
<!-- :::danger: -->
<!-- [Addresses must not be withdrawn from more than once](../concepts/addresses-and-signatures.md#address-reuse). Therefore, input transactions must withdraw all IOTA tokens from an address even if the sender does not want to transfer all of them to the recipient. The remaining IOTA tokens can be deposited into a remainder address (usually the sender's address) in an output transaction. -->
<!-- ::: -->

### 出力トランザクション
<!-- ### Output transaction -->

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

## バンドルの検証方法
<!-- ## How bundles are validated -->

バンドルを[ノード](root://node-software/0.1/iri/introduction/overview.md)に送信すると、各トランザクションは検証され、台帳に追加されます。
<!-- After you send a bundle to a [node](root://node-software/0.1/iri/introduction/overview.md), it validates each transaction and appends each one to its ledger. -->

[チップ選択](root://the-tangle/0.1/concepts/tip-selection.md)の間、ノードはその`trunkTransaction`フィールドを辿ることによって、[バンドル内の各トランザクションを見つけて検証します](root://node-software/0.1/iri/concepts/transaction-validation.md#bundle-validator)。ノードが先頭トランザクション（または[`lastIndex`フィールド](../references/structure-of-a-transaction.md)）までのすべてのトランザクションを検証したら、バンドルは有効と見なされます。
<!-- During [tip selection](root://the-tangle/0.1/concepts/tip-selection.md), a node finds and [validates each transaction in your bundle](root://node-software/0.1/iri/concepts/transaction-validation.md#bundle-validator) by traversing its `trunkTransaction` field. When the node has validated all transactions up to the head (or [`lastIndex` field](../references/structure-of-a-transaction.md)), your bundle is considered valid. -->

![Example of a bundle of 4 transactions](../images/bundle.png)

## バンドル例
<!-- ## Example bundles -->

### セキュリティレベル1のアドレスからの取り出し
<!-- ### Withdraw from address with security level 1 -->

次のバンドルは、セキュリティレベル1のアドレスから80iを受信者に転送します。
<!-- This bundle transfers 80i to a recipient from an address with a security level of 1. -->

| トランザクションインデックス | トランザクションの内容 | トークン量 |
| --- | --- | --- |
| 0 | 受信者のアドレス | 80（受信者への送信量）|
| 1 | 送信者のアドレスと署名 | -100（送信者のアドレスのトークン合計量 マイナス表示される）|
| 2 | 残りのIOTAトークンを転送するためのアドレス（通常は送信者のアドレスの1つ）| 20（トランザクション1の送信者のアドレスの残りのトークン量）|

### セキュリティレベル2のアドレスからの取り出し
<!-- ### Withdraw from address with security level 2 -->

次のバンドルは、セキュリティレベル2のアドレスから80iを受信者に転送します。
<!-- This bundle transfers 80i to a recipient from an address with a security level of 2. -->

| トランザクションインデックス | トランザクションの内容 | トークン量 |
| --- | --- | --- |
| 0 | 受信者のアドレス | 80（受信者への送信量）|
| 1 | 送信者のアドレスと署名の1番目の断片 | -100（送信者のアドレスのトークン合計量 マイナス表示される）|
| 2 | 送信者のアドレスと署名の最後の断片 | 0 |
| 3 | 残りのIOTAトークンを転送するためのアドレス（通常は送信者のアドレスの1つ）| 20（トランザクション1の送信者のアドレスの残りのトークン量）|

### セキュリティレベル3のアドレスからの取り出し
<!-- ### Withdraw from an address with security level 3 -->

次のバンドルは、セキュリティレベル3のアドレスから80iを受信者に転送します。
<!-- This bundle transfers 80i to a recipient from an address with a security level of 3. -->

| トランザクションインデックス | トランザクションの内容 | トークン量 |
| --- | --- | --- |
| 0 | 受信者のアドレス | 80（受信者への送信量）|
| 1 | 送信者のアドレスと署名の1番目の断片 | -100（送信者のアドレスのトークン合計量 マイナス表示される）|
| 2 | 送信者のアドレスと署名の2番目の断片 | 0 |
| 3 | 送信者のアドレスと署名の最後の断片 | 0 |
| 4 | 残りのIOTAトークンを転送するためのアドレス（通常は送信者のアドレスの1つ）| 20（トランザクション1の送信者のアドレスの残りのトークン量）|
