# コンパス設定オプション
<!-- # Compass configuration options -->

**コンパス設定オプションを使用すると、ニーズに応じてプライベートタングルをカスタマイズできます。たとえば、特定のテスト用に軽量のプライベートタングルを作成したり、Mainnetに似た、よりリソース集約型のプライベートタングルを作成したりすることができます。**
<!-- **The Compass configuration options allow you to customize your private Tangle, depending on your needs. For example, you could create a lightweight private Tangle for a specific test, or create a more resource intensive one that's similar to the Mainnet.** -->

| **パラメータ** | **説明** | **メモ** |
| :--------------- | :------- | :------- |
| `seed` | コンパスがマイルストーンバンドルに署名するために使用するシード | このシードをバックアップして安全に保管してください。 |
| `security` | コンパスの秘密鍵/アドレスペアのセキュリティレベル | セキュリティレベルが高ければ高いほど、署名済みアドレスの署名はより大きく、より安全になります。より高いセキュリティレベルでは、トランザクションに署名するためにより多くの計算が必要であり、そして署名を含むためにより多くのトランザクションがバンドル内で必要となります。その結果、低電力デバイスではセキュリティレベル2を使用し、大規模企業ではセキュリティレベル3を使用したいと思うかもしれません。 |
| `depth` | コンパスの秘密鍵/アドレスペアの数に影響する指数。ペアの総数は、次の式に依存します：2<sup>depth</sup>。 | `depth`が大きいほど、マークル木の計算にかかる時間は長くなりますが、コンパスが署名して送信できるバンドルは多くなります。使用できる最大の`depth`は、IRIノードの[`MAX_DEPTH`パラメータ](root://node-software/0.1/iri/references/iri-configuration-options.md#max-depth)に依存します。 |
| `mwm` | [最小重量値](root://getting-started/0.1/transactions/proof-of-work.md#minimum-weight-magnitude) | `mwm`が大きいほど、トランザクションのプルーフオブワークを行うことが難しくなります。 |
| `tick` | マイルストーンの作成後にコンパスが待機するミリ秒数 | `tick`が長いほど、ネットワークで確定できるトランザクションは少なくなりますが、ネットワークの稼働時間は長くなります。マイルストーンの間隔は、コンパスが`tick`の後にマイルストーンバンドルを作成、署名、および送信するのにかかる時間の長さによっても異なります。 |
| `host` | コンパスがマイルストーンを送信する先のノードのURL |  |

<!-- |**Parameter**|**Description**|**Notes**| -->
<!-- |:----------------------|:--------------|:--------| -->
<!-- |`seed` |Seed that Compass uses to sign milestone bundles |Back up this seed and keep it secure| -->
<!-- |`security`|Security level of Compass' private key/address pairs|The greater the security level, the larger and more secure the signature of a spent address is against brute force attacks. A greater security level also means that more computations must be done to sign a bundle and that more transactions are needed in a bundle to contain the signature. As a result, low-powered devices may want to use security level 2, whereas a large-scale company may want to use security level 3.| -->
<!-- |`depth`|Exponent that affects how many private key/address pairs Compass has. The total number pairs depends on this formula: 2<sup>depth</sup>.|The greater the depth, the longer it takes to compute the Merkle tree, but the more bundles Compass can sign and send. The maximum depth you can use depends on the [`MAX_DEPTH` parameter](root://node-software/0.1/iri/references/iri-configuration-options.md#max-depth) of your IRI node.| -->
<!-- |`mwm`|[Minimum weight magnitude](root://getting-started/0.1/transactions/proof-of-work.md#minimum-weight-magnitude)|The higher the MWM, the harder it is to do the proof of work for a transaction.| -->
<!-- |`tick`|Number of milliseconds Compass waits after creating a milestone|The longer the tick, the fewer transactions can be confirmed in your network, but the more uptime your network will have. The interval between milestones also depends on the length of time it takes Compass to create, sign and send a milestone bundle after the tick. | -->
<!-- |`host`|URL of the node to which Compass sends milestones|| -->
