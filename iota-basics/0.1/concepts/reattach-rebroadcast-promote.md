# 再添付、再ブロードキャスト、促進
<!-- # Reattach, rebroadcast, and promote -->

**バンドルは、ネットワークの負荷が増加するなどの、さまざまな理由でペンディングの場合があります。バンドルが確定される可能性を高めるために、バンドルを再添付、再ブロードキャスト、または促進することができます。 **
<!-- **A bundle may be pending for many reasons such as an increased load on the network. To increase the chances of a bundle being confirmed, you can reattach, rebroadcast, or promote a bundle** -->

## 再添付
<!-- ## Reattach -->

バンドルを再添付するとは、新しいバンドルを作成して、新たにタングルの別の部分に添付することを意味します。バンドルを再添付するとき、オリジナルのバンドルのフィールドの大部分は同じものを使用します。しかし新しいチップトランザクションを行い、そして再びプルーフオブワークを行います。その結果、変更される唯一のフィールドは次のとおりです。
<!-- To reattach a bundle means to create a new one and attach it to a different part of the Tangle. When you reattach a bundle, you keep most of the original bundle's fields the same, but you request new tip transactions, and do the proof of work again. As a result, the only fields that change are the following: -->

* [`hash`](../references/structure-of-a-transaction.md)
* [`trunkTransaction`](../references/structure-of-a-transaction.md)
* [`branchTransaction`](../references/structure-of-a-transaction.md)
* [`attachmentTimestamp`](../references/structure-of-a-transaction.md)
* [`nonce`](../references/structure-of-a-transaction.md)

トランザクションが10分以上ペンディングの場合は、バンドルを再添付することをお勧めします。10分以上が経過すると、バンドルの末尾トランザクションが他のトランザクションのチップ選択で選択される可能性が低くなり、確定する可能性も低くなります。
<!-- You may want to reattach a bundle if its transactions have been pending for more than ten minutes. After this time, the tail transaction in the pending bundle is unlikely to be selected during tip selection, which makes it unlikely to be confirmed. -->

IOTAトークンを転送するバンドルを再添付すると、確定されるのは1つだけです。二重支出につながるので、他方のトランザクションはペンディングのままになります。
<!-- When you reattach a bundle that transfers IOTA tokens, only one will ever be confirmed. The others will remain pending because they will lead to double-spends. -->

## 再ブロードキャスト
<!-- ## Rebroadcast -->

バンドルを再ブロードキャストするとは、同じバンドルをノードに再度送信することを意味します。
<!-- To rebroadcast a bundle means to send the same bundle to a node again. -->

トランザクションを送ったノードが隣接ノードにトランザクションを転送しなかったと考えられる場合、バンドルを再ブロードキャストすることができます。ノードが、オフラインになった場合や、またはバンドルを受信した後に負荷が高くなり、トランザクションを隣接ノードに転送できていない時があります。
<!-- You may want to rebroadcast a bundle if you think that the node you sent it to didn't forward the transactions to its neighbors. Nodes might not forward transactions to neighbors if they either go offline or are under heavy load after receiving a bundle. -->

## 促進
<!-- ## Promote -->

バンドルを促進するとは、末尾トランザクションの[累積荷重](root://the-tangle/0.1/concepts/tip-selection.md)を増やすことによって、チップ選択で選択される可能性を高めることを意味します。バンドルを促進するとき、末尾トランザクションと最新のマイルストーンの両方を参照するゼロトークントランザクションを作成して送信します。
<!-- To promote a bundle means to increases its chances of being selected during tip selection by increasing the [cumulative weight](root://the-tangle/0.1/concepts/tip-selection.md) of its tail transaction. When you promote a bundle, you create and send a zero-value transaction that references both its tail transaction and the latest milestone. -->

促進しているバンドルが矛盾した状態（二重支払い）になるか、参照しているマイルストーンが最新のマイルストーンよりも6個古く無ければ、バンドルを促進するほうがバンドルを再添付するよりも効果的です。
<!-- Promoting a bundle is often more effective than reattaching a bundle, unless the bundle you're promoting leads to an inconsistent state (double-spend) or is older than the last six milestones. -->
