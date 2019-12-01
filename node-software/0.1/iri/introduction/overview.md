# IOTAリファレンス実装概要
<!-- # IOTA reference implementation overview -->

**IRI（IOTAリファレンス実装）は、[パブリックIOTAネットワーク](root://getting-started/0.1/network/iota-networks.md)のノードで現在実行されているIOTAプロトコルを定義するオープンソースのJavaソフトウェアであり、クライアントは相互にIOTAトークンを転送できます。**
<!-- **The IRI (IOTA reference implementation) is open-source Java software that defines the IOTA protocol that currently runs on nodes in the [public IOTA networks](root://getting-started/0.1/network/iota-networks.md), where clients can transfer the IOTA token among each other.** -->

IRIノードはIOTAネットワークの中核であり、以下の主要機能を担います。
<!-- IRI nodes are the core of an IOTA network, and are responsible for the following key functions: -->

- トランザクションを検証する。
<!-- - Validate transactions -->
- 有効なトランザクションを台帳に保管する。
<!-- - Store valid transactions in a ledger -->
- クライアントがIRIと対話し、トランザクションを台帳に追加できるようにする。
<!-- - Allow clients to interact with the IRI and have their transactions appended to the ledger -->

自身のIRIノードを実行することで、次のような利点があります。
<!-- By running your own IRI node you have the following benefits: -->

- 他の誰かのIRIノードに接続しなくても、IOTAネットワークに直接アクセスできます。
<!-- - You have direct access to an IOTA network instead of having to connect to someone else's IRI node -->
- 台帳の数を追加し、[隣接ノードの](root://getting-started/0.1/network/nodes.md#neighbors)台帳のトランザクションを検証することにより、IOTAネットワークの分散を支援します。
<!-- - You help the IOTA network to become more distributed by adding to the number of ledgers and validating the transactions in your [neighbor's](root://getting-started/0.1/network/nodes.md#neighbors) ledgers -->

## 制限事項
<!-- ## Limitations -->

IRIはトランザクションを受信して台帳に記録しますが、トランザクションを作成したり署名したりすることはありません。トランザクションを作成または署名するには、[トリニティ](root://wallets/0.1/trinity/introduction/overview.md)や[クライアントライブラリ](root://client-libraries/0.1/introduction/overview.md)などのクライアントソフトウェアを使用して、トランザクションをIRIノードに送信する必要があります。
<!-- The IRI receives transactions and records them in its ledger, it doesn't create or sign transactions. To create or sign transactions, you must use client software such as [Trinity](root://wallets/0.1/trinity/introduction/overview.md) or a [client library](root://client-libraries/0.1/introduction/overview.md) and send the transactions to an IRI node. -->

## 次のステップ
<!-- ## Next steps -->

[Linux](../how-to-guides/run-an-iri-node-on-linux.md)または[Docker](../how-to-guides/run-an-iri-node-in-docker.md)でIRIノードを実行して、トランザクションの検証と保存を開始します。
<!-- Run an IRI node on [Linux](../how-to-guides/run-an-iri-node-on-linux.md) or [Docker](../how-to-guides/run-an-iri-node-in-docker.md) to get started with validating and storing transactions. -->
