# JavaScript で条件付きデポジットアドレスを生成する
<!-- # Generate a conditional deposit address in JavaScript -->

**このガイドでは、条件付きデポジットアドレスを生成し、マグネットリンクにシリアル化し、テスト IOTA トークンを送信します。**
<!-- **In this guide, you generate a conditional deposit address, serialize it into a magnet link, and send test IOTA tokens to it.** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには、次のパッケージをインストールする必要があります。
<!-- To complete this guide, you need to install the following packages: -->

--------------------
### npm
```bash
npm install @iota/account @iota/cda ntp-client
```
---
### Yarn
```bash
yarn add @iota/account @iota/cda ntp-client
```
--------------------

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. 持ちたい条件付きデポジットアドレスの[条件を計画します](../introduction/overview.md#advice-for-creating-cdas)。
<!-- 1. [Plan the conditions](../introduction/overview.md#advice-for-creating-cdas) that you would like your conditional deposit address to have -->

2. 新しい CDA を作成します。この CDA は翌日に期限切れになります。
  <!-- 2. Create a new CDA. This one expires tomorrow. -->

    ```js
    const timeSource = () => util.promisify(ntpClient.getNetworkTime)("time.google.com", 123);

    timeSource().then((time => account.generateCDA({
        timeoutAt: time + 24 * 60 * 60 * 1000, // 1 day from now
    }).then(cda => {

    });
    ```

    :::info:
    デフォルトでは、この方法はアカウントのセキュリティレベル設定を使用して CDA を生成します。異なるセキュリティレベルで CDA を生成するには、`generateCDA()` メソッドに `security` 引数を渡します。
    :::
    <!-- :::info: -->
    <!-- By default, this method generates a CDA, using your account's security level settings. To generate a CDA with a different security level, pass the `generateCDA()` method a `security` argument. -->
    <!-- ::: -->

    ```js
    account.generateCDA({
        timeoutAt: Date.now() + 24 * 60 * 60 * 1000,
        security: 3
    }).then(cda => {
    ```

3. `AsMagnetLink()` メソッドを使用して CDA をマグネットリンクにシリアル化し、コンソールに出力します。
  <!-- 3. Use the `AsMagnetLink()` method to serialize the CDA into a magnet link and print it to the console -->

    ```js
    const magnetLink = CDA.serializeCDAMagnet(cda);
    console.log(magnetLink);
    ```

    :::info:
    CDA の最後の9つのトライトはチェックサムであり、これはアドレスとそのアドレスのすべての条件のハッシュです。
    :::
    <!-- :::info: -->
    <!-- The last 9 trytes of a CDA are the checksum, which is a hash of the address and all of its conditions. -->
    <!-- ::: -->

4. アドレスをコピーして[デブネット蛇口](https://faucet.devnet.iota.org)にペーストし、トークンがアドレスに転送されるのを待ちます。
  <!-- 4. Copy and paste your address into the [Devnet faucet](https://faucet.devnet.iota.org), then wait for the tokens to be transferred to your address -->

    :::info:
    デブネット蛇口から IOTA トークンをリクエストする前に、チェックサムを必ず削除してください。
    :::
    <!-- :::info: -->
    <!-- Make sure to remove the checksum before requesting IOTA tokens from the Devnet faucet. -->
    <!-- ::: -->

    アドレスの例
    <!-- For example: -->

    ```bash
    DL9CSYICJVKQRUTWBFUCZJQZ9WNBSRJOA9MGOISQZGGHOCZTXVSKDIZN9HBORNGDWRBBAFTKXGEJIAHKD
    ```

:::success:
これで IOTA トークンを含む CDA ができたので、支払いを行うことができます。
:::
<!-- :::success: -->
<!-- Now you have a CDA that contains IOTA tokens, you can make payments to it. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

これらのコードサンプルは[GitHub](https://github.com/iota-community/account-module) でホストされています。
<!-- These code samples are hosted on [GitHub](https://github.com/iota-community/account-module). -->

開始するには、デバイスに [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) がインストールされている必要があります。
<!-- To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device. -->

JavaScript 開発環境がない場合、または JavaScript クライアントライブラリを初めて使用する場合は、[スタートガイド](../../getting-started/js-quickstart.md)を完了してください。
<!-- If you don't have a JavaScript development environment, or if this is your first time using the JavaScript client library, complete our [getting started guide](../../getting-started/js-quickstart.md). -->

コマンドラインで、次を実行します。
<!-- In the command-line, do the following: -->

```bash
git clone https://github.com/iota-community/account-module.git
cd account-module/js/account-module
npm i
node generate-cda/generate-cda.js
```

コンソールにマグネットリンクが表示されます。
<!-- You should see the magnet link in the console. -->

```bash
iota://DL9CSYICJVKQRUTWBFUCZJQZ9WNBSRJOA9MGOISQZGGHOCZTXVSKDIZN9HBORNGDWRBBAFTKXGEJIAHKDJUYJJCFHC/?timeout_at=1574514007&multi_use=0
```

このマグネットリンクをコピーして他の人に送信すると、IOTA トークンをマグネットリンクのアドレスにデポジットすることができます。
<!-- You can copy this magnet link and send it to someone else so they can deposit IOTA tokens into it. -->

## 次のステップ
<!-- ## Next steps -->

[アカウントで支払いを行う](../js/make-payment.md)。
<!-- [Start making payments with your account](../js/make-payment.md). -->
