# 残高を読み込む
<!-- # Read your balance -->

**トリニティは、[シード](root://getting-started/0.1/clients/seeds.md)に関連付けられているすべての[アドレス](root://getting-started/0.1/clients/addresses.md)の残高を[ノード](root://getting-started/0.1/network/nodes.md)に尋ねることにより、定期的に残高を更新します。このガイドでは、表示された残高が正しくない場合の残高の読み方とトリニティのトラブルシューティング方法を学びます。**
<!-- **Trinity regularly updates your balance by asking [nodes](root://getting-started/0.1/network/nodes.md) for the balance of all [addresses](root://getting-started/0.1/clients/addresses.md) associated with your [seed](root://getting-started/0.1/clients/seeds.md). In this guide, you learn how to read your balance and troubleshoot Trinity if the displayed balance is incorrect.** -->

合計残高はホームページの上部に表示されます。[個々のアドレスの残高も表示](../how-to-guides/manage-your-account.md#view-the-addresses-of-an-account)できます。
<!-- Your total balance is displayed at the top of the home page. You can also [view the balance of individual addresses](../how-to-guides/manage-your-account.md#view-the-addresses-of-an-account). -->

トリニティがノードに接続できない場合、不正確な残高が表示される場合があります。
<!-- If Trinity can't connect to a node, it may display an incorrect balance. -->

この問題を解決するために、トリニティは生成されたアドレスのリストを保持して、トリニティが次にノードに接続したときに再同期できるようにします。
<!-- To fix this problem, Trinity keeps a list of your generated addresses so that you can re-synchronize it the next time Trinity connects to a node. -->

残高が間違っている（および[グローバルスナップショット](../how-to-guides/perform-a-snapshot-transition.md)が発生していない）と思われる場合は、**設定** > **アカウント** > **アカウント管理** > **ツール** > **アカウントを同期**に移動して、トリニティを同期できます。 。
<!-- If you think your balance is wrong (and a [global snapshot](../how-to-guides/perform-a-snapshot-transition.md) hasn't occurred), you can synchronize Trinity by going to **Settings** > **Account** > **Account management** > **Tools** > **Sync account**. -->

![Manual update](../images/sync.jpg)
