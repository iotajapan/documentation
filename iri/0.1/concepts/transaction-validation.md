# トランザクション検証
<!-- # Transaction validation -->

**IOTAネットワーク内の各IRIノードは、偽造トランザクションが決して確定されないようにトランザクションを検証する責任があります。**
<!-- **Each IRI node in an IOTA network is responsible for validating transactions to make sure that counterfeit transactions are never confirmed.** -->

台帳の整合性を保護するために、IRIノードは有効なトランザクションのみをそれぞれの[台帳](../concepts/the-ledger.md)に追加します。
<!-- To protect the integrity of the ledger, IRI nodes append only valid transaction to their [ledgers](../concepts/the-ledger.md). -->

IRIノードは次の段階でトランザクションを検証します。
<!-- IRI nodes validate transactions during the following stages: -->
- 新規トランザクションの受け取り時
<!-- - On receipt of new transactions -->
- チップ選択プロセスの実行中
<!-- - During the tip selection process -->

## 新規トランザクションの受け取り時のトランザクション検証
<!-- ## Transaction validation on receipt of new transactions -->

IRIノードは、クライアントと隣接ノードの両方から新しいトランザクションを受信します。
<!-- IRI nodes receive new transactions from both clients and neighbor nodes. -->

IRIノードが新しいトランザクションを受信すると、トランザクションバリデータはトランザクションの以下のことをチェックします。
<!-- When an IRI node receives a new transaction, the transaction validator checks it for the following: -->

- プルーフオブワークが完了している。
<!-- - The proof of work was done -->
- バンドル内のすべてのトランザクションのIOTAトークンの値が、IOTAトークンの総供給量の値を超えていない。
<!-- - The value of any transaction in the bundle doesn’t exceed the total global supply -->
- トランザクションが最新のスナップショットよりも古くなく、ノードの現在時刻から2時間以上遅れていない。
<!-- - The transaction is not older than the last snapshot and not newer than two hours ahead of the node’s current time -->
- IOTAトークントランザクションの場合、アドレスの最後のトリットが0である。
<!-- - The last trit of an address is 0 for value transactions -->

## チップ選択プロセス中の検証
<!-- ## Validation during the tip selection process -->

クライアントがチップトランザクションをIRIノードに要求すると、[チップ選択](root://the-tangle/0.1/concepts/tip-selection.md)が行われます。
<!-- When clients ask an IRI node for tip transactions, it does the [tip selection](root://the-tangle/0.1/concepts/tip-selection.md). -->

チップ選択プロセス中にIRIノードが辿る各トランザクションのバンドルは、バンドルバリデータと台帳バリデータによってチェックされます。
<!-- The bundles of each transaction that the IRI node traverses during the tip selection process are checked by the bundle validator and the ledger validator. -->

### バンドルバリデータ
<!-- ### Bundle validator -->

バンドルバリデータは、バンドル内のすべてのトランザクションが有効であることを確認します。
<!-- The bundle validator makes sure that all transactions in a bundle are valid. -->

[重み付きランダムウォーク](root://the-tangle/0.1/concepts/tip-selection.md)中に、バンドルバリデータは次のものについてトランザクションのバンドルをチェックします。
<!-- During a [weighted random walk](root://the-tangle/0.1/concepts/tip-selection.md), the bundle validator checks the bundle of transactions for the following: -->

- バンドル内のすべてのトランザクションのIOTAトークンの値が、IOTAトークンの総供給量の値を超えていない。
<!-- - The value of any transaction in the bundle doesn’t exceed the total global supply -->
- バンドル内のすべてのトランザクションのIOTAトークンの値を合計すると0になる。
<!-- - The total value of all transactions in the bundle is 0 (all IOTA tokens that are withdrawn are also deposited into other addresses) -->
- IOTAトークン移転用のトランザクションの署名がすべて有効である。
<!-- - Any signatures in value transactions are valid -->

### 台帳バリデータ
<!-- ### Ledger validator -->

台帳バリデータは、二重支払いが確定されないようにします。
<!-- The ledger validator makes sure that double-spends are never confirmed. -->

[重み付きランダムウォーク](root://the-tangle/0.1/concepts/tip-selection.md)中に、台帳バリデータは、バンドル内のすべてのアドレスの値をチェックすることによって、各バンドルが二重支払いにつながらないことをチェックします。二重支払いが見つかった場合、重み付きランダムウォークは1つのトランザクションを遡り、チップトランザクションへの別の経路を見つけます。
<!-- During a [weighted random walk](root://the-tangle/0.1/concepts/tip-selection.md), the ledger validator checks that each bundle does not lead to a double-spend by checking the values of all addresses in a bundle. If a double-spend is found, the weighted random walk steps back one transaction and finds another route to a tip transaction. -->
