# アドレスと署名
<!-- # Addresses and signatures -->

**IOTAネットワークでトランザクションを送信するには、シードと呼ばれる秘密のパスワードが必要です。シードにより、シードから導出されたすべてのアドレスにアクセスできます。これらのアドレスはIOTAトークンを保持しているので、IOTAネットワーク内のすべてのノードに保存され、最新の状態に保たれる残高を持っています。IOTAトークンを使用するには、トランザクションを作成し、そのトランザクションが入っているバンドルに署名して、IOTAトークンを保持しているアドレスを所有していることをノードに証明する必要があります。**
<!-- **To send transactions in an IOTA network, you need a secret password called a seed, which gives you access to all your addresses. These addresses hold your IOTA tokens and as such have a balance that's stored and kept up to date on all nodes in an IOTA network. To spend IOTA tokens, you must create a transaction and sign the bundle it's in to prove to a node that you own the address that holds them.** -->

IOTAネットワーク内のすべてのノードは、すべてのアドレスのプラスの残高を記録します。
<!-- All nodes in an IOTA network keep a record of the positive balances of all addresses. -->

この記録は以下のようになります。アドレスはセミコロンの左側にあり、残高は右側にあります。
  <!-- This record looks something like this, where the address is on the left of the semicolon and the balance is on the right: -->

    ```bash
    ADDRESS....ENDOFADDRESS;1000
    ```

ノードは、どのクライアントがアドレスを所有しているのかを知りません。なぜならノードはクライアントのシードを持っていないからです。そのため、ノードは暗号化を使用してトランザクションを検証します。
<!-- Nodes don't know which client owns an address because they don't have the clients' seeds. So, nodes use cryptography to validate a transaction. -->

ノードがトランザクションを検証するとき、トランザクションがIOTAトークンを取り出す場合、そのトランザクションはアドレスを所有するシードの所有者によって作成されたことを確認します。ノードはトランザクションの署名を確認して、アドレスの所有権の確認を行います。
<!-- When a node validates a transaction, it makes sure that, if it withdraws IOTA tokens, it was created by the owner of the seed that owns the address. The node does this by checking the transaction signature. -->

有効な署名を作成するには、IOTAトークンを取り出す元のアドレスに対応する秘密鍵が必要です。
<!-- To create a valid signature, you need the private key that corresponds to the address from which IOTA tokens are being withdrawn. -->

この秘密鍵を作成する唯一の方法は、アドレスの作成に使用されたシードを所有することです。このように、署名は、秘密鍵の所有権、つまりシードの所有権を証明することによって、アドレスの所有権を証明します。
<!-- The only way to create this private key is by owning the seed that was used to create the address. This way, signatures prove ownership of an address by proving ownership of the private key and thus the seed. -->

## IOTAでのシードの使い方
<!-- ## How seeds are used in IOTA -->

