# ノードクォーラム
<!-- # Node quorum -->

**トリニティをより堅牢にするために、[ノードクォーラム](root://getting-started/0.1/network/nodes.md#node-quorum)から残高などの情報をリクエストし、結果を比較できます。トリニティでは、クォーラムはハードコードされたノード、リモートパブリックノード、および/またはユーザー定義のカスタムノードで構成されます**
<!-- **To make Trinity more robust, it can request information such as balances from a [node quorum](root://getting-started/0.1/network/nodes.md#node-quorum) and compare the results. In Trinity, the quorum consists of hard-coded nodes, remote public nodes and/or any user-defined custom nodes** -->

トリニティがノードクォーラムに情報のリクエストを送信すると、それぞれからの結果が比較されます。少なくとも67％（4つのうち3つ）のノードが同じ結果を返す場合、トリニティは情報が正しいという高いレベルの信頼性を持ちます。一致するノード数が少ない場合、ノードクォーラムは失敗し、トリニティは安全な代替結果を表示します。
<!-- When Trinity sends a request for information to a node quorum, it compares the results from each of them. If at least 67% (3 out of 4) nodes return the same result, then Trinity has a high level of confidence that the information is correct. If fewer nodes agree, the quorum fails and Trinity displays a _safe_ fallback result. -->

現時点で、トリニティは次の目的でノードクォーラムを使用しています。
<!-- At the moment, Trinity uses a node quorum for the following: -->

| **ノードクォーラムへのリクエスト** | **安全な代替結果（失敗した場合）** | **安全な代替結果の理由** |
| :--- | :--- | :--- |
| アドレスが使用済みかどうか | アドレスは使用済み | ユーザーが所有していないトークンを使用するのを防ぐため。 |
| トランザクションの送信前、送信中、送信後のアドレスの残高 | 残高は0 | ユーザーが自分の残高が最新のものであると誤解して誤用するのを防ぎ、ユーザーが[使用済みアドレス](root://getting-started/0.1/clients/addresses.md#spent-addresses)からトークンを取り出すリスクを回避するため。 |

<!-- | **Request to a node quorum**|**Safe fallback result (in case of failure)** |**Reason for safe fallback result**| -->
<!-- |:--|:--|:---| -->
<!-- |Whether an address is spent| The address is spent| To stop users from trying to spend tokens that they don't have| -->
<!-- |The balances of an address before, during, and after sending a transaction| Zero balance| To stop users from being misled into believing that their balance is up to date, thus avoiding the risk of a user withdrawing from a [spent address](root://getting-started/0.1/clients/addresses.md#spent-addresses) -->
