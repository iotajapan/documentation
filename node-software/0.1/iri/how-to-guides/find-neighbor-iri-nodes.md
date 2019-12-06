# 隣接ノードを見つける
<!-- # Find neighbors -->

**台帳のトランザクションをネットワークの残りの部分と同期するには、ノードが隣接ノードと通信する必要があります。同じネットワーク上の隣接ノードを見つけるには、隣接ノードの所有者に隣接ノードのURLまたはIPアドレスを尋ねて、かつ自分のノードのURLまたはIPアドレスを相手に与える必要があります。**
<!-- **To synchronize the transactions in their ledgers with the rest of the network, nodes must communicate with their neighbors. To find neighbors on the same network, you must ask the owners for the URL or IP address of their IRI nodes and give them yours.** -->

1. [Discord](https://discord.iota.org)の`#nodesharing`チャンネルに移動します。
<!-- 1. Go to the `nodesharing` channel on [Discord](https://discord.iota.org) -->

2. 誰かのノードのURLまたはIPアドレスを尋ね、その人にあなたのノードのURLまたはIPアドレスを渡します。6〜7個の隣接ノードに接続することをお勧めします。
<!-- 2. Ask for the URL or IP address of someone's node, and give that person the URL or IP address of your node. We recommend connecting to between 6 and 7 neighbors. -->

3. 隣接ノードのURLまたはIPアドレスを[`NEIGHBORS`](../references/iri-configuration-options.md#neighbors)設定パラメーターに追加します。
<!-- 3. Add the URLs or IP addresses of your neighbor IRI nodes to the [`NEIGHBORS`](../references/iri-configuration-options.md#neighbors) configuration parameter -->

## 次のステップ
<!-- ## Next steps -->

[IRIノードを実行する](../how-to-guides/install-iri.md)。
<!-- [Run IRI](../how-to-guides/install-iri.md). -->
