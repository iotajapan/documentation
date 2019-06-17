# バンドルとは？
<!-- # What is a bundle? -->

バンドルは、データを送信したり、ノードにIOTAトークンを特定のアドレスから特定のアドレスに転送するように指示する1つ以上のトランザクショングループです。バンドル内の各トランザクションの運命は、残りの部分に依存します。つまりすべてのトランザクションが有効か、またはすべて無効かです。
<!-- **A bundle is a group of one or more transactions, which send data or instruct a node to transfer IOTA tokens from certain addresses to others. The fate of each transaction in a bundle depends on the rest. Either all transactions are valid or none of them are.** -->

バンドルの構造は、先頭トランザクション、本体トランザクション、および末尾トランザクションから構成されます。末尾トランザクションはインデックス0、先頭トランザクションはバンドル内の最後のトランザクションです。
<!-- The structure of a bundle consists of a head, a body, and a tail, where the tail is index 0 and the head is the last transaction in the bundle. -->

末尾トランザクションから始まるすべてのトランザクションは、次のインデックスを持つトランザクションへの参照によって接続されます。たとえば、末尾トランザクションはトランザクション1に接続され、トランザクション1はトランザクション2に接続され、以下同様に続きます。これらの接続により、ノードはバンドルを再構築してその内容を検証できます。
<!-- All transactions, starting from the tail, are connected by reference to the one with the next index. For example, the tail transaction is connected to transaction 1, which is connected to transaction 2, and so on. These connections allow nodes to reconstruct bundles and validate their contents. -->

タングルにトランザクションを添付するために、先頭トランザクションはタングル内の他の2つのバンドルの末尾トランザクションに接続されます。 （末尾トランザクションと本体トランザクションは、これらの2つの末尾トランザクションの1つにも接続されています。）この2つの末尾トランザクションは、[チップ選択](root://the-tangle/0.1/concepts/tip-selection.md)時にノードによって選択されます。
<!-- To attach transactions to the Tangle, the head transaction is connected to the tails of two other bundles in the Tangle. (The tail and body transactions are connected to one of those tails as well.) These tail transactions are selected by nodes during [tip selection](root://the-tangle/0.1/concepts/tip-selection.md). -->

## IOTAトークンを転送するバンドルの例
<!-- ## Example of a bundle that transfers IOTA tokens -->

IOTAトークンを転送するには、バンドルに少なくとも1つの入力トランザクションと1つの出力トランザクションが含まれている必要があります。これらのバンドルは転送バンドルと呼ばれます。
<!-- To transfer IOTA tokens, a bundle must contain at least one input and one output transaction. These bundles are called transfer bundles. -->

今回の例では、受取人Aに100Miを送信したいとし、トークンが3つのアドレス0、1、2に別れているとします。
<!-- In this example, you want to send 100Mi to recipient A, and your balance is distributed among three addresses: -->

* **アドレス0:** 20Mi
* **アドレス1:** 30Mi
* **アドレス2:** 55Mi
<!-- * **Address 0:** 20Mi -->
<!-- * **Address 1:** 30Mi -->
<!-- * **Address 2:** 55Mi -->

受信者Aに100Miを送信すると、以下のトランザクションが作成され、転送バンドルとしてノードに送信されます。
<!-- When you send 100Mi to recipient A, the following transactions are created and sent to a node as a transfer bundle: -->

* **入力トランザクション：**送信者のアドレス0、1、2から100Miを取り出し、送信者がその3つのアドレスの所有権を有していることを検証するために署名をチェックするトランザクション
* **出力トランザクション：**受取人Aのアドレスへ100Miを送金するトランザクション
* **出力トランザクション：**アドレス2から残りの5Miをアドレス3に送金するトランザクション
<!-- * **Input transaction:** Withdraw 100Mi from my address and check the signature to verify that I own it -->
<!-- * **Output transaction:** Deposit 100Mi to the recipient's address -->
<!-- * **Output transaction:** Deposit the remaining 5Mi from address 2 into address 3 -->

:::danger:
[1度IOTAトークンを取り出したアドレスからは2度とIOTAトークンを取り出してはいけません](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse)。したがって、転送バンドルでは、IOTAトークンを取り出したアドレスの残りのIOTAトークンを新しいアドレスに送金するために、追加の出力トランザクションが必要になります。
:::
<!-- :::danger: -->
<!-- [You must not withdraw from an address more than once](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse). So, a transfer bundle may require an extra output transaction to deposit the remaining balance of a withdrawn address into a new address. -->
<!-- ::: -->

[バンドルの詳細を学んでみましょう](root://iota-basics/0.1/concepts/bundles-and-transactions.md)。
<!-- [Learn more about bundles](root://iota-basics/0.1/concepts/bundles-and-transactions.md). -->
