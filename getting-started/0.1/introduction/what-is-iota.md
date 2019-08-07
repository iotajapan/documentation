# IOTAとは？
<!-- # What is IOTA? -->

**IOTAは、IOTAネットワーク内のデバイスがイミュータブルなデータと価値（IOTAトークン）を無料で相互に転送できるようにする分散型台帳技術です。**
<!-- **IOTA is a distributed ledger technology that allows devices in an IOTA network to transfer immutable data and value among each other for free.** -->

IOTAは、M2M経済圏における、効率性の向上、生産量の増加、データの信頼性を保証することを目的としています。
<!-- IOTA aims to improve efficiency, increase production, and ensure data integrity in a machine-to-machine economy. -->

<dl><dt>M2M経済圏</dt><dd>人間の介入なしに、任意のデバイスがデータと価値を他のデバイスに転送できる経済圏。</dd></dl>
<!-- <dl><dt>machine-to-machine economy</dt><dd>Economy in which any device can transfer data and value to other devices without human intervention.</dd></dl> -->

<iframe src="https://www.youtube.com/embed/Gr-LstcDcAw" width="400" height="200"></iframe>

## IOTAはどのように機能するのか？
<!-- ## How does IOTA work? -->

IOTAはパーミッションレスです。つまり、誰でもネットワークを使用して支払いをしたり、データを送信したりできます。サインアップしたり、個人情報を提供したり、サブスクリプションを支払う必要はありません。代わりに、IOTAネットワークに送信するすべてのものが暗号化によって検証されます。 IOTAプロトコルの暗号化機能により、次の利点が得られます。
<!-- IOTA is permissionless, which means that anyone can use the network to make payments or send data. You don't need to sign up, give away your personal details, or pay a subscription. Instead, everything that you send to an IOTA network is verified through cryptography. The cryptographic functions in the IOTA protocol allow you to benefit from the following: -->

* **真正性：** トランザクションを送信し、トランザクション中のデータまたはIOTAトークンを所有していることを証明します。
<!-- * **Authenticity:** Prove that you sent a transaction and that you own the data or IOTA tokens in it -->
* **完全性：** トランザクションが変更されていないことを証明します。
<!-- * **Integrity:** Prove that your transaction is unchanged -->
* **秘密性：** 暗号化によってデータにアクセスできるユーザーを制御します。
<!-- * **Confidentiality:** Control who has access to your data through encryption -->

IOTAを使用するために必要なのは[シード](../introduction/what-is-a-seed.md)のみです。これはIOTAの暗号化機能のマスターキーです。
<!-- To use IOTA, all you need is a [seed](../introduction/what-is-a-seed.md), which is the master key to the cryptographic functions in IOTA. -->

シードがある場合、シードを使用して、データまたはIOTAトークンを転送できるアドレスを作成できます。
<!-- When you have a seed, you can use it to create addresses to which you can transfer data or IOTA tokens. -->

IOTAで何かを転送するには、[トランザクション](../introduction/what-is-a-transaction.md)と呼ばれるオブジェクトに命令をパッケージ化します。次に、そのトランザクションを[バンドル](../introduction/what-is-a-bundle.md)にグループ化し、[ノード](../introduction/what-is-a-node.md)に送信します。ノードは、トランザクションの検証と実行を行います。
<!-- To transfer anything in IOTA, you package an instruction into an object called a [transaction](../introduction/what-is-a-transaction.md). Then, you group that transaction into a [bundle](../introduction/what-is-a-bundle.md), and send it to a [node](../introduction/what-is-a-node.md), which is responsible for validating it and carrying it out. -->

### ネットワークの基本
<!-- ### Network basics -->

ノードは、IOTAネットワークを構成するデバイスです。他の分散システムと同様に、IOTAネットワーク内のノードは相互に接続されているため、相互にゴシップ情報があります。したがって、1つのノード（世界のどこにいても）がトランザクションを受信すると、ネットワーク内の他のすべてのノードに転送されます。このようにして、同じIOTAネットワーク内のすべてのノードがすべてのトランザクションを検証して保存できます。
<!-- Nodes are the devices that make up an IOTA network. Like any distributed system, the nodes in an IOTA network are interconnected such that they gossip information among each other. So, when one node (no matter where it is in the world) receives a transaction, it will be forwarded to every other node in the network. This way, all nodes in the same IOTA network can validate all transactions and store them. -->

ノードが保存するトランザクションのコレクションは[タングル](../introduction/what-is-the-tangle.md)と呼ばれ、ノードだけがタングルに直接アクセスできます。タングルは、有向非循環グラフ（DAG）と呼ばれるデータ構造で、各トランザクションは、自分より前にある2つのトランザクションのハッシュ値を参照します。このように、すべてのトランザクションはイミュータブルであり、ノードが信頼性を検証するために遡ることができる参照の履歴があります。
<!-- The collection of transactions that the nodes store is called [the Tangle](../introduction/what-is-the-tangle.md), and only nodes have direct access to it. The Tangle is a data structure called a directed acyclic graph (DAG), where each transaction references the hashes of two transactions that came before it. This way, all transactions are immutable and have a history of references that nodes can traverse to validate their trustworthiness. -->

