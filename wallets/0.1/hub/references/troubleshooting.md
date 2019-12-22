# トラブルシューティング
<!-- # Troubleshooting -->

**ハブのセットアップまたは使用中に、これらの一般的な問題の一部が見つかる場合があります。**
<!-- **You may find some of these common issues while setting up or using Hub.** -->

## The gRPC binary module was not installed

[APIを使い始める](../how-to-guides/get-started-with-the-grpc-api.md)でgRPCクライアントを実行しようとすると、このエラーが表示されることがあります。
<!-- You may see this error when you try to run the gRPC client in our [Get started with the API](../how-to-guides/get-started-with-the-grpc-api.md) guide. -->

この問題の解決策については、[StackOverflowのこのトピック](https://stackoverflow.com/questions/50479816/node-v57-linux-x64-glibc-grpc-node-node-missing-when-using-clasp-on-linux)を参照します。
<!-- For a solution to this issue, [read this StackOverflow topic](https://stackoverflow.com/questions/50479816/node-v57-linux-x64-glibc-grpc-node-node-missing-when-using-clasp-on-linux). -->

## 確定に時間がかかり、バンドルが未確定のまま
<!-- ## Slow confirmation times and unconfirmed bundles -->

ハブがスイープを送信すると、バンドルの確定に時間がかかることがあります。
<!-- When Hub sends a sweep, you may find that the bundle takes a long time to be confirmed. -->

この問題は、次のいずれかによって発生する可能性があります。
<!-- This issue could be caused by any of the following: -->

### ネットワークで大量のトランザクションが発生している
<!-- ### The network is experiencing a high volume of transactions -->

メインネット上で、コーディネーターは2分間隔でマイルストーンを送信します。その結果、ネットワークに送信されるトランザクションが増えるほど、確定に時間がかかるようになります。
<!-- On the Mainnet, the Coordinator sends milestones at two-minute intervals. As a result, the more transactions that are sent to the network, the longer confirmations can take. -->

この場合、ハブがネットワークの現在の確定率に従ってトランザクションの再添付を実行するように、[再添付を設定する](../how-to-guides/configure-hub.md#--attachmentinterval)必要があります。
<!-- In this case, you should make sure to [configure reattachments](../how-to-guides/configure-hub.md#--attachmentinterval) so that Hub does them according to the current confirmation rate of the network. -->

### ノードで大量のトランザクションが発生している
<!-- ### The node is experiencing a high volume of transactions -->

ハブがパブリックノードに接続されている場合、そのノードは異なるクライアントから多くのトランザクションを受信している可能性があります。その結果として、ノードがハブのバンドル内のトランザクションを検証するのに時間がかかる場合があります。
<!-- If Hub is connected to a public node, that node may be receiving lots of transactions from different clients. As a result, the node may take a long time to validate the transactions in Hub's bundle. -->

ネットワークに直接アクセスできるように、[自分自身のノードを実行](root://node-software/0.1/iri/how-to-guides/quickstart.md)してハブを接続することをお勧めします。
<!-- We recommend [running your own node](root://node-software/0.1/iri/how-to-guides/quickstart.md) and connected Hub to it so that you have your own direct access to the network. -->

### スウィープに含まれるトランザクションが多すぎる
<!-- ### Sweeps contains too many transactions -->

スウィープに含まれるトランザクションが多すぎる場合、それらすべてのプルーフオブワークを完了するのに長い時間がかかります。その結果、ハブのバンドル内のブランチおよびトランクトランザクションは、トランザクションがノードに送信されるまでに有効でなくなる可能性があります。
<!-- If a sweep contains too many transactions, it takes a long time to complete the proof of work for all of them. As a result, the branch and trunk transactions in Hub's bundle may no longer be valid by the time the transactions are sent to the node. -->

バンドルの確定にかかる時間を短縮するには、[スウィープを構成](../how-to-guides/configure-hub.md#--sweepinterval)して、ハブがより頻繁に送信し、ハブに含まれるトランザクションが少なくなるようにします。
<!-- To decrease the time it takes for bundles to be confirmed, you can [configure sweeps](../how-to-guides/configure-hub.md#--sweepinterval) so that Hub sends them more often and so that they contain fewer transactions. -->

### 再添付はあまりにも長過ぎる
<!-- ### Reattachments are too infrequent -->

ハブが再添付の間隔を長くしすぎると、バンドルの確定に必要以上に時間がかかる場合があります。
<!-- If Hub waits too long between reattachments, bundles can take longer than necessary to be confirmed. -->

バンドルの確定時間を改善するには、ハブがより頻繁に再添付を実行するように、ネットワークの現在の確定率に応じて[再添付の設定](../how-to-guides/configure-hub.md#--attachmentinterval)を行います。
<!-- To improve the confirmation time of bundles, you can [configure reattachments](../how-to-guides/configure-hub.md#--attachmentinterval) so that Hub does them more often, according to the current confirmation rate of the network. -->
