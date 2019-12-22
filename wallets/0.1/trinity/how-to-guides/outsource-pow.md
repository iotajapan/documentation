# プルーフオブワークを外部委託する
<!-- # Outsource proof of work -->

**デフォルトで[プルーフオブワーク](root://getting-started/0.1/transactions/proof-of-work.md)は、トリニティを実行しているコンピューターで実行されます。このガイドでは、接続しているノードの1つにプルーフオブワークを行うように依頼する方法を学習します。**
<!-- **By default [proof of work](root://getting-started/0.1/transactions/proof-of-work.md) is done on the computer that is running Trinity. In this guide, you learn how to ask one of the connected nodes to do proof of work instead.** -->

1. [**設定**] > [**ノード**] > [**カスタムノードを追加**]に移動し、追加する1つ以上のノードの URL または IP アドレスを入力します。
  <!-- 1. Go to **Settings** > **Node** > **Add custom nodes**,  and enter the URL or IP address of one or more nodes that you want to add -->

    :::info:
    トリニティのすべてのノードは、HTTPSを介して通信する必要があります。
    :::
    <!-- :::info: -->
    <!-- All nodes in Trinity must communicate over HTTPS. -->
    <!-- ::: -->

2. **プルーフオブワークの外部委託**オプションを有効にします。
<!-- 2. Enable the **Outsource proof of work** option -->

3. **プルーフオブワークノード**のドロップダウンで、ノードを選択します。
  <!-- 3. In the **PROOF OF WORK NODE** dropdown, select your node -->

    :::warning:Devnetノード
    ノードがDevnetノードである場合、[主要Devnetノードに接続する](../how-to-guides/connect-to-a-custom-node.md)も必要です。
    :::
    <!-- :::warning:Devnet nodes -->
    <!-- If your node is a Devnet node, you must also [connect to a primary Devnet node](../how-to-guides/connect-to-a-custom-node.md). -->
    <!-- ::: -->

4. **保存**をクリックします。
  <!-- 4. Click **Save** -->

:::success:おめでとうございます！:tada:
トランザクションを送信するたびに、プルーフオブワークがカスタムプルーフオブワークノードによって実行されます。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- Whenever you send a transaction, the proof of work is done by your custom proof-of-work node. -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

[トランザクションを送信する](../how-to-guides/send-a-transaction.md)
<!-- [Send a transaction](../how-to-guides/send-a-transaction.md) -->
