# 最小重量値
<!-- # Minimum weight magnitude -->

**各トランザクションには有効なプルーフオブワークが含まれている必要があります。これはスパム防止の手法です。最小重量値（MWM）は、プルーフオブワーク中に行われる作業量を定義する変数です。クライアントとしてIOTAネットワークと対話するときは、そのネットワークに適したMWMを使用する必要があります。そうでなければ、トランザクションは有効にならず、ノードはトランザクションを拒否します。**
<!-- **Each transaction must include a valid proof of work, which is a spam prevention technique. The minimum weight magnitude (MWM) is a variable that defines how much work is done during proof of work. When you interact with an IOTA network as a client, you must use the correct MWM for that network. Otherwise, your transaction won't be valid and the nodes will reject it.** -->

## プルーフオブワークについて
<!-- ## About proof of work -->

プルーフオブワーク（PoW）は、特定の要件を満たすために試行錯誤を使用して計算されるデータの一部です。PoWの重要なことは、製造が困難であるが検証が容易であるということです。
<!-- A proof of work (PoW) is a piece of data that is calculated using trial and error to meet certain requirements. The key to PoW is that it's difficult to produce, but easy to verify. -->

大量の電子メールスパムを減らすための概念として、PoWが導入されました。この概念は[ハッシュキャッシュ](https://en.wikipedia.org/wiki/Hashcash)として知られており、ハッシュキャッシュはすべてのEメールの内容についてプルーフオブワークを要求することによってEメールのスパムを防ぎます。
<!-- PoW was introduced as a concept to reduce large amounts of email spam. This concept is known as [hashcash](https://en.wikipedia.org/wiki/Hashcash), and is a method of preventing email spam by requiring a proof of work for the contents of every email. -->

１つの電子メールについてPoWを計算することは容易ですが、大量のスパム電子メールのPoWを計算するには時間がかかり、高い計算能力が必要です。
<!-- PoW is easy to calcluate for one email, but costs a lot in time and computational power to calculate for mass spam emails. -->

### IOTAにおけるプルーフオブワーク
<!-- ### Proof of work in IOTA -->

ハッシュキャッシュと同様に、各トランザクションは検証される前にPoWを含まなければなりません。このPoWは、大量のスパムトランザクションを作成するのにかかる時間と計算能力を増大させることによって、IOTAネットワークにスパム保護を提供します。
<!-- Similar to hashcash, each transaction must include a PoW before it can be validated. This PoW provides spam protection for an IOTA network by increasing the time and computational power it takes to create mass spam transactions. -->

トランザクションのPoWを計算するには、以下の[トランザクションの内容](root://iota-basics/0.1/references/structure-of-a-transaction.md)をトライトからトリットに変換し、次にそれらのトリットをハッシュ化してトランザクションハッシュを生成します。
<!-- To calculate the PoW for a transaction, the following [contents of the transaction](root://iota-basics/0.1/references/structure-of-a-transaction.md) are converted from trytes to trits, then those trits are hashed to result in a transaction hash: -->

* **バンドルハッシュ：** ハッシュはバンドル内のすべてのトランザクションの`address`、`obsoleteTag`、`timestamp`、`value`、`currentIndex`、そして`lastindex`フィールドを使って計算されます。これらのフィールドは**バンドルエッセンス**と呼ばれます。
<!-- * **Bundle hash:** Hash that's calculated using the `address`, `obsoleteTag`, `timestamp`, `value`, `currentIndex`, and `lastindex` fields of all transactions in a bundle. These fields are called the **bundle essence**. -->
* **署名：** トランザクションの署名（アドレスからIOTAトークンを取り出す場合）
<!-- * **Signature:** Signature of the transaction (if it withdraws IOTA tokens from an address) -->
* **トランクトランザクションとブランチトランザクション：** トランザクションが参照および承認する2つのトランザクション
<!-- * **Trunk transaction and branch transaction:** Two transactions that the transaction references and approves -->

### 最小重量値
<!-- ### Minimum weight magnitude -->

最小重量値（MWM）は、プルーフオブワークの要件を定義します。
<!-- Minimum weight magnitude (MWM) defines the requirements for proof of work. -->

プルーフオブワークの間、トランザクションハッシュは、MWMと同じ数の0トリットで終了するまで繰り返しハッシュ化されます。MWMが高いほど、プルーフオブワークは難しくなります。
<!-- During proof of work, the transaction hash is repeatedly hashed until it ends in the same number of 0 trits as the MWM. The higher the MWM, the harder the proof of work. -->

:::info:
[000トリットはトライトでは9としてエンコードされています](root://iota-basics/0.1/references/tryte-alphabet.md)。
:::
<!-- :::info: -->
<!-- [Three 0 trits are encoded as a 9 in trytes](root://iota-basics/0.1/references/tryte-alphabet.md). -->
<!-- ::: -->

[IOTAネットワーク](root://getting-started/0.1/references/iota-networks.md)内のすべてのノードは、ハッシュ値が事前に定義されたMWMと同じかそれ以上の数の0トリットで終わるトランザクションを受け入れます。トランザクション末尾の0の個数がMWMよりも少ない場合、ネットワーク内のノードはトランザクションを拒否します。
<!-- All nodes in an [IOTA network](root://getting-started/0.1/references/iota-networks.md) accept transactions whose hashes end in the same or higher number of 0 trits as their predefined MWM. If a transaction ends in fewer 0 trits than the MWM, the nodes in that network will reject it. -->

たとえば、Mainnetでは、少なくとも14のMWMを使用する必要があります。ハッシュ値が9個（DevnetのMWM）または7個（SpamnetのMWM）の0トリットで終わるトランザクションをMainnet上のノードに送信するしても受け入れられません。
<!-- For example, on the Mainnet, you must use at least a MWM of 14. If you were to send a transaction whose hash ends in 9 (the MWM on the Devnet) or 7 (the MWM on the Spamnet) 0 trits, no nodes on the Mainnet would accept it. -->

:::info:
MWMが1増えるごとに、PoWの難しさは3倍になります。
:::
<!-- :::info: -->
<!-- Every increment of the MWM triples the difficulty of the PoW. -->
<!-- ::: -->

トランザクションハッシュが正しい数の0トリット（[最小重量値](root://iota-basics/0.1/concepts/minimum-weight-magnitude.md)）で終了する場合、そのトランザクションは有効と見なされます。
<!-- If the transaction hash ends in the correct number of 0 trits ([minimum weight magnitude](root://iota-basics/0.1/concepts/minimum-weight-magnitude.md)), it's considered valid. -->

### `nance`フィールド
<!-- ### The `nonce` field -->

トランザクションハッシュが正しい数の0トリットで終わっていない場合、トランザクションの`nonce`フィールドの値がインクリメントされ、トランザクションハッシュは再びハッシュ化されます。
<!-- If the transaction hash doesn't end in the correct number of 0 trits, the value of the transaction's `nonce` field is incremented and the transaction hash is hashed again. -->

このプロセスは、MWMの正しい数の0トリットで終了するトランザクションハッシュが見つかるまで続きます。
<!-- This process continues until a transaction hash is found that ends in the correct number of 0 trits for the MWM. -->

トランザクションの`nonce`フィールドには、IRIノードがPoWを検証するために使用する27トライトの文字列が含まれています。以下に例を示します。
<!-- The `nonce` field of a transaction contains a string of 27 trytes that IRI nodes use to validate the PoW, for example: -->

```javascript
{
...
nonce: "POWSRVIO9GW99999FMGEGVMMMMM"
...
}

```

トランザクションの内容はハッシュ化されているため、内容のいずれかが変更されると、トランザクションのハッシュ値が変更され、プルーフオブワークが無効になります。
<!-- Because the the contents of the transaction are hashed, if any of the contents change, the transaction hash will change and make the proof of work invalid. -->

:::info:
PoWを計算する関数は[PearlDiver](https://github.com/iotaledger/iri/blob/fcf2d105851ee891b093e2857592fa05258ec5be/src/main/java/com/iota/iri/crypto/PearlDiver.java)と呼ばれます。
:::
<!-- :::info: -->
<!-- The function that calculates PoW is called the [PearlDiver](https://github.com/iotaledger/iri/blob/fcf2d105851ee891b093e2857592fa05258ec5be/src/main/java/com/iota/iri/crypto/PearlDiver.java). -->
<!-- ::: -->

### プルーフオブワークを行うためのオプション
<!-- ### Options for doing proof of work -->

クライアントには、PoWを実行するための以下のオプションがあります。
<!-- Clients have the following options for doing PoW: -->

| **オプション** | **メリット** | **デメリット** |
| :------------- | :----------- | :------------- |
| ノード上のリモートPoWを選択する | リモートPoWは、クライアントがノードに対してトランザクションのためのPoWをするように要求するときです。クライアントはリモートPoWを行うために[`attachToTangle`エンドポイント](root://node-software/0.1/iri/references/api-reference.md#attachToTangle)を呼び出します。このようにして、クライアントは、PoWを行うために必要とされる計算能力の使用を回避することができます。 | ノードがどれくらい強力であるか、そしてノードが受信するリクエストの数に応じて、ノードはタイムアウトし、PoWを完了しないかもしれません。 |
| ローカルPoWを行う | ローカルPoWは、クライアントがタングルに添付したい各トランザクションに対してクライアントがPoWを行うときです。このようにして、クライアントは、信頼性の低いノードに依存してPoWを実行することはありません。 | クライアントデバイスは、満足のいく時間内にPoWを完了するのに十分強力でなければなりません。 |
| 有料サービスにPoWをアウトソーシングする | 有料のPoWサービスは、IOTAトランザクションを受け入れ、PoWを完了し、PoWの結果をクライアントに返すサードパーティのサーバです。専用のサードパーティサーバによって、PoWはより速くより確実に行われます。そのようなサービスの例は[powsrv.io](https://powsrv.io/#quickstart)です。 | これはサービスを利用するのにお金がかかり、利用者はこれらを管理できません。 |
| [PoWプロキシサーバをインストールする](root://node-software/0.1/iri/how-to-guides/install-a-pow-proxy.md) | あなた自身の専用サーバによってPoWはより速くより確実に行えます。 | PoWプロキシサーバを維持する必要があります。 |

<!-- |**Option**|**Advantages**|**Disadvantages**| -->
<!-- |:-------|:---------|:------------| -->
<!-- |Choose remote PoW on a node|Remote PoW is when clients asks a node to do PoW for a transaction. Clients do this by calling the [`attachToTangle` endpoint](root://node-software/0.1/iri/references/api-reference.md#attachToTangle). This way, clients can avoid using the computational power needed to do PoW.|Depending on how powerful the node is and how many requests it receives, it may time out and not complete the PoW | -->
<!-- |Do local PoW|Local PoW is when clients do PoW for each transaction that they want to attach to the Tangle. This way, clients aren't reliant on unreliable nodes to do PoW.|The client device may not be powerful enough to complete PoW in a satisfactory amount of time| -->
<!-- |Outsource PoW to a paid service|A paid PoW service is a third-party server that accepts IOTA transactions, completes PoW and returns it to the client. PoW is done faster more more reliably by a dedicated third-party server. An example of such a service is [powsrv.io](https://powsrv.io/#quickstart)|It costs money to use the service and you don't have control over it| -->
<!-- |[Install a PoW proxy server](root://node-software/0.1/iri/how-to-guides/install-a-pow-proxy.md)|PoW is done faster more more reliably by your own dedicated server|You need to maintain the PoW proxy server| -->
