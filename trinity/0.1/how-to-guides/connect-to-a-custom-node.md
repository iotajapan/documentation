# カスタムノードに接続する
<!-- # Connect to custom nodes -->

**デフォルトの[ノード定足数](../concepts/node-quorum.md)の代わりに、またはそれに加えて、1つ以上のカスタムノードを接続できます。たとえば、あなた自身のノードに接続することができたり、さらにはDevnet上のノードに接続したりすることができます。**
<!-- **You can connect to one or more custom nodes either instead of the default [node quorum](../concepts/node-quorum.md) or in addition to it. For example, you may want to be able to connect to your own nodes or even connect to nodes on the Devnet.** -->

:::info:
トリニティのすべてのノードはHTTPSプロトコルを使用する必要があります。
:::
<!-- :::info: -->
<!-- All nodes in Trinity must use the HTTPS protocol. -->
<!-- ::: -->

![Node management in Trinity](../images/node-management.png)

## カスタムノードに接続する
<!-- ## Connect to custom nodes -->

単一のカスタムノードに接続するだけでなく、複数のカスタムノードを追加してそれらをノード定足数として使用することもできます。このようにして、内蔵のリモートノードリストを使うことに制限されません。
<!-- As well as connecting to a single custom node, you can also add multiple custom nodes and use them as a node quorum. This way, you aren't restricted to using the built-in remote list of nodes that Trinity uses. -->

1. **設定** > **ノード** > **カスタムノードの追加**に移動し、追加するノードのURLまたはIPアドレスを入力します
  <!-- 1. Go to **Settings** > **Node** > **Add custom nodes**,  and enter the URL or IP address of the nodes you want to add -->

    :::info:
    カスタムノードの定足数に接続するには、複数のカスタムノードを追加します。
    :::
    <!-- :::info: -->
    <!-- To connect to a quorum of custom nodes, add more than one. -->
    <!-- ::: -->

2. **自動でノード管理を行う**オプションを無効にします。
  <!-- 2. Disable the **Automatic node management** option -->

3. **主要ノードの自動切替え**オプションを無効にします。
  <!-- 3. Disable the **Primary node autoswitching** option -->

    :::warning:
    このオプションを有効にすると、主要ノードがオフラインになった場合に、トリニティのリストのデフォルトノードとは異なるノードに切り替わります。

    カスタムノードがオフラインになると、トリニティはどのノードにも接続できず、表示されている情報を更新することができなくなります。
    :::
    <!-- :::warning: -->
    <!-- When enabled, this option switches to a different node from Trinity's list of defaults if the primary one goes offline. -->
    <!--  -->
    <!-- If your node goes offline, Trinity won't be able to connect to any nodes to update the information it displays. -->
    <!-- ::: -->

4. ドロップダウンメニューからカスタムノードを選択します。
  <!-- 4. Select your node from the dropdown menu -->

    :::warning:Devnetノード
    カスタムノードがDevnetノードの場合は、**リモートノードリストを使う**および**主要ノードの自動切替え**オプションも無効にする必要があります。リモートノードリストのノードはMainnetノードなので、Devnetと互換性がありません。
    :::
    <!-- :::warning:Devnet nodes -->
    <!-- If your custom node is a Devnet node, you must also disable the **Use remote list** and **Primary node autoswitching** options. The nodes in that list are Mainnet nodes, so they're incompatible with the Devnet. -->
    <!-- ::: -->

5. **保存**をクリックします。
  <!-- 5. Click **Save** -->

:::success:おめでとうございます！:tada:
タングルについてのトリニティが表示するすべての情報は、カスタムノードから送信されました。
:::
<!-- :::success:Congratulations! :tada: -->
<!-- All the information that Trinity displays about the Tangle is now sent from your custom nodes. -->
<!-- ::: -->
