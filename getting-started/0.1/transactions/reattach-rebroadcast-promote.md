# 再アタッチ，再ブロードキャスト，プロモート
<!-- # Reattach, rebroadcast, and promote -->

**[タングル]上のトランザクションは，ネットワークの負荷の増加や，一貫性のないサブタングルに接続されている場合など，多くの理由でペンディング状態のままになる場合があります．バンドルが確定される可能性を高めるために，バンドルを再アタッチ，再ブロードキャスト，またはプロモートできます．**
<!-- **A transaction in the [Tangle](../network/the-tangle.md) may remain in a pending state for many reasons such as an increased load on the network or if it was attached to an inconsistent subtangle. To increase the chances of a bundle becoming confirmed, you can reattach, rebroadcast, or promote it.** -->

## 再アタッチ
<!-- ## Reattach -->

トランザクションは，有効であると見なされなくなったタングルの一部にアタッチされることがあります．その結果，そのトランザクションは[マイルストーン](../network/the-coordinator.md#milestones)によって参照されることはないため，確定されることはありません．
<!-- Sometimes, transactions are attached to a part of the Tangle that is no longer considered valid. As a result, that transaction will never be referenced by a [milestone](../network/the-coordinator.md#milestones), thus it will never be confirmed. -->

バンドルを再アタッチするとは，新しいバンドルを作成し，タングルの別の部分にアタッチすることを意味します．このようにして，コンセンサスに含まれる別の機会をトランザクションに与えます．
<!-- To reattach a bundle means to create a new one and attach it to a different part of the Tangle. This way, you give your transaction another chance at being included in the consensus. -->

トランザクションが10分以上ペンディング状態にいる場合，バンドルを再アタッチすることができます．この時間を過ぎると，マイルストーンによって参照される可能性は低くなります．
<!-- You may want to reattach a bundle if its transactions have been pending for more than ten minutes. After this time, it is unlikely to be referenced by a milestone. -->

バンドルを再アタッチすると，元のバンドルのフィールドのほとんどが同じままになりますが，新しいチップトランザクション（タングルの新しい部分）をリクエストし，プルーフオブワークを再度行います．その結果，変更されるフィールドは次のとおりです．
<!-- When you reattach a bundle, you keep most of the original bundle's fields the same, but you request new tip transactions (the new part of the Tangle), and do the proof of work again. As a result, the only fields that change are the following: -->

- `hash`
- `trunkTransaction`
- `branchTransaction`
- `attachmentTimestamp`
- `nonce`

IOTA トークンを転送するバンドルを再アタッチすると，1つだけが確定されます．他方は，二重支払い（同じ IOTA トークンを異なるトランザクションで2回使用する）につながるため，ペンディング状態のままになります．
<!-- When you reattach a bundle that transfers IOTA tokens, only one will ever be confirmed. The others will remain pending because they will lead to double spends (spending the same IOTA tokens twice in different transactions). -->

## 再ブロードキャスト
<!-- ## Rebroadcast -->

トランザクションがノードに送信されている間，ノードがオフラインになる場合があります．この場合，ノードはそのトランザクションを隣接ノードに転送しない可能性があり，ネットワークの残りの部分はトランザクションを認識できません．その結果，そのトランザクションは[コーディネーター](../network/the-coordinator.md)によって参照されることはないため，確定されることはありません．
<!-- While your transactions are being sent to a node, it may go offline. In this case, the node may not forward your transactions to its neighbors, and the rest of the network won't ever see your transactions. As a result, that transaction will never be referenced by the [Coordinator](../network/the-coordinator.md), thus it will never be confirmed. -->

バンドルを再ブロードキャストするとは，同じバンドルをノードに再度送信することを意味します．このようにして，トランザクションをネットワークの残りの部分に転送される別の機会を与えます．
<!-- To rebroadcast a bundle means to send the same bundle to a node again. This way, you give your transactions another chance at being forwarded to the rest of the network. -->

## プロモート
<!-- ## Promote -->

バンドルをプロモートするとは，その末尾トランザクションの累積荷重を増やすことにより，[チップ選択](root://node-software/0.1/iri/concepts/tip-selection.md)中に選択される可能性を増やすことを意味します．バンドルをプロモートするとき，末尾トランザクションと最新のマイルストーンの両方を参照するゼロトークントランザクションを作成して送信します．
<!-- To promote a bundle means to increase its chances of being selected during [tip selection](root://node-software/0.1/iri/concepts/tip-selection.md) by increasing the cumulative weight of its tail transaction. When you promote a bundle, you create and send a zero-value transaction that references both its tail transaction and the latest milestone. -->

バンドルをプロモートする方が，バンドルを再アタッチするよりも効果的です．ただし，プロモートしているバンドルが一貫性のない状態（二重支払い）になったり，最後の6つのマイルストーンより古い場合を除きます．
<!-- Promoting a bundle is often more effective than reattaching a bundle, unless the bundle you're promoting leads to an inconsistent state (double spend) or is older than the last six milestones. -->

## 関連ガイド
<!-- ## Related guides -->

[JavaScript でトランザクションが確定したかどうかを確認する](root://client-libraries/0.1/how-to-guides/js/check-transaction-confirmation.md)．
<!-- [Check if a transaction is confirmed in JavaScript](root://client-libraries/0.1/how-to-guides/js/check-transaction-confirmation.md). -->

[JavaScript でトランザクションの確定を助ける](root://client-libraries/0.1/how-to-guides/js/confirm-pending-bundle.md)．
<!-- [Help a transaction to be confirmed in JavaScript](root://client-libraries/0.1/how-to-guides/js/confirm-pending-bundle.md). -->
