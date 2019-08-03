# トランザクションを送信する
<!-- # Send a transaction -->

**トリニティには、ボタンをクリックするだけで複数のトランザクションを送信できるユーザーインターフェースがあります。**
<!-- **Trinity has a user interface that allows you to send multiple transactions at the click of a button.** -->

トリニティウォレットに複数のアカウントがある場合は、必ずドロップダウンメニューから正しいアカウントを選択してください。
<!-- If you have multiple accounts in your Trinity wallet, make sure that you select the correct account from the dropdown menu. -->

:::info:
トリニティのアドレスに送信するには、9トライトのチェックサムを含める必要があります。トリニティで新しいアドレスを生成した場合、そのアドレスには既にチェックサムが含まれています。
:::
<!-- :::info: -->
<!-- To send to an address in Trinity, it must include the 9-tryte checksum. If you generated a new address in Trinity, that address will already include the checksum. -->
<!-- ::: -->

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
    送金する金額は、IOTA単位または[選択した通貨](../how-to-guides/change-the-general-settings.md)で入力できます。送金している[IOTA単位](root://dev-essentials/0.1/references/units-of-iota-tokens.md)（i、Mi、Gi）に細心の注意を払ってください。
    :::
    <!-- :::info: -->
    <!-- The amount to send can be entered in units of IOTA or in the [currency of your choice](../how-to-guides/change-the-general-settings.md). Pay close attention to the [IOTA units](root://dev-essentials/0.1/references/units-of-iota-tokens.md) (i, Mi, Gi,) that you're sending. -->
    <!-- ::: -->

    :::info:
    ハードウェアウォレットにリンクされているアカウントは、メッセージ付きのバリュートランザクションを送信できません。

    ハードウェアウォレットを使用している場合は、IOTAトークンまたはメッセージの片方を送信できますが、両方を送信することはできません。
    :::
    <!-- :::info: -->
    <!-- Accounts that are linked to hardware wallets can't send value transactions with a message. -->
    <!--  -->
    <!-- If you're using a hardware wallet, you can send either IOTA tokens or a message, but not both. -->
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
アドレスフィールドは、[上級モード](../how-to-guides/change-the-advanced-settings.md)でのみ表示されます。
:::
<!-- :::info: -->
<!-- The Addresses field is displayed only in [Advanced mode](../how-to-guides/change-the-advanced-settings.md). -->
<!-- ::: -->
