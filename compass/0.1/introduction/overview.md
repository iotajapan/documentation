# コンパス概要
<!-- # Compass overview -->

**コンパスはコーディネーターのオープンソース実装です。テストおよび開発の目的で、コンパスを使用して独自のIOTAネットワークを運用することができます。**
<!-- **Compass is an open-source implementation of the Coordinator. You can use Compass to run your own IOTA network for testing and development purposes.** -->

IOTAは、トランザクションを検証して、そのトランザクションを格納するノードの分散ネットワークです。ノードがアドレスの残高を更新する前に、更新された残高につながるすべてのトランザクションについて合意に達する必要があります。ノードがトランザクションに関して合意に達すると、それらのトランザクションは確定されます。
<!-- IOTA is a distributed network of nodes that validate transactions and store them. Before nodes can update the balance of an address, they must reach a consensus on any transactions that lead to the updated balance. When nodes reach a consensus, on transactions, those transactions are confirmed. -->

IOTA Mainnet上で、[コーディネーター](root://the-tangle/0.1/concepts/the-coordinator.md)はマイルストーンを含むバンドルを作成、署名、および送信します。Mainnet上のノードは、合意に達するためにマイルストーンを使用します。マイルストーンによって参照され承認されたトランザクションはすべて確定されます。
<!-- On the IOTA Mainnet, the [Coordinator](root://the-tangle/0.1/concepts/the-coordinator.md) creates, signs, and sends bundles that contain milestones. The nodes on this network use these milestone to reach a consensus. Any transaction that's referenced and approved by a milestone is confirmed. -->

コンパスはコーディネーターのオープンソース実装です。コンパスを使用すると、自分のIOTAネットワーク内のノードが、コーディネーターのマイルストーンではなくコンパスのマイルストーンを使って合意に達することができます。
<!-- Compass is an open-source implementation of the Coordinator. You can use Compass to allow the nodes in your own IOTA network to reach a consensus on Compass' milestones instead of the Coordinator's ones. -->

ノードが合意に達することを許可するには、コンパスマイルストーンを認識するようにノードを設定する必要があります。その後、定期的に1つのノードにマイルストーンを送信するようにコンパスを設定できます。
<!-- To allow your nodes to reach a consensus, you must configure your nodes to recognize Compass milestones. Then, you can configure Compass to send milestones to one of your nodes at regular intervals. -->

:::info:
[自分のIOTAネットワークを作成する](../how-to-guides/set-up-a-private-tangle.md).
:::
<!-- :::info: -->
<!-- [Set up your own IOTA network](../how-to-guides/set-up-a-private-tangle.md). -->
<!-- ::: -->

## リポジトリ
<!-- ## Repository -->

[GithubのCompassソースコード](https://github.com/iotaledger/compass)に行く。
<!-- Go to the Compass source code on [Github](https://github.com/iotaledger/compass) -->

## 参考文献
<!-- ## Further reading -->

- [タングルおよびその他のプロトコル機能について議論しているIOTA論文](https://www.iota.org/research/academic-papers)
<!-- - [IOTA papers discussing the Tangle and other protocol features](https://www.iota.org/research/academic-papers) -->
- [コーディネーターの削除について議論する一連の投稿](https://blog.iota.org/coordinator-part-1-the-path-to-coordicide-ee4148a8db08)
<!-- - [A series of posts discussing the removal of the Coordinator](https://blog.iota.org/coordinator-part-1-the-path-to-coordicide-ee4148a8db08) -->
