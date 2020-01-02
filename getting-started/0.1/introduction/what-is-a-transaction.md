# トランザクションとは？
<!-- # What is a transaction? -->

**IOTA ネットワークに何か送信したい場合は、トランザクションの形でノードに送信する必要があります。トランザクションは、入力トランザクションまたは出力トランザクションのいずれかになり得る単一の操作です。**
<!-- **If you want to send anything to an IOTA network, you must send it to a node in the form of a transaction. A transaction is a single operation that can be either an input or an output.** -->

**入力トランザクション**はアドレスから IOTA トークンを取り出します。このトランザクションには、バンドルに署名してアドレスの所有権を証明する署名が含まれている必要があります。
<!-- An **input transaction** withdraws IOTA tokens from an address. This transaction must contain the signature that signs the bundle and proves ownership of the address. -->

**出力トランザクション**は、IOTA トークンを受信者のアドレスにデポジットし、または IOTA トークンが含まれていません（ゼロトークントランザクション）。
<!-- An **output transaction** deposits IOTA tokens into a recipient's address or contains no value (a zero-value transaction). -->

ノードにトランザクションを送信する前に、トランザクションは常にバンドルにグループ化されています。
<!-- Before you send transactions to a node they're always grouped into a bundle. -->

ノードがどのトランザクションに関連情報が含まれているかを知るためにバンドルが必要です。たとえば、トランザクションの署名が大きすぎる場合、同じバンドル内の複数のゼロトークン出力トランザクションにわたって断片化されています。
<!-- Bundles are necessary so that nodes know which transactions contain related information. For example, if a transaction's signature is too large, it's fragmented over several zero-value output transactions in the same bundle. -->

別の例として、IOTA トークンを転送するには、IOTA トークンを取り出すための入力トランザクションと、トークンを新しいアドレスにデポジットするための出力トランザクションが少なくとも1つ必要です。そのためには、それらのトランザクションを同じバンドルにまとめます。
<!-- Another example is that to transfer IOTA tokens, you need at least one input transaction to withdraw the IOTA tokens and one output transaction to deposit the tokens into a new address. To do so, you group those transactions in the same bundle. -->
