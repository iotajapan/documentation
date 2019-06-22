# コーディネーター
<!-- # The Coordinator -->

**コーディネーターは、同じアドレスから定期的にトランザクションのバンドルを作成、署名、および送信するクライアントアプリケーションです。これらのバンドルには、ノードが合意に達するために使用するマイルストーンと呼ばれるトランザクションが含まれています。マイルストーンがタングル内のトランザクションを直接または間接的に参照して承認すると、ノードはそのトランザクションの状態とその履歴全体を確定済みとして記録します。**
<!-- **The Coordinator is a client application that creates, signs, and sends bundles of transactions from the same address at regular intervals. These bundles contain transactions called milestones that nodes use to reach a consensus. When milestones directly or indirectly reference and approve a transaction in the Tangle, nodes mark the state of that transaction and its entire history as confirmed.** -->

## マイルストーン
<!-- ## Milestones -->

コーディネーターは定期的にマイルストーンをノードに送信します。ノードは合意に達するためにこれらのマイルストーンを使用します。
<!-- The Coordinator sends milestones to nodes at regular intervals. Nodes use these milestones to reach a consensus. -->

どのトランザクションがマイルストーンであるかを判断するために、同じIOTAネットワーク内のすべてのノードはコーディネーターのアドレスを知っています。
<!-- To determine which transactions are milestones, all nodes in the same IOTA network know the address of the Coordinator. -->

ノードがコーディネーターのアドレスから送信されたトランザクションを確認したら、次の手順を実行してそれを検証します。
<!-- When nodes see a transaction that's been sent from the Coordinator's address, they validate it by doing the following: -->

* 二重支払いにつながらないことを確かめる
<!-- * Make sure that it doesn't lead to a double-spend -->
* 署名を確認する
<!-- * Verify its signature -->

IOTAはWinternitzワンタイム署名方式（W-OTS）を使用するため、秘密鍵は1つのバンドルにのみサインする必要があります。コーディネーターが複数のバンドルに署名しつつ、かつ署名が1つのアドレスに対して検証できるようにするために、コーディネーターのアドレスは下記のコーディネーターのマークル木から取得されます。
<!-- Because IOTA uses the Winternitz one-time signature scheme (W-OTS), a private key should sign only one bundle. To allow the Coordinator to sign multiple bundles whose signatures can still be verified against one address, that address is derived from the Coordinator's Merkle tree. -->

### コーディネーターのマークル木
<!-- ### The Coordinator's Merkle tree -->

