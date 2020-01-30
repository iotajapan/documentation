# JavaScript でシードステートをインポート/エクスポートする
<!-- # Import/export an existing seed state in JavaScript -->

**アカウントを使用して支払いを行うと，アカウントはシードステートを更新します．このガイドでは，アカウントのシードステートをエクスポートして，別のデバイスにインポートしたり，単にバックアップしたりできるようにする方法を学習します．**
<!-- **When you use your account to make payments, your account updates your seed state. In this guide, you learn how to export your account's seed state so that you can import it on another device or simply back it up.** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには，次のパッケージをインストールする必要があります．
<!-- To complete this guide, you need to install the following packages: -->

--------------------
### npm
```bash
npm install @iota/account ntp-client
```
---
### Yarn
```bash
yarn add @iota/account ntp-client
```
--------------------

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは，[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します．
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. アカウントのシードステートをエクスポートします．
  <!-- 1. Export your account's seed state -->

    ```js
    account.exportState().then(state => {

    });
    ```

2. シードステートをシリアル化し，JSON ファイルに保存します．
  <!-- 2. Serialize your seed state and save it to a JSON file -->

    ```js
    let JSONstate = JSON.stringify(state);
    fs.writeFile('exported-seed-state.json', JSONstate,
    function(err, result) {
        if (err) {
            console.log('error', err);
        } else {
            console.log('Seed state saved')
        });
    ```

    :::info:
    定期的にシードステートをバックアップすることをお勧めします．
    :::
    <!-- :::info: -->
    <!-- It's best practice to back up your seed state at regular intervals. -->
    <!-- ::: -->

3. エクスポートしたシードステートを読み込みます．
  <!-- 3. Read your exported seed state -->

    ```js
    let JSONSeedState = fs.readFileSync("exported-seed-state.json");
    ```

4. JSON シードステートをオブジェクトにデシリアライズします．
  <!-- 4. Deserialize your JSON seed state into an object -->

    ```js
    let state = JSON.parse(JSONSeedState);
    ```

5. シードステートをアカウントのデーターベースにインポートします．
  <!-- 5. Import your seed state into your account's database -->

    ```js
    account.importState(state).then(err => {
        if (err) {
            console.log('error', err);
            // データベースを閉じ，すべての進行中の再アタッチを停止します
            account.stop();
        } else {
            console.log('Seed state imported')
        }
    });
    ```

    :::warning:
    シードステートをインポートすると，アカウントのデータベース内の既存のシードステートが上書きされます．
    :::
    <!-- :::warning: -->
    <!-- When you import a seed state, you overwrite any existing seed state in your account's database. -->
    <!-- ::: -->

:::success:おめでとうございます:tada:
シードステートをエクスポートおよびインポートする方法を学習しました．
:::
<!-- :::success:Congratulations! :tada: -->
<!-- You've learned how to export and import your seed state. -->
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
node export-account/export-account.js
```

シードステートを含む `exported-seed-state.json` ファイルが必要です．このファイルを使用して，シードステートを別のデバイスにインポートできます．
<!-- You should have an `exported-seed-state.json` file that contains your seed state. You can use this file to import your seed state on another device. -->

## 次のステップ
<!-- ## Next steps -->

[GitHub](https://github.com/iotaledger/iota.js/tree/next/packages/account/src) を参照し，アカウントモジュールについて引き続き学習する．
<!-- Take a look at [GitHub](https://github.com/iotaledger/iota.js/tree/next/packages/account/src) to continue learning about the account module. -->