ノードのみがタングルにアクセスできるため、IOTAプロトコルは、クライアントがノードに接続してタングルへのアクセスを要求するクライアント/サーバーモデルを使用しています。
<!-- Because only nodes have access to the Tangle, the IOTA protocol uses a client/server model where clients connect to nodes to request access to it. -->

## IOTAトークンとは？
<!-- ## What is the IOTA token? -->

最も基本的なレベルでは、IOTAトークンはIOTAネットワーク内のノードによって保持されている所有権の記録です。
  <!-- At its most basic level, the IOTA token is a record of ownership that's held by the nodes in an IOTA network. -->

  ```bash
  ADDRESS....ENDOFADDRESS;1000
  ```

セミコロンの左側にアドレスがあります。これらはネットワーク内の各クライアントに一意のものです。セミコロンの右側には、そのアドレスに属するIOTAトークンの量があります。この場合は1,000トークンです。
<!-- On the left of the semicolon is an address. These are unique to each client in the network. On the right of the semicolon is an amount of IOTA tokens that belong to that address, in this case 1,000 tokens. -->

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

IOTAは、さまざまなデバイス間でデータや価値（IOTAトークン）の転送プロセスを合理化、保護、および自動化できるオープンソーステクノロジーです。
<!-- IOTA is an open-source technology that can streamline, secure, and automate any process that sends data or transfers value among different devices. -->

### 信頼性
<!-- ### Trust -->

IOTAネットワーク内の各ノードはトランザクションを検証してから、同じことを行う他のノードにトランザクションを送信します。その結果、すべての有効なトランザクションはすべてのノードによって合意され、ネットワーク内の単一のノードを信頼する必要がなくなります。
<!-- Each node in an IOTA network validates transactions, then sends them to other nodes that do the same. As a result, all valid transactions are agreed on by all nodes, removing the need to trust a single one in the network. -->

### 不変性
<!-- ### Immutability -->

台帳にあるすべてのトランザクションはイミュータブルかつ透明です。
<!-- All transactions in the ledger are immutable and transparent. -->

タングルの各トランザクションは、自分より前にある2つのトランザクションのハッシュ値を参照します。その結果、各トランザクションは自分自身を自身の参照にコミットします。
<!-- Each transaction in the Tangle references the hashes of two previous ones. As a result, each transaction is commits itself to its references. -->

トランザクションの内容が変更されると、ハッシュ値が無効になり、トランザクションも無効になります。
<!-- If the contents of any transaction were to change, the hashes would be invalid, making the transactions invalid. -->

### 安全性
<!-- ### Security -->

IOTAは、ネットワークを保護し、攻撃者がIOTAトークンを盗むのを防ぐために、量子耐性暗号を使用しています。
<!-- IOTA uses quantum-resistant cryptography to secure the network and prevent attackers from stealing IOTA tokens. -->

IOTAネットワークは、P2Pネットワークです。中央機関がトランザクションの台帳を管理するのではなく、すべてのノードがコピーを保持し、IOTAプロトコルを介してその内容に関する合意を自動化します。
<!-- IOTA networks are peer-to-peer networks. No central authority controls the ledger of transactions, instead all nodes hold a copy and automate the agreement on its contents through the IOTA protocol. -->

### コスト削減
<!-- ### Cost saving -->

IOTAは無料で利用でき、購読料を支払う必要も、契約にサインする必要もありません。トランザクション手数料も無料です。
<!-- IOTA is free to use. You don't need to pay a subscription, or sign a contract. Even transactions are feeless. -->

### スケーラビリティ
<!-- ### Scalability -->

タングルに添付される各トランザクションごとに、前の2つのトランザクションが検証されます。ネットワークを介して伝播する新しいトランザクションが多いほど、他のトランザクションの検証が高速になるため、このプロセスによってIOTAは非常にスケーラブルになります。
<!-- For each transaction that's attached to the Tangle, two previous transactions are validated. This process makes IOTA incredibly scalable because the more new transactions that propagate through the network, the faster other transactions are validated. -->

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

* [初心者向けチュートリアルでIOTAの旅を始めましょう](../tutorials/get-started.md)。
<!-- * [Start your IOTA journey with our beginner tutorials](../tutorials/get-started.md) -->

* IOTAでアプリケーションを開発するために[必要な基本概念を学ぶ](root://dev-essentials/0.1/introduction/overview.md)。
<!-- * [Learn the essential concepts](root://dev-essentials/0.1/introduction/overview.md) that you need to develop applications on IOTA. -->

* IOTAを既に使用している[いくつかのアプリケーション](../references/use-cases.md)を参照する。
<!-- * Take a look at some [applications that are already using IOTA](../references/use-cases.md) -->
