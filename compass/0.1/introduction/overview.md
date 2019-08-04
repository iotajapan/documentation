# プライベートタングル概要
<!-- # Private Tangle overview -->

**プライベートタングルは運用者が管理するIOTAネットワークで、運用者が知っているノードだけを含みます。プライベートタングルは、パブリックIOTAネットワークと同じテクノロジを使用しますが、コンパスと呼ばれるコーディネータのオープンソース実装を実行してプライベートタングルを制御します。コンパスを使用すると、プライベートタングルに添付されているトランザクションについてノードが合意に達することができます。コンパスが停止した場合、IOTAネットワーク内のトランザクションは、再起動するまで確定されません。**
<!-- **A private Tangle is an IOTA network that you control and that contains only nodes that you know. A private Tangle uses the same technology as the public IOTA networks, except you control it by running an open-source implementation of the Coordinator called Compass. You can use Compass to allow nodes to reach a consensus on transactions attached to your private Tangle. If Compass stops, no transactions in your IOTA network will be confirmed until it starts again.** -->

## プライベートタングルを設定する理由
<!-- ## Reasons to set up a private Tangle -->

次のような理由でプライベートタングルを設定したいと思うかもしれません。
<!-- You may want to set up a private Tangle for the following reasons: -->

**テクノロジーを探求する：** IOTAの使用経験がほとんどないかまったくない場合は、IOTAのテクノロジーと利点を理解するのに役立つプライベートタングルを設定できます。
<!-- **Explore the technology:** If you have little or no experience with IOTA, you can set up your own private Tangle to help you understand the technology and how you can benefit from it. -->

**より速いIOTAネットワークを設定する：** 開発するユースケースが現在のパブリックIOTAネットワークが提供しているネットワーク速度よりも速いネットワーク速度を必要とする場合、独自のタングルを設定して毎秒のトランザクション数を増やすことができます。たとえば、[最小重量値](root://dev-essentials/0.1/concepts/minimum-weight-magnitude.md)の値を小さくして、プルーフオブワークをより速くすることができます。
<!-- **Set up a faster IOTA network:** If your use cases need a faster network speed than the public IOTA networks can currently provide, you can set up your own private Tangle to increase the number of transactions per second. For example, you could lower the value of the [minimum weight magnitude](root://dev-essentials/0.1/concepts/minimum-weight-magnitude.md) to make proof of work quicker. -->

**アプリケーションの開発とテストを行う：** IOTA上でアプリケーションを開発またはテストしたい場合は、トランザクションが一般に公開されないように、プライベートタングル上で実行することができます。プライベートタングルを使用すると、ネットワークをリセットして同じ時点からすべてのテストを開始できるという利点もあります。
<!-- **Develop and test an application:** If you want to develop or test an application on IOTA, you may want to do so on a private Tangle so that your transactions aren't visible to the public. Having a private Tangle also has the added benefit of being able to reset the network to start all tests from the same point. -->

**テクノロジーを紹介する：** 自分のアイデアを他の人に見せたい場合は、プライベートタングルを設定して、プレゼンテーション中にすべてがスムーズに実行されるようにすることができます。
<!-- **Showcase the technology:** If you want to show your ideas to others, you may want to set up a private Tangle so you can make sure that everything runs smoothly during your presentation. -->

## プライベートタングルの仕組み
<!-- ## How a private Tangle works -->

IOTAは、トランザクションを検証して、そのトランザクションを保存するノードの分散ネットワークです。ノードがアドレスの残高を更新する前に、更新された残高につながるすべてのトランザクションについて合意に達する必要があります。ノードがトランザクションに関して合意に達すると、それらのトランザクションは確定されます。
<!-- IOTA is a distributed network of nodes that validate transactions and store them. Before nodes can update the balance of an address, they must reach a consensus on any transactions that lead to the updated balance. When nodes reach a consensus, on transactions, those transactions are confirmed. -->

IOTA Mainnet上で、[コーディネーター](root://dev-essentials/0.1/concepts/the-tangle.md#the-coordinator)はマイルストーンを含むバンドルを作成、署名、および送信します。Mainnet上のノードは、合意に達するためにマイルストーンを使用します。マイルストーンによって参照され承認されたトランザクションはすべて確定されます。
<!-- On the IOTA Mainnet, the [Coordinator](root://dev-essentials/0.1/concepts/the-tangle.md#the-coordinator) creates, signs, and sends bundles that contain milestones. The nodes on this network use these milestone to reach a consensus. Any transaction that's referenced and approved by a milestone is confirmed. -->

コンパスはコーディネーターのオープンソース実装です。コンパスを使用すると、自分のIOTAネットワーク内のノードが、コーディネーターのマイルストーンではなくコンパスのマイルストーンを使って合意に達することができます。
<!-- Compass is an open-source implementation of the Coordinator. You can use Compass to allow the nodes in your own IOTA network to reach a consensus on Compass' milestones instead of the Coordinator's ones. -->

ノードが合意に達することを許可するには、コンパスマイルストーンを認識するようにノードを設定する必要があります。その後、定期的に1つのノードにマイルストーンを送信するようにコンパスを設定します。
<!-- To allow your nodes to reach a consensus, you must configure your nodes to recognize Compass milestones. Then, you can configure Compass to send milestones to one of your nodes at regular intervals. -->

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

## 次のステップ
<!-- ## Next steps -->

[プライベートタングルを設定します](../how-to-guides/set-up-a-private-tangle.md)。
<!-- [Set up a private Tangle](../how-to-guides/set-up-a-private-tangle.md). -->
