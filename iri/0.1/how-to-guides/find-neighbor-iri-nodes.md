# 隣接IRIノードを見つける
<!-- # Find neighbor IRI nodes -->

**台帳内のトランザクションをネットワークの他の部分と同期させるには、IRIノードは隣接ノードと呼ばれる他のIRIノードと通信する必要があります。同じネットワーク上で隣接IRIノードを見つけるには、IRIノード所有者にIRIノードのURLまたはIPアドレスを尋ね、自分のIRIノードのURLまたはIPアドレスを渡す必要があります。**
<!-- **To synchronize the transactions in their ledgers with the rest of the network, IRI nodes must communicate with other IRI nodes, which are called neighbors. To find neighbor IRI nodes on the same network, you must ask the owners for the URL or IP address of their IRI nodes and give them yours.** -->

1. [Discord](https://discordapp.com/invite/fNGZXvh)の#nodesharingチャンネルに移動します。
<!-- 1. Go to the #nodesharing channel on [Discord](https://discordapp.com/invite/fNGZXvh) -->
2. 誰かのIRIノードのURLまたはIPアドレスを尋ね、その人にあなたのIRIノードのURLまたはIPアドレスを渡します。6〜7個の隣接IRIノードに接続することをお勧めします。
<!-- 2. Ask for the URL or IP address of someone's IRI node, and give that person the URL or IP address of your IRI node. We recommend connecting to between 6 and 7 neighbor IRI nodes. -->
3. 隣接IRIノードのURLまたはIPアドレスを[`NEIGHBORS`](../references/iri-configuration-options.md#neighbors)設定パラメータに追加します。
<!-- 3. Add the URLs or IP addresses of your neighbor IRI nodes to the [`NEIGHBORS`](../references/iri-configuration-options.md#neighbors) configuration parameter -->
