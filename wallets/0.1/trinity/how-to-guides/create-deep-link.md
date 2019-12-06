# ディープリンクを作成する
<!-- # Create a deep link -->

**Deep links are customizable links that auto-populate the transaction fields in Trinity. You can add deep links to your website or ecommerce platform to make IOTA payments quicker and easier for customers. In this guide, you create a deep link that, when entered in a browser, opens Trinity and auto-populates the amount and message of a transaction.**

All deep links must start with the `iota://` URI followed by a 90-tryte address (with [checksum](root://getting-started/0.1/clients/checksums.md)).

If you want to specify an amount or message, you can use the following query parameters after the address:

- `amount`
- `message`

For example (if you have Trinity installed, you can click this link to test it):

<iota://XNGPUCURQLLJFGXNDCMROGYNWAZP9AFWSVEUAIWIESOSPYDUPWSPSREEBWJPD9ZWZPAJKBHPLG99DJWJCZUHWTQTDD/?amount=1000000&message=shirt>

:::info:
ハードウェアウォレットにリンクされているアカウントは、IOTAトークンまたはメッセージのみを送信できます。
:::
<!-- :::info: -->
<!-- Accounts that are linked to hardware wallets can send only IOTA tokens or a message. -->
<!-- ::: -->

1. **設定** > **詳細設定**に進み、ディープリンクを有効にします。
  <!-- 1. Go to **Settings** > **Advanced settings** and enable deep linking -->

2. ホームページに戻り、**受取**をクリックして新しいアドレスを生成します。
  <!-- 2. Go back to the home page and click **Receive** to generate a new address -->

3. 新しいアドレスを使用して、ディープリンクを作成します。
<!-- 3. Create a deep link, using the new address -->

4. ウェブサイトやeコマースプラットフォームにディープリンクを埋め込みます。
  <!-- 4. Embed the deep link on your website or ecommerce platform -->

    :::info:
    現在、ディープリンクでメッセージにスペースを追加することはできません。
    :::
    <!-- :::info: -->
    <!-- You currently can't add spaces to the message in deep links. -->
    <!-- ::: -->

5. ディープリンクをテストするには、リンクをブラウザのアドレスバーにコピーして貼り付けます。トリニティが開くと、自動入力されたトランザクションフィールドが表示されます。
  <!-- 5. To test your deep link, copy and paste the link into a browser's address bar. When Trinity opens, you will see the auto-populated transaction fields. -->

    :::warning:
    フィッシング詐欺に注意してください。リンクが公式のトリニティウォレットを開くことを確認してください。
    :::
    <!-- :::warning: -->
    <!-- Beware of phishing scams. Make sure the link opens the official Trinity wallet. -->
    <!-- ::: -->

## 次のステップ
<!-- ## Next steps -->
