# JavaScript でアカウントを作成する
<!-- # Create an account in JavaScript -->

**このガイドでは、ローカルデータベースでシードステートを追跡するアカウントを作成し、利用可能な残高を表示する方法を学習します。**
<!-- **In this guide, you create an account to keep track of your seed state in a local database and learn how to display your available balance.** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには、次のパッケージをインストールする必要があります。
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

このガイドでは、以下のネットワーク設定で[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet) with the following network settings: -->

- **[最小重量値](root://getting-started/0.1/network/minimum-weight-magnitude.md)**: 9
<!-- - **[Minimum weight magnitude](root://getting-started/0.1/network/minimum-weight-magnitude.md)**: 9 -->

- **[深度](root://getting-started/0.1/transactions/depth.md)**: 3
<!-- - **[Depth](root://getting-started/0.1/transactions/depth.md)**: 3 -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1\. あたらしいシードを作成し、バックアップします。
<!-- 1\. Create a new seed and back it up -->

:::info:
既存のシードは、シードステートが不明であるため、使用することは安全ではありません。既存のシードは、アカウントが認識していない使用済みアドレスを持っている場合が有ります。
:::
<!-- :::info: -->
<!-- Existing seeds are not safe to use because their state is unknown. As such, these seeds may have spent addresses that the account is not aware of. -->
<!-- ::: -->

--------------------
### Linux
```bash
cat /dev/urandom |tr -dc A-Z9|head -c${1:-81}
```
---
### macOS
```bash
cat /dev/urandom |LC_ALL=C tr -dc 'A-Z9' | fold -w 81 | head -n 1
```
---
### Windows PowerShell
```bash
$b=[byte[]] (1..81);(new-object Security.Cryptography.RNGCryptoServiceProvider).GetBytes($b);-join($b|%{[char[]] (65..90+57..57)[$_%27]})
```
--------------------

2\. アカウントを使用するシードを定義します。
<!-- 2\. Define the seed that your account will use -->

```js
const seed = 'PUEOTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX';
```

3\. ネットワーク設定を定義します。
<!-- 3\. Define your network settings -->

```js
// ノードに接続します
const provider = 'https://nodes.devnet.iota.org:443';

// タングル内でチップ選択の開始地点（現在のマイルストーンから戻る個数）
const depth = 3;

// デブネット用の最小重量値
const minWeightMagnitude = 9;

// 各再アタッチラウンド間の待機時間
const delay = 1000 * 30;

// トランザクションがプロモートできなくなり、自動的に再アタッチされる深度
const maxDepth = 6;
```

4\. `promise` を返す `timeSource` 関数を作成します。これは、アカウントが CDA がまだアクティブかどうかを判断するために使用します。この例では、[ntp-client] パッケージを使用して [Google NTP（ネットワークタイムプロトコル）サーバー]に接続します。
<!-- 4\. Create a `timeSource` function that returns a promise, which the account will use to decide if your CDAs are still active. In this example, we use the [ntp-client](https://www.npmjs.com/package/ntp-client) package to connect to the [Google NTP (network time protocol) servers](https://developers.google.com/time/faq). -->

```js
const timeSource = () => util.promisify(ntpClient.getNetworkTime)("time.google.com", 123);
```

4\. アカウントを作成してノードに接続します。
<!-- 4\. Create your account and connect it to a node -->

```js
const account = createAccount({
      seed,
      provider,
      depth,
      minWeightMagnitude,
      delay,
      maxDepth,
      timeSource
});
```

デフォルトでは、アカウントには、アカウントが送信するペンディング中のバンドルのテールトランザクションを再アタッチしてプロモートするプラグインが含まれています。
<!-- By default, the account includes a plugin that reattaches and promotes the tail transactions of any pending bundles that your account sends. -->

:::info:
CDAのデフォルトのセキュリティレベルは2です。この設定を変更するには、アカウントに `security` フィールドを渡します。
:::
<!-- :::info: -->
<!-- The default security level for CDAs is 2. You can change this setting by passing the account a `security` field. -->
<!-- ::: -->

6\. アカウントとすべてのプラグインを起動し、データベースを開きます。
<!-- 6\. Start the account and any plugins, and open the database -->

```js
account.start();
```

7\. アカウントの残高を確認します。
<!-- 7\. Check your account's balance -->

```js
account.getAvailableBalance()
.then(balance => {
    console.log(`Your total available balance is: ${balance}`);
})
.catch(error => {
    console.log(error);
    // データベースを閉じ、すべての進行中の再アタッチを停止します
    account.stop();
});
```

残高が表示されます。
<!-- You should see your balance. -->

:::success:Congratulations! :tada:
You've created an account that will automatically promote and reattach transactions as well as manage the state of your seed.
:::

## コードを実行する
<!-- ## Run the code -->

これらのコードサンプルは [GitHub](https://github.com/iota-community/account-module) でホストされています。
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
node create-account/create-account.js
```

新しいアカウントの残高が表示されます。
<!-- You should see the balance of your new account. -->

シードステートを追跡するデータベースファイルもあります。
<!-- You'll also have a database file that keeps track of your seed state. -->

## 次のステップ
<!-- ## Next steps -->

アカウントで特定のイベントが発生すると、アカウントはそのイベントを発行するので、発行されたイベントをリッスンすることができます。たとえば、アカウントの新規支払いをモニタリングすることができます。そのためには、[イベントリスナーを作成する](../js/listen-to-events.md)必要があります。
<!-- After certain events happen in your account, it emits them, and allows you to listen for them. For example, you may want to monitor your account for new payments. To do so, you need to [create an event listener](../js/listen-to-events.md). -->

[LevelDB データベース](https://github.com/google/leveldb)のデフォルトの `leveldown` フレーバーを使用する代わりに、[PersistenceAdapter](https://github.com/iotaledger/iota.js/tree/next/packages/persistence#PersistenceAdapter) インターフェイスを使用してカスタムインターフェイスを作成できます。
<!-- Instead of using the default `leveldown` flavor of the [LevelDB database](https://github.com/google/leveldb), you can use the [PersistenceAdapter](https://github.com/iotaledger/iota.js/tree/next/packages/persistence#PersistenceAdapter) interface to create a custom one. -->

:::warning:
新しいアカウントごとに新しいアダプターインスタンスが作成されるため、各アカウントで同時に使用できるアダプターインスタンスは1つだけです。
:::
<!-- :::warning: -->
<!-- A new adapter instance is created for each new account, therefore you can use only one adapter instance for each account at the same time. -->
<!-- ::: -->
