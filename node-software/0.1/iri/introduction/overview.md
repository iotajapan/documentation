# IOTA リファレンス実装概要
<!-- # IOTA reference implementation overview -->

**IRI（IOTA リファレンス実装）は，IOTA プロトコル用のオープンソース Java ソフトウェアです．このソフトウェアは現在，クライアントが相互に IOTA トークンを転送できる[パブリック IOTA ネットワーク](root://getting-started/0.1/network/iota-networks.md)のノードで実行されます．**
<!-- **The IRI (IOTA reference implementation) is open-source Java software for the IOTA protocol. This software currently runs on nodes in the [public IOTA networks](root://getting-started/0.1/network/iota-networks.md), where clients can transfer the IOTA token among each other.** -->

IRI ノードは IOTA ネットワークのコアであり，次の機能を担当します．
<!-- IRI nodes are the core of an IOTA network, and are responsible for the following functions: -->

- トランザクションを検証する．
<!-- - Validate transactions -->
- 有効なトランザクションを台帳に保管する．
<!-- - Store valid transactions in a ledger -->
- クライアントが IRI と対話し，トランザクションを台帳に追加できるようにする．
<!-- - Allow clients to interact with the IRI and have their transactions appended to the ledger -->

自分自身の IRI ノードを実行することで，次のような利点があります．
<!-- By running your own IRI node you have the following benefits: -->

- 他の誰かの IRI ノードに接続しなくても，IOTA ネットワークに直接アクセスできます．
<!-- - You have direct access to an IOTA network instead of having to connect to someone else's IRI node -->
- 台帳の数を追加し，[隣接ノードの](root://getting-started/0.1/network/nodes.md#neighbors)台帳のトランザクションを検証することにより，IOTA ネットワークの分散を支援します．
<!-- - You help the IOTA network to become more distributed by adding to the number of ledgers and validating the transactions in your [neighbor's](root://getting-started/0.1/network/nodes.md#neighbors) ledgers -->

## 制限事項
<!-- ## Limitations -->

IRI はトランザクションを受け取り，台帳に記録しますが，トランザクションを作成したり署名したりすることはありません．トランザクションを作成または署名するには，[トリニティ](root://wallets/0.1/trinity/introduction/overview.md)や[クライアントライブラリ](root://client-libraries/0.1/introduction/overview.md)などのクライアントソフトウェアを使用して，トランザクションを IRI ノードに送信する必要があります．
<!-- IRI receives transactions and records them in a ledger, it doesn't create or sign transactions. To create or sign transactions, you must use client software such as [Trinity](root://wallets/0.1/trinity/introduction/overview.md) or a [client library](root://client-libraries/0.1/introduction/overview.md) and send the transactions to an IRI node. -->

## ブログ投稿
<!-- ## Blog posts -->

IRI に関する次のブログ投稿を読んでください．
<!-- Read the following blog posts about IRI: -->

- [Introducing Local Snapshots](https://blog.iota.org/coming-up-local-snapshots-7018ff0ed5db)
- [Networking Rewrite](https://blog.iota.org/iri-1-8-0-with-networking-rewrite-9d1e2be001e7)

## リポジトリ
<!-- ## Repository -->

[Github の IRI ソースコード](https://github.com/iotaledger/iri)を参照してください．
<!-- Go to the IRI source code on [Github](https://github.com/iotaledger/iri). -->

## Discord チャンネル
<!-- ## Discord channels -->

[公式の Discord チャンネルに参加する](https://discord.iota.org)と，次のことができます．
<!-- [Join our Discord channel](https://discord.iota.org) where you can: -->

- IOTA 開発者およびコミュニティとの議論に参加する．
<!-- - Take part in discussions with IOTA developers and the community -->
- 助けを求める．
<!-- - Ask for help -->
- 他の人を助けるためにあなたの知識を共有する．
<!-- - Share your knowledge to help others -->

IRI に関する以下のチャンネルがあります．
<!-- We have the following channels for IRI: -->

- **#iri-dev：**開発者がトピックについて議論し，GitHub の更新が表示される読み取り専用チャンネル
<!-- - **#iri-dev:** A read-only channel where developers discuss topics and where any GitHub updates are displayed -->

- **#iri-discussion：**IRI について誰でも自由に議論できるオープンチャンネル
<!-- - **#iri-discussion:** An open channel where anyone is free to discuss IRI -->

- **#fullnodes：**ノードソフトウェアについて誰でも自由に議論できるオープンチャネル
<!-- - **#fullnodes:** An open channel where anyone is free to discuss node software -->

- **#nodesharing：**誰でも自由に隣接ノードを見つけることができるオープンチャネル
<!-- - **#nodesharing:** An open channel where anyone is free to find neighbors -->

## 次のステップ
<!-- ## Next steps -->

[IRI を実行する](../how-to-guides/install-iri.md)．
<!-- [Run IRI](../how-to-guides/install-iri.md) to get started. -->
