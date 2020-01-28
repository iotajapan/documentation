# IRI ノードでのトランザクション検証
<!-- # Transaction validation in IRI nodes -->

**IRI ノードは、次の2つの段階でトランザクションを検証します。**
<!-- **IRI nodes validate transactions during the following stages: On receipt of new transactions and during the tip selection process** -->
- **新規トランザクションを受信した時**
<!-- - On receipt of new transactions -->
- **チップ選択プロセスの実行中**
<!-- - During the tip selection process -->

## 新しいトランザクション
<!-- ## New transactions -->

IRI ノードが新しいトランザクションを受信すると、トランザクションバリデータはトランザクションの以下のことをチェックします。
<!-- When an IRI node receives a new transaction, the transaction validator checks it for the following: -->

- プルーフオブワークが完了している。
<!-- - The proof of work was done -->
- バンドル内のすべてのトランザクションの IOTA トークンの量が、IOTA トークンの総供給量を超えていない。
<!-- - The value of any transaction in the bundle doesn’t exceed the total global supply -->
- トランザクションが最新のスナップショットよりも古くなく、ノードの現在時刻から2時間以上遅れていない。
<!-- - The transaction is not older than the last snapshot and not newer than two hours ahead of the node’s current time -->
- IOTA トークン移転用のトランザクションの場合、アドレスの最後のトリットが0である。
<!-- - The last trit of an address is 0 for value transactions -->

## チップ選択
<!-- ## Tip selection -->

チップ選択プロセス中に IRI ノードが辿る各トランザクションのバンドルは、バンドルバリデータと台帳バリデータによってチェックされます。
<!-- The bundles of each transaction that the IRI node traverses during the tip selection process are checked by the bundle validator and the ledger validator. -->

### バンドルバリデータ
<!-- ### Bundle validator -->

バンドルバリデータは、バンドル内のすべてのトランザクションが有効であることを確認します。
<!-- The bundle validator makes sure that all transactions in a bundle are valid. -->

[重み付きランダムウォーク](../concepts/tip-selection.md)中に、バンドルバリデータは次のことをチェックします。
<!-- During a [weighted random walk](../concepts/tip-selection.md), the bundle validator checks the bundle of transactions for the following: -->

- バンドル内のすべてのトランザクションの IOTA トークンの量が、IOTA トークンの総供給量を超えていない。
<!-- - The value of any transaction in the bundle doesn’t exceed the total global supply -->
- バンドル内のすべてのトランザクションの IOTA トークンの値を合計すると0になる。
<!-- - The total value of all transactions in the bundle is 0 (all IOTA tokens that are withdrawn are also deposited into other addresses) -->
- IOTA トークン移転用のトランザクションの署名がすべて有効である。
<!-- - Any signatures in value transactions are valid -->

### 台帳バリデータ
<!-- ### Ledger validator -->

台帳バリデータは、二重支払いが確定されないようにします。
<!-- The ledger validator makes sure that double spends are never confirmed. -->

[重み付きランダムウォーク](../concepts/tip-selection.md)中に、台帳バリデータは、バンドル内のすべてのアドレスの IOTA トークンの量をチェックすることによって、各バンドルが二重支払いにつながらないことをチェックします。二重支払いが見つかった場合、重み付きランダムウォークは1つトランザクションを遡り、チップトランザクションへの別の経路を見つけます。
<!-- During a [weighted random walk](../concepts/tip-selection.md), the ledger validator checks that each bundle does not lead to a double-spend by checking the values of all addresses in a bundle. If a double spend is found, the weighted random walk steps back one transaction and finds another route to a tip transaction. -->
