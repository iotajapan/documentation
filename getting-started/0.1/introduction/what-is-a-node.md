# ノードとは？
<!-- # What is a node? -->

**ノードは、IOTAネットワーク内のタングルへの読み書きアクセス権を持つデバイスです。すべてのノードはタングルのコピーとIOTAトークンを保持するアドレスの記録を保存します。クライアントはIOTAネットワーク内の任意のノードにバンドルを送信できるので、トランザクションを検証でき、トランザクションをタングルに添付し、他のノード転送できます。
<!-- **A node is a device that has read and write access to the Tangle in an IOTA network. All nodes store a copy of the Tangle and a record of addresses that hold IOTA tokens. Clients can send their bundles to any node in an IOTA network so it can validate the transactions, attach them to the Tangle, and forward them to other nodes.** -->

IOTAは無許可型DLTです。したがって、個人や企業を含め、誰でもノードを実行できます。
<!-- IOTA is a permissionless DLT. So, anyone can run a node, including individuals and businesses. -->

大量のAPI呼び出しはコストがかかる可能性があるため、ノード所有者はノードを一般に公開しないことがよくあります。そのため、IOTAネットワークに直接アクセスするために独自のノードを実行することをお勧めします。
<!-- Node owners often don't open them to the public because a high volume of API calls can be costly. As a result, we suggest that you run your own node for direct access to an IOTA network. -->

自分のノードを実行できない場合は、トリニティの公開ノードを使用するか、[IOTA Dance](https://iota.dance)などのWebサイトを使用して公開ノードの一覧を見つけることができます。
<!-- If you can't run your own node, you can use the public ones in Trinity or use websites such as [IOTA Dance](https://iota.dance) to find a list them. -->

IOTAネットワークを使用するには、[クライアントライブラリ](root://client-libraries/0.1/introduction/overview.md)を介してノードと対話できます。[トリニティ](root://wallets/0.1/trinity/introduction/overview.md)などの多くのIOTAアプリケーションは、舞台裏でこれらのクライアントライブラリの1つを使用します。
<!-- To use any IOTA network, you can interact with a node through the [client libraries](root://client-libraries/0.1/introduction/overview.md). Many IOTA applications, such as [Trinity](root://wallets/0.1/trinity/introduction/overview.md), use one of these client libraries behind the scenes. -->

ノードがパブリックIOTAネットワーク上で実行する[IRIノードソフトウェアについての詳細を学びます](root://node-software/0.1/iri/introduction/overview.md)。
<!-- [Learn more about the IRI node software](root://node-software/0.1/iri/introduction/overview.md) that nodes run on the public IOTA networks. -->
