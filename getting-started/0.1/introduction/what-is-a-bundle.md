# バンドルとは？
<!-- # What is a bundle? -->

**バンドルとは、データを格納したり、特定のアドレスから他のアドレスにIOTAトークンを転送するようにノードに指示することができる1つ以上のトランザクションのグループです。すべてのトランザクションは、単一のものであっても、常にバンドルとしてノードに送信されます。**
<!-- **A bundle is a group of one or more transactions that can contain data and/or instruct a node to transfer IOTA tokens from certain addresses to others. All transactions, even single ones, are always sent to a node as a bundle.** -->

バンドルの構造は、先頭トランザクション、本体トランザクション、および末尾トランザクションから構成されます。末尾トランザクションはインデックス0、先頭トランザクションはバンドル内の最後のトランザクションです。
<!-- The structure of a bundle consists of a head, a body, and a tail, where the tail is index 0 and the head is the last transaction in the bundle. -->

末尾トランザクションから始まるすべてのトランザクションは、次のインデックスを持つトランザクションを参照します。たとえば、末尾トランザクションはトランザクション1を参照し、トランザクション1はトランザクション2を参照します。これらの接続により、ノードはバンドルを再構築してその内容を検証できます。
<!-- All transactions, starting from the tail, reference the one with the next index. For example, the tail transaction references transaction 1, which references transaction 2, and so on. These connections allow nodes to reconstruct bundles and validate their contents. -->

バンドル内の各トランザクションの運命は、残りの部分によって決まります。すべてのトランザクションが有効か、またはすべて無効かです。
<!-- The fate of each transaction in a bundle depends on the rest. Either all transactions are valid or none of them are. -->

## IOTAトークンを転送するバンドルの例
<!-- ## Example of a bundle that transfers IOTA tokens -->

IOTAトークンを転送するには、バンドルに少なくとも1つの入力トランザクションと1つの出力トランザクションが含まれている必要があります。これらのバンドルは転送バンドルと呼ばれます。
<!-- To transfer IOTA tokens, a bundle must contain at least one input and one output transaction. These bundles are called transfer bundles. -->

今回の例では、受取人Aに100Miを送信したいとし、トークンが3つのアドレス0、1、2に別れているとします。
<!-- In this example, you want to send 100 Mi to recipient A, and your balance is distributed among three addresses: -->

* **アドレス0:** 20 Mi
* **アドレス1:** 30 Mi
* **アドレス2:** 55 Mi
<!-- * **Address 0:** 20 Mi -->
<!-- * **Address 1:** 30 Mi -->
<!-- * **Address 2:** 55 Mi -->

受信者Aに100Miを送信すると、以下のトランザクションが作成され、転送バンドルとしてノードに送信されます。
<!-- When you send 100 Mi to recipient A, the following transactions are created and sent to a node as a transfer bundle: -->

* **入力トランザクション：**送信者のアドレス0、1、2から100Miを取り出し、送信者がその3つのアドレスの所有権を有していることを検証するために署名をチェックするトランザクション
* **出力トランザクション：**受取人Aのアドレスへ100Miを送金するトランザクション
* **出力トランザクション：**アドレス2から残りの5Miをアドレス3に送金するトランザクション
<!-- * **Input transaction:** Withdraw 100 Mi from my address and check the signature to verify that I own it -->
<!-- * **Output transaction:** Deposit 100 Mi to the recipient's address -->
<!-- * **Output transaction:** Deposit the remaining 5 Mi from address 2 into address 3 -->

:::danger:アドレスの全残高を常に取り出す
IOTAはバンドルに署名するために[Winternitzワンタイム署名方式](root://dev-essentials/0.1/concepts/addresses-and-signatures.md#address-reuse)を使用しています。この署名方式は、アドレスから一度だけ安全にIOTAトークンを取り出せることを意味しています。よって、常にアドレスから全残高を取り出す必要があります。残高の一部だけを他の人に転送したい場合は、残りの残高を自分の未使用のアドレスの1つなどに転送する必要があります。
:::
<!-- :::danger:Always withdraw the total balance of an address -->
<!-- IOTA uses the [Winternitz one-time signature scheme](root://dev-essentials/0.1/concepts/addresses-and-signatures.md#address-reuse) to sign bundles. This signature scheme means that addresses can be safely withdrawn from only once, so you must always withdraw the total balance from an address. If you want to transfer only some of that balance to another person, you can transfer the remaining balance to one of your own unspent addresses. -->
<!-- ::: -->

[バンドルとトランザクションの詳細を学ぶ](root://dev-essentials/0.1/concepts/bundles-and-transactions.md)。
<!-- [Learn more about bundles and transactions](root://dev-essentials/0.1/concepts/bundles-and-transactions.md). -->
