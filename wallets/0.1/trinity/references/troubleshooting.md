# トラブルシューティング
<!-- # Troubleshooting -->

**このリファレンスガイドを使用して、トリニティに関連する問題を解決してください。**
<!-- **Use this reference guide to resolve issues related to Trinity.** -->

問題の解決策が見つからない場合は、公式IOTA [Discord](https://discord.iota.org/)の`ヘルプ`チャンネルでトリニティチームに連絡してください。
<!-- If you can't find the solution to your issue, reach out to the Trinity team on the `help` channel of the official IOTA [Discord](https://discord.iota.org/). -->

## 不正確な残高
<!-- ## Incorrect balance -->

トリニティがノードに接続できない場合、不正確な残高が表示される場合があります。
<!-- If Trinity can't connect to a node, it may display an incorrect balance. -->

この問題を解決するために、トリニティは生成されたアドレスのリストを保持して、トリニティが次にノードに接続したときに再同期できるようにします。
<!-- To fix this problem, Trinity keeps a list of your generated addresses so that you can re-synchronize it the next time Trinity connects to a node. -->

残高が間違っている（および[グローバルスナップショット](../how-to-guides/perform-a-snapshot-transition.md)が発生していない）と思われる場合は、**設定** > **アカウント** > **アカウント管理** > **ツール** > **アカウントを同期**に移動して、トリニティを同期できます 。
<!-- If you think your balance is wrong (and a [global snapshot](../how-to-guides/perform-a-snapshot-transition.md) hasn't occurred), you can synchronize Trinity by going to **Settings** > **Account** > **Account management** > **Tools** > **Sync account**. -->

![Manual update](../images/sync.jpg)

## ペンディングトランザクション
<!-- ## Pending transaction -->

[タングル](root://getting-started/0.1/network/the-tangle.md)のトランザクションが長時間ペンディングになっている場合、[自動促進設定](../how-to-guides/auto-promote.md)が**有効**に設定されていることを確認してください。
<!-- If a transaction on the [Tangle](root://getting-started/0.1/network/the-tangle.md) is pending for a long time, make sure that the [Auto-promotion setting](../how-to-guides/auto-promote.md) is set to **Enabled**. -->

:::info:
自動促進は、モバイル端末では、トリニティが前面に表示されている場合にのみ利用できます。
:::
<!-- :::info: -->
<!-- Auto-promotion is available on mobile devices only when Trinity is in the foreground. -->
<!-- ::: -->

## トランザクションを送信できません
<!-- ## Unable to send a transaction -->

以下のいずれかの理由により、トリニティはトランザクションを送信するのを阻止することがあります。
<!-- Trinity may stop you from sending a transaction for any of the following reasons: -->

- [使用済みアドレス](root://getting-started/0.1/clients/addresses.md#spent-addresses)に資金がある場合、トリニティはIOTAトークンを保護するためにそのアドレスからの取り出しを停止します。
<!-- - If you have funds on a [spent address], Trinity stops you withdrawing from that address to protect your IOTA tokens. -->
- 送信先のアドレスが使用済みアドレスの場合、トリニティはそのアドレスへの送信を停止して、IOTAトークンを保護します。この場合、受信者に新しいアドレスを尋ねてください。
<!-- - If the address you are sending to is spent, Trinity will stop you from sending to that address to protect your IOTA tokens. In this case, ask the recipient for a new address. -->
- 複数のトランザクションを送信する場合、最初のトランザクションが確定されるのを待ってから別のトランザクションを送信する必要がある場合があります。
<!-- - If you are sending more than one transaction, you may need to wait for your first transaction to be confirmed before sending another one -->

## デバイスを紛失した
<!-- ## Lost access to a device -->

デバイスにアクセスできなくなると、パスワードなしでは誰もアカウントにアクセスできなくなります。アカウントを回復するには、トリニティを別のデバイスにインストールし、バックアップしたアカウントのシードを入力します。
<!-- If you lose access to your device, no one can access your account without your password. To recover your account, install Trinity on another device and enter your account's seed that you backed up. -->

:::info:
保護を強化するために、新しいアカウントを作成し、IOTAトークンをその新しいアカウントのシードに属するアドレスに転送してください。
:::
<!-- :::info: -->
<!-- For extra protection, create a new account and transfer your IOTA tokens to an address that belongs to that new account's seed. -->
<!-- ::: -->
