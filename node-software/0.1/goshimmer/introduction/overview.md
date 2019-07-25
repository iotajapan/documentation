# GoShimmer

**GoShimmerは、CoordicideにつながるIOTAネットワークの進行中のプロトタイプです。Coordicideの目標は、コーディネーターなしでネットワークが合意に達することで、IOTAネットワークを分散化することができます。この目標を達成するために、GoShimmerはそれぞれが特定の役割を持つモジュールで構成されています。すべてのモジュールが利用可能になると、このネットワークはCoordicide testnetになります。これは、次のIOTAプロトコルのリリース候補です。**
<!-- **GoShimmer is a work-in-progress prototype of an IOTA network that will lead to Coordicide. The goal of Coordicide is for the network to reach a consensus without the Coordinator, allowing IOTA networks to be decentralized. To reach this goal, GoShimmer consists of modules that each have a specific role. When all the modules become available, this network will become the Coordicide testnet, which is a release candidate for the next IOTA protocol.** -->

現時点では、GoShimmerには以下のモジュールが含まれています。
<!-- At the moment, GoShimmer includes the following modules: -->

* **自動ピアリング：** ネットワーク上の新しい各ノードは、4つの隣接ノードへの接続を試み、さらに4つの隣接ノードからの接続を受け入れます。
<!-- * **Auto-peering:** Each new node on the network tries to connect to four neighbors and accepts connections from a further four neighbors -->
* **ノードID：** 各ノードは、固有の公開/秘密鍵ペアを作成します。公開鍵は、自動ピアリング中にノードを識別するために使用されます。将来、これらのIDはノードがマナを受け取ることを可能にするでしょう。
<!-- * **Node identities:** Each node creates a unique public/private key pair. The public key is used to identify nodes during auto-peering. In the future, these identities will allow nodes to receive mana. -->

:::info:
[完全なCoordicideソリューション中のマナと他のモジュールについてもっと知る](https://coordicide.iota.org)。
:::
<!-- :::info: -->
<!-- [Find out more about mana and the other modules in the full Coordicide solution](https://coordicide.iota.org). -->
<!-- ::: -->
