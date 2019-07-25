# 自分のノードにスパムトランザクションを送信する
<!-- # Send spam transactions to your node -->

**ノードが1秒間に処理できるトランザクション数をテストするには、Web APIエンドポイントを使用してスパムトランザクションを送信します。**
<!-- **To test how many transactions per second your node can process, you can use a web API endpoint to send it spam transactions.** -->

`spammer`APIエンドポイントはあなたのノードにスパムトランザクションを送ることを可能にします。
<!-- The `spammer` API endpoint allows you to send your node spam transactions. -->

1. Webブラウザを開き、アドレスバーに次のように入力します。インターネットを通して自分のノードにアクセスしたいならば、`localhost`を自分のIPアドレスに置き換えます。
  <!-- 1. Open a web browser and enter the following into the address bar. If you want to access your node through the Internet, replace `localhost` with your IP address. -->

    `http://localhost:8080/spammer?cmd=start`

    :::info:
    デフォルトでは、このエンドポイントは毎秒1,000トランザクション（TPS）を送信します。TPSを変更したい場合は、`tps`クエリパラメータを追加することができます。たとえば、10,000TPSを送信するには、次のようにエンドポイントにリクエストを送信します。`http://localhost:8080/spammer?cmd=start&tps=10000`
    :::
    <!-- :::info: -->
    <!-- By default, this endpoint sends 1,000 transactions per second (TPS). If you want to change the TPS, you can add the `tps` query parameter. For example, to send 10,000 TPS, send a request to the following endpoint `http://localhost:8080/spammer?cmd=start&tps=10000` -->
    <!-- ::: -->

2. ノードがトランザクションを受信していることを確認するには、ユーザーインターフェイスの右上にある統計を参照します。
<!-- 2. To check that your node is receiving transactions, see the statistics in the top-right of the user interface -->

    :::info:
    表示されたTPSは設定した`tps`パラメータの値より低いかもしれません。この違いの理由は使用しているコンピュータハードウェアの限界によるものかもしれません。
    :::
    <!-- :::info: -->
    <!-- The displayed TPS may be lower than the value of the `tps` parameter you used. The reason for this difference may be due to limits with your computer hardware. -->
    <!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

ノードがトランザクションを受信しているので、トランザクションデータを見るためにノード上で[トランザクションイベントを購読する](../how-to-guides/subscribe-to-events.md)ことができます。
<!-- Now that your node is receiving transactions, you can [subscribe to the transaction event](../how-to-guides/subscribe-to-events.md) on your node to see that data. -->
