# ノード定足数
<!-- # Node quorum -->

**タングルからの情報の供給源として1つのノードだけに頼るとき、あなたはそれが正しいという確信が持てません。例えば、そのノードはあなたの利用可能な残高について間違った情報をあなたに送るかもしれません。トリニティが表示する情報に対する信頼性を高めるために、トリニティは複数のノードに接続して結果を比較することができます。このノードグループはノード定足数と呼ばれ、ハードコードされたノード、リモートパブリックノード、および/またはユーザー定義のカスタムノードから構成されます。**
<!-- **When you rely only on one node as a source of information from the Tangle, you can't be confident that it's correct. For example, that node could send you the wrong information about your available balance. To increase your confidence in the information that Trinity displays, it can connect to multiple nodes and compare the results. This group of nodes is called a node quorum, which consists of hard-coded nodes, remote public nodes and/or any user-defined custom nodes** -->

トリニティがノード定足数に情報のリクエストを送信すると、それぞれからの結果が比較されます。少なくとも67％（4つのうち3つ）のノードが同じ結果を返す場合、トリニティは情報が正しいという高いレベルの信頼性を持ちます。一致するノード数が少ない場合、ノード定足数は失敗し、トリニティは安全な代替結果を表示します。
<!-- When Trinity sends a request for information to a node quorum, it compares the results from each of them. If at least 67% (3 out of 4) nodes return the same result, then Trinity has a high level of confidence that the information is correct. If fewer nodes agree, the quorum fails and Trinity displays a _safe_ fallback result. -->

現時点で、トリニティは次の目的でノード定足数を使用しています。
<!-- At the moment, Trinity uses a node quorum for the following: -->

| **ノード定足数へのリクエスト** | **安全な代替結果（失敗した場合）** | **安全な代替結果の理由** |
| :--- | :--- | :--- |
| アドレスが使用済みかどうか | アドレスは使用済み | ユーザーが所有していないトークンを使用するのを防ぐため。 |
| トランザクションの送信前、送信中、送信後のアドレスの残高 | 残高は0 | ユーザーが自分の残高が最新のものであると誤解して誤用するのを防ぎ、ユーザーが署名済みアドレスからトークンを取り出すリスクを回避するため。[これがなぜ重要なのか](root://dev-essentials/0.1/concepts/addresses-and-signatures.md#addressreuse)を知る。 |

<!-- | **Request to a node quorum**|**Safe fallback result (in case of failure)** |**Reason for safe fallback result**| -->
<!-- |:--|:--|:---| -->
<!-- |Whether an address is spent| The address is spent| To stop users from trying to spend tokens that they don't have| -->
<!-- |The balances of an address before, during, and after sending a transaction| Zero balance| To stop users from being misled into believing that their balance is up to date, thus avoiding the risk of a user withdrawing from a spent address. [Find out why this is important](root://dev-essentials/0.1/concepts/addresses-and-signatures.md#addressreuse). -->
