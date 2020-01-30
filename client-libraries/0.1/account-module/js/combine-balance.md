# JavaScript で1つの CDA に残高をまとめる
<!-- # Combine your balance into one CDA in JavaScript -->

**残高の大半をできるだけ少ない条件付きデポジットアドレス（CDA）に保持することをお勧めします．これにより，支払いがより高速になり，必要なトランザクションが少なくなります．このガイドでは，利用可能な残高全体を新しい CDA に移行します．**
<!-- **You may want to keep the majority of your balance on as few conditional deposit addresses (CDA) as possible. This way, making payments is faster and requires fewer transactions. In this guide, you transfer your entire available balance to a new CDA.** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには，次のパッケージをインストールする必要があります．
<!-- To complete this guide, you need to install the following packages: -->

--------------------
### npm
```bash
npm install @iota/account @iota/transaction-converter ntp-client
```
---
### Yarn
```bash
yarn add @iota/account @iota/transaction-converter ntp-client
```
--------------------

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは，[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します．
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. アカウントの利用可能残高を期待する CDA を作成します．
  <!-- 1. Create a CDA that expects your account's available balance -->

    ```js
    account.getAvailableBalance()
    .then(balance => {
        const cda = account.generateCDA({
                timeoutAt: Date.now() + 24 * 60 * 60 * 1000,
                expectedAmount: balance
        })
    ```

    :::info:
    アカウントの利用可能残高は，すべての期限切れの CDA の合計残高です．期限切れの CDA に IOTA トークンを送信してはならないため，この残高は取り出しても安全です．

    アカウントの合計残高には，期限切れと同様にアクティブな CDA が含まれています．
    :::
    <!-- :::info: -->
    <!-- You account's available balance is the total balance of all expired CDAs. This balance is safe to withdraw because no one should send IOTA tokens to an expired CDA. -->

    <!-- Your account's total balance includes CDAs that are still active as well as expired. -->
    <!-- ::: -->

2. CDA にデポジットを送信します．
  <!-- 2. Send a deposit to your CDA -->

    ```js
        .then(cda => {
                    account.sendToCDA({
                    ...cda,
                    value: balance
                })
                .then(trytes => {

                    console.log(trytes)
                    // 末尾トランザクションを取得し，オブジェクトへ変換します
                    let bundle = TransactionConverter.asTransactionObject(trytes[0]);
                    let bundleHash = bundle.bundle;
                    let address = bundle.address
                    let value = bundle.value;
                    console.log(`Sent ${value} IOTA tokens to ${address} in bundle:  ${bundleHash}`);
                })
            })
        }).catch(error => {
        console.log(error);
        // データベースを閉じ，進行中の再アタッチを停止します
        account.stop();
    });
    ```

    アドレスに送信された IOTA トークンの量とバンドルハッシュを確認する必要があります．
    <!-- You should see how many IOTA tokens were sent to your address as well as the bundle hash: -->

    ```bash
    Sent 1000 to TIZJIRDCZPRJMMVKSGROPKE9VGIQKOLOUSX9MCUTOEQBBHPMLYBVKBPCXJKY9SDWX9FVMOZTWNMVVEYKX in bundle:  RXIA9CBEOASNY9IRIARZFGDLK9YNGW9ZHJGJLUXOUKVGCZLPNDKALFHZWHZKQQXFTIHEIJJPN9EURO9K9
    ```

:::success:
現在，利用可能な合計残高は単一のアドレスにあります．
:::
<!-- :::success: -->
<!-- Now your total available balance is in a single address. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

これらのコードサンプルは [GitHub](https://github.com/iota-community/account-module) でホストされています．
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
node combine-balance/combine-balance.js
```

デポジットが送信されたことがわかります．
<!-- You should see that the deposit was sent. -->

末尾トランザクションが確定されるまで，シードステートにはこのペンディング中のバンドルに含まれます．
<!-- Your seed state will contain this pending bundle until its tail transaction is confirmed. -->

## 次のステップ
<!-- ## Next steps -->

[シードステートをエクスポートして，バックアップするか，別のデバイスにインポートする](../js/export-seed-state.md)．
<!-- [Try exporting your seed state so you back it up or import it onto another device](../js/export-seed-state.md). -->
