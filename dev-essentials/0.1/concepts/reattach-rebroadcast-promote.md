# 再添付、再ブロードキャスト、促進
<!-- # Reattach, rebroadcast, and promote -->

**バンドルは、ネットワークの負荷が増加するなどの、さまざまな理由でペンディングの場合があります。バンドルが確定される可能性を高めるために、バンドルを再添付、再ブロードキャスト、または促進することができます。 **
<!-- **A bundle may be pending for many reasons such as an increased load on the network. To increase the chances of a bundle becoming confirmed, you can reattach, rebroadcast, or promote it** -->

## 再添付
<!-- ## Reattach -->

トランザクションは、長らく有効と見なされていないタングルの一部に添付される可能性があります。その結果、そのトランザクションはコンセンサスに含まれることはありません。
<!-- Your transaction may have been attached to a part of the Tangle that is no longer considered valid. As a result, that transaction will never be included in the consensus. -->

バンドルを再添付するとは、新しいバンドルを作成し、タングルの別の部分に添付することを意味します。このようにして、コンセンサスに含まれる別の機会をトランザクションに与えます。
<!-- To reattach a bundle means to create a new one and attach it to a different part of the Tangle. This way, you give your transaction another chance at being included in the consensus. -->

バンドルを再添付すると、元のバンドルのフィールドのほとんどが同じままになりますが、新しいチップトランザクション（タングルの新しい部分）を要求し、プルーフオブワークを再度行います。その結果、変更されるフィールドは次のとおりです。
<!-- When you reattach a bundle, you keep most of the original bundle's fields the same, but you request new tip transactions (the new part of the Tangle), and do the proof of work again. As a result, the only fields that change are the following: -->

* [`hash`](../references/structure-of-a-transaction.md)
* [`trunkTransaction`](../references/structure-of-a-transaction.md)
* [`branchTransaction`](../references/structure-of-a-transaction.md)
* [`attachmentTimestamp`](../references/structure-of-a-transaction.md)
* [`nonce`](../references/structure-of-a-transaction.md)

トランザクションが10分以上ペンディングの場合は、バンドルを再添付することをお勧めします。10分以上が経過すると、バンドルの末尾トランザクションが他のトランザクションのチップ選択で選択される可能性が低くなり、コンセンサスに含まれる可能性も低くなります。
<!-- You may want to reattach a bundle if its transactions have been pending for more than ten minutes. After this time, the tail transaction in the pending bundle is unlikely to be selected during tip selection, which makes it unlikely to be included in the consensus. -->

IOTAトークンを転送するバンドルを再添付すると、確定されるのは1つだけです。二重支出（同じIOTAトークンを2回使うこと）につながるので、一方のトランザクションはペンディングのままになります。
<!-- When you reattach a bundle that transfers IOTA tokens, only one will ever be confirmed. The others will remain pending because they will lead to double-spends (spending the same IOTA tokens twice). -->

## 再ブロードキャスト
<!-- ## Rebroadcast -->

トランザクションがノードに送信されている間に、ノードはオフラインになる場合があります。その場合、ノードはトランザクションを隣接ノードに転送しない場合があります。その結果、ネットワークの残りの部分にはトランザクションが表示されず、コンセンサスに含まれません。
<!-- While your transactions are being sent to a node, it may go offline. In this case, the node may not forward your transactions to its neighbors. As a result, the rest of the network won't ever see your transactions, and they won't be included in the consensus. -->

バンドルを再ブロードキャストするとは、同じバンドルをノードに再度送信することを意味します。このようにして、トランザクションをネットワークの残りの部分に転送される別の機会を与えます。
<!-- To rebroadcast a bundle means to send the same bundle to a node again. This way, you give your transactions another chance at being forwarded to the rest of the network. -->

## 促進
<!-- ## Promote -->

バンドルを促進するとは、末尾トランザクションの[累積荷重](root://node-software/0.1/iri/concepts/tip-selection.md)を増やすことによって、チップ選択で選択される可能性を高めることを意味します。バンドルを促進するとき、末尾トランザクションと最新のマイルストーンの両方を参照するゼロトークントランザクションを作成して送信します。
<!-- To promote a bundle means to increases its chances of being selected during tip selection by increasing the [cumulative weight](root://node-software/0.1/iri/concepts/tip-selection.md) of its tail transaction. When you promote a bundle, you create and send a zero-value transaction that references both its tail transaction and the latest milestone. -->

促進しているバンドルが矛盾した状態（二重支払い）になるか、参照しているマイルストーンが最新のマイルストーンよりも6個古く無ければ、バンドルを促進するほうがバンドルを再添付するよりも効果的です。
<!-- Promoting a bundle is often more effective than reattaching a bundle, unless the bundle you're promoting leads to an inconsistent state (double-spend) or is older than the last six milestones. -->
