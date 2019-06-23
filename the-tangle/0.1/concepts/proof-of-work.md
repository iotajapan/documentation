# プルーフオブワーク
<!-- # Proof of work -->

**プルーフオブワークは、計算が難しいが検証が簡単な数学的問題に対する答えです。 IOTAでは、プルーフオブワークはスパムトランザクションからネットワークを保護します。バンドル内の各トランザクションには、有効であることを証明するプルーフオブワークが含まれている必要があります。**
<!-- **Proof of work is the answer to a mathematical problem that's difficult to calculate, but easy to verify. In IOTA, proof of work protects the network from spam transactions. Each transaction in a bundle must include a proof of work to be valid.** -->

プルーフオブワーク（PoW）は試行錯誤を用いて計算され、それ故、PoWは計算能力の使用を必要とする。
<!-- Proof of work (PoW) is calculated using trial and error, therefore it requires the use of computational power. -->

もともと、PoWは大量の電子メールスパムを減らすための概念として導入されました。この概念は[hashcash](https://en.wikipedia.org/wiki/Hashcash)として知られており、すべてのEメールの内容についてプルーフオブワークを要求することによってEメールのスパムを防ぐ方法です。
<!-- Originally, PoW was introduced as a concept to reduce large amounts of email spam. This concept is known as [hashcash](https://en.wikipedia.org/wiki/Hashcash), and is a method of preventing email spam by requiring a proof of work for the contents of every email. -->

## IOTAでのプルーフオブワーク
<!-- ## Proof of work in IOTA -->

hashcashと同様に、各トランザクションはそれが検証されることができる前にPoWを含まなければなりません。このPoWは、有効なトランザクションを作成するのにかかる時間および計算能力を増大させることによって、IOTAネットワークにスパム防止を提供します。さらに、スパムトランザクションがネットワークに及ぼす影響を減らすために、ノードは有効なPoWを含まないトランザクションを無視します。
<!-- Similar to hashcash, each transaction must include a PoW before it can be validated. This PoW provides spam protection for an IOTA network by increasing the time and computational power it takes to create a valid transaction. Furthermore, to reduce the effect that spam transactions have on the network, nodes ignore transactions that don't contain a valid PoW. -->

PoWは、クライアントによって行われてもよく、または[`attachToTangle`](root://iri/0.1/references/api-reference.md#attachToTangle)エンドポイントを呼び出すことによってノードに委託（リモートプルーフオブワーク）することもできます。
<!-- PoW can be done by clients or it can be outsourced to a node (known as remote proof of work) by calling the [`attachToTangle` endpoint](root://iri/0.1/references/api-reference.md#attachToTangle). -->

トランザクションを作成するのに使用しているデバイスが合理的な時間内にPoWを完了する計算能力を有していない場合、クライアントはリモートプルーフオブワークを使用したいと思うかもしれない。
<!-- Clients may want to use remote PoW if the device they're using to create transactions doesn't have the necessary computational power to calculate PoW in a reasonable amount of time. -->

## プルーフオブワークの計算方法
<!-- ## How proof of work is calculated -->

トランザクションのPoWを計算するために、以下の[トランザクションの内容](root://iota-basics/0.1/references/structure-of-a-transaction.md)がトライトからトリットに変換され、次にそれらのトリットがハッシュ化されてトランザクションハッシュとなります。
<!-- To calculate the PoW for a transaction, the following [contents of the transaction](root://iota-basics/0.1/references/structure-of-a-transaction.md) are converted from trytes to trits, then those trits are hashed to result in a transaction hash: -->

* **バンドルハッシュ：** バンドル内のすべてのトランザクションの`address`、`obsoleteTag`、`timestamp`、`value`、`currentIndex`、および`lastindex`の各フィールドを使用して計算されるハッシュ値。これらのフィールドは**バンドルエッセンス**と呼ばれます。
<!-- * **Bundle hash:** Hash that's calculated using the `address`, `obsoleteTag`, `timestamp`, `value`, `currentIndex`, and `lastindex` fields of all transactions in a bundle. These fields are called the **bundle essence**. -->
* **署名：** トランザクションの署名（アドレスからIOTAトークンを取り出す場合）。
<!-- * **Signature:** Signature of the transaction (if it withdraws IOTA tokens from an address) -->
* **トランクトランザクションとブランチトランザクション：** トランザクションが参照および承認する2つのトランザクション。
<!-- * **Trunk transaction and branch transaction:** Two transactions that the transaction references and approves -->

トランザクションハッシュが正しい数の0トリット（[最小重量値](root://iota-basics/0.1/concepts/minimum-weight-magnitude.md)）で終了する場合、トランザクションは有効と見なされます。
<!-- If the transaction hash ends in the correct number of 0 trits ([minimum weight magnitude](root://iota-basics/0.1/concepts/minimum-weight-magnitude.md)), it's considered valid. -->

:::info:
[3つの0のトリット（000トリット）がトライトの9としてエンコードされます](root://iota-basics/0.1/references/tryte-alphabet.md)。
:::
<!-- :::info: -->
<!-- [Three 0 trits are encoded to a 9 in trytes](root://iota-basics/0.1/references/tryte-alphabet.md). -->
<!-- ::: -->

トランザクションハッシュが正しい数の0トリットで終わっていない場合、トランザクションの`nonce`フィールドの値がインクリメントされ、トランザクションハッシュが再度ハッシュ化されます。
<!-- If the transaction hash doesn't end in the correct number of 0 trits, the value of the transaction's `nonce` field is incremented and the transaction hash is hashed again. -->

このプロセスは、正しい数の0トリットで終わるトランザクションハッシュが見つかるまで続きます。
<!-- This process continues until a transaction hash is found that ends in the correct number of 0 trits. -->

トランザクションの`nonce`フィールドには、IRIノードがPoWを検証するために使用する27トライトの文字列が含まれています。次に例を示します。
<!-- The `nonce` field of a transaction contains a string of 27 trytes that IRI nodes use to validate the PoW, for example: -->

```javascript
{
    ...
    nonce: "POWSRVIO9GW99999FMGEGVMMMMM"
    ...
}

```

トランザクションの内容はハッシュ化されているため、トランザクションの内容のいずれかが変更されると、トランザクションハッシュが変更され、PoWが無効になります。
<!-- Because the the contents of the transaction are hashed, if any of the contents change, the transaction hash will change and make the proof of work invalid. -->

:::info:
PoWを計算する関数は[PearlDiver](https://github.com/iotaledger/iri/blob/fcf2d105851ee891b093e2857592fa05258ec5be/src/main/java/com/iota/iri/crypto/PearlDiver.java)と呼ばれます。
:::
<!-- :::info: -->
<!-- The function that calculates PoW is called the [PearlDiver](https://github.com/iotaledger/iri/blob/fcf2d105851ee891b093e2857592fa05258ec5be/src/main/java/com/iota/iri/crypto/PearlDiver.java). -->
<!-- ::: -->
