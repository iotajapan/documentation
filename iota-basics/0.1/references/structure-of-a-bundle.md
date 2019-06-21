# バンドルの構造
<!-- # Structure of a bundle -->

**以下の表はバンドルの構造を表しています。**
<!-- **This table displays the structure of a bundle.** -->

先頭トランザクションを除くバンドル内のすべてのトランザクションは、[`trunkTransaction`フィールド](../references/structure-of-a-transaction.md)を介して互いに接続されています。これらの接続により、ノードは同じバンドル内のすべてのトランザクションを見つけて検証することができます。
<!-- All transactions in a bundle, except the head, are connected to each other through the [`trunkTransaction` field](../references/structure-of-a-transaction.md). These connections allow nodes to find all transactions in the same bundle and validate them. -->

![Connections in a bundle](../images/bundle-structure.png)

| トランザクションの現在のインデックス | `trunkTransaction`フィールド | `branchTransaction`フィールド | 説明 |
| :--- | :--- | :--- | :--- |
| 0 | このバンドルのトランザクション1 | 別のバンドルの末尾トランザクションハッシュ。このトランザクションハッシュは[チップ選択プロセス](root://the-tangle/0.1/concepts/tip-selection.md)から返されます。 | このトランザクションは**末尾トランザクション**と呼ばれます。 |
| 1 | このバンドルのトランザクション2 | トランザクション0と同じbranchTransactionハッシュ | このトランザクションは、`trunkTransaction`フィールドを介してトランザクション0にリンクされています。 |
| 2 | このバンドルのトランザクション3 | トランザクション0、1と同じbranchTransactionハッシュ | このトランザクションは、`trunkTransaction`フィールドを介してトランザクション1にリンクされています。 |
| 最後のインデックス | このバンドルの他のすべてのトランザクションと同じbranchTransactionハッシュ | 別のバンドルの末尾トランザクションハッシュ。このトランザクションハッシュは[チップ選択プロセス](root://the-tangle/0.1/concepts/tip-selection.md)から返されます。 | このトランザクションを**先頭トランザクション**と呼びます。バンドルは任意の数のトランザクションで構成できます。 |
