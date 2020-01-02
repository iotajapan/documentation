# 最小重量値(MWM)
<!-- # Minimum weight magnitude -->

** [トランザクション](../transactions/transactions.md)に対して[プルーフオブワーク](../transactions/proof-of-work.md)を行う場合、最小重量値（MWM）を指定できます。これは、実行される作業量を定義します。ノードは、構成に応じて、特定の最小重量値だけを受け入れます。最小重量値が低すぎると、ノードはトランザクションを無効として拒否します。**
<!-- **When doing [proof of work](../transactions/proof-of-work.md) for a [transaction](../transactions/transactions.md), you can specify a minimum weight magnitude (MWM), which defines how much work is done. Nodes accept a certain minimum weight magnitude, depending on their configuration. If you use a minimum weight magnitude that is too low, nodes will reject the transaction as invalid.** -->

## 最小重量値を選択するためのアドバイス
<!-- ## Advice for choosing a minimum weight magnitude -->

最小重量値を選択するときは、次の質問を考慮する必要があります。
<!-- When choosing a minimum weight magnitude, you should consider the following questions. -->

### どの IOTA ネットワークを使用するか
<!-- ### Which IOTA network are you using? -->

すべてのノードは、定義済みの MWM と同じかそれ以上の0トリットでハッシュが終了するトランザクションのみを受け入れます。トランザクションが MWM よりも少ない0トリットで終了する場合、そのネットワークのノードはそのトランザクションを拒否します。
<!-- All nodes accept transactions whose hashes end in the same or higher number of 0 trits as their predefined MWM. If a transaction ends in fewer 0 trits than the MWM, the nodes in that network will reject it. -->

たとえば、メインネットでは、少なくとも14の MWM を使用する必要があります。ハッシュが9個（デブネットの MWM）の0トリットで終了するトランザクションの場合、メインネットのノードはそのトランザクションを受け入れません。メインネットのノードは、トランザクションハッシュが少なくとも14個の0トリットで終了することを要求します。
<!-- For example, on the Mainnet, you must use at least a MWM of 14. If you were to send a transaction whose hash ended in nine (the MWM on the Devnet) 0 trits, nodes on the Mainnet would not accept it. Those nodes would expect the transaction hash to end in at least fourteen 0 trits. -->

### IOTA をテストする場合
<!-- ### Are you testing IOTA? -->

MWM の増分ごとに、PoW の難易度が3倍になります。その結果、IOTA をテストしたい場合、プルーフオブワークがより迅速かつ簡単に行えるデブネットを使用することができます。
<!-- Every increment of the MWM triples the difficulty of the PoW. As a result, if you're testing IOTA, you may want to use the Devnet where proof of work is quicker and easier to do. -->

