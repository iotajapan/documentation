# カスタムノードに接続する
<!-- # Connect to custom nodes -->

**トリニティは、デフォルトノードの[クォーラム]を介してタングルと対話します。自分自身のノードの1つをクォーラムに追加するか、Devnet上のノードなどのカスタムノードをクォーラムに接続するか、特定のノードに接続してプルーフオブワークに使用するかなどを選択できます。**
<!-- **Trinity interacts with the Tangle through a [quorum](../concepts/node-quorum.md) of default nodes. You can choose to add one of your own nodes to the quorum, connect to a quorum of custom nodes such as those on the Devnet, or connect to a specific node to use for outsourced proof of work.** -->

![Node management in Trinity](../images/node-management.png)

## トリニティをカスタムノードのクォーラムに接続する
<!-- ## Connect Trinity to a quorum of custom nodes -->

単一のカスタムノードに接続するだけでなく、複数のカスタムノードを追加してそれらをノードクォーラムとして使用することもできます。このようにして、トリニティ内蔵のリモートノードリストを使うことに制限されません。
<!-- As well as connecting to a single custom node, you can also add multiple custom nodes and use them as a node quorum. This way, you aren't restricted to using the built-in remote list of Trinity nodes. -->

1. **設定** > **ノード** > **カスタムノードの追加**に移動し、追加するノードのURLまたはIPアドレスを入力します
  <!-- 1. Go to **Settings** > **Node** > **Add custom nodes**,  and enter the URL or IP address of the nodes you want to add -->

    :::info:
    トリニティのすべてのノードは、HTTPSを介して通信する必要があります。
    :::
    <!-- :::info: -->
    <!-- All nodes in Trinity must communicate over HTTPS. -->
    <!-- ::: -->

2. If you want to outsource [proof of work](root://getting-started/0.1/transactions/proof-of-work.md), enable the **Outsource proof of work** option

    :::info:
    By default proof of work is done on the computer that is running Trinity.
    :::

3. Disable the **Automatic node management** option

4. Disable the **Primary node autoswitching** option

    :::warning:
    When enabled, this option allows Trinity to connect to one of its default nodes if the primary one goes offline.

    このオプションを無効にすると、主要ノードがオフラインになった場合、トリニティはどのノードにも接続できなくなります。
    :::
    <!-- :::warning: -->
    <!-- When enabled, this option allows Trinity to connect to one of its defaults nodes if the primary one goes offline. -->

    <!-- By disabling this option, Trinity won't be able to connect to any nodes if the primary node goes offline. -->
    <!-- ::: -->

4. ドロップダウンメニューからカスタムノードを選択します。
  <!-- 5. Select your node from the dropdown menu -->

    :::warning:Devnetノード
    カスタムノードがDevnetノードの場合は、**リモートノードリストを使う**オプションも無効にする必要があります。そのリストのノードはMainnetノードであるため、Devnetと互換性がありません。
    :::
    <!-- :::warning:Devnet nodes -->
    If your custom node is a Devnet node, you must also disable the **Use remote list** option. The nodes in that list are Mainnet nodes, so they're incompatible with the Devnet.
    <!-- ::: -->

6. If you added more than one custom node, choose the size of your node quorum

7. Click **Save**

:::success:おめでとうございます！:tada:
タングルについてのトリニティが表示するすべての情報は、カスタムノードから送信されました。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- All the information that Trinity displays about the Tangle is now sent from your custom nodes. -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

[あなたの新しいノードにトランザクションを送信する](../how-to-guides/send-a-transaction.md)。
<!-- [Send a transaction](../how-to-guides/send-a-transaction.md) to your new nodes. -->
