# IOTAの基本概要
<!-- # IOTA basics overview -->

**IOTAネットワークは、ノードとクライアントで構成されています。ノードは、トランザクションの台帳への読み取り/書き込みのアクセス権を持つデバイスです。クライアントはシードを持つデバイスです。シードにより、クライアントは[アドレス](../concepts/addresses-and-signatures.md)にアクセスできます。アドレスには残高があり、残高がアドレスの中のIOTAトークンの量を定義します。 IOTAトークンを自分のアドレスから取り出すには、クライアントがトランザクションのバンドルをノードに送信して、ノードがトランザクションを検証して台帳を更新できるようにする必要があります。**
<!-- **An IOTA network consists of nodes and clients. A node is a device that has read/write access to a ledger of transactions. A client is a device that has a seed. A seed gives a client access to [addresses](../concepts/addresses-and-signatures.md). Addresses have a balance, which defines the amount of IOTA tokens in them. To withdraw IOTA tokens from their addresses, clients must send bundles of transactions to a node so that the nodes can validate the transactions and update their ledgers.** -->

## クライアント
<!-- ## Clients -->

口座番号と分類コードのように、IOTAのアドレスは、各シードごとの固有の81文字（[トライト](../concepts/trinary.md)）の文字列です。
<!-- Like an account number and sort code, an address in IOTA is a unique string of 81 characters ([trytes](../concepts/trinary.md)) that are unique to each seed. -->

:::info:
時にアドレスは90文字です。余分な9文字はチェックサムと呼ばれ、アドレスが正しいことを確認するのに役立ちます。
:::
<!-- :::info: -->
<!-- Sometimes addresses have 90 characters. The extra 9 characters are called the checksum, which helps you make sure your address is correct. -->
<!-- ::: -->

ネットワーク上のクライアントは、[トランザクションを含むバンドル](../concepts/bundles-and-transactions.md)内の互いのアドレスに、互いのデータまたはIOTAトークンを送信します。
<!-- Clients on the network send each other data or IOTA tokens to each other's addresses in [bundles, which contain transactions](../concepts/bundles-and-transactions.md). -->

トランザクションは、[IOTAネットワーク内のノードの規則（プロトコル）に従って構造化](../references/structure-of-a-transaction.md)されているオブジェクトです。各トランザクションは、IOTAトークンの取り出し、IOTAトークンの預け入れ、または単にトランザクションの保管をノードに指示する操作です。
<!-- Transactions are objects that are [structured according to the rules (protocol) of the nodes in the IOTA network](../references/structure-of-a-transaction.md). Each transaction is an operation that instructs a node to withdraw IOTA tokens, deposit IOTA tokens, or simply store the transaction. -->

