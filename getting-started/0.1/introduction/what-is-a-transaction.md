# トランザクションとは？
<!-- # What is a transaction? -->

**トランザクションは、単独で使用することも、他のトランザクションと一緒にパッケージ化することもできる単一の操作です。単独型のトランザクションは、たとえばIOTAトークンを含まないデータのみを送信するものです。**
<!-- **A transaction is a single operation that can stand alone or be packaged with other transactions. Stand-alone transactions are those that contain no value, for example to send only data.** -->

トランザクションは、次のいずれかのタイプになります。
<!-- Transactions can be one of the following types: -->

* **入力トランザクション：**アドレスからIOTAトークンを取り出すトランザクション。このトランザクションには、バンドルに署名してアドレスの所有権を証明する署名が含まれている必要があります。署名が大きすぎると、バンドル内のゼロトークンの出力トランザクションで断片化されます。
<!-- * **Input transaction:** Withdraws IOTA tokens from an address. This transaction must contain the signature that signs the bundle and proves ownership of the address. If the signature is too large, it's fragmented over zero-value output transactions in the bundle. -->
* **出力トランザクション：**IOTAトークンを受信者のアドレスに入金するトランザクション。もしくはトークンが含まれていないトランザクション（ゼロトークントランザクション）。
<!-- * **Output transaction:** Deposits IOTA tokens into a recipient's address or contains no value (a zero-value transaction). -->

[バンドルとトランザクションの詳細を学んでみましょう](root://iota-basics/0.1/concepts/bundles-and-transactions.md)。
<!-- [Learn more about bundles and transactions](root://iota-basics/0.1/concepts/bundles-and-transactions.md). -->
