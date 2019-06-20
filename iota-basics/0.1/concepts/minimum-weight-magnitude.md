# 最小重量値
<!-- # Minimum weight magnitude -->

**最小重量値（MWM）は、プルーフオブワーク中に行われる作業量を定義する変数です。プルーフオブワークの間、トランザクションハッシュの末尾に、MWMと同じ数の0トリットが並ぶまで繰り返しハッシュ化を行います。 MWMが高いほど、プルーフオブワークは難しくなります。クライアントとしてIOTAネットワークと対話するときは、ネットワークに適したMWMを使用する必要があります。そうでなければ、トランザクションは有効にならず、ノードはトランザクションを拒否します。**
<!-- **The minimum weight magnitude (MWM) is a variable that defines how much work is done during proof of work. During proof of work, the transaction hash is repeatedly hashed until it ends in the same number of 0 trits as the MWM. The higher the MWM, the harder the proof of work. When you interact with an IOTA network as a client, you must use the correct MWM for that network. Otherwise, your transaction won't be valid and the nodes will reject it.** -->

[IOTAネットワーク](root://getting-started/0.1/references/iota-networks.md)内のすべてのノードは、ハッシュが事前定義されたMWMと同じかそれ以上の数の0トリットで終わるトランザクションを受け入れます。
<!-- All nodes in an [IOTA network](root://getting-started/0.1/references/iota-networks.md) accept transactions whose hashes end in the same or higher number of 0 trits as their predefined MWM. -->

トランザクション末尾の0トリットの個数が少ない場合、ネットワーク内のノードはそのトランザクションを拒否します。
<!-- If a transaction ends in fewer 0 trits, the nodes in that network will reject it. -->

たとえば、メインネットでは、MWMは14です。したがって、すべてのトランザクションハッシュ値は、14個の0トリットで終了する必要があります。
<!-- For example, on the Mainnet, the MWM is 14. So, all transaction hashes must end in that number of 0 trits. -->

ハッシュ値の末尾が9個（デブネット上のMWM）または7個（スパムネット上のMWM）の0トリットで終わるトランザクションを送信しようとした場合、メインネット上のノードはそのトランザクションを受け入れません。
<!-- If you were to send a transaction whose hash ends in 9 (the MWM on the Devnet) or 7 (the MWM on the Spamnet) 0 trits, no nodes on the Mainnet would accept it. -->

:::info:
MWMが1増えるごとに、PoWの難しさは3倍になります。
:::
<!-- :::info: -->
<!-- Every increment of the MWM triples the difficulty of the PoW. -->
<!-- ::: -->
