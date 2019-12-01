# GoShimmer

**GoShimmerは、ノードがコーディネーターなしでコンセンサスに到達できるようにするノードソフトウェアの進行中のプロトタイプであり、IOTAネットワークの分散化を可能にします。**
<!-- **GoShimmer is a work-in-progress prototype of a node software that allows nodes to reach a consensus without the Coordinator, allowing IOTA networks to be decentralized.** -->

## GoShimmerノードとは？
<!-- ## What is a GoShimmer node? -->

GoShimmerノードは、以下のコーディサイドモジュールを含むプロトタイプソフトウェアを実行します。
<!-- GoShimmer nodes run the prototype software that includes the following Coordicide modules: -->

- **自動ピアリング：** ネットワーク上の新しい各ノードは、4つの隣接ノードへの接続を試み、さらに4つの隣接ノードからの接続を受け入れます。
<!-- - **Auto-peering:** Each new node on the network tries to connect to four neighbors and accepts connections from a further four neighbors -->
- **ノードID：** 各ノードは、固有の公開/秘密鍵ペアを作成します。公開鍵は、自動ピアリング中にノードを識別するために使用されます。将来、これらのIDはノードがマナを受け取ることを可能にするでしょう。
<!-- - **Node identities:** Each node creates a unique public/private key pair. The public key is used to identify nodes during auto-peering. In the future, these identities will allow nodes to receive mana. -->

すべてのモジュールが使用可能になると、GoShimmerノードがコーディサイドテストネットになります。これは、次のIOTAプロトコルのリリース候補です。
<!-- When all the modules become available, the GoShimmer nodes will become the Coordicide testnet, which is a release candidate for the next IOTA protocol. -->

## GoShimmerノードを実行する理由
<!-- ## Why run a GoShimmer node? -->

GoShimmerノードを実行することにより、IOTAプロトコルの次の段階での最先端の開発をテストできます。
<!-- By running a GoShimmer node, you can test the cutting-edge developments in the next stage of the IOTA protocol. -->

## 次のステップ
<!-- ## Next steps -->

[GoShimmerを実行](../how-to-guides/run-the-node.md)を使用して、モジュールのテストを開始します。
<!-- [Run GoShimmer](../how-to-guides/run-the-node.md) to get started with testing the modules. -->