シードはIOTAプロトコル内の[Keccak-384](https://keccak.team/keccak.html)を使用したKerl [ハッシュ関数](https://www.techopedia.com/definition/19744/hash-function)へのマスターキーです。このハッシュ関数は、シード、インデックス、およびセキュリティレベルを取得して、アドレスと秘密鍵のペアを作成します。
<!-- Seeds are the master keys to the Kerl [hash function](https://www.techopedia.com/definition/19744/hash-function) in the IOTA protocol, which uses [Keccak-384](https://keccak.team/keccak.html). This hash function takes a seed, an index, and a security level to create an address and private key pair: -->

* **シード：** クライアントによって選択された一意な81[トライト](../references/tryte-alphabet.md)
<!-- * **Seed:** Unique 81 [trytes](../references/tryte-alphabet.md) chosen by the client -->
* **インデックス：** どのアドレスと秘密鍵のペアを作成するかを変更する番号
<!-- * **Index:** Number that changes which address and private key pair is created -->
* **セキュリティレベル：** 秘密鍵の長さに影響を与える1から3の間の数
<!-- * **Security level:** Number between 1 and 3 that affects the length of a private key -->

秘密鍵は、アドレスの所有権を証明するために、アドレスからIOTAトークンを取り出すバンドルに署名するために使用されます。各アドレスには対応する秘密鍵があります。そのため、アドレスからIOTAトークンを取り出すには、アドレスと同じシード、インデックス、およびセキュリティレベルから秘密鍵を作成して、所有していることを証明する必要があります。
<!-- A private key is used to sign bundles that withdraw IOTA tokens from an address to prove you own it. Each address has a corresponding private key. So, to withdraw IOTA tokens from an address, you need to prove you own it by creating a private key from the same seed, index, and security level as the address. -->

同じシード、インデックス、およびセキュリティレベルを使用すると、ハッシュ関数は常に同じアドレスと秘密鍵のペアを返します。シードを使用して、インデックスを増加させ、3つのセキュリティレベルすべてを使用することによって、ほぼ無制限の数のアドレスと秘密鍵のペア（9<sup>57</sup>）を作成することができます。
<!-- If you use the same seed, index, and security level, the hash function will always return the same address and private key pair. Seeds can be used to create an almost unlimited number of addresses and private key pairs (9<sup>57</sup>) by incrementing the index and using all three security levels. -->

:::info:
これを試すには、JavaScriptクライアントライブラリを使って[新しいアドレスを作成します](../how-to-guides/create-an-address.md)。
:::
<!-- :::info: -->
<!-- You can try this by using our JavaScript client library to [create a new address](../how-to-guides/create-an-address.md). -->
<!-- ::: -->

アドレスを生成する最初の手順は、シード、インデックス、およびセキュリティレベルから秘密鍵を取得することです。
<!-- The first step to generate an address is to derive a private key from the seed, index, and security level. -->

## 秘密鍵の導出方法
<!-- ## How private keys are derived -->

各秘密鍵は、シード、インデックス、およびセキュリティレベルをKerlでハッシュ化することによって導出されます。
<!-- Each private key is derived by hashing a seed, an index, and a security level with Kerl. -->

まず、シードとインデックスを組み合わせてハッシュ化し、81トライトの**subseed **を導出します。
<!-- First, the seed and index are combined and hashed to derive an 81-tryte **subseed**: -->

    ```bash
    Kerl(seed + index)
    ```

秘密鍵を導出するために、セキュリティレベルごとに27回[スポンジ関数](https://keccak.team/sponge_duplex.html)で**subseed**を吸収して圧搾します。
<!-- To derive a private key, the subseed is absorbed and squeezed in a [sponge function](https://keccak.team/sponge_duplex.html) 27 times per security level. -->

スポンジ関数の結果は、[セキュリティレベル](../references/security-levels.md)に応じて長さが異なる秘密鍵です。セキュリティレベルが高いほど、秘密鍵はより長くより安全になります。
<!-- The result of the sponge function is a private key with a length that varies, depending on the [security level](../references/security-levels.md). The greater the security level, the longer and more secure the private key. -->

## アドレスの導出方法
<!-- ## How addresses are derived -->

アドレスを導出するために、秘密鍵は**1つが81トライトのセグメント**に分割されます。その後、各セグメントは26回ハッシュ関数に通されます。 27個のハッシュ化されたセグメントのグループは**キーフラグメント**と呼ばれます。
<!-- To derive an address, the private key is split into **81-tryte segments**. Then, each segment is hashed 26 times. A group of 27 hashed segments is called a **key fragment**. -->

秘密鍵は2,187、4374、または6,561トライトで構成されているため、秘密鍵にはセキュリティレベルごとに1つのキーフラグメントがあります。たとえば、セキュリティレベル1の秘密鍵は2,187トライトで構成されています。これは27個のセグメントからなり、1つのキーフラグメントになります。セキュリティレベル2では2つのキーフラグメント、セキュリティレベル3では3つのキーフラグメントとなります。
<!-- Because a private key consists of 2,187, 4,374, or 6,561 trytes, a private key has one key fragments for each security level. For example, a private key with security level 1 consists of 2,187 trytes, which is 27 segments, which results in one key fragment. -->

各キーフラグメントは、セキュリティレベルごとに1つの**キーダイジェスト**を導出するために1回ハッシュ化されます。たとえば、1つのキーフラグメントによって1つのキーダイジェストが生成されます。
<!-- Each key fragment is hashed once to derive one **key digest** for each security level. For example, one key fragment results in one key digest. -->

そして、キーダイジェストが結合されて1回ハッシュ化され、81トライトのアドレスが導出されます。
<!-- Then, the key digests are combined and hashed once to derive an 81-tryte address. -->

:::info:
トリニティなどのアプリケーションでは、末尾に9トライトのチェックサムを含むアドレスを使用する必要があります。
:::
<!-- :::info: -->
<!-- Some application such as Trinity require you to use addresses that include a 9-tryte checksum on the end. -->
<!-- ::: -->

![Address generation](../images/address-generation.png)

:::info:アドレスを導出する
JavaScriptクライアントライブラリを使用して[秘密鍵からアドレスを導出します](../how-to-guides/derive-addresses-from-private-keys.md)。
:::
<!-- :::info:Want to try this out? -->
<!-- Use the JavaScript client library to [derive addresses from private keys](../how-to-guides/derive-addresses-from-private-keys.md). -->
<!-- ::: -->

## 秘密鍵を使用してバンドルに署名する方法
<!-- ## How private keys are used to sign bundles -->

秘密鍵はアドレスからIOTAトークンを取り出すトランザクションのバンドルハッシュに署名し、その署名はトランザクションの[`signatureMessageFragment`フィールド](../references/structure-of-a-transaction.md)に置かれます。
<!-- Private keys sign the bundle hash of the transaction that withdraws IOTA tokens from the address, then the signature is put in the [`signatureMessageFragment` field](../references/structure-of-a-transaction.md) of the transaction. -->

バンドルハッシュに署名することで、攻撃者がバンドルハッシュを変更して署名を無効にすることなくバンドルを傍受してトランザクションを変更することは不可能です。
<!-- By signing the bundle hash, it's impossible for attackers to intercept a bundle and change any transaction without changing the bundle hash and invalidating the signature. -->

:::info:
バンドルハッシュは、各トランザクションの`address`、`value`、`obsoleteTag`、`currentIndex`、`lastIndex`、そして`timestamp`フィールドの値のハッシュ値から導出されます。このバンドルハッシュは、パッケージを保証するために各トランザクションの`bundle`フィールドに含まれています。これらのフィールドのいずれかの値が変更されると、ノードはバンドルハッシュを無効にします。
:::
<!-- :::info: -->
<!-- The bundle hash is derived from a hash of the values of each transaction's `address`, `value`, `obsoleteTag`, `currentIndex`, `lastIndex` and `timestamp` fields. This bundle hash is included in each transaction's `bundle` field to seal the package. If the values of any of these fields were to change, the nodes would invalidate the bundle hash. -->
<!-- ::: -->

署名は、Winternitzワンタイム署名方式（W-OTS）を使用して作成されます。この署名スキームは量子耐性があります。つまり、署名は[量子コンピュータ](https://en.wikipedia.org/wiki/Quantum_computing)からの攻撃に対して耐性があります。しかし、この署名方式は未知数の秘密鍵の一部も明らかにします。
<!-- Signatures are created using the Winternitz one-time signature scheme (W-OTS). This signature scheme is quantum resistant, meaning that signatures are resistant to attacks from [quantum computers](https://en.wikipedia.org/wiki/Quantum_computing). But, this signature scheme also reveals an unknown amount of the private key. -->
<a id="address-reuse"></a>

:::danger:署名済みアドレス
1つのアドレスから2回以上IOTAトークンを取り出すと（署名すると）、より多くの秘密鍵が漏洩するため、攻撃者はその署名に総当たり攻撃を行いIOTAトークンを盗むことができます。
:::
<!-- :::danger:Spent addresses -->
<!-- If an address is withdrawn from (spent) more than once, more of the private key is revealed, so an attacker could brute force its signature and steal the IOTA tokens. -->
<!-- ::: -->

アドレスから一度だけ取り出すのが常に安全であることを保証するために、最初にバンドルハッシュを正規化して、秘密鍵の半分だけが署名で明らかになるようにします。
<!-- To make sure that it's always safe to withdraw from an address once, first the bundle hash is normalized to make sure that only half of the private key is revealed in the signature. -->

秘密鍵が持つキーフラグメントの数に応じて、27、54、または81トライトの正規化バンドルハッシュが選択されます。これらのトライトはキーフラグメント内のセグメントの個数に対応しています。
<!-- Depending on the number of key fragments that a private key has, 27, 54, or 81 trytes of the normalized bundle hash are selected. These trytes correspond to the number of segments in a key fragment. -->

正規化バンドルハッシュの各トライトは、[10進数に変換](../references/tryte-alphabet.md)されます。そして、それぞれについて次の計算が実行されます。
<!-- The selected trytes of the normalized bundle hash are [converted to their decimal values](../references/tryte-alphabet.md). Then, the following calculation is performed on each of them: -->

    ```bash
    13 - 10進数の値
    <!-- 13 - decimal value -->
    ```

この計算結果は、署名フラグメントを導出するためにキーフラグメント内の27個のセグメントそれぞれがハッシュ化される回数です。各署名フラグメントには2,187トライトが含まれています。
<!-- The result of this calculation is the number of times that each of the 27 segments in the key fragment must be hashed to derive the signature fragment. Each signature fragment contains 2,187 trytes. -->

トランザクションの[`signatureMessageFragment`フィールド](../references/structure-of-a-transaction.md)に含めることができるのは2187トライトだけなので、1より大きいセキュリティレベルを持つインプットアドレスは、ゼロトークンのアウトプットトランザクションの`signatureMessageFragment`フィールドに残りの署名を分けて入れる必要があります。
<!-- Because a transaction's [`signatureMessageFragment` field](../references/structure-of-a-transaction.md) can contain only 2187 trytes, any input address with a security level greater than 1 must fragment the rest of the signature over zero-value output transactions. -->

## ノードが署名を検証する方法
<!-- ## How nodes verify signatures -->

ノードは、署名とバンドルハッシュを使用して入力トランザクションのアドレスを導出することによって、トランザクション内の署名を検証します。
<!-- Nodes verify a signature in a transaction by using the signature and the bundle hash to find the address of the input transaction. -->

署名を検証するために、トランザクションのバンドルハッシュは正規化されます。
<!-- To verify a signature, the bundle hash of a transaction is normalized. -->

署名の長さに応じて、正規化バンドルハッシュの27、54、または81トライトが選択されます。これらのトライトは、署名フラグメント内の81トライトセグメントの数に対応しています。
<!-- Depending on the length of the signature, 27, 54, or 81 trytes of the normalized bundle hash are selected. These trytes correspond to the number of 81-tryte segments in a signature fragment. -->

正規化バンドルハッシュの各トライトは[10進数に変換](../references/tryte-alphabet.md)されます。そして、それぞれについて次の計算が実行されます。
<!-- The selected trytes of the normalized bundle hash are [converted to decimal values](../references/tryte-alphabet.md). Then, the following calculation is performed on each of them: -->

    ```bash
    13 + 10進数の値
    <!-- 13 + decimal value -->
    ```

この計算の結果は、署名フラグメント内の27個のセグメントからキーフラグメントを導出するために、各セグメントをハッシュ化する回数です。
<!-- The result of this calculation is the number of times that each of the 27 segments in the signature fragments must be hashed to derive the key fragments. -->

各キーフラグメントは、キーダイジェストを導出するために1回ハッシュ化されます。そしてキーダイジェストは結合され、81トライトのアドレスを導出するために1回ハッシュ化されます。
<!-- Each key fragment is hashed once to derive the **key digests**, which are combined and hashed once to derive an 81-tryte address. -->

こうやって導出したアドレスがトランザクションのアドレスと一致する場合、署名は有効であり、IOTAトークンの取り出しは受け入れられます。
<!-- If the address matches the one in the transaction, the signature is valid and the withdrawal is accepted. -->
