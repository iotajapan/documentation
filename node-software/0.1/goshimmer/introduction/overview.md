# GoShimmer

**GoShimmer は、ノードがコーディネーターなしでコンセンサスに到達できるようにするノードソフトウェアの進行中のプロトタイプであり、IOTA ネットワークの分散化を可能にします。**
<!-- **GoShimmer is a work-in-progress prototype of a node software that allows nodes to reach a consensus without the Coordinator, allowing IOTA networks to be decentralized.** -->

GoShimmer ノードは、以下のコーディサイドモジュールを含むプロトタイプソフトウェアを実行します。
<!-- GoShimmer nodes run the prototype software that includes the following Coordicide modules: -->

- **自動ピアリング：**ネットワーク上の新しい各ノードは、4つの隣接ノードへの接続を試み、さらに4つの隣接ノードからの接続を受け入れます。
<!-- - **Auto-peering:** Each new node on the network tries to connect to four neighbors and accepts connections from a further four neighbors -->
- **ノード ID：**各ノードは、固有の公開/秘密鍵ペアを作成します。公開鍵は、自動ピアリング中にノードを識別するために使用されます。将来、これらの ID はノードがマナを受け取ることを可能にするでしょう。
<!-- - **Node identities:** Each node creates a unique public/private key pair. The public key is used to identify nodes during auto-peering. In the future, these identities will allow nodes to receive mana. -->

GoShimmer ノードを実行することにより、IOTA プロトコルの次の段階で最先端の開発をテストできます。
<!-- By running a GoShimmer node, you can test the cutting-edge developments in the next stage of the IOTA protocol. -->

## 制限事項
<!-- ## Limitations -->

GoShimmer は、研究部門が [Coordicide](https://coordicide.iota.org) をテストするために開発しているプロトタイプノードソフトウェアです。
<!-- GoShimmer is a prototype node software that the Research Department are developing to test [Coordicide](https://coordicide.iota.org). -->

すべてのモジュールが利用可能になると、GoShimmer ノードは Coordicide テストネットになり、次の IOTA プロトコルのリリース候補になります。
<!-- When all the modules become available, the GoShimmer nodes will become the Coordicide testnet, which is a release candidate for the next IOTA protocol. -->

## ブログの投稿
<!-- ## Blog posts -->

GoShimmer に関する次のブログ投稿を読んでください。
<!-- Read the following blog posts about GoShimmer: -->

- [Open Sourcing the GoShimmer Prototype](https://blog.iota.org/open-sourcing-of-the-goshimmer-prototype-891c0a8eafcb)

## リポジトリ
<!-- ## Repository -->

Github の [GoShimmer ソースコード](https://github.com/iotaledger/goshimmer)を参照してください。
<!-- Go to the GoShimmer source code on [Github](https://github.com/iotaledger/goshimmer). -->

## Discord チャンネル
<!-- ## Discord channels -->

[公式の Discord チャンネルに参加する](https://discord.iota.org)と、次のことができます。
<!-- [Join our Discord channel](https://discord.iota.org) where you can: -->

- IOTA 開発者およびコミュニティとの議論に参加する。
<!-- - Take part in discussions with IOTA developers and the community -->
- 助けを求める。
<!-- - Ask for help -->
- 他の人を助けるためにあなたの知識を共有する。
<!-- - Share your knowledge to help others -->

GoShimmer に関するチャンネルは以下の2つです。
<!-- We have the following channels for GoShimmer: -->

- **#goshimmer-dev：**開発者がトピックについて議論し、GitHub の更新が表示される読み取り専用チャンネル
<!-- - **#goshimmer-dev:** A read-only channel where developers discuss topics and where any GitHub updates are displayed -->

- **#goshimmer-discussion：**誰でも自由に GoShimmer について議論できるオープンチャンネル
<!-- - **#goshimmer-discussion:** An open channel where anyone is free to discuss GoShimmer -->

## 次のステップ
<!-- ## Next steps -->

[GoShimmer を実行](../how-to-guides/run-the-node.md)を使用して、モジュールのテストを開始します。
<!-- [Run GoShimmer](../how-to-guides/run-the-node.md) to get started with testing the modules. -->
