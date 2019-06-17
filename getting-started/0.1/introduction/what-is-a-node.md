# ノードとは？
<!-- # What is a node? -->

**ノードとは、偽造物がないことを確認するためにトランザクションを検証する責任があるコンピュータです。 IOTAネットワーク内のクライアントは、トランザクションを検証してタングルに添付できるように、バンドルをノードに送信する必要があります。**
<!-- **A node is a computer that's responsible for validating transactions to make sure that counterfeit ones are never confirmed. Clients in an IOTA network must send their bundles to nodes so that the transactions can be validated and attached to the Tangle.** -->

クライアントがノードにバンドルを送信すると、ノードはその中のトランザクションが有効であることを検証するためにいくつかの基本的なチェックを行います。たとえば、トランザクションの値がIOTAトークンの総供給量を超えていないことをチェックします。この時点で、トランザクションは[タングル](../introduction/what-is-the-tangle.md)と呼ばれるデータ構造に「添付された」と見なされます。
<!-- When a client sends a bundle to a node, the node does some basic checks to make sure that the transactions in it are valid, for example that the values of transactions don't exceed the total global supply. At this point, the transactions are considered 'attached' to a data structure called [the Tangle](../introduction/what-is-the-tangle.md). -->

トランザクションを検証した後、ノードはそのトランザクションを接続先のノードに送信し、ネットワーク全体で検証できるようにします。このようにして、すべてのノードがすべてのトランザクションを受け取り検証します。
<!-- After validating a transaction, a node sends it to neighbors so that it can be validated by the whole network. This way, all nodes see and validate all transactions. -->

IOTAネットワークを使用するには、[クライアントライブラリ](root://client-libraries/0.1/introduction/overview.md)を介してノードと対話することです。 [トリニティ](root://trinity/0.1/introduction/overview.md)などの多くのIOTAアプリケーションは、舞台裏でこれらのクライアントライブラリの1つを使用します。
<!-- To use any IOTA network, you can interact with a node through the [client libraries](root://client-libraries/0.1/introduction/overview.md). Many IOTA applications, such as [Trinity](root://trinity/0.1/introduction/overview.md), use one of these client libraries behind the scenes. -->

IOTAは無許可型のDLTです。したがって、ノードを個人や企業を含む誰でもが実行することができます。大量のAPI呼び出しはコストがかかる可能性があるため、ノード所有者はそれらを一般に公開しないことがよくあります。そのため、IOTAネットワークに直接アクセスするためには独自のノードを実行することをお勧めします。
<!-- IOTA is a permissionless DLT. Therefore, nodes can be run by anyone, including individuals and businesses. Node owners often don't open them to the public because a high volume of API calls can be costly. Therefore, we suggest that you run your own node for direct access to an IOTA network. -->

自分のノードを実行できない場合は、トリニティの公開ノードを使用するか、[IOTA Dance](https://iota.dance)などのWebサイトを使用して公開ノードの一覧を見つけることができます。
<!-- If you can't run your own node, you can use the public ones in Trinity or use websites such as [IOTA Dance](https://iota.dance) to find a list them. -->

[ノードソフトウェアの詳細を学んでみましょう](root://iri/0.1/introduction/overview.md)。
<!-- [Learn more about node software](root://iri/0.1/introduction/overview.md). -->
