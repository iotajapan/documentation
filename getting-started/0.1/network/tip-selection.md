# チップ選択
<!-- # Tip selection -->

**タングル内の各トランザクションは、2つの以前のトランザクションを参照する必要があります。チップ選択は、ノードが自身の台帳内のサブグラフから2つのランダムなチップトランザクションを選択するプロセスです。**
<!-- **Each transaction in the Tangle must reference two previous transactions. Tip selection is the process whereby a node selects two random tip transactions from a subgraph of its ledger.** -->

一般に、チップ選択アルゴリズムは親を持たないトランザクションを選択します。これらのトランザクションはチップと呼ばれるため、「チップ選択」という名前が付けられています。
<!-- In general, the tip selection algorithm selects transactions that have no parents. These transactions are called tips, hence the name 'tip selection'. -->

チップ選択アルゴリズムは [IRI ノードソフトウェア](root://node-software/0.1/iri/introduction/overview.md)に組み込まれていますが、ネットワークによって強制されることはありません。代わりに、ノードにはチップ選択アルゴリズムを使用するインセンティブが与えられます。
<!-- Although the tip selection algorithm is embedded in the [IRI node software](root://node-software/0.1/iri/introduction/overview.md), it isn't enforced by the network. Instead, nodes are given an incentive to use the tip selection algorithm. -->

## チップ選択プロセス
<!-- ## The tip selection process -->

クライアントが [`getTransactionsToApprove`](root://node-software/0.1/iri/references/api-reference.md#getTransactionsToApprove) API エンドポイントを呼び出すと、ノードは台帳のサブグラフ（サブタングルとも呼ばれます）を選択し、2つの重み付きランダムウォークを実行します。各重み付きランダムウォークは、チップトランザクションハッシュを返します。
<!-- When a client calls the [`getTransactionsToApprove`](root://node-software/0.1/iri/references/api-reference.md#getTransactionsToApprove) API endpoint, the node selects a subgraph (also known as a subtangle) of the ledger and does two weighted random walks through it. Each weighted random walk returns a tip transaction hash. -->

## サブグラフ選択
<!-- ### Subgraph selection -->

サブグラフは、マイルストーントランザクションとチップトランザクションの間のすべてのトランザクションを含む台帳の一部です。
<!-- A subgraph is a section of the ledger that contains all transactions between a milestone transaction and tip transactions. -->

チップ選択は、計算能力を節約するために台帳のサブグラフで実行されます。ノードが重み付きランダムウォークに含めるトランザクションが多いほど、チップ選択プロセスにかかる時間が長くなります。
<!-- The tip selection is done on a subgraph of the ledger to save computational power. The more transactions that a node includes in the weighted random walk, the longer the tip selection process takes. -->

ヒント選択プロセスの場合、サブグラフのマイルストーントランザクションはクライアントによって定義され、以下を実行して計算されます。
<!-- For the tip selection process, the milestone transaction for the subgraph is defined by the client, and is calculated by doing the following: -->

`latestMilestoneIndex` - `depth`

この計算の結果は、サブグラフの形成に使用されるマイルストーントランザクションのインデックスに等しくなります。
<!-- The result of this calculation is equal to the index of the milestone transaction that is used to form the subgraph. -->

### 重み付きランダムウォーク
<!-- ### Weighted random walk -->

重み付きランダムウォークは、ノードがサブグラフ内のチップトランザクションへのパスを見つけるために使用するアルゴリズムです。
<!-- A weighted random walk is an algorithm that nodes use to find a path to a tip transaction in a subgraph. -->

新しいトランザクションへのパスを選択する確率を高めるために、アルゴリズムはより高い評価を持つトランザクションを通るパスを優先します。この評価は累積荷重と呼ばれます。
<!-- To increase the probability of selecting a path to new transactions, the algorithm favors a path through transactions that have a higher rating. This rating is called a cumulative weight. -->

トランザクションの累積荷重は、次の変数を使用して計算されます。
<!-- The cumulative weight of a transaction is calculated using the following variables: -->
- **フューチャーセット：**トランザクションを承認するトランザクション群
<!-- - **Future set:** Transactions that approves the transaction -->
- **[`ALPHA` 構成パラメーター](root://node-software/0.1/iri/references/iri-configuration-options.md#alpha)：**チップ選択プロセスのランダム性に影響する数値
<!-- - **[`ALPHA` configuration parameter](root://node-software/0.1/iri/references/iri-configuration-options.md#alpha):** A number that affects the randomness of the tip selection process -->

ノードは、フューチャーセットが小さいトランザクションよりもフューチャーセットが大きいトランザクションの方が確定される確率が高いために高い評価を与えます。ただし、ノードがフューチャーセット変数のみに基づいてトランザクションを評価する場合、台帳は他の多くのトランザクションによって参照されるトランザクションだけが続く長く狭いチェーンになります。これは、新しいトランザクションが他のトランザクションが参照する前に十分に大きなフューチャーセットを持つまで待機する必要があるため、新しいトランザクションが台帳に追加される速度を遅くします。そのため、新しいトランザクションが台帳に追加される速度を上げるために、ノードは `ALPHA` 構成パラメーターを使用して累積荷重を計算します。
<!-- Nodes gives a high rating to a transaction with a large future set because it has a higher probability of being confirmed than one with a small future set. However, if a node were to rate transactions based only on this variable, the ledger would become a long, narrow chain of transactions, which are referenced by many other transactions. This would slow the rate of new transactions being appended to the ledger because new transactions would have to wait until they had a large enough future set before other transactions would reference them. So, to increase the speed at which new transactions are appended to the ledger, nodes also use the `ALPHA` configuration parameter to calculate the cumulative weight. -->

`ALPHA` 構成パラメーターは、各トランザクションの累積荷重がランダム性の要素で計算されることを確認します。`ALPHA` 構成パラメーターを使用すると、ノードは小さなフューチャーセットを持ついくつかのトランザクションを選択できます。そうすることで、新しいトランザクションが台帳に追加される速度が向上します。
<!-- The `ALPHA` configuration parameter makes sure that the cumulative weight of each transaction is calculated with an element of randomness. This parameter allows nodes to select some transactions that have a small future set and by doing so, increase the speed at which new transactions are appended to the ledger. -->

重み付きランダムウォークの詳細と、`ALPHA` 構成パラメーターの最適値を取り巻く理論に関する詳細な説明については、[ブログ投稿](https://blog.iota.org/confirmation-rates-in-the-tangle-186ef02878bb)を参照してください。
<!-- For more information about the weighted random walk, and for an in-depth explanation about the theories surrounding the best value for the `ALPHA` configuration parameter, read our [blog post](https://blog.iota.org/confirmation-rates-in-the-tangle-186ef02878bb). -->

## バンドルと整合性
<!-- ## Bundles and consistency -->

IOTA のトランザクションはバンドルで送信されます。したがって、ランダムウォーカーが承認者を横断するとき、その承認者はバンドルの途中にいる可能性があります。バンドルを検証するために、ランダムウォーカーはトランクトランザクションをトラバースしてテールトランザクションを見つけます。
<!-- Transactions in IOTA are sent in bundles. Therefore, when the walker traverses an approver, that approver may be in the middle of a bundle. To validate the bundle, the walker finds the tail transaction by traversing the trunk transactions. -->

2つのチップトランザクションの相互の整合性がチェックされ、どちらも無効ではないことが確認されます。したがって、クライアントトランザクションは、別のトランザクションによって承認される可能性が高い2つの有効なトランザクションを参照するため、累積荷重が増加します。
<!-- The two tip transactions are checked for consistency between each other to make sure that neither one is invalid. Therefore, the clients transaction references two valid transactions that have a better chance of being approved by another transaction, thus increasing its cumulative weight. -->

## チップ選択を使用するインセンティブ
<!-- ## Incentives to use tip selection -->

チップ選択アルゴリズムは強制されていません。代わりに、ノードはチップ選択アルゴリズムを使用して、トランザクションが確定される可能性を最大限に高めるインセンティブを持っています。
<!-- The tip selection algorithm is not enforced. Instead, nodes have an incentive to use it to have the best chance of their transactions becoming confirmed. -->

ノードがチップ選択アルゴリズムを使用したかどうか、または利己的な目的のためにカスタムチップトランザクションを返すように変更したかどうかを確認することはできません。
<!-- It's impossible to check if a node used the tip selection algorithm or even changed it to return custom tip transactions for its own purposes. -->

ただし、[ホワイトペーパー](https://iota.org/IOTA_Whitepaper.pdf)で説明されているように、チップトランザクションはランダムに選択する必要があります。
<!-- However, as discussed in the [whitepaper](https://iota.org/IOTA_Whitepaper.pdf), it's necessary that tip transactions are selected at random. -->

ノードが自分のチップトランザクション（たとえば、自分のトランザクションを承認するトランザクション）を選択した場合、少数の幸運なトランザクションのみが確定されるため、このランダム性は重要です。この理論については、論文[タングル内での均衡](https://arxiv.org/abs/1712.05385)で議論しています。
<!-- This randomness is important because if nodes chose to select their own tip transactions (for example, those that approve their own transactions), only a few lucky transactions would ever be confirmed. We discuss this theory in the paper [Equilibria in the tangle](https://arxiv.org/abs/1712.05385). -->

![Nodes select the best tip transactions](https://cdn-images-1.medium.com/max/1600/1*Qs_KFwcXxXKuoERjfJ5xsw.jpeg)

上図では、黒のトランザクションが最適であると見なされ、新しいトランザクションはそれぞれ黒のトランザクションを参照しようとします。この状況により、すべてのトランザクションが確定される割合が低下します。
<!-- In this diagram, the black transactions are considered the best, and each new transaction tries to reference them. This situation reduces the rate at which all transactions are confirmed. -->

すべてのノードがチップ選択アルゴリズムを使用すると、すべてのチップトランザクションがランダムに選択されます。このランダム性により、新しいトランザクションがタングルにアタッチされ、最終的に確定される割合が増加します。
<!-- When all nodes use the tip selection algorithm, all tip transactions are selected at random. This randomness increases the rate at which new transactions are attached to the Tangle and eventually confirmed. -->

![Comparison between a random tip selection and a non-random tip selection](https://cdn-images-1.medium.com/max/1600/1*qvNmyzQijU3PpMYvYtaxGg.jpeg)
