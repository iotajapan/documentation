# トランザクションを送信する
<!-- # Send a transaction -->

**トリニティのユーザーインターフェイスを使用すると、ボタンをクリックするだけでトランザクションを送信できます。**
<!-- **Trinity's user interface allows you to send transactions at the click of a button.** -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完了するには、次のものが必要です。
<!-- To complete this guide, you need the following: -->

- トリニティのアカウント
<!-- - A Trinity account -->
- トランザクションに含めるアドレス
<!-- - An address to include in the transaction -->

:::info:
Addresses must include the 9-tryte [checksum](root://getting-started/0.1/clients/checksums.md). If you generated a new address in Trinity, that address will already include the checksum.
:::

---
1. **送金**をクリックします。
  <!-- 1. Click **Send** -->

2. 受取人のアドレスを入力します。
  <!-- 2. Enter a recipient's address -->

  :::danger:重要
  Androidデバイスでは、どのアプリケーションでもキーボードの押下の内容を読み取り、変更することができます。アドレスをトリニティに貼り付けるときは注意して、貼り付けたアドレスがコピーしたものと同じであることを確認してください。
  :::
  <!-- :::danger:Important -->
  <!-- On Android devices, any application can read the content of keyboard presses and modify it. Be careful when pasting an address into Trinity and make sure that the address you have pasted is the same one that you copied. -->
  <!-- ::: -->

3. メッセージまたは一定量のIOTAトークン、あるいはその両方を入力します。
  <!-- 3. Enter either a message, or an amount of IOTA tokens, or both -->

    :::info:
    送金する金額は、[IOTAトークンの単位](root://getting-started/0.1/clients/token.md#units-of-iota-tokens)または[選択した通貨](../how-to-guides/change-the-general-settings.md)で入力できます。送信する金額に注意してください。
    :::
    <!-- :::info: -->
    <!-- The amount to send can be entered in [units of IOTA tokens](root://getting-started/0.1/clients/token.md#units-of-iota-tokens) or in the [currency of your choice](../how-to-guides/change-the-general-settings.md). Pay close attention to the amount that you're sending. -->
    <!-- ::: -->

    :::info:
    アカウントがハードウェアウォレットにリンクされている場合、IOTAトークンまたはメッセージのいずれかを送信できますが、両方は送信できません。
    :::
    <!-- :::info: -->
    <!-- If your account is linked to a hardware wallet, you can send either IOTA tokens or a message, but not both. -->
    <!-- ::: -->

    ![IOTA units](../images/sending-value-highlighted.jpg)

4. **送金**をクリックします。
  <!-- 4. Click **Send** -->

    ![Sending IOTA tokens](../images/send.jpg)

    :::info:
    トランザクションの送信中にトリニティモバイルを最小化した場合、送信プロセスは、ユーザーがトリニティモバイルを再度開くまで一時停止します。
    :::
    <!-- :::info: -->
    <!-- If you minimize Trinity Mobile while sending a transaction, the sending process pauses until you reopen it. -->
    <!-- ::: -->

トランザクションを受信または送信すると、トランザクションがホームページに表示されます。一覧からトランザクションをクリックして、トランザクションに関する情報を表示できます。
<!-- When you either receive or send a transaction, it appears on the home page. Click a transaction from the list to view information about it. -->

![A received transaction](../images/trinity-receive-message.png)

:::info:
アドレスフィールドは、[上級モード](../how-to-guides/change-the-general-settings.md)でのみ表示されます。
:::
<!-- :::info: -->
<!-- The Addresses field is displayed only in [Advanced mode](../how-to-guides/change-the-general-settings.md). -->
<!-- ::: -->
