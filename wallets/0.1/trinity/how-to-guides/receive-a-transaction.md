# トランザクションを受信する
<!-- # Receive a transaction -->

**トリニティには、トランザクションを受信し、ボタンをクリックするだけで新しいアドレスを生成することができるユーザー・インターフェースがあります。**
<!-- **Trinity has a user interface that allows you to receive transactions and generate new addresses at the click of a button.** -->

:::danger:重要
アドレスの1つからIOTAトークンを取り出すトランザクションを送信した場合、そのアドレスからは2度とIOTAトークンを取り出してはいけません。代わりに、新しいアドレスを生成する必要があります。
:::
<!-- :::danger:Important -->
<!-- If you send a transaction that withdraws IOTA tokens from one of your addresses, you must never withdraw from that address again. Instead, you must generate a new address. -->
<!-- ::: -->

1. **受取**をクリックします。
<!-- 1. Click **Receive** -->
2. 新しいアドレスを生成するには、**アドレスを生成**をクリックします。
<!-- 2. Click **Sync address** (**Generate address** on a mobile device) to generate a new address -->
3. アドレスをコピーして送信者に渡すか、[QRコードを作成する](../how-to-guides/create-a-qr-code.md)か、または[ディープリンクを作成します](../how-to-guides/create-deep-link.md)。
  <!-- 3. Either copy the address and give it to the sender, [create a QR code](../how-to-guides/create-a-qr-code.md), or [create a deep link](../how-to-guides/create-deep-link.md). -->

    :::danger:重要
    Androidデバイスでは、どのアプリケーションでもキーボードの押下の内容を読み取り、変更することができます。アドレスをトリニティに貼り付けるときは注意して、貼り付けたアドレスがコピーしたものと同じであることを確認してください。
    :::
    <!-- :::danger:Important -->
    <!-- On Android devices, any application can read the content of keyboard presses and modify it. Be careful when pasting an address into another app and make sure that the address you have pasted is the same one that you copied. -->
    <!-- ::: -->

    ![Receiving a transaction](../images/receive.jpg)

トランザクションを受信または送信すると、トランザクションがホームページに表示されます。一覧からトランザクションをクリックして、トランザクションに関する情報を表示できます。
<!-- When you either receive or send a transaction, it appears on the home page. Click a transaction from the list to view information about it. -->

![A received transaction](../images/trinity-receive-message.png)

:::info:
アドレスフィールドは、[上級モード](../how-to-guides/change-the-advanced-settings.md)でのみ表示されます。
:::
<!-- :::info: -->
<!-- The Addresses field is displayed only in [Advanced mode](../how-to-guides/change-the-general-settings.md). -->
<!-- ::: -->
