# プルーフオブワーク
<!-- # Proof of work -->

**プルーフオブワーク（PoW）は，特定の要件を満たすために試行錯誤を使用して計算されるデータです．[hashcash](https://en.wikipedia.org/wiki/Hashcash) などのスパム防止対策として，各[トランザクション](../transactions/transactions.md)が有効であるためにプルーフオブワークを含める必要があります．このプルーフオブワークは実行が困難ですが，[ノード](../network/nodes.md)が検証するのは簡単です．**
<!-- **A proof of work (PoW) is a piece of data that is calculated using trial and error to meet certain requirements. As a spam prevention measure such as [hashcash](https://en.wikipedia.org/wiki/Hashcash), each [transaction](../transactions/transactions.md) must include a proof of work to be valid. This proof of work is difficult to do, but easy for [nodes](../network/nodes.md) to validate.** -->

## プルーフオブワークの計算方法
<!-- ## How proof of work is calculated -->

[トランザクション](../transactions/transactions.md)の PoW を計算するには，[Curl](https://github.com/iotaledger?utf8=%E2%9C%93&q=curl&type=&language=) [ハッシュ関数](https://en.wikipedia.org/wiki/Hash_function)を使用して，すべてのトランザクションフィールドの値を[トライト](../introduction/ternary.md)に変換し，ハッシュします．
<!-- To calculate the PoW for a [transaction](../transactions/transactions.md), the values of all the transaction fields are converted to [trits](../introduction/ternary.md) and hashed, using the [Curl](https://github.com/iotaledger?utf8=%E2%9C%93&q=curl&type=&language=) [hash function](https://en.wikipedia.org/wiki/Hash_function). -->

このプロセスは，トランザクションハッシュが（[最小重量値](root://getting-started/0.1/transactions/proof-of-work.md#minimum-weight-magnitude)）と同じ数の0トリットで終了するまで続きます．
<!-- This process continues until the transaction hash ends in the same number of 0 trits as the ([minimum weight magnitude](root://getting-started/0.1/transactions/proof-of-work.md#minimum-weight-magnitude)). -->

トランザクションハッシュが正しい0トリット数で終了しない場合は常に，トランザクションの `nonce` フィールドの値がインクリメントされ，トランザクションハッシュが再度ハッシュされます．
<!-- Whenever the transaction hash doesn't end in the correct number of 0 trits, the value of the transaction's `nonce` field is incremented and the transaction hash is hashed again. -->

## プルーフオブワークを行うためのオプション
<!-- ## Options for doing proof of work -->

PoW を実行するには，次のオプションがあります．
<!-- You have the following options for doing PoW. -->

|**オプション**|**利点**|**欠点**|
|:-------|:---------|:------------|
|ノードでリモート PoW を選択する|リモート PoW は，ノードにトランザクションの PoW を行うように要求する場合です．リモート PoW を行うには，[`attachToTangle` エンドポイント]を呼び出します．これにより，ローカルでの PoW の実行に必要な計算能力の使用を回避できます．|ノードの能力と受信するリクエストの数に応じて，タイムアウトし，PoW を完了できない場合があります．|
|ローカルで PoW を実行する|ローカル PoW は，タングルにアタッチするトランザクションごとに PoW を行うときです．この方法では，PoW を行うために信頼できないノードに依存しません．|お使いのデバイスは，十分な時間で PoW を完了するのに十分なほど強力ではない場合があります．|
|PoW を有料サービスに外注する|有料 PoW サービスは，IOTA トランザクションを受け入れ，PoW を完了し，それをお客様に返すサードパーティのサーバーです．PoW は，専用のサードパーティサーバーにより，より高速に，より確実に実行されます．そのようなサービスの例は [powsrv.io](https://powsrv.io/#quickstart) です．|このサービスを使用するには費用がかかり，使用者はサービスを制御することはできません．|
|[PoW プロキシサーバーをインストールする](root://utils/0.1/official/proof-of-work-proxy/overview.md)|PoW は，自分自身の専用サーバーによってより速く，より確実に実行されます．|PoW プロキシサーバーを維持する必要があります．|

<!-- |**Option**|**Advantages**|**Disadvantages**| -->
<!-- |:-------|:---------|:------------| -->
<!-- |Choose remote PoW on a node|Remote PoW is when you ask a node to do PoW for a transaction. You do this by calling the [`attachToTangle` endpoint](root://node-software/0.1/iri/references/api-reference.md#attachToTangle). This way, you can avoid using the computational power needed to do PoW.|Depending on how powerful the node is and how many requests it receives, it may time out and not complete the PoW | -->
<!-- |Do local PoW|Local PoW is when you do PoW for each transaction that you want to attach to the Tangle. This way, you aren't reliant on unreliable nodes to do PoW.|Your device may not be powerful enough to complete PoW in a satisfactory amount of time| -->
<!-- |Outsource PoW to a paid service|A paid PoW service is a third-party server that accepts IOTA transactions, completes PoW and returns it to you. PoW is done faster more more reliably by a dedicated third-party server. An example of such a service is [powsrv.io](https://powsrv.io/#quickstart)|It costs money to use the service and you don't have control over it| -->
<!-- |[Install a PoW proxy server](root://utils/0.1/official/proof-of-work-proxy/overview.md)|PoW is done faster and more reliably by your own dedicated server|You need to maintain the PoW proxy server| -->
