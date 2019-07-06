# ディープリンクを作成する
<!-- # Create a deep link -->

**ディープリンクは、トリニティのトランザクションフィールドに自動入力されるカスタマイズ可能なリンクです。 Webサイトまたはeコマースプラットフォームにディープリンクを追加して、顧客にとってIOTA支払いをより迅速かつ容易にすることができます。**
<!-- **Deep links are customizable links that auto-populate the transaction fields in Trinity. You can add deep links to your website or ecommerce platform to make IOTA payments quicker and easier for customers.** -->

ディープリンクをクリックすると、トリニティが開き、リンクのパラメータを使用してトランザクションフィールドに自動入力されます。
<!-- When you click a deep link, Trinity is opened and the parameters of the link are used to auto-populate the transaction fields. -->

ディープリンクのパラメータを使用して、トランザクションのアドレス、金額、およびメッセージフィールドを指定できます。
<!-- You can use these parameters to specify the address, amount, and message fields of a transaction. -->

:::warning:
フィッシング詐欺に注意してください。リンクをクリックすると、公式のトリニティウォレットが開きます。
:::
<!-- :::warning: -->
<!-- Beware of phishing scams. Make sure the link opens the official Trinity wallet. -->
<!-- ::: -->

すべてのディープリンクは、`iota://`URIで始まり、その後に90トライトのアドレス（チェックサム付き）が続く必要があります。
<!-- All deep links must start with the `iota://` URI followed by a 90-tryte address (with checksum). -->

金額またはメッセージを指定する場合は、次のクエリパラメータを使用できます。
<!-- If you want to specify an amount or message, you can use the following query parameters: -->

* `amount`
* `message`

例（以下のリンクをクリックするとテストが開きます）：
<!-- For example (you can click this link to test it): -->

<iota://XNGPUCURQLLJFGXNDCMROGYNWAZP9AFWSVEUAIWIESOSPYDUPWSPSREEBWJPD9ZWZPAJKBHPLG99DJWJCZUHWTQTDD/?amount=1000000&message=shirt>

:::info:
ハードウェアウォレットにリンクされているアカウントは、メッセージ付きのバリュートランザクションを送信できません。
:::
<!-- :::info: -->
<!-- Accounts that are linked to hardware wallets can't send value transactions with a message. -->
<!-- ::: -->

1. **設定** > **詳細設定**に進み、ディープリンクを有効にします。
  <!-- 1. Go to **Settings** > **Advanced settings** and enable deep linking -->

2. ホームページに戻り、**受取**をクリックして新しいアドレスを生成します。
  <!-- 2. Go back to the home page and click **Receive** to generate a new address -->

3. 新しいアドレスを使用して、トリニティの外部に手動でディープリンクを作成します。
  <!-- 3. Manually create a deep link outside of Trinity, using the new address -->

4. ウェブサイトやeコマースプラットフォームにディープリンクを埋め込みます。
  <!-- 4. Embed the deep link on your website or ecommerce platform -->

    :::info:
    現在、ディープリンクでメッセージにスペースを追加することはできません。
    :::
    <!-- :::info: -->
    <!-- You currently can't add spaces to the message in deep links. -->
    <!-- ::: -->

ディープリンクをテストするには、リンクをブラウザのアドレスバーにコピーして貼り付けます。トリニティが開き、自動入力されたトランザクションフィールドが表示されます。
<!-- To test your deep link, copy and paste the link into a browser's address bar. Trinity will open and you will see the auto-populated transaction fields. -->
