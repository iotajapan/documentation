# ディープリンクを作成する
<!-- # Create a deep link -->

**ディープリンクは，トリニティを開いてトランザクションフィールドに自動入力するカスタマイズ可能なリンクです．ウェブサイトまたはEコマースプラットフォームにディープリンクを追加して，顧客に IOTA 支払いを迅速かつ簡単にすることができます．このガイドでは，ブラウザに入力すると，トリニティを開き，トランザクションの IOTA トークンの量とメッセージを自動入力するディープリンクを作成します．**
<!-- **Deep links are customizable links that open Trinity and auto-populate transaction fields. You can add deep links to your website or ecommerce platform to make IOTA payments quicker and easier for customers. In this guide, you create a deep link that, when entered in a browser, opens Trinity and auto-populates the amount and message of a transaction.** -->

1. **設定** > **詳細設定** に行き，ディープリンクを有効にします．
<!-- 1. Go to **Settings** > **Advanced settings** and enable deep linking -->

2. ホームページに戻り，**受取**をクリックして新しいアドレスを生成します．
<!-- 2. Go back to the home page and click **Receive** to generate a new address -->

3. 新しいアドレスを使用して，ディープリンクを作成します．
  <!-- 3. Create a deep link, using the new address -->

    すべてのディープリンクは，`iota://` URI で始まり，その後に[チェックサム](root://getting-started/0.1/clients/checksums.md)付きの90トライトのアドレスが続く必要があります．
    <!-- All deep links must start with the `iota://` URI followed by a 90-tryte address (with [checksum](root://getting-started/0.1/clients/checksums.md)). -->

    金額またはメッセージを指定する場合は，アドレスの後に次のクエリパラメーターを使用できます．
    <!-- If you want to specify an amount or message, you can use the following query parameters after the address: -->

    - `amount`
    - `message`

    たとえば（トリニティがインストールされている場合は，このリンクをクリックしてテストできます）：
    <!-- For example (if you have Trinity installed, you can click this link to test it): -->

    <iota://XNGPUCURQLLJFGXNDCMROGYNWAZP9AFWSVEUAIWIESOSPYDUPWSPSREEBWJPD9ZWZPAJKBHPLG99DJWJCZUHWTQTDD/?amount=1000000&message=shirt>

    :::info:
    ハードウェアのウォレットにリンクされているアカウントは，IOTA トークンまたはメッセージだけを送信することができます．
    :::
    <!-- :::info: -->
    <!-- Accounts that are linked to hardware wallets can send only IOTA tokens or a message. -->
    <!-- ::: -->

4. web サイトや e コマースプラットフォームにディープリンクを埋め込みます．
  <!-- 4. Embed the deep link on your website or ecommerce platform -->

    :::info:
    現在，ディープリンクでメッセージにスペースを追加することはできません．
    :::
    <!-- :::info: -->
    <!-- You currently can't add spaces to the message in deep links. -->
    <!-- ::: -->

5. ディープリンクをテストするには，リンクをブラウザのアドレスバーにコピーして貼り付けます．トリニティが開くと，自動入力されたトランザクションフィールドが表示されます．
  <!-- 5. To test your deep link, copy and paste the link into a browser's address bar. When Trinity opens, you will see the auto-populated transaction fields. -->

    :::warning:
    フィッシング詐欺に注意してください．リンクが公式のトリニティウォレットを開くことを確認してください．
    :::
    <!-- :::warning: -->
    <!-- Beware of phishing scams. Make sure the link opens the official Trinity wallet. -->
    <!-- ::: -->

:::success:
これで，ディープリンクを使用して，トリニティを自動的に開き，トランザクションフィールドに入力できます．
:::
<!-- :::success: -->
<!-- Now you can use deep links to automatically open Trinity and populate the transaction fields. -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

[QR コードを作成する](../how-to-guides/create-a-qr-code.md)
<!-- [Create a QR code](../how-to-guides/create-a-qr-code.md) -->
