# ノードにスパムトランザクションを送信する
<!-- # Send spam transactions to your node -->

**ノードが1秒間に処理できるトランザクションの数をテストするために，`spammer` APIエンドポイントを使用してスパムトランザクションを送信できます．**
<!-- **To test how many transactions per second your node can process, you can use a `spammer` API endpoint to send it spam transactions.** -->

自分のノードにスパムトランザクションを送信すると，ノードはスパムトランザクションを処理し，自身の台帳にスパムトランザクションを追加し，そして処理のためにスパムトランザクションを隣接ノードに転送します．
<!-- When you send your node spam transactions, it processes them, adds them to its ledger, and forwards them to its neighbors for processing. -->

1. Web ブラウザを開き，アドレスバーに次を入力します．インターネット経由でノードにアクセスする場合は，`localhost` を IP アドレスに置き換えます．
  <!-- 1. Open a web browser and enter the following into the address bar. If you want to access your node through the Internet, replace `localhost` with your IP address. -->

    `http://localhost:8080/spammer?cmd=start`

    :::info:
    デフォルトでは，このエンドポイントは1,000トランザクション/秒（TPS）を送信します．TPS を変更する場合は，`tps` クエリパラメーターを追加できます．たとえば，10,000 TPS を送信するには，次のエンドポイント http://localhost:8080/spammer?cmd=start&tps=10000 にリクエストを送信します．
    :::
    <!-- :::info: -->
    <!-- By default, this endpoint sends 1,000 transactions per second (TPS). If you want to change the TPS, you can add the `tps` query parameter. For example, to send 10,000 TPS, send a request to the following endpoint `http://localhost:8080/spammer?cmd=start&tps=10000` -->
    <!-- ::: -->

2. ノードがトランザクションを受信していることを確認するには，Web ブラウザで http://localhost:8081/dashboard に移動してダッシュボードを開きます．
  <!-- 2. To check that your node is receiving transactions, open the dashboard by going to `http://localhost:8081/dashboard` in a web browser -->

    ![GoShimmer dashboard](../images/goshimmer-web-ui.png)

    :::info:
    表示された TPS は設定した `tps` パラメータの値より低いかもしれません．この違いの理由は使用しているコンピュータハードウェアの限界によるものかもしれません．
    :::
    <!-- :::info: -->
    <!-- The displayed TPS may be lower than the value of the `tps` parameter you used. The reason for this difference may be due to limits with your computer hardware. -->
    <!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

ノードがトランザクションを受信しているので，[着信トランザクションをモニタリング](../how-to-guides/subscribe-to-events.md)してトランザクションデータを表示できます．
<!-- Now that your node is receiving transactions, you can [monitor it for incoming transaction](../how-to-guides/subscribe-to-events.md) to see the transaction data. -->
