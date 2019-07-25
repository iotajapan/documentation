# IOTAリファレンス実装概要
<!-- # IOTA reference implementation overview -->

**IRI（IOTAリファレンス実装）は、IOTAプロトコルを定義するオープンソースのJavaソフトウェアです。現在IRIはクライアントがIOTAトークンを相互に転送できるIOTA Mainnet上で実行されているソフトウェアです。**
<!-- **The IRI (IOTA reference implementation) is open-source Java software that defines the IOTA protocol. The IRI is the software that currently runs on the IOTA Mainnet, where clients can transfer the IOTA token among each other.** -->

IRIノードはIOTAネットワークの中核であり、以下の主要機能を担います。
<!-- IRI nodes are the core of an IOTA network, and are responsible for the following key functions: -->

- [トランザクションを検証する](../concepts/transaction-validation.md)。
<!-- - [Validate transactions](../concepts/transaction-validation.md) -->
- [有効なトランザクションを台帳に保管する](../concepts/the-ledger.md)。
<!-- - [Store valid transactions in a ledger](../concepts/the-ledger.md) -->
- [クライアントがIRIと対話できるようにし](../how-to-guides/interact-with-an-iri-node.md)、トランザクションを台帳に追加にする。
<!-- - [Allow clients to interact with the IRI](../how-to-guides/interact-with-an-iri-node.md) and have their transactions appended to the ledger -->

IRIノードがないと、誰が誰に何を送信したかを記録する方法がないため、トランザクションを送信できません。
Without IRI nodes, no one would be able to send transactions because there would be no way of recording who sent what to whom.

自身のIRIノードを実行することで、次のような利点があります。
<!-- By running your own IRI node you have the following benefits: -->

- 他の誰かのIRIノードに接続しなくても、IOTAネットワークに直接アクセスできます。
<!-- - You have direct access to an IOTA network instead of having to connect to someone else's IRI node -->
- 台帳の数を増やし、近隣IRIノードのトランザクションを検証することで、IOTAネットワークをより分散させることができます。
<!-- - You help the IOTA network to become more distributed by adding to the number of ledgers and validating your neighbor IRI node's transactions -->

## 制限事項
<!-- ## Limitations -->

IRIはトランザクションを受け取り、トランザクションを台帳に記録します。トランザクションを作成または署名することはありません。トランザクションを作成または署名するには、[トリニティ](root://trinity/0.1/introduction/overview.md)または[クライアントライブラリ](root://client-libraries/0.1/introduction/overview.md)などのクライアントソフトウェアを使用して、トランザクションをIRIノードに送信する必要があります。
<!-- The IRI receives transactions and records them in its ledger, it doesn't create or sign transactions. To create or sign transactions, you must use client software such as [Trinity](root://trinity/0.1/introduction/overview.md) or a [client library](root://client-libraries/0.1/introduction/overview.md) and send the transactions to an IRI node. -->
