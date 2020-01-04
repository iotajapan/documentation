# 深度
<!-- # Depth -->

**ノードにトランザクションを送信する場合、深度引数を指定できます。これは、[ノード](../network/nodes.md)が[チップ選択アルゴリズム](../network/the-tangle.md#tip-selection)を開始する過去のマイルストーン数を定義します。深度が大きいほど、[タングル](../network/the-tangle.md)内のより深くからチップ選択が開始されます。深度が大きくなると、ノードがチップの選択を完了するのにかかる時間が長くなり、計算能力が高くなります。**
<!-- **When sending a transaction to a node, you can specify a depth argument, which defines how many milestones in the past the [node](../network/nodes.md) starts the [tip selection algorithm](../network/the-tangle.md#tip-selection). The greater the depth, the farther back in the [Tangle](../network/the-tangle.md) the node starts. A greater depth increases the time that nodes take to complete tip selection, making them use more computational power.** -->

## 深度を選択するためのアドレス
<!-- ## Advice for choosing a depth -->

深度を選択するときは、次の質問を考慮する必要があります。
<!-- When choosing a depth, you should consider the following question. -->

### ノードが許可する最大の深度は？
<!-- ### What is the maximum depth that your node will allow? -->

深度を制限するために、ノードは [`MAX-DEPTH`](root://node-software/0.1/iri/references/iri-configuration-options.md#max-depth) 構成オプションで最大値を定義できます。
<!-- To restrict the depth, nodes can define a maximum value for it in the [`MAX-DEPTH`](root://node-software/0.1/iri/references/iri-configuration-options.md#max-depth) configuration option. -->

自分自身のノードに接続していない場合は、自分自身のノードに接続することをお勧めします。こうすることで、有効な深度を使用していることを確認できます。
<!-- If you aren't connected to your own node, you should consider doing so. This way, you can make sure that you're using a valid depth. -->

サードパーティのノードを使用する場合は、[`getNodeAPIConfiguration`](root://node-software/0.1/iri/references/api-reference.md#getnodeapiconfiguration) エンドポイントを呼び出すことにより、`MAX-DEPTH` 構成オプションの値を確認できます。
<!-- If you want to use a third-party node, you can check the value of this configuration option by calling the [`getNodeAPIConfiguration`](root://node-software/0.1/iri/references/api-reference.md#getnodeapiconfiguration) endpoint. -->

## 関連ガイド
<!-- ## Related guides -->

[JavaScript で "hello world" トランザクションを送信する](root://client-libraries/0.1/how-to-guides/js/send-your-first-bundle.md)。
<!-- [Send a "hello world" transaction in JavaScript](root://client-libraries/0.1/how-to-guides/js/send-your-first-bundle.md). -->

[JavaScript で IOTA トークンを転送する](root://client-libraries/0.1/how-to-guides/js/transfer-iota-tokens.md)。
<!-- [Transfer IOTA tokens in JavaScript](root://client-libraries/0.1/how-to-guides/js/transfer-iota-tokens.md). -->

[トリニティでトランザクションを送信する](root://wallets/0.1/trinity/how-to-guides/send-a-transaction.md)。
<!-- [Send a transaction in Trinity](root://wallets/0.1/trinity/how-to-guides/send-a-transaction.md). -->
