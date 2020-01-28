# チップ選択アルゴリズム
<!-- # The tip selection algorithm -->

**以下の情報は、クライアントが [getTransactionsToApprove](root://node-software/0.1/iri/references/api-reference.md#getTransactionsToApprove) エンドポイントを呼び出したときの IRI の動作を説明しています。**
<!-- **The following information describes what IRI does when a client calls the [getTransactionsToApprove](root://node-software/0.1/iri/references/api-reference.md#getTransactionsToApprove) endpoint.** -->

このエンドポイントが呼び出されると、ノードはチップ選択アルゴリズムを開始します。これは次のステージに分かれています。
<!-- When this endpoint is called, a node starts the tip selection algorithm, which is separated into the following stages: -->

1. 準備
<!-- 1. Preparation -->
2. 評価計算
<!-- 2. Rating calculation -->
3. 重み付きランダムウォーク
<!-- 3. Weighted random walk -->

### 準備
<!-- ### Preparation -->

アルゴリズムの目的は、API 呼び出しの成功結果として、競合しない2つのチップトランザクションを返すことです。
<!-- The algorithm's goal is to return two non-conflicting tip transactions as a successful result of the API call. -->

アルゴリズムは台帳の部分グラフを選択し、それを通る2つの重み付きランダムウォークを行います。各重み付きランダムウォークはチップトランザクションハッシュを返します。
<!-- The algorithm selects a subgraph of the ledger and does two weighted random walks through it. Each weighted random walk returns a tip transaction hash. -->

2つの[重み付きランダムウォーク](#重み付きランダムウォーク)は、同じマイルストーントランザクションから開始されます（`latestSolidMilestone` - `depth`）。
<!-- Both [weighted random walks](#weighted-random-walk) start from the same milestone transaction (`latestSolidMilestone - depth`). -->

クライアントが API 呼び出しに `reference` 引数を指定した場合、そのトランザクションが部分グラフ内にある場合にのみ、`branchTransaction` ウォークは `reference` 引数内のトランザクションから開始されます。
<!-- If the client specifies a `reference` argument to the API call, the `branchTransaction` walk will start from the transaction in the `reference` argument only if that transaction is in the subgraph. -->

`reference` 引数のトランザクションが `depth` マイルストーンインデックスよりも古い場合、API 呼び出しは失敗し、次のエラーメッセージが表示されます。：`Reference transaction is too old.`
<!-- If the transaction in the `reference` argument is older than the `depth` milestone index, the API call fails with the following error message: Reference transaction is too old. -->

### 評価計算
<!-- ### Rating calculation -->

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

`entryPoint` トランザクションで呼び出されるすべての評価計算機は、トランザクション ID とそれに対応する評価値を整数で表したマッピングを返します。
<!-- Every rating calculator, being invoked with an `entryPoint` transaction, should return a mapping of transaction IDs with their corresponding rating value expressed as integers. -->

#### フューチャーセット作成
<!-- #### Future set creation -->

ソートされた部分グラフに含まれるすべてのトランザクションに対して、直接承認トランザクションと間接承認トランザクションを含むフューチャーセットが作成されます。各トランザクションの評価は、`フューチャーセット + 1（トランザクションの自重）`のサイズです。
<!-- For every transaction included in our sorted subgraph, a future set is created, containing direct and indirect approvers. The rating of each transaction is the size of its future set + 1 (the transaction's own weight). -->

```python
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

- トランザクションの識別子を格納しながらスペースを確保するために、トランザクションのハッシュバイトの一部のみを格納し、それを `PREFIX_LENGTH` の長さに切り捨てます。現在、この値は44バイトにハードコードされています。これは220トリットに相当します。
<!-- - In order to preserve space while storing transaction's identifiers, we only store a portion of the transaction's hash bytes, truncating it to the `PREFIX_LENGTH` length. Currently, this value has been hardcoded to 44 bytes, corresponding to 220 trits. -->

- アルゴリズムのメモリ消費量を制限するために、評価スコアが高くなってもウォーカーの偏りが大きくなることはないという仮定の下、検討中のトランザクションに対して最大 `MAX_FUTURE_SET_SIZE` 個の承認トランザクションを保存できます。この値は、ヒューリスティクスに5000にハードコードされています。この最適化は、トランザクションの未来集合が `MAX_FUTURE_SET_SIZE` に制限されている可能性が高いため、実行時のメモリ使用量を制限しながら、考慮される部分グラフの始めにより近いところでランダムに振る舞うようにしています。しかし、望ましい動作は逆です。ウォークの始まりでは、本ブランチに向かって強くランダムに偏りながらも、チップに近づくほどよりランダムになって、多くのチップが選択される機会を広げることが理想です。
<!-- - In order to cap the memory consumption of the algorithm, we allow to store up to `MAX_FUTURE_SET_SIZE` number of approvers for the transaction we are considering, under the assumption that a higher rating score won't contribute significantly to bias the walker. This value has been heuristically hardcoded to 5000. Please note that this optimization, while capping memory usage during runtime, makes the walk to behave more randomly closer the beginning of the considered subgraph since the future sets of those transactions are more likely to have been capped to `MAX_FUTURE_SET_SIZE`. The desired behavior is instead the contrary: we would like the beginning of the walk to be strongly biased towards the main branch while being more random closer to the tips, spreading the chance for any of them to get selected.  -->

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

#### WalkerAlpha の実装
<!-- #### WalkerAlpha Implementation -->

評価は正規化され、`alpha` 構成オプションを使用して`荷重`に変換されます。最後に、0とすべての`荷重`の合計の間の`ランダム`な値が生成され、0の値に達するまで承認トランザクションの荷重によって減算されます。`ランダム`な値を0に変えた承認トランザクションが、ウォークの次のステップとして選択されます。
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

- トランザクションが参照するタングルの一部が未知であるため、トランザクションが凝固ではなく、トランザクションのステートを再構築することができない場合。
<!-- - It is not solid and we cannot reconstruct its state, since a portion of the Tangle that this transaction references is unknown. -->

- トランザクションが過去の遠すぎるトランザクション、つまり `latestSolidMilestone - maxDepth` を超えるトランザクションを参照している場合。
<!-- - It references a transaction that's too far in the past, namely beyond `latestSolidMilestone - maxDepth`. -->

- 台帳のステートが、不足している資金の取り出しや預け入れ、二重支出など、一貫性がない場合。
<!-- - The ledger state is not consistent, such as trying to withdraw or deposit missing funds or double-spending. -->

- バリデータは有効性がチェックされたトランザクションのリストを管理します。新しいトランザクションが検証されるたびに、新しいトランザクションもチェックリストに対してチェックされます。
<!-- - The validator maintains a list of transactions that have been checked for validity. Every time a new transaction is validated, it is also checked against these. -->

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
両方のウォークで同じバリデーターオブジェクトが渡されるため、相互に一貫性のある2つのチップトランザクションが発生します。
:::
<!-- :::info: -->
<!-- The same validator object is passed for both walks, resulting in two tip transactions that are consistent with each other. -->
<!-- ::: -->
