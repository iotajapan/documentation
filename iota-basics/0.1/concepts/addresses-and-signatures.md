# アドレスと署名
<!-- # Addresses and signatures -->

**IOTAネットワーク内の各クライアントには、シードと呼ばれる秘密のパスワードがあります。シードは、アドレスの導出とバンドルの署名に使用されます。アドレスはIOTAトークンを保持するアカウントで、署名はアドレスの所有権を証明します。**
<!-- **Each client in an IOTA network has a secret password called a seed, which is used to derive addresses and to sign bundles. Addresses are the accounts that hold IOTA tokens and signatures prove ownership of an address.** -->

IOTAネットワークを使用するには、クライアントは[シードを作成して秘密にする](root://getting-started/0.1/tutorials/get-started.md)必要があります。シードとは、クライアントにアドレスへのアクセスを許可する81[トライト](../references/tryte-alphabet.md)の文字列です。
<!-- To use an IOTA network, clients must [create a seed and keep it private](root://getting-started/0.1/tutorials/get-started.md). A seed is a string of 81 [trytes](../references/tryte-alphabet.md) that gives a client access to addresses. -->

シードは、IOTAプロトコルの暗号学的ハッシュ関数のマスターキーです。各シードは、ほぼ無制限の固有の秘密鍵とアドレス（9<sup>57</sup>個）を導出することができます。
<!-- Seeds are the master keys to the cryptographic hashing function in the IOTA protocol. Each seed can derive an almost unlimited number of unique private keys and addresses (9<sup>57</sup>). -->

各秘密鍵は、シード、インデックス、およびセキュリティレベルによって固有のものであり、対応する1つのアドレスを導出するために使用されます。秘密鍵とアドレスは1対1のペアと考えることができます。アドレスは公開されており、クライアントはトランザクションの[`アドレス`フィールド]を使用してIOTAトークンとメッセージを送信できます。秘密鍵は秘密であり、アドレスからIOTAトークンを引き出すバンドルに署名するために使用されます。
<!-- Each private key is unique to a seed, index, and security level, and can be used to derive one corresponding address. A private key and an address can be thought of as a pair. Addresses are public and clients can send IOTA tokens and messages to them using the [`address` field] of a transaction. A private key is private and is used to sign bundles that withdraw IOTA tokens from the address. -->

秘密鍵とアドレスの各ペアには、独自のインデックスと[セキュリティレベル](../references/security-levels.md)があります。セキュリティレベルは秘密鍵の長さに影響します。セキュリティレベルが高いほど、秘密鍵が長くなり、トランザクションの署名がより安全になります。
<!-- Each pair of private keys and addresses has its own index and [security level](../references/security-levels.md). The security level affects the length of the private key. The greater the security level, the longer the private key, and the more secure a transaction's signature. -->

IOTAでは、署名方法の性質上、[各アドレスから1度だけしかIOTAトークンを取り出さない方が良い](#address-reuse)ため、秘密鍵とアドレスのペアが複数個必要となります。そのため、アドレスからIOTAトークンを取り出すたびに、インデックスをインクリメントするかセキュリティレベルを変更して、[新しいアドレスを作成する](../how-to-guides/create-an-address.md)必要があります。
<!-- In IOTA, multiple pairs of private keys and addresses are needed because [each address can be withdrawn from (spent) only once](#address-reuse). So, each time you withdraw from an address, you must [create a new address](../how-to-guides/create-an-address.md) by either incrementing the index or changing the security level. -->

:::info:
秘密鍵とアドレスのペアのセキュリティレベルが高いほど、攻撃者が使用済みアドレスの署名に対し総当たり攻撃を成功させることはより困難になります。
:::
<!-- :::info: -->
<!-- The greater the security level of a private key and address pair, the more difficult it is for an attacker to brute force the signature of a spent address. -->
<!-- ::: -->

:::warning:シードと秘密鍵を安全に保管してください。
シードはすべての秘密鍵とアドレスへの鍵です。そして、秘密鍵は1つのアドレスへの鍵です。

シードと秘密鍵は絶対に他人にバレないように安全に保管しなければなりません。
:::
<!-- :::warning:Keep seeds and private keys secure -->
<!-- A seed is the key to all your private keys and addresses. And, a private key is the key to one address. -->
<!--  -->
<!-- You must keep your seeds and private keys secure. -->
<!-- ::: -->

### 秘密鍵の導出方法
<!-- ### How private keys are derived -->

各秘密鍵は、シード、インデックス、およびセキュリティレベルを使い暗号学的ハッシュ関数から導出されます。
<!-- Each private key is derived from a cryptographic hashing function that takes a seed, an index, and a security level. -->

[Keccak-384ハッシュ関数](https://keccak.team/keccak.html)を使用してシードとインデックスを足し合わせたものをハッシュ化し、81トライトの**サブシード**を導出します。
<!-- The seed and index are combined and hashed, using the [Keccak-384 hashing function](https://keccak.team/keccak.html) to derive an 81-tryte **subseed**: -->

    hash(seed + index)

秘密鍵を導出するために、サブシードは[スポンジ関数](https://en.wikipedia.org/wiki/Sponge_function)に渡されます。スポンジ関数はサブシードを1度吸収し、セキュリティレベルごとに27回圧搾します（セキュリティレベル1だと27回、セキュリティレベル2だと54回、セキュリティレベル3だと81回圧搾します）。
<!-- To derive a private key, the subseed is passed to a [cryptographic sponge function](https://en.wikipedia.org/wiki/Sponge_function), which absorbs it and squeezes it 27 times per security level. -->

スポンジ関数の結果が、秘密鍵であり、[セキュリティレベル](../references/security-levels.md)に応じて、2,187、4,374、または6,561トライトとなります。
<!-- The result of the sponge function is a private key that consists of 2,187, 4,374, or 6,561 trytes, depending on the [security level](../references/security-levels.md). -->

### アドレスの導出方法
<!-- ### How addresses are derived -->

アドレスを導出するために、秘密鍵は**1つが81トライトのセグメント**に分割されます。その後、各セグメントは26回ハッシュ関数に通されます。 27個のハッシュ化されたセグメントのグループは**キーフラグメント**と呼ばれます。
<!-- To derive an address, the private key is split into **81-tryte segments**. Then, each segment is hashed 26 times. A group of 27 hashed segments is called a **key fragment**. -->

秘密鍵は2,187、4374、または6,561トライトで構成されているため、秘密鍵にはセキュリティレベルごとに1つのキーフラグメントがあります。たとえば、セキュリティレベル1の秘密鍵は2,187トライトで構成されています。これは27個のセグメントからなり、1つのキーフラグメントになります。セキュリティレベル2では2つのキーフラグメント、セキュリティレベル3では3つのキーフラグメントとなります。
<!-- Because a private key consists of 2,187, 4,374, or 6,561 trytes, a private key has one key fragments for each security level. For example, a private key with security level 1 consists of 2,187 trytes, which is 27 segments, which results in one key fragment. -->

各キーフラグメントは、セキュリティレベルごとに1つの**キーダイジェスト**を導出するために1回ハッシュ化されます。たとえば、1つのキーフラグメントによって1つのキーダイジェストが生成されます。
<!-- Each key fragment is hashed once to derive one **key digest** for each security level. For example, one key fragment results in one key digest. -->

そして、キーダイジェストが結合されて1回ハッシュ化され、81トライトのアドレスが導出されます。
<!-- Then, the key digests are combined and hashed once to derive an 81-tryte address. -->

:::info:アドレスを生成してみますか？
[秘密鍵からアドレスを導出する](../how-to-guides/derive-addresses-from-private-keys.md)では、JavaScriptクライアントライブラリを用いてアドレスを導出しています。
:::
<!-- :::info:Want to try this out? -->
<!-- Use the JavaScript client library to [derive addresses from private keys](../how-to-guides/derive-addresses-from-private-keys.md). -->
<!-- ::: -->

![Address generation](../images/address-generation.png)

### 秘密鍵でバンドルに署名する方法
<!-- ### How private keys sign bundles -->

秘密鍵は、アドレスからIOTAトークンを取り出すトランザクションのバンドルハッシュに署名し、その署名をトランザクションの[`signatureMessageFragment`フィールド](../references/structure-of-a-transaction.md)に入れます。
<!-- Private keys sign the bundle hash of the transaction that withdraws from the address and put that signature in the [`signatureMessageFragment` field](../references/structure-of-a-transaction.md) of the transaction. -->

バンドルハッシュに署名することで、攻撃者がバンドルハッシュを変更して署名が無効になることなく、バンドルを傍受してトランザクションを変更することは不可能です。
<!-- By signing the bundle hash, it's impossible for attackers to intercept a bundle and change any transaction without changing the bundle hash and invalidating the signature. -->

署名は、Winternitzワンタイム署名方式（W-OTS）を使用して作成されます。この署名スキームは量子耐性があり、署名は[量子コンピュータ](https://en.wikipedia.org/wiki/Quantum_computing)からの攻撃に対して耐性があることを意味します。
<!-- Signatures are created using the Winternitz one-time signature scheme (W-OTS). This signature scheme is quantum resistant, meaning that signatures are resistant to attacks from [quantum computers](https://en.wikipedia.org/wiki/Quantum_computing). -->

バンドルハッシュに署名するには、まず秘密鍵の半分だけが署名に表示されるように正規化します。
<!-- To sign a bundle hash, first it's normalized to make sure that only half of the private key is revealed in the signature. -->

バンドルハッシュが正規化されていない場合、W-OTSは未知数の秘密鍵を明らかにしてしまいます。秘密鍵の半分が明らかになるように正規化することで、アドレスから1度だけは安全にIOTAトークンを取り出すことができるようになります。
<!-- If the bundle hash weren't normalized, the W-OTS would reveal an unknown amount of the private key. By revealing half of the private key, an address can safely be withdrawn from once. -->
<a id="address-reuse"></a>

:::danger:使用済みアドレス
1つのアドレスから2回以上IOTAトークンを取り出すと（署名すると）、より多くの秘密鍵が漏洩するため、攻撃者はその署名に総当たり攻撃を行いIOTAトークンを盗むことができます。
:::
<!-- :::danger:Spent addresses -->
<!-- If an address is withdrawn from (spent) more than once, more of the private key is revealed, so an attacker could brute force its signature and steal the IOTA tokens. -->
<!-- ::: -->

秘密鍵が持つキーフラグメントの数に応じて、27、54、または81トライトの正規化バンドルハッシュが選択されます。これらのトライトはキーフラグメント内のセグメントの個数に対応しています。
<!-- Depending on the number of key fragments that a private key has, 27, 54, or 81 trytes of the normalized bundle hash are selected. These trytes correspond to the number of segments in a key fragment. -->

正規化バンドルハッシュの各トライトは、[10進数に変換](../references/tryte-alphabet.md)されます。そして、それぞれについて次の計算が実行されます。
<!-- The selected trytes of the normalized bundle hash are [converted to their decimal values](../references/tryte-alphabet.md). Then, the following calculation is performed on each of them: -->

    13 - 10進数の値
    <!-- 13 - decimal value -->

この計算結果は、署名フラグメントを導出するためにキーフラグメント内の27個のセグメントそれぞれがハッシュ化される回数です。各署名フラグメントには2,187トライトが含まれています。
<!-- The result of this calculation is the number of times that each of the 27 segments in the key fragment must be hashed to derive the signature fragment. Each signature fragment contains 2,187 trytes. -->

トランザクションの[`signatureMessageFragment`フィールド](../references/structure-of-a-transaction.md)に含めることができるのは2187トライトだけなので、1より大きいセキュリティレベルを持つインプットアドレスは、ゼロトークンのアウトプットトランザクションの`signatureMessageFragment`フィールドに残りの署名を分けて入れる必要があります。
<!-- Because a transaction's [`signatureMessageFragment` field](../references/structure-of-a-transaction.md) can contain only 2187 trytes, any input address with a security level greater than 1 must fragment the rest of the signature over zero-value output transactions. -->

### ノードによる署名の検証方法
<!-- ### How nodes verify signatures -->

ノードは、署名とバンドルハッシュを使用して入力トランザクションのアドレスを導出することによって、トランザクション内の署名を検証します。
<!-- Nodes verify a signature in a transaction by using the signature and the bundle hash to find the address of the input transaction. -->

署名を検証するために、トランザクションのバンドルハッシュは正規化されます。
<!-- To verify a signature, the bundle hash of a transaction is normalized. -->

署名の長さに応じて、正規化バンドルハッシュの27、54、または81トライトが選択されます。これらのトライトは、署名フラグメント内の81トライトセグメントの数に対応しています。
<!-- Depending on the length of the signature, 27, 54, or 81 trytes of the normalized bundle hash are selected. These trytes correspond to the number of 81-tryte segments in a signature fragment. -->

正規化バンドルハッシュの各トライトは[10進数に変換](../references/tryte-alphabet.md)されます。そして、それぞれについて次の計算が実行されます。
<!-- The selected trytes of the normalized bundle hash are [converted to decimal values](../references/tryte-alphabet.md). Then, the following calculation is performed on each of them: -->

    13 + 10進数の値
    <!-- 13 + decimal value -->

この計算の結果は、署名フラグメント内の27個のセグメントからキーフラグメントを導出するために、各セグメントをハッシュ化する回数です。
<!-- The result of this calculation is the number of times that each of the 27 segments in the signature fragments must be hashed to derive the key fragments. -->

各キーフラグメントは、キーダイジェストを導出するために1回ハッシュ化されます。そしてキーダイジェストは結合され、81トライトのアドレスを導出するために1回ハッシュ化されます。
<!-- Each key fragment is hashed once to derive the **key digests**, which are combined and hashed once to derive an 81-tryte address. -->

こうやって導出したアドレスがトランザクションのアドレスと一致する場合、署名は有効であり、IOTAトークンの取り出しは受け入れられます。
<!-- If the address matches the one in the transaction, the signature is valid and the withdrawal is accepted. -->
