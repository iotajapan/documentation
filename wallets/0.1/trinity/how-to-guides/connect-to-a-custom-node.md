# カスタムノードに接続する
<!-- # Connect to custom nodes -->

**トリニティは，デフォルトノードの[クォーラム](../concepts/node-quorum.md)を介して[タングル](root://getting-started/0.1/network/the-tangle.md)と対話します．このガイドでは，自分自身のノードの1つをクォーラムに追加する方法，[デブネット](root://getting-started/0.1/network/iota-networks.md)などのカスタムノードのクォーラムに接続する方法，または特定のノードに接続してプルーフオブワークを外部委託する方法を学習します．**
<!-- **Trinity interacts with the [Tangle](root://getting-started/0.1/network/the-tangle.md) through a [quorum](../concepts/node-quorum.md) of default nodes. In this guide, you learn how to add one of your own nodes to the quorum, connect to a quorum of custom nodes such as those on the [Devnet](root://getting-started/0.1/network/iota-networks.md), or connect to a specific node to use for outsourced proof of work.** -->

:::danger:
2020年2月11日、IOTA 財団は、一部のユーザーのシードと Trinity パスワードが侵害された Trinity ウォレットに対する攻撃に気付きました。[Trinity アカウントの保護](../how-to-guides/protect-trinity-account.md)に関するアドバイスを確認してください。
:::
<!-- :::danger: -->
<!-- On 11 February 2020, the IOTA Foundation became aware of an attack on the Trinity wallet, during which some users’ seeds and Trinity passwords were compromised. Please check our advice for [protecting your Trinity account](../how-to-guides/protect-trinity-account.md). -->
<!-- ::: -->

![Node management in Trinity](../images/node-management.png)

## トリニティをカスタムノードに接続する
<!-- ## Connect Trinity to custom nodes -->

単一のカスタムノードに接続するだけでなく，複数のカスタムノードを追加してそれらをノードクォーラムとして使用することもできます．このようにして，トリニティ内蔵のリモートノードリストを使うことに制限されません．
<!-- As well as connecting to a single custom node, you can also add multiple custom nodes and use them as a node quorum. This way, you aren't restricted to using the built-in remote list of Trinity nodes. -->

--------------------
### デスクトップ
<!-- ### Desktop -->

1. **Trinity** > **設定** > **ノード** > **カスタムノードの追加**に移動し，追加するノードの URL または IP アドレスを入力します
  <!-- 1. Go to **Trinity** > **Settings** > **Node** > **Add custom nodes**,  and enter the URL or IP address of the nodes you want to add -->

    トリニティのすべてのノードは，HTTPS を介して通信する必要があります．
    <!-- All nodes in Trinity must communicate over HTTPS. -->

2. [プルーフオブワーク]を外部委託する場合は，[**プルーフオブワークを外部委託する**]オプションを有効にします．
  <!-- 2. If you want to outsource [proof of work](root://getting-started/0.1/transactions/proof-of-work.md), enable the **Outsource proof of work** option -->

    デフォルトでは，トリニティを実行しているコンピューターでプルーフオブワークが行われます．
    <!-- By default proof of work is done on the device that is running Trinity. -->

3. **自動ノード管理**オプションを無効にして、特定のノードに接続できるようにします。
<!-- 3. Disable the **Automatic node management** option to be able to connect to a specific node -->

4. **主要ノードの自動切り替え**オプションを無効にして、Trinity が常に選択したノードへの接続を試行するようにします。
<!-- 4. Disable the **Primary node autoswitching** option to make sure that Trinity always tries to connect to your chosen node -->

    このオプションを有効にすると，トリニティは，主要ノードがオフラインになった場合にデフォルトノードの1つに接続できます．このオプションを無効にすると，主要ノードがオフラインになった場合，トリニティはどのノードにも接続できなくなります．
    <!-- When enabled, this option allows Trinity to connect to one of its default nodes if the primary one goes offline. By disabling this option, Trinity won't be able to connect to any nodes if the primary node goes offline. -->

5. ドロップダウンメニューからノードを選択します。
  <!-- 5. Select your node from the dropdown menu -->

    カスタムノードがデブネットノードである場合、**リモートリストを使用する**オプションも無効にする必要があります。このリスト内のノードはメインネットノードであるため、デブネットと互換性がありません。
    <!-- If your custom node is a Devnet node, you must also disable the **Use remote list** option. The nodes in that list are Mainnet nodes, so they're incompatible with the Devnet. -->

6. 複数のカスタムノードを追加した場合は、ノードクォーラムのサイズを選択します。
<!-- 6. If you added more than one custom node, choose the size of your node quorum -->

7. **保存**をクリックします．
<!-- 7. Click **Save** -->
---
### モバイル
<!-- ### Mobile -->

1. **設定** > **ノード** > **カスタムノードの追加**に移動し，追加するノードの URL または IP アドレスを入力します
  <!-- 1. Go to **Settings** > **Node** > **Add custom nodes**,  and enter the URL or IP address of the nodes you want to add -->

    トリニティのすべてのノードは，HTTPS を介して通信する必要があります．
    <!-- All nodes in Trinity must communicate over HTTPS. -->

2. [プルーフオブワーク]を外部委託する場合は，[**プルーフオブワークを外部委託する**]オプションを有効にします．
  <!-- 2. If you want to outsource [proof of work](root://getting-started/0.1/transactions/proof-of-work.md), enable the **Outsource proof of work** option -->

    デフォルトでは，トリニティを実行しているコンピューターでプルーフオブワークが行われます．
    <!-- By default proof of work is done on the device that is running Trinity. -->

3. **自動ノード管理**オプションを無効にして、特定のノードに接続できるようにします。
<!-- 3. Disable the **Automatic node management** option to be able to connect to a specific node -->

4. **主要ノードの自動切り替え**オプションを無効にして、Trinity が常に選択したノードへの接続を試行するようにします。
<!-- 4. Disable the **Primary node autoswitching** option to make sure that Trinity always tries to connect to your chosen node -->

    このオプションを有効にすると，トリニティは，主要ノードがオフラインになった場合にデフォルトノードの1つに接続できます．このオプションを無効にすると，主要ノードがオフラインになった場合，トリニティはどのノードにも接続できなくなります．
    <!-- When enabled, this option allows Trinity to connect to one of its default nodes if the primary one goes offline. By disabling this option, Trinity won't be able to connect to any nodes if the primary node goes offline. -->

    カスタムノードがデブネットノードの場合は，**リモートノードリストを使う**オプションも無効にする必要があります．このリスト内のノードはメインネットノードであるため，デブネットと互換性がありません．
    <!-- If your custom node is a Devnet node, you must also disable the **Use remote list** option. The nodes in that list are Mainnet nodes, so they're incompatible with the Devnet. -->

6. 複数のカスタムノードを追加した場合は，ノードクォーラムのサイズを選択します．
<!-- 6. If you added more than one custom node, choose the size of your node quorum -->

7. **保存**をクリックします．
<!-- 7. Click **Save** -->
--------------------

:::success:
タングルについてのトリニティが表示するすべての情報は，カスタムノードから送信されました．
:::
<!-- :::success:Congratulations! :tada: -->
<!-- All the information that Trinity displays about the Tangle is now sent from your custom nodes. -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

[あなたの新しいノードにトランザクションを送信する](../how-to-guides/send-a-transaction.md)．
<!-- [Send a transaction](../how-to-guides/send-a-transaction.md) to your new nodes. -->