:::info:
IOTA[クライアントライブラリ](root://client-libraries/0.1/introduction/overview.md)には、アドレスやバンドルを作成するためのツールなど、必要なすべてのツールが含まれています。

シードを使用するコードはすべてクライアント側で実行されます。シードがデバイスを離れることはありません。
:::
<!-- :::info: -->
<!-- The IOTA [client libraries](root://client-libraries/0.1/introduction/overview.md) contain all the tools you need, including those to create addresses and bundles. -->
<!--  -->
<!-- Any code that uses a seed is executed on the client side. Your seed never leaves your device. -->
<!-- ::: -->

## ノード
<!-- ## Nodes -->

ノードは郵便局と銀行を合わせたようなものです。ノードはノードが受け取るすべてのトランザクションとネットワーク内のすべてのアドレスのゼロではない残高の台帳を保持します。
<!-- Nodes are like a cross between a post office and a bank. They keep a ledger of every transaction that they receive and the non-zero balances of all addresses in the network. -->

ノードには[API](root://iri/0.1/references/api-reference.md)があります。APIにより、クライアントは台帳からバンドルを読み取り、バンドルを送信できます。
<!-- Nodes have an [API](root://iri/0.1/references/api-reference.md), which allows clients to read from the ledger and send bundles. -->

ノードは一連の規則に従って[トランザクションを検証](root://iri/0.1/concepts/transaction-validation.md)します。そのうちの1つは、IOTAトークンを取り出すトランザクションに有効な[署名](../concepts/addresses-and-signatures.md)が含まれているかを検証することです。トランザクションが有効であると見なされると、ノードはトランザクションを台帳に追加し、影響を受けるアドレスの残高を更新します。
<!-- Nodes [validate transactions](root://iri/0.1/concepts/transaction-validation.md) according to a set of rules, one of which states that withdrawals must contain a valid [signature](../concepts/addresses-and-signatures.md). When a transaction is considered valid, the node adds it to its ledger and updates the balances of the affected addresses. -->

### 信頼性
<!-- ### Trust -->

ノードを本当に信頼して良いかどうか疑問に思うかもしれません。結局のところ、ノードに接続することがタングルから読み書きする唯一の方法です。ノードが[`getBalances`](root://iri/0.1/references/api-reference.md#getBalances)などのAPIエンドポイントへの応答を改ざんするとどうなるのでしょうか？自分の本当の残高がどのようなものであるかをどのようにして知るのでしょうか？
<!-- You might be wondering how you can trust a node. After all, connecting to a node is the only way to read from and write to the Tangle. What if a node were to change the response to an API endpoint such as [`getBalances`](root://iri/0.1/references/api-reference.md#getBalances)? How would you know what your real balance is? -->

IOTAは[分散台帳技術](root://getting-started/0.1/introduction/what-is-dlt.md)です。分散という言葉が重要です。ノードがトランザクションを受け取り、検証し、台帳に追加しても、そこで停止することはありません。 IOTAプロトコルは、すべてのノードが隣接ノードと呼ばれる他のノードにトランザクションを転送しなければなりません。このようにして、すべてのノードが一貫したトランザクションの分散型台帳を受信、検証、および保存することで、個々を信頼する必要性がなくなります。その結果、あるノードが怪しい場合は、複数のノードにリクエストを送信し、返されたデータから整合性を確認することができます。
<!-- Well, IOTA is a [distributed ledger technology](root://getting-started/0.1/introduction/what-is-dlt.md). The word _distributed_ is the key. When a node receives a transaction, validates it, and appends it to its ledger, it doesn't stop there. The IOTA protocol states that all nodes must forward transactions onto other nodes, called their neighbors. This way, all nodes receive, validate, and store a consistent, distributed ledger of transactions, removing the need to trust any individual. As a result, you can send requests to multiple nodes and check the consistency of the returned data. -->

### 不変性
<!-- ### Immutability -->

ノードがトランザクションを変更できないのはなぜでしょう？
<!-- What stops a node from being able to change a transaction? -->

トランザクションの不変性への最初のステップは、トランザクションを81トライトのハッシュ値に変換することです。このハッシュ値はトランザクションに固有のものです。トランザクションの内容の1文字でも変更されると、ハッシュ値は無効になります。
<!-- The first step to transaction immutability is to hash its contents into 81 trytes. This hash is unique to the transaction. If one character of the transaction's contents were to be changed, the hash would be invalid. -->

次のステップは、`branchTransaction`フィールドと`trunkTransaction`フィールドのトランザクションハッシュを参照して、トランザクション（子）を他の2つ（子の親）に接続することです。その時、子トランザクションの運命はその親にバインドされます。どちらかの親の内容が変更されると、その親のトランザクションのハッシュ値は無効になり、子も無効になります。
<!-- The next step is to connect the transaction (called a child) to two others (called its parents) by referencing their transaction hashes in the `branchTransaction` and `trunkTransaction` fields. Now, the fate of the child transaction is bound to its parent. If the contents of either parents change, their transaction hashes will be invalid, making the child invalid. -->

台帳のこのつながった構造は、[タングル](root://the-tangle/0.1/introduction/overview.md)と呼ばれるもので、チップトランザクションと呼ばれる新しい孤児（親を持たないトランザクション）が、2人の親を参照する必要があるトランザクションハッシュ値のタングル集合です。その結果、トランザクションの子の数が多いほど、関連付けられているトランザクションのハッシュ値が多くなり、不変のものと見なされます。
<!-- This connected structure in the ledger is what's called [the Tangle](root://the-tangle/0.1/introduction/overview.md), a tangled family of transaction hashes where any new orphaned child (with no parents), called a tip transaction, must reference two parents. As a result, the more children a transaction has, the more transaction hashes that are connected to it, and the more immutable it is considered. -->

### スケーラビリティ
<!-- ### Scalability -->

スケーラビリティは、増え続ける作業を処理するためのネットワークの能力のことです。何十億ものIoTデバイスがネットワーク上でやり取りすると予想するIOTAでは、スケーラビリティが不可欠です。
<!-- Scalability is the capability of a network to handle a growing amount of work. In IOTA, where billions of Internet-of-things devices are expected to transact on the network, scalability is essential. -->

すでに、トランザクションがタングルの中での不変性を強化するためにお互いを参照するという手法を見てきました。しかし、タングル全体としても親の選び方は重要です。
<!-- You've already seen that transactions reference each other to strengthen their immutability in the Tangle. But, what's also important about the the Tangle is how parents are chosen. -->

クライアントがトランザクションをノードに送信する前に、そのトランザクションは2つの親を参照する必要があります。トランザクションは、バンドル内の最後のトランザクションまで、常に`trunkTransaction`フィールドで互いを参照します。それでは、`branchTransaction`フィールドやバンドル内の最後のトランザクションの`branchTransaction`フィールドと`trunkTransaction`フィールドについてはどうでしょうか。
<!-- Before a client can send a transaction to a node, that transaction must reference two parents. Transactions, up to the last one in a bundle, will always reference each other in their `trunkTransaction` fields. So, what about the `branchTransaction` field and the `trunkTransaction` and `branchTransaction` fields of the last transaction in the bundle? -->

これらのフィールドの親は、[チップ選択](root://the-tangle/0.1/concepts/tip-selection.md)中にノードによって選択されます。これはノードがある古いトランザクションから開始し、親を持たないトランザクション（チップトランザクション）が見つかるまで、その子、孫などを辿るプロセスです。
<!-- The parents in these fields are chosen by a node during [tip selection](root://the-tangle/0.1/concepts/tip-selection.md). A process where a node starts from an old transaction and traverses its children, grandchildren, and so on, until it finds one without any parents (the selected tip). -->

トランザクションを辿っている間、ノードは[辿っている最中のバンドル全体を検証する](root://iri/0.1/concepts/transaction-validation.md#bundle-validator)必要があります。結果として、ノードはチップトラザクションの履歴を検証し、それらのトランザクションハッシュ値を参照することで、**子はその親のバンドルと子に関連した全体の履歴を承認します**。
<!-- While traversing transactions, the node must [validate their entire bundle](root://iri/0.1/concepts/transaction-validation.md#bundle-validator). As a result, by having the node validate the history of the tip transactions and by referencing their transaction hashes, **a child approves its parents' bundles and their entire history**. -->

すべてのバンドルが2つの新しいバンドルを承認するため、ネットワーク内のバンドルが多いほど、新しいバンドルが早く承認されます。
<!-- Because every bundle approves two new bundles, the more bundles in the network, the faster new ones are approved. -->
