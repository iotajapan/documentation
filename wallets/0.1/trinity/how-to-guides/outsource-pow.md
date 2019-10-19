# プルーフオブワークを外部委託する
<!-- # Outsource proof of work -->

**トランザクションを送信する前に、[プルーフオブワーク](root://dev-essentials/0.1/concepts/minimum-weight-magnitude.md)を含める必要があります。プルーフオブワークには、エネルギーを使用して計算を行うコンピューターが必要です。デフォルトでは、トリニティを実行しているコンピューターでプルーフオブワークが行われます。**
<!-- **Before you send a transaction, it must include a [proof of work](root://dev-essentials/0.1/concepts/minimum-weight-magnitude.md). Proof of work requires a computer to use energy to do computations. By default proof of work is done on the computer that is running Trinity.** -->

**プルーフオブワークの外部委託**オプションを使用して、特定のノードにトランザクションのプルーフオブワークを行うように依頼できます。
<!-- You can use the **Outsouce proof of work** option to ask a specific node to do the proof of work for your transactions. -->

:::info:
トリニティのすべてのノードは、HTTPSを介して通信する必要があります。
:::
<!-- :::info: -->
<!-- All nodes in Trinity must communicate over HTTPS. -->
<!-- ::: -->

1. [**設定**] > [**ノード**] > [**カスタムノードを追加**]に移動し、追加するノードのURLまたはIPアドレスを入力します。
  <!-- 1. Go to **Settings** > **Node** > **Add custom nodes**,  and enter the URL or IP address of the nodes you want to add -->

    :::info:
    クォーラムのカスタムノードに接続するには、複数のノードを追加します。
    :::
    <!-- :::info: -->
    <!-- To connect to a quorum of custom nodes, add more than one. -->
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
