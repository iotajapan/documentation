# チップ選択
<!-- # Tip selection -->

**タングル内の各トランザクションは、前の2つのトランザクションを参照して承認しなければなりません。チップ選択は、ノードが自身の台帳の部分グラフから2つのランダムなチップトランザクションを選択するプロセスです。**
<!-- **Each transaction in the Tangle must reference and approve two previous transactions. Tip selection is the process whereby a node selects two random tip transactions from a subgraph of its ledger.** -->

一般的に、チップ選択アルゴリズムは、親を持たないチップトランザクションを選択します。
<!-- In general, the tip selection algorithm selects tip transactions that have no parents. -->

チップ選択アルゴリズムは[IOTAノードソフトウェア](root://iri/0.1/introduction/overview.md)に組み込まれていますが、ネットワークによって強制されているわけではありません。代わりに、ノードに[チップ選択アルゴリズムを使用するインセンティブ](../concepts/incentives-in-the-tangle.md)が与えられています。
<!-- Although the tip selection algorithm is embedded in the [IOTA node software](root://iri/0.1/introduction/overview.md), it isn't enforced by the network. Instead, nodes are given an [incentive to use the tip selection algorithm](../concepts/incentives-in-the-tangle.md). -->

## チップ選択プロセス
<!-- ## The tip selection process -->

ノードは台帳の部分グラフ（部分タングルとも呼ばれます）を選択し、その部分グラフを通る2つの重み付きランダムウォークを行います。各重み付きランダムウォークはチップトランザクションハッシュを返します。
<!-- The node selects a subgraph (also known as a subtangle) of the ledger and does two weighted random walks through it. Each weighted random walk returns a tip transaction hash. -->

### 部分グラフ選択
<!-- ### Subgraph selection -->

部分グラフは、マイルストーントランザクションとチップトランザクションの間のすべてのトランザクションを含む台帳の一部です。
<!-- A subgraph is a section of the ledger that contains all transactions between a milestone transaction and tip transactions. -->

チップ選択は、計算能力を節約するために台帳の部分グラフで行われます。ノードが重み付きランダムウォークを行うトランザクションが多いほど、チップ選択プロセスにかかる時間が長くなります。
<!-- The tip selection is done on a subgraph of the ledger to save computational power. The more transactions that a node includes in the weighted random walk, the longer the tip selection process takes. -->

チップ選択プロセスでは、部分グラフのマイルストーントランザクションはクライアントによって定義され、次のようにして計算されます。
<!-- For the tip selection process, the milestone transaction for the subgraph is defined by the client, and is calculated by doing the following: -->

`latestMilestoneIndex` - `depth`

この計算結果は、部分グラフの作成に使用されたマイルストーントランザクションのインデックスと等しくなります。
<!-- The result of this calculation is equal to the index of the milestone transaction that is used to form the subgraph. -->

:::info:
`depth`パラメータの値が大きいほど、ノードが実行しなければならない計算が増えます。 `depth`パラメータの値を制限するために、ノードは [`MAX-DEPTH`](root://iri/0.1/references/iri-configuration-options.md#max-depth)設定オプションを変更することができます。
:::
<!-- :::info: -->
<!-- The greater the value of the `depth` parameter, the more computations the node must do. To restrict the value of the `depth` parameter, nodes can change the [`MAX-DEPTH`](root://iri/0.1/references/iri-configuration-options.md#max-depth) configuration option. -->
<!-- ::: -->

### 重み付きランダムウォーク
<!-- ### Weighted random walk -->

重み付きランダムウォークは、ノードが部分グラフ内のチップトランザクションへの経路を見つけるために使用するアルゴリズムです。
<!-- A weighted random walk is an algorithm that nodes use to find a path to a tip transaction in a subgraph. -->

新しいトランザクションへの経路を選択する確率を高めるために、アルゴリズムはより高い評価を有するトランザクションを通る経路を支持します。この評価は累積荷重と呼ばれます。
<!-- To increase the probability of selecting a path to new transactions, the algorithm favors a path through transactions that have a higher rating. This rating is called a cumulative weight. -->

トランザクションの累積荷重は、以下の変数を使用して計算されます。
<!-- The cumulative weight of a transaction is calculated using the following variables: -->
* **未来集合：** トランザクションを直接・間接的に承認するトランザクション
<!-- * **Future set:** Transactions that approves the transaction -->
* **[`ALPHA`設定パラメータ](root://iri/0.1/references/iri-configuration-options.md#alpha)：**チップ選択プロセスのランダムさに影響する数値
<!-- * **[`ALPHA` configuration parameter](root://iri/0.1/references/iri-configuration-options.md#alpha):** A number that affects the randomness of the tip selection process -->

未来集合が小さいトランザクションよりも未来集合が大きいトランザクションの方が確定する可能性が高いため、ノードは未来集合が大きいトランザクションに高い評価を与えます。
ただし、ノードが未来集合の大きさだけでトランザクションを評価すると、台帳の形状が、多くのトランザクションで少数のトランザクションだけを参照する長く狭いチェーンになってしまいます。
これにより、新しいトランザクションの未来集合が十分大きくなる前に、多くのトランザクションが既に大きな未来集合を持つトランザクションを参照してしまうため、台帳に追加される新規トランザクションの速度は遅くなります。
そのため、新しいトランザクションが台帳に追加される速度を上げるために、ノードは未来集合と`ALPHA`設定パラメータを併用して累積荷重を計算します。
<!-- Nodes gives a high rating to a transaction with a large future set because it has a higher probability of being confirmed than one with a small future set. However, if a node were to rate transactions based only on this variable, the ledger would become a long, narrow chain of transactions, which are referenced by many other transactions. This would slow the rate of new transactions being appended to the ledger because new transactions would have to wait until they had a large enough future set before other transactions would reference them. So, to increase the speed at which new transactions are appended to the ledger, nodes also use the `ALPHA` configuration parameter to calculate the cumulative weight. -->

`ALPHA`設定パラメータは、各トランザクションの累積荷重がランダム性の要素を使用して計算されるようにします。このパラメータを使用すると、ノードは未来集合が小さいトランザクションをいくつか選択できます。そうすることで、新しいトランザクションが台帳に追加される速度を上げることができます。
<!-- The `ALPHA` configuration parameter makes sure that the cumulative weight of each transaction is calculated with an element of randomness. This parameter allows nodes to select some transactions that have a small future set and by doing so, increase the speed at which new transactions are appended to the ledger. -->

重み付きランダムウォークの詳細、および`ALPHA`設定パラメータの最適値を取り巻く理論に関する詳細な説明については、[ブログの投稿](https://blog.iota.org/confirmation-rates-in-the-tangle-186ef02878bb)を参照してください。
<!-- For more information about the weighted random walk, and for an in-depth explanation about the theories surrounding the best value for the `ALPHA` configuration parameter, read our [blog post](https://blog.iota.org/confirmation-rates-in-the-tangle-186ef02878bb). -->

## チップ選択アルゴリズムの詳細な説明
<!-- ## In-depth explanation of the tip selection algorithm -->

以下の情報は、クライアントが[getTransactionsToApprove](root://iri/0.1/references/api-reference.md#getTransactionsToApprove)エンドポイントを呼び出したときにノードが何をしているかを説明しています。
<!-- The following information describes what nodes do when a client calls the [getTransactionsToApprove](root://iri/0.1/references/api-reference.md#getTransactionsToApprove) endpoint. -->

クライアントは、トランザクションを送信したいときにこのエンドポイントを呼び出します。エンドポイントによって、2つのチップトランザクションハッシュが生成され、それらが新しいトランザクションの`trunkTransaction`フィールドと`branchTransaction`フィールドで使用されます。
<!-- Clients call this endpoint when they want to send a transaction. The endpoint results in two tip transaction hashes, which are used in the `trunkTransaction` and `branchTransaction` fields of the new transaction. -->

:::info:
[トランザクションの構造](root://iota-basics/0.1/references/structure-of-a-transaction.md)について詳しく調べる。
:::
<!-- :::info: -->
<!-- Find out more about the [structure of a transaction](root://iota-basics/0.1/references/structure-of-a-transaction.md). -->
<!-- ::: -->

このエンドポイントが呼び出されると、ノードはチップ選択アルゴリズムを開始します。これは次のステージに分かれています。
<!-- When this endpoint is called, a node starts the tip selection algorithm, which is separated into the following stages: -->

1. 準備
<!-- 1. Preparation -->
2. 評価計算
<!-- 2. Rating calculation -->
3. 重み付きランダムウォーク
<!-- 3. Weighted random walk -->

---

### 準備
<!-- ### Preparation -->

アルゴリズムの目的は、API呼び出しの成功結果として、競合しない2つのチップトランザクションを返すことです。
<!-- The algorithm's goal is to return two non-conflicting tip transactions as a successful result of the API call. -->

アルゴリズムは台帳の部分グラフを選択し、それを通る2つの重み付きランダムウォークを行います。各重み付きランダムウォークはチップトランザクションハッシュを返します。
<!-- The algorithm selects a subgraph of the ledger and does two weighted random walks through it. Each weighted random walk returns a tip transaction hash. -->

2つの[重み付きランダムウォーク](#重み付きランダムウォーク)は、同じマイルストーントランザクションから開始されます（`latestSolidMilestone` - `depth`）。
<!-- Both [weighted random walks](#weighted-random-walk) start from the same milestone transaction (`latestSolidMilestone - depth`). -->

クライアントがAPI呼び出しに`reference`引数を指定した場合、そのトランザクションが部分グラフ内にある場合にのみ、`branchTransaction`ウォークは`reference`引数内のトランザクションから開始されます。
<!-- If the client specifies a `reference` argument to the API call, the `branchTransaction` walk will start from the transaction in the `reference` argument only if that transaction is in the subgraph. -->

`reference`引数のトランザクションが`depth`マイルストーンインデックスよりも古い場合、API呼び出しは失敗し、次のエラーメッセージが表示されます。：`Reference transaction is too old.`
<!-- If the transaction in the `reference` argument is older than the `depth` milestone index, the API call fails with the following error message: Reference transaction is too old. -->

## 評価計算
<!-- ## Rating calculation -->

このアルゴリズムは、部分グラフ内のすべてのトランザクションの評価を計算します。これらの評価は、ウォーカーの経路を偏らせるために、重み付きランダムウォーク中に、後で荷重に変換されます。
<!-- The algorithm computes the rating of every transaction in the subgraph. These ratings will be subsequently transformed into weights during the weighted random walk to bias the walker's path. -->

評価計算は一度だけ実行され、2つのチップトランザクションを選択するために使用されます。
<!-- The rating calculation is performed only once, and used to select both tip transactions. -->

#### インターフェース
<!-- #### Interface -->

さまざまな評価アルゴリズムを作成できるように、すべての評価計算機が従うべき汎用インターフェースを作成しました。
<!-- To allow you to create different rating algorithms, we created a generic interface to which every rating calculator should adhere. -->

```java
Map<TxId -> Integer> calculate(TxId entryPoint)
```

`entryPoint`トランザクションで呼び出されるすべての評価計算機は、トランザクションIDとそれに対応する評価値を整数で表したマッピングを返します。
<!-- Every rating calculator, being invoked with an `entryPoint` transaction, should return a mapping of transaction IDs with their corresponding rating value expressed as integers. -->

#### 未来集合作成
<!-- #### Future set creation -->

ソートされた部分グラフに含まれるすべてのトランザクションに対して、直接承認トランザクションと間接承認トランザクションを含む未来集合が作成されます。各トランザクションの評価は、`未来集合 + 1（トランザクションの自重）`のサイズです。
<!-- For every transaction included in our sorted subgraph, a future set is created, containing direct and indirect approvers. The rating of each transaction is the size of its future set + 1 (the transaction's own weight). -->

```java
entryPoint = latestSolidMilestone - depth

entryPointTrunk = entryPoint

entryPointBranch = reference or entry point

ratings = CumulativeWeightCalculator.calculate(entryPointTrunk)

class CumulativeWeightCalculator(RatingCalculator):

    def calculate(startTx):

        rating = dict()

        subgraph = Tangle(startTx)

        topologicalSubgraph = sortTopologically(subgraph)

        for tx in topologicalSubgraph:

            rating[tx] = len(futureSet(tx)) + 1

        return rating

```

#### 最適化
<!-- #### Optimizations -->

* トランザクションの識別子を格納しながらスペースを確保するために、トランザクションのハッシュバイトの一部のみを格納し、それを`PREFIX_LENGTH`の長さに切り捨てます。現在、この値は44バイトにハードコードされています。これは220トリットに相当します。
<!-- * In order to preserve space while storing transaction's identifiers, we only store a portion of the transaction's hash bytes, truncating it to the `PREFIX_LENGTH` length. Currently, this value has been hardcoded to 44 bytes, corresponding to 220 trits. -->

* アルゴリズムのメモリ消費量を制限するために、評価スコアが高くなってもウォーカーの偏りが大きくなることはないという仮定の下、検討中のトランザクションに対して最大`MAX_FUTURE_SET_SIZE`個の承認トランザクションを保存できます。この値は、ヒューリスティクスに5000にハードコードされています。この最適化は、トランザクションの未来集合が`MAX_FUTURE_SET_SIZE`に制限されている可能性が高いため、実行時のメモリ使用量を制限しながら、考慮される部分グラフの始めにより近いところでランダムに振る舞うようにしています。しかし、望ましい動作は逆です。ウォークの始まりでは、本ブランチに向かって強くランダムに偏りながらも、チップに近づくほどよりランダムになって、多くのチップが選択される機会を広げることが理想です。
<!-- * In order to cap the memory consumption of the algorithm, we allow to store up to `MAX_FUTURE_SET_SIZE` number of approvers for the transaction we are considering, under the assumption that a higher rating score won't contribute significantly to bias the walker. This value has been heuristically hardcoded to 5000. Please note that this optimization, while capping memory usage during runtime, makes the walk to behave more randomly closer the beginning of the considered subgraph since the future sets of those transactions are more likely to have been capped to `MAX_FUTURE_SET_SIZE`. The desired behavior is instead the contrary: we would like the beginning of the walk to be strongly biased towards the main branch while being more random closer to the tips, spreading the chance for any of them to get selected. -->

### 重み付きランダムウォーク
<!-- ### Weighted random walk -->

トランザクションの評価が計算された後、重み付きランダムウォークが始まります。
<!-- After the transactions' ratings have been computed, the weighted random walk starts. -->

#### インターフェース
<!-- #### Interface -->

さまざまな重み付きランダムウォークアルゴリズムを作成できるように、すべての実装が従うべき汎用インターフェイスを作成しました。
<!-- To allow you to create different weighted random walk algorithms, we created a generic interface to which every implementation should adhere. -->

```java
TxId walk(TxId entryPoint, Map<TxId -> Integer> ratings, WalkValidator validator)
```

この関数は選択されたチップトランザクションを返します。
<!-- This function should return the selected tip transaction. -->

#### WalkerAlphaの実装
<!-- #### WalkerAlpha Implementation -->

評価は正規化され、`alpha`設定オプションを使用して`荷重`に変換されます。最後に、0とすべての`荷重`の合計の間の`ランダム`な値が生成され、0の値に達するまで承認トランザクションの荷重によって減算されます。`ランダム`な値を0に変えた承認トランザクションが、ウォークの次のステップとして選択されます。
<!-- Ratings are normalized and transformed into `weights` with the help of the `alpha` configuration option. Finally, a `random` value between 0 and the sum of all the weights is generated and subtracted by the approvers' weights until reaching the value of 0. The approver that turned the `random` value to 0 is selected as the next step in the walk. -->

```python
class WalkerAlpha(Walker):

    def walk(entryPoint, ratings, validator):

        step = entryPoint

        prevStep = None

        while step:

            approvers = getApprovers(step)

            prevStep = step

            step = nextStep(ratings, approvers, validator)

        # When there are no more steps, this transaction is a tip

        return prevStep



    def nextStep(ratings, approvers, validator):

        approversWithRating = approvers.filter(a => ratings.contains(a))

        # There is no valid approver, this transaction is a tip

        if len(approversWithRating) == 0:

            return None

        approversRatings = approverswithRating.map(a => ratings.get(a))

        weights = ratingsToWeights(approversRatings)

        approver = weightedChoice(approversWithRating, weights)

        if approver is not None:

            tail = validator.findTail(approver)

            # If the selected approver is invalid, step back and try again

            if validator.isInvalid(tail):

                approvers = approvers.remove(approver)

                return nextStep(ratings, approvers, validator)

            return tail

        return None



    def weightedChoice(approvers, weights):

        randomNumber = random(0, sum(weights))

        for approver in approvers:

            randomNumber = randomNumber - weights.get(approver)

            if randomNumber <= 0:

                return approver

    def ratingsToWeights(ratings):

        highestRating = max(ratings)

        normalizedRatings = ratings.map(r => r - highestRating)

        weights = normalizedRatings.map(r => math.exp(r * alpha))

        return weights

```
#### バリデータの条件
<!-- #### Validator conditions -->

次のいずれかが発生した場合、トランザクションは無効と見なされます。
<!-- A transaction is considered invalid if any of the following occur: -->

* トランザクションが参照するタングルの一部が未知であるとき、トランザクションは[ソリッド](root://iota-basics/0.1/references/glossary.md#solid)ではなくなる為、トランザクションの状態を再構築することができない場合。
<!-- * It is not solid and we cannot reconstruct its state, since a portion of the Tangle that this transaction references is unknown. -->

* トランザクションが過去の遠すぎるトランザクション、つまり`latestSolidMilestone - maxDepth`を超えるトランザクションを参照している場合。
<!-- * It references a transaction that's too far in the past, namely beyond `latestSolidMilestone - maxDepth`. -->

* 台帳の状態が、不足している資金の取り出しや預け入れ、二重支出など、一貫性がない場合。
<!-- * The ledger state is not consistent, such as trying to withdraw or deposit missing funds or double-spending. -->

* バリデータは有効性がチェックされたトランザクションのリストを管理します。新しいトランザクションが検証されるたびに、新しいトランザクションもチェックリストに対してチェックされます。
<!-- * The validator maintains a list of transactions that have been checked for validity. Every time a new transaction is validated, it is also checked against these. -->

```python
class WalkValidator:

    previousTransactions = []

    def isInvalid(transaction):

        previousTransactions.append(transaction)

        if notSolid(transaction):

            return True

        if belowMaxDepth(transaction):

            return True

        if inconsistent(transaction):

            return True

        if inconsistentWithPreviousTransactions(transaction):

            return True

        return False

```

:::info:
2つのウォークに同じバリデータオブジェクトが渡されるため、互いに一貫性のある2つのチップトランザクションが生成されます。
:::
<!-- :::info: -->
<!-- The same validator object is passed for both walks, resulting in two tip transactions that are consistent with each other. -->
<!-- ::: -->

#### バンドルと一貫性
<!-- #### Bundles and consistency -->

IOTAのトランザクションはバンドルにまとめて送信されます。したがって、ウォーカーが承認トランザクションを辿るとき、承認トランザクションがバンドルの途中にいる可能性もあります。故にバンドルを検証するには、ウォーカーはトランクトランザクションを辿ることによって末尾トランザクションを見つけます。
<!-- Transactions in IOTA are sent in bundles. Therefore, when the walker traverses an approver, that approver may be in the middle of a bundle. To validate the bundle, the walker finds the tail transaction by traversing the trunk transactions. -->

2つのチップトランザクションは、どちらも無効ではないことを確認するために、互いの間の一貫性についてチェックします。したがって、クライアントのトランザクションは、他のトランザクションによって承認される可能性が高い2つの有効なトランザクションを参照するため、累積荷重が大きくなります。
<!-- The two tip transactions are checked for consistency between each other to make sure that neither one is invalid. Therefore, the clients transaction references two valid transactions that have a better chance of being approved by another transaction, thus increasing its cumulative weight. -->
