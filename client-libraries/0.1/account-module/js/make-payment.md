# JavaScript でアカウントで支払いを行う
<!-- # Make payments with your account in JavaScript -->

**このガイドでは，アカウントを使用して IOTA トークンを事前定義された条件付きデポジットアドレス（CDA）にデポジットします．**
<!-- **In this guide, you use your account to deposit IOTA tokens into a pre-defined conditional deposit address (CDA).** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには，次のパッケージをインストールする必要があります．
<!-- To complete this guide, you need to install the following packages: -->

--------------------
### npm
```bash
npm install @iota/account @iota/cda @iota/transaction-converter ntp-client
```
---
### Yarn
```bash
yarn add @iota/account @iota/cda @iota/transaction-converter ntp-client
```
--------------------

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは，[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します．
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

支払いを行うには，アカウントに IOTA トークンを含む1つ以上の CDA が必要です．
<!-- To make a payment, your account needs to have one or more CDAs that contains IOTA tokens. -->

1. IOTA トークンを含む CDA がない場合は，[このガイド](../js/generate-cda.md)に従って CDA を生成し，テスト IOTA トークンを送信します．
<!-- 1. If you dont have a CDA that contains IOTA tokens, follow this guide to [generate a CDA](../js/generate-cda.md) and send test IOTA tokens to it -->

2. `parseCDAMagnet()` メソッドを使用してマグネットリンクを CDA にデシリアライズします．
<!-- 2. Use the `parseCDAMagnet()` method to deserialize the magnet link into a CDA -->

    ```js
    const magnetLink = "iota://BWNYWGULIIAVRYOOFWZTSDFXFPRCFF9YEHGVBOORLGCPCJSKTHU9OKESUGZGWZXZZDLESFPPTGEHVKTTXG9BQLSIGP/?timeout_at=5174418337&multi_use=1&expected_amount=0";

    const cda = CDA.parseCDAMagnet(
        magnetLink
    );
    ```

    :::info:
    指定されたマグネットリンクは，100年以上で期限切れになる CDA の例です．別の CDA に支払いを行う場合は，代わりにその CDA を使用してください．
    :::
    <!-- :::info: -->
    <!-- The given magent link is for an example CDA that expires in over 100 years. If you want to make a payment to a different CDA, use that one instead. -->
    <!-- ::: -->

3. CDA がまだアクティブであることを確認します．
  <!-- 3. Make sure that the CDA is still active -->

    ```js
    // Get the current time to use to compare to the CDA's timeout
    // CDA のタイムアウトと比較するために使用する現在の時間を取得します
    ntpClient.getNetworkTime("time.google.com", 123, function(err, date) {
        if(err) {
            console.error(err);
            return;
        // Compare the current time with the timeout of the CDA
        } else if (!(CDA.isAlive(date, cda))) {
            isActive = false
        }
    });
    ```

3. CDA にデポジットを送信します．
  <!-- 3. Send a deposit to the CDA -->

    ```js
    // CDA が有効な場合にのみ支払いを送信します
    if (isActive) {
        account.sendToCDA({
            ...cda,
            value: 1000
        })
        .then((trytes) => {
            // テールトランザクションを取得し，オブジェクトに変換します
            let bundle = TransactionConverter.asTransactionObject(trytes[trytes.length - 1]);
            let bundleHash = bundle.bundle;
            let address = bundle.address
            let value = bundle.value;
            console.log(`Sent ${value} IOTA tokens to ${address} in bundle:  ${bundleHash}`);
        })
        .catch(error => {
            console.log(error);
            // データベースを閉じ，すべての進行中の再アタッチを停止します
            account.stop();
        });

    } else {
        console.log('CDA is expired. Use an active CDA.');
        // データベースを閉じ，すべての進行中の再アタッチを停止します
        account.stop();
        return;
    }
    ```

    アドレスに送信された IOTA トークンの量と，トランザクションのバンドルハッシュを確認する必要があります．
    <!-- You should see how many IOTA tokens were sent to your address as well as the bundle hash for your transactions: -->

    ```bash
    Sent 1000 to TIZJIRDCZPRJMMVKSGROPKE9VGIQKOLOUSX9MCUTOEQBBHPMLYBVKBPCXJKY9SDWX9FVMOZTWNMVVEYKX in bundle:  RXIA9CBEOASNY9IRIARZFGDLK9YNGW9ZHJGJLUXOUKVGCZLPNDKALFHZWHZKQQXFTIHEIJJPN9EURO9K9
    ```

アカウントは，確定されるまでバンドルの[再アタッチとプロモート](root://getting-started/0.1/transactions/reattach-rebroadcast-promote.md)を実行します．
<!-- Your account will [reattach and promote](root://getting-started/0.1/transactions/reattach-rebroadcast-promote.md) your bundle until it's confirmed. -->

`stopAttaching()` メソッドを呼び出すことで，再アタッチルーチンを停止できます．
<!-- You can stop the reattachment routine by calling the `stopAttaching()` method. -->

再アタッチルーチンを再起動するには，ネットワーク設定で `startAttaching()` メソッドを呼び出します．
<!-- To restart the reattachment routine, call the `startAttaching()` method with your network settings. -->

```js
account.stopAttaching();

account.startAttaching({
    depth: 3,
    minWeightMagnitude: 9,
    delay: 30 * 1000
    maxDepth: 6
});
```

## コードを実行する
<!-- ## Run the code -->

これらのコードサンプルは[GitHub](https://github.com/iota-community/account-module) でホストされています．
<!-- These code samples are hosted on [GitHub](https://github.com/iota-community/account-module). -->

開始するには，デバイスに [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) がインストールされている必要があります．
<!-- To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device. -->

JavaScript 開発環境がない場合，または JavaScript クライアントライブラリを初めて使用する場合は，[スタートガイド](../../getting-started/js-quickstart.md)を完了してください．
<!-- If you don't have a JavaScript development environment, or if this is your first time using the JavaScript client library, complete our [getting started guide](../../getting-started/js-quickstart.md). -->

コマンドラインで，次を実行します．
<!-- In the command-line, do the following: -->

```bash
git clone https://github.com/iota-community/account-module.git
cd account-module/js/account-module
npm i
node make-payment/make-payment.js
```

デポジットが送信されたことがわかります．
<!-- You should see that the deposit was sent. -->

シードステートには，確定されるまでこのペンディング中のバンドルが含まれます．
<!-- Your seed state will contain this pending bundle until it is confirmed. -->

## 次のステップ
<!-- ## Next steps -->

[シードステートをエクスポートして，バックアップするか，別のデバイスにインポートする](../js/export-seed-state.md)．
<!-- [Try exporting your seed state so you back it up or import it onto another device](../js/export-seed-state.md). -->
