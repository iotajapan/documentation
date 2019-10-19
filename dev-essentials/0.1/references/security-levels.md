# セキュリティレベル
<!-- # Security levels -->

**以下の表には、秘密鍵とアドレスのペアで考えられるセキュリティレベルが表示されています。**
<!-- **This table displays the possible security levels of a private key and address pair.** -->

アドレスを作成するとき、アドレスの派生元のシード、インデックス、およびセキュリティレベルを指定できます。
<!-- When you create an address, you can specify a seed, an index, and a security level from which to derive it. -->

セキュリティレベルは、使用済みアドレスの署名がブルートフォース攻撃に対してどれだけ安全かを決定します。セキュリティレベルが高いほど、署名は大きくなり、より安全になります。ただし、セキュリティレベルが高いということは、トランザクションに署名するためにより多くの計算が必要であり、計算結果の署名が大きすぎて1つのトランザクションに収まらないことを意味します。その結果、低電力デバイスはセキュリティレベル2を使用したい場合がありますが、大規模企業はセキュリティレベル3を使用したい場合があります。
<!-- The security level dictates how secure the signature of a spent address is against brute force attacks. The greater the security level, the larger and more secure the signature. But, a greater security level also means that more computations are needed to sign a transaction, and the resulting signature is too large to fit in one transaction. As a result, low-powered devices may want to use security level 2, whereas a large-scale company may want to use security level 3. -->

:::info:
[アドレスと署名](../concepts/addresses-and-signatures.md)を参照してください。
:::
<!-- :::info: -->
<!-- [Learn more about addresses and signatures](../concepts/addresses-and-signatures.md). -->
<!-- ::: -->


| **セキュリティレベル** | **秘密鍵と署名の長さ（トライト）** |
| :--- | :--- |
| 1 | 2,187（非推奨）|
| 2 | 4,374（トリニティで使用） |
| 3 | 6,561（最も安全） |
<!-- | **Security Level** | **Private key/signature length in trytes** | -->
<!-- | :-------------- | :-------------------------- | -->
<!-- | 1              | 2,187 (not recommended)| -->
<!-- | 2              | 4,374 (used by Trinity)         | -->
<!-- | 3              | 6,561 (most secure)           | -->
