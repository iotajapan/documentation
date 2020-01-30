# 署名
<!-- # Signatures -->

**署名は，所有権のデジタル証明です．[アドレス](../clients/addresses.md)の所有権を証明するには，秘密鍵でバンドルに署名して，そのバンドル（アドレス）の所有権を私が有していることを他の人が確認できるようにします．**
<!-- **Signatures are digital proof of ownership. To prove ownership of an [address](../clients/addresses.md) you sign a bundle with its private key so that others can verify that you own it.** -->

## 署名スキーム
<!-- ## Signature scheme -->

IOTA は Winternitz [ワンタイム署名スキーム](https://en.wikipedia.org/wiki/Hash-based_cryptography#One-time_signature_schemes)（W-OTS）を使用して[デジタル署名](https://en.wikipedia.org/wiki/Digital_signature)を生成します．この署名方式は量子ロバストです．つまり，署名は量子コンピューターからの攻撃に対して耐性があります．ただし，このスキームは，秘密鍵の未知の量を公開してしまいます．その結果，アドレスから一度だけ IOTA トークンを取り出すことは安全です．その後，IOTA トークンを取り出されたアドレスは[使用済みアドレス](../clients/addresses.md#spent-addresses)になります．
<!-- IOTA uses the Winternitz [one-time signature scheme](https://en.wikipedia.org/wiki/Hash-based_cryptography#One-time_signature_schemes) (W-OTS) to generate [digital signatures](https://en.wikipedia.org/wiki/Digital_signature). This signature scheme is quantum robust, meaning that signatures are resistant to attacks from quantum computers. But, the scheme also reveals an unknown amount of the private key. As a result, it's safe to withdraw from an address only once, after which it is a [spent address](../clients/addresses.md#spent-addresses). -->

### 署名の生成方法
<!-- ### How signatures are generated -->

バンドルに一度だけ署名することが常に安全であることを保証するために，最初に[バンドル](../transactions/bundles.md)ハッシュが[正規化](https://en.wikipedia.org/wiki/Canonical_form#Computing)され，秘密鍵の半分のみが署名で公開されるようにします．
<!-- To make sure that it's always safe to sign a bundle once, first the [bundle](../transactions/bundles.md) hash is [normalized](https://en.wikipedia.org/wiki/Canonical_form#Computing) so that only half of the private key is revealed in the signature. -->

[秘密鍵が持つキーフラグメントの数](../clients/addresses.md#how-addresses-are-generated)に応じて，正規化されたバンドルハッシュの前方から27トライト，54トライト，または81トライトが選択されます．
<!-- Depending on the [number of key fragments that a private key has](../clients/addresses.md#how-addresses-are-generated), 27, 54, or 81 trytes of the normalized bundle hash are selected. -->

正規化されたバンドルハッシュの選択されたトライトは，[10進数の値に変換されます](../introduction/ternary.md#tryte-encoding)．次に，それぞれに対して次の計算が実行されます．
<!-- The selected trytes of the normalized bundle hash are [converted to their decimal values](../introduction/ternary.md#tryte-encoding). Then, the following calculation is performed on each of them: -->

```bash
13 - 10進数の値
```

この計算の結果は，[Kerl](https://github.com/iotaledger/kerl) [ハッシュ関数](https://en.wikipedia.org/wiki/Hash_function)を使用して，秘密鍵内の27個のセグメントのそれぞれがハッシュされる回数です．
<!-- The result of this calculation is the number of times that each of the 27 segments in the private key are hashed, using the [Kerl](https://github.com/iotaledger/kerl) [hash function](https://en.wikipedia.org/wiki/Hash_function). -->

27個のセグメントの各ハッシュは，合わさって2,187（81*27）トライトからなる署名フラグメントとなります．
<!-- Each hash of 27 segments is a signature fragment, which contains 2,187 trytes. -->

トランザクションの `signatureMessageFragment` フィールドに含めることができるのは2,187トライのみであるため，セキュリティレベルが1を超えるアドレスは，1つのトランザクションに収まるには大きすぎる署名になります．その結果，残りの署名は，同じバンドル内のゼロトークントランザクション間で断片化されます．
<!-- Because a transaction's `signatureMessageFragment` field can contain only 2,187 trytes, any address with a security level greater than 1 results in a signature that's too large to fit in one transaction. As a result, the rest of the signature is fragmented across zero-value transactions in the same bundle. -->

## 署名の検証方法
<!-- ## How nodes validate signatures -->

ノードは，署名とバンドルハッシュを使用してトランザクションのアドレスを計算することにより，トランザクションの署名を検証します．
<!-- Nodes validate a signature in a transaction by using the signature and the bundle hash to find the address of the transaction. -->

署名を検証するために，ノードはバンドルハッシュを正規化します．次に，署名の長さに応じて，ノードは正規化されたバンドルハッシュの27トライト，54トライト，または81トライトを選択します．これらのトライトは，署名フラグメント内のセグメントの数に対応しています．正規化されたバンドルハッシュの選択されたトライトは，[10進数の値に変換されます](../introduction/ternary.md#tryte-encoding)．次に，それぞれに対して次の計算が実行されます．
<!-- To validate a signature, nodes normalize the bundle hash. Then, depending on the length of the signature, the node selects 27, 54, or 81 trytes of the normalized bundle hash. These trytes correspond to the number of segments in a signature fragment. The selected trytes of the normalized bundle hash are [converted to their decimal values](../introduction/ternary.md#tryte-encoding). Then, the following calculation is performed on each of them: -->

```bash
13 + 10進数の値
```

この計算の結果は，署名フラグメントの27個のセグメントのそれぞれをハッシュして，キーフラグメントを導出する必要がある回数です．
<!-- The result of this calculation is the number of times that each of the 27 segments in the signature fragments must be hashed to derive the key fragments. -->

各キーフラグメントは1回ハッシュされてキーダイジェストが得られます．キーダイジェストは結合され，1回ハッシュされて81トライトのアドレスが導出されます．
<!-- Each key fragment is hashed once to derive the key digests, which are combined and hashed once to derive an 81-tryte address. -->

この導出されたアドレスがトランザクション内のアドレスと一致する場合，署名は有効であり，トランザクションは有効と見なされます．
<!-- If the address matches the one in the transaction, the signature is valid and the transaction is considered valid. -->
