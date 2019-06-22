# セキュリティレベル
<!-- # Security levels -->

**以下の表には、秘密鍵とアドレスのペアで考えられるセキュリティレベルが表示されています。セキュリティレベルが高ければ高いほど、使用済みアドレスの署名はより大きく、より安全になります。**
<!-- **This table displays the possible security levels of a private key and address pair. The greater the security level, the larger and more secure the signature of a spent address is against brute force attacks.** -->

セキュリティレベルが高いほど、トランザクションに署名するためにより多くの計算を行わなければならず、署名を含めるにはバンドル内により多くのトランザクションが必要になります。その結果、低電力機器はセキュリティレベル2を使用したいと思うかもしれませんが、大規模企業はセキュリティレベル3を使用したいと思うかもしれません。
<!-- A greater security level means that more computations must be done to sign a transaction and that more transactions are needed in a bundle to contain the signature. As a result, low-powered devices may want to use security level 2, whereas a large-scale company may want to use security level 3. -->

| **セキュリティレベル** | **秘密鍵と署名の長さ** |
| :--- | :--- |
| 1 | 2,187トライト（非推奨）|
| 2 | 4,374トライト（トリニティで使用） |
| 3 | 6,561トライト（最も安全） |