マークル木は、リーフのデータをハッシュすることから始まり、マークルルート（コーディネーターのアドレス）で終わるデータ構造です。
<!-- A Merkle tree is a data structure that starts by hashing data at the leaves and ends at the Merkle root (the Coordinator's address). -->

![Example Merkle tree](../images/merkle-tree-example.png)

コーディネーターは、このマークル木の各リーフに対して、1つの署名付きバンドルを署名して送信できます。
<!-- The Coordinator can sign and send one signed bundle for each leaf in its Merkle tree. -->

上の例では、4つのリーフがあり、それぞれがコーディネーターの公開鍵と秘密鍵のペアの1つを表します。これらの鍵ペアは事前に作成され、コーディネーターのアドレスを計算するために使用されます。マークル木内の鍵ペアの総数は、数式:2<sup>depth</sup>の`depth`によって異なります。上の例では、マークル木の深さは2です。
<!-- In this example, we have four leaves, which each represent one of the Coordinator's public/private key pairs. These key pairs are created in advance and used to calculate the the Coordinator's address. The total number of key pairs in a Merkle tree depends on its depth in this formula: 2<sup>depth</sup>. In this example, the Merkle tree's depth is 2. -->

:::info:
Mainnetでは、コーディネーターのマークル木の深さは23です。したがって、コーディネーターの公開鍵と秘密鍵のペアは8,388,608で、同じ数のマイルストーンを送信できます。
:::
<!-- :::info: -->
<!-- On the Mainnet, the Coordinator's Merkle tree has a depth of 23. So, the Coordinator has 8,388,608 public/private key pairs and can send the same number of milestones. -->
<!-- ::: -->

コーディネーターのアドレスを計算するために、公開鍵はペアでハッシュ化されます。
<!-- To calculate the Coordinator's address, the public keys are hashed in pairs: -->

* **ノード1：** Hash(Hash(リーフ1の公開鍵) Hash(リーフ2の公開鍵))
<!-- * **Node 1:** Hash(Hash(public key of leaf 1) Hash(public key of leaf 2)) -->
* **ノード2：** Hash(Hash(リーフ3の公開鍵) Hash(リーフ4の公開鍵))
<!-- * **Node 2:** Hash(Hash(public key of leaf 3) Hash(public key of leaf 4)) -->
* **コーディネーターのアドレス：** Hash(Hash(ノード1) Hash(ノード2))
<!-- * **Coordinator's address:** Hash(Hash(node 1) Hash(node 2)) -->

ノード1は、リーフ1の公開鍵とリーフ2の公開鍵の両方をハッシュ化した結果のハッシュ値です。ノード2は、リーフ3の公開鍵とリーフ4の公開鍵の両方をハッシュ化した結果のハッシュ値です。コーディネーターのアドレスは、ノード1とノード2のハッシュ値をハッシュ化した結果のハッシュ値です。
<!-- Node 1 is a hash of the result of hashing both the public key of leaf 1 and the public key of leaf 2. Node 2 is a hash of the result of hashing both the public key of leaf 3 and the public key of leaf 4. The Coordinator's address is a hash of the result of hashing the hash of node 1 and node 2. -->

:::info:
コーディネータの秘密鍵は、シード、インデックス、およびセキュリティレベルから派生します。

Mainnetでは、これらの秘密鍵はセキュリティレベル2です。結果として、マイルストーン署名は1つのトランザクションに収まるには大きすぎるため、2つに分割する必要があります。

[秘密鍵の導出方法](root://iota-basics/0.1/concepts/addresses-and-signatures.md)を学ぶ。
:::
<!-- :::info: -->
<!-- The Coordinator's private keys are derived from a seed, an index, and a security level. -->
<!--  -->
<!-- On the Mainnet, these private keys are security level 2. As a result, the milestone signature is too large to fit in one transaction and must be fragmented across two. -->
<!--  -->
<!-- [Learn more about how private keys are derived](root://iota-basics/0.1/concepts/addresses-and-signatures.md). -->
<!-- ::: -->

### ノードがマイルストーンを検証する方法
<!-- ### How nodes verify a milestone -->

マイルストーンを検証するには、ノードはマークル木を再構築してマークルルートを見つける必要があります。再構築されたマークルルートがコーディネーターのアドレスと同じ場合、ノードはマイルストーンがコーディネーターによって送信されたことを確認できます。
<!-- To verify a milestone, nodes must rebuild the Merkle tree to find the Merkle root. If the rebuilt Merkle root is the same as the Coordinator's address, nodes know the milestone was sent by the Coordinator. -->

ノードがマークル木を再構築できるようにするために、コーディネーターはバンドル内に次のマイルストーントランザクションを送信します。
<!-- To allow nodes to rebuild the Merkle tree, the Coordinator sends the following milestone transactions in the bundle: -->

* 断片化された署名を含む2つのトランザクション
<!-- * Two transactions that contain the fragmented signature -->
* [`signatureMessageFragment`](root://iota-basics/0.1/references/structure-of-a-transaction.md)フィールドに、それを再構築できるようにするためにマークル木から足りないデータが含まれている1つのトランザクション
* One transaction whose [`signatureMessageFragment`](root://iota-basics/0.1/references/structure-of-a-transaction.md) field contains enough missing data from the Merkle tree to be able to rebuild it

![Example Merkle tree](../images/merkle-tree-example.png)

たとえば、ノードとして、リーフ1の秘密鍵で署名されたバンドルを見ました。
<!-- For example, as a node, we have seen a bundle that was signed with the private key of leaf 1. -->

まず、署名を検証してリーフ1の公開鍵を見つけます。
<!-- First, we verify the signature to find out the public key of leaf 1. -->

:::info:
[ノードによる署名の検証方法](root://iota-basics/0.1/concepts/addresses-and-signatures.md)を学ぶ。
:::
<!-- :::info: -->
<!-- [Learn how nodes verify signatures](root://iota-basics/0.1/concepts/addresses-and-signatures.md#how-nodes-verify-signatures) -->
<!-- ::: -->

マークルルートを計算するために、バンドルの3番目のマイルストーンには次のものが含まれています。
<!-- To help us calculate the Merkle root, the third milestone in the bundle contains the following: -->

* リーフ2の公開鍵
<!-- * The public key of leaf 2 -->
* ノード2のハッシュ値
<!-- * The hash of node 2 -->

ここで、リーフ1と2の公開鍵をハッシュ化してノード1のハッシュ値を見つけます。次に、ノード1と2のハッシュ値をハッシュ化してマークルルートを見つけます。
<!-- Now, we hash the public keys of leaves 1 and 2 to find the hash of node 1. Then we hash the hash of nodes 1 and 2 to find the Merkle root. -->

マークルルートがコーディネータのアドレスと同じ場合、バンドルはコーディネーターのマークル木の秘密鍵の1つで署名されています。
<!-- If the Merkle root is the same as the Coordinator's address, the bundle was signed with one of the private keys in the Coordinator's Merkle tree. -->

:::info:独自のコーディネーターを走らせたいですか？
[コンパス](root://compass/0.1/how-to-guides/create-an-iota-network)を使用して、独自のIOTAネットワークでマイルストーンを作成、署名、および送信する。
:::
<!-- :::info:Want to run your own Coordinator? -->
<!-- Use Compass to create, sign, and send milestones in your own IOTA network. -->
<!-- ::: -->
