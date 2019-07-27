# IOTAとは？
<!-- # What is IOTA? -->

**IOTAは、IOTAネットワーク内のコンピュータがイミュータブルなデータと価値（IOTAトークン）を相互に転送できるようにする分散型台帳技術です。**
<!-- **IOTA is a distributed ledger technology that allows computers in an IOTA network to transfer immutable data and value among each other.** -->

IOTAは、M2M経済圏における、効率性の向上、生産量の増加、データの信頼性を保証することを目的としています。
<!-- IOTA aims to improve efficiency, increase production, and ensure data integrity in a machine-to-machine economy. -->

<dl><dt>M2M経済圏</dt><dd>人間の介入なしに、任意のコンピュータがデータと価値を他のコンピュータに転送できる経済圏。</dd></dl>
<!-- <dl><dt>machine-to-machine economy</dt><dd>Economy in which any computer can transfer data and value to other computers without human intervention.</dd></dl> -->

IOTAが実際に、サプライチェーンをどのように改善できるかについては[このビデオ](https://www.youtube.com/embed/Gr-LstcDcAw)をご覧ください。
<!-- To see IOTA in action, watch [this video](https://www.youtube.com/embed/Gr-LstcDcAw) about how it can improve supply chains. -->

## IOTAはどのように機能するのか？
<!-- ## How does IOTA work? -->

クライアントは[ノード](../introduction/what-is-a-node.md)を介して互いにデータとIOTAトークンを送信します。
<!-- Clients send data and IOTA tokens to each other through [nodes](../introduction/what-is-a-node.md). -->

IOTAトークンを送受信するために、クライアントは[バンドル](../introduction/what-is-a-bundle.md)と呼ばれる[トランザクション](../introduction/what-is-a-transaction.md)のパッケージをノードに送信します。バンドル内のトランザクションは、あるアドレスから別のアドレスにIOTAトークンを転送するようにノードに指示します。これらのアドレスは、[シード](../introduction/what-is-a-seed.md)と呼ばれるクライアント固有の秘密のパスワードから派生しています。
<!-- To send and receive IOTA tokens, clients send packages of [transactions](../introduction/what-is-a-transaction.md) called [bundles](../introduction/what-is-a-bundle.md) to nodes. The transactions in a bundle instruct the node to transfer IOTA tokens from one address to another. These addresses are derived from a client's unique secret password called a [seed](../introduction/what-is-a-seed.md). -->

バンドルが[タングル](../introduction/what-is-the-tangle.md)内で確定されると、IOTAトークンの転送完了です。
<!-- When the bundle is confirmed in [the Tangle](../introduction/what-is-the-tangle.md), the IOTA tokens are transferred. -->

## IOTAトークンとは？
<!-- ## What is the IOTA token? -->

最も基本的なレベルでは、IOTAトークンはIOTAネットワーク内のノードによって保持されている所有権の記録です。
<!-- At its most basic level, the IOTA token is a record of ownership that's held by the nodes in an IOTA network. -->

    ADDRESS....ENDOFADDRESS;1000

上記の文字列は不可解に見えるかもしれませんが、分解してみましょう。セミコロンの左側にアドレスがあります。これはネットワーク内の各クライアントに固有のものです。セミコロンの右側には、そのアドレスに属するIOTAトークンの量があります。この場合は1,000個のIOTAトークンです。
<!-- These characters might look cryptic, but let's break it down. On the left of the semicolon is an address. These are unique to each client in the network. On the right of the semicolon is an amount of IOTA tokens that belong to that address, in this case 1,000 tokens. -->

IOTAトークンの送信が完了には、すべてのノードがトークンを送信する[トランザクションを検証したとき](root://node-software/0.1/iri/concepts/transaction-validation.md)、およびマイルストーンによって参照されたときだけです。
<!-- You own IOTA tokens only when all nodes [validate the transaction](root://node-software/0.1/iri/concepts/transaction-validation.md) that sent the tokens to you, and when it's referenced by a milestone. -->

## IOTAトークンの価値の源泉とは？
<!-- ## What makes the IOTA token valuable? -->

IOTAトークンは、以下の理由で価値があります。
<!-- The IOTA token is valuable for the following reasons: -->

* **有限：** すべてのノードは最大数2,779,530,283 277,761トークンがネットワークに存在することに同意します。この最大数はネットワークに組み込まれており、変更することはできません。
<!-- * **It's finite:** All nodes agree that a maximum of 2,779,530,283 277,761 tokens exist in the network. This maximum number is built into the network and can't ever be changed. -->
* **便利：** IOTAネットワークで価値を移転するためには、IOTAトークンを使わなければなりません。
<!-- * **It's useful:** To transfer value in an IOTA network, you must use the IOTA token. -->

## IOTAの利点とは？
<!-- ## What are the benefits of IOTA? -->

IOTAは、さまざまなデバイス間でデータを送信したり、価値を転送したりするプロセスを合理化、保護、および自動化できるオープンソース技術です。
<!-- IOTA is an open-source technology that can streamline, secure, and automate any process that sends data or transfers value among different devices. -->

### 信頼性
<!-- ### Trust -->

IOTAネットワーク内の各ノードはトランザクションを検証してから、同じことを行う他のノードにトランザクションを送信します。その結果、すべての有効なトランザクションはすべてのノードによって合意され、ネットワーク内の単一のノードを信頼する必要がなくなります。
<!-- Each node in an IOTA network validates transactions, then sends them to other nodes that do the same. As a result, all valid transactions are agreed on by all nodes, removing the need to trust a single one in the network. -->

### 不変性
<!-- ### Immutability -->

台帳にあるすべてのトランザクションはイミュータブルかつ透明です。
<!-- All transactions in the ledger are immutable and transparent. -->

### 安全性
<!-- ### Security -->

IOTAは、ネットワークを保護し、攻撃者がIOTAトークンを盗むのを防ぐために、量子耐性暗号を使用しています。
<!-- IOTA uses quantum-resistant cryptography to secure the network and prevent attackers from stealing IOTA tokens. -->

IOTAネットワークは、P2Pネットワークです。中央機関がトランザクションの台帳を管理するのではなく、すべてのノードが台帳のコピーを保持し、IOTAプロトコルを含むソフトウェアを実行して、台帳の内容に関する合意を自動化します。
<!-- IOTA networks are peer-to-peer networks. No central authority controls the ledger of transactions, instead all nodes hold a copy and run the software that contains the IOTA protocol to automate the agreement on its contents. -->

### コスト削減
<!-- ### Cost saving -->

IOTAは無料で利用でき、購読料を支払う必要も、契約にサインする必要もありません。トランザクションも自由に送れます。
<!-- IOTA is free to use. You don't need to pay a subscription, or sign a contract. Even transactions are free to send. -->

### スケーラビリティ
<!-- ### Scalability -->

台帳に追加される各トランザクションごとに、前の2つのトランザクションが検証されます。ネットワークを介して伝播する新しいトランザクションが多いほど、他のトランザクションの検証が高速になるため、このプロセスによってIOTAは非常にスケーラブルになります。
<!-- For each transaction that's appended to the ledger, two previous transactions are validated. This process makes IOTA incredibly scalable because the more new transactions that propagate through the network, the faster other transactions are validated. -->

## IOTAはどういった産業で役に立つのか？
<!-- ## For what industries is IOTA useful? -->
次のような多くの産業が、IOTAを使用することでメリットが得られます。
<!-- Many industries such as the following could benefit from using IOTA: -->

* [モビリティ](https://www.iota.org/verticals/mobility-automotive)
* [グローバル貿易とサプライチェーン](https://www.iota.org/verticals/global-trade-supply-chains)
* [工業用IoT](https://www.iota.org/verticals/industrial-iot)
* [ヘルスケア](https://www.iota.org/verticals/ehealth)
* [エネルギー](https://www.iota.org/verticals/smart-energy)
<!-- * [Mobility](https://www.iota.org/verticals/mobility-automotive) -->
<!-- * [Global trade and supply chains](https://www.iota.org/verticals/global-trade-supply-chains) -->
<!-- * [Industrial IoT (Internet of things)](https://www.iota.org/verticals/industrial-iot) -->
<!-- * [Healthcare](https://www.iota.org/verticals/ehealth) -->
<!-- * [Energy](https://www.iota.org/verticals/smart-energy) -->

## どのようにIOTAを始めれば良いか？
<!-- ## How do I get started? -->

* [こちらからはじめましょう](../tutorials/get-started.md)。
<!-- * [Start your IOTA journey](../tutorials/get-started.md) -->

* IOTAを既に使用している[いくつかのアプリケーション](../references/use-cases.md)をご覧ください。
<!-- * Take a look at some [applications that are already using IOTA](../references/use-cases.md) -->
