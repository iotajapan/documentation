# トリニティが読み取れる QR コードを作成する
<!-- # Create a QR Code that Trinity can read -->

**送信者にアドレスやメッセージなどのトランザクション情報を入力させないようにするために，トリニティのトランザクションフィールドに自動入力する QR コードを作成できます．このガイドでは，IOTA ユーティリティ Web サイトを使用して QR コードを作成します．**
<!-- **To avoid making senders enter transaction information such as your address or a message, you can create a QR code that auto-populates the transaction fields in Trinity. In this guide, you create a QR code, using the IOTA utility website.** -->

QRコードは，アドレス，金額，およびメッセージの各フィールドに自動入力することで，トリニティでトランザクションを送信するプロセスをスピードアップします．送信者が QR コードをスキャンできるようにすることで，e コマース Web サイトなどの受信者は，顧客が購入を完了するのにかかる時間を短縮できます．
<!-- QR codes speed up the process of sending a transaction in Trinity by auto-populating the Address, Value, and Message fields. By allowing senders to scan a QR code, recipients such as ecommerce websites can reduce the time it takes for customers to complete a purchase. -->

![Example QR code](../images/qr-code.png)

1. トリニティで新しいアドレスを生成します．
  <!-- 1. Generate a new address in Trinity -->

    :::info:
    トリニティのアドレスに送信するには，アドレスに9トライトのチェックサムを含める必要があります．トリニティで新しいアドレスを生成した場合，そのアドレスにはすでにチェックサムが含まれています．
    :::
    <!-- :::info: -->
    <!-- To send to an address in Trinity, it must include the 9-tryte checksum. If you generated a new address in Trinity, that address will already include the checksum. -->
    <!-- ::: -->

2. [公式 IOTA ユーティリティ Web サイト](https://utils.iota.org/qr-create)にアクセスします．
<!-- 2. Go to the [official IOTA utility website](https://utils.iota.org/qr-create) -->

3. フィールドに入力します．
<!-- 3. Complete the fields -->

4. QR コードを PNG ファイルとして保存します．
<!-- 4. Save the QR code as a PNG file -->

5. 送信者に QR コードを送信するか，Web サイトに追加します．
<!-- 5. Send the QR code to your sender, or add it to your website -->

:::success:
送信者がトリニティで QR コードをスキャンすると，トリニティのトランザクションフィールドに QR コード内のトランザクションフィールド情報が自動的に入力されます．
:::
<!-- :::success: -->
<!-- When the sender scans the QR code in Trinity, the transaction fields will be auto-populated with the ones from the QR code. -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

[ディープリンクを作成する](../how-to-guides/create-deep-link.md)
<!-- [Create a deep link](../how-to-guides/create-deep-link.md) -->
