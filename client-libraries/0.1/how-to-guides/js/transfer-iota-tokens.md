# Node.js でマイクロペイメントを送信する
<!-- # Send a micropayment in Node.js -->

**このガイドでは、[転送バンドル](root://getting-started/0.1/transactions/bundles.md)を[ノード](root://getting-started/0.1/network/nodes.md)に送信して、1 IOTA のマイクロペイメントを送信します。**
<!-- **In this guide, you send a micropayment of 1 IOTA by sending a [transfer bundle](root://getting-started/0.1/transactions/bundles.md) to a [node](root://getting-started/0.1/network/nodes.md).** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには、次のパッケージをインストールする必要があります。
<!-- To complete this guide, you need to install the following package: -->

--------------------
### npm
```bash
npm install @iota/core
```
---
### Yarn
```bash
yarn add @iota/core
```
--------------------

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは、以下のネットワーク設定で[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します。
<!-- In this guide, we connect to a [node](root://getting-started/0.1/network/nodes.md) on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet) with the following network settings: -->

- **[最小重量値](root://getting-started/0.1/network/minimum-weight-magnitude.md)**: 9
<!-- - **[Minimum weight magnitude](root://getting-started/0.1/network/minimum-weight-magnitude.md)**: 9 -->

- **[深さ](root://getting-started/0.1/transactions/depth.md)**: 3
<!-- - **[Depth](root://getting-started/0.1/transactions/depth.md)**: 3 -->

## 手順1. テスト IOTA トークンを取得する
<!-- ## Step 1. Get test IOTA tokens -->

テスト IOTA トークンをデブネットで送信するには、ノードはシードに属するアドレスの1つにおいて0より大きい残高のレコードを持っている必要があります。デブネットで使用するテスト IOTA トークンを取得するには、デブネット蛇口を使用できます。
<!-- To send test IOTA tokens on the Devnet, the nodes must have a record of a greater than 0 balance for one of the addresses that belongs to your seed. To get test IOTA tokens to use on the Devnet, you can use the Devnet faucet. -->

1\. 新しいシードを作成し、シードをバックアップします。
<!-- 1\. Create a new seed and back it up -->

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

2\. [シードの新しいアドレスを生成します](../js/generate-an-address.md)。
<!-- 2\. [Generate a new address for your seed](../js/generate-an-address.md) -->

3\. アドレスをコピーし、[デブネット蛇口](https://faucet.devnet.iota.org)にペーストし、テスト IOTA トークンがアドレスに転送されるのを待ちます。
<!-- 3\. Copy and paste your address into the [Devnet faucet](https://faucet.devnet.iota.org), then wait for the tokens to be transferred to your address -->

## 手順2. 転送バンドルを作成し、送信する
<!-- ## Step 2. Create and send a transfer bundle -->

あるアドレスから別のアドレスにテスト IOTA トークンを転送するには、転送バンドルを作成してノードに送信する必要があります。この転送バンドルには、アドレスから IOTA トークンを取り出すための入力トランザクションと、それらを別のアドレスに預け入れるための出力トランザクションが必要です。
<!-- To transfer your test tokens from one address to another, you need to create and send a transfer bundle. This bundle needs an input transaction to withdraw the IOTA tokens from your address and an output transaction to deposit them into another address. -->

1. パッケージをリクワイアします。
  <!-- 1. Require the packages -->

    ```js
    const Iota = require('@iota/core');
    ```

2. ノードに接続します。
  <!-- 2. Connect to a node -->

    ```js
    const iota = Iota.composeAPI({
    provider: 'https://nodes.devnet.iota.org:443'
    });
    ```

3. 深さと最小重量値を定義します。
  <!-- 3. Define the depth and the minimum weight magnitude -->

    ```js
    const depth = 3;
    const minimumWeightMagnitude = 9;
    ```

4. シードを定義します。このシードを、テスト IOTA トークンを持つアドレスを所有するシードに置き換えます。
  <!-- 4. Define your seed. Replace this seed with one that owns an address with test IOTA tokens -->

    ```js
    const seed =
    'PUEOTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX';
    ```

    :::info:
    このバンドルは IOTA トークンを転送するため、シードを使用してバンドルに署名します。したがって、このシードのアドレスの1つには少なくとも1つの IOTA トークンが含まれている必要があります。
    :::
    <!-- :::info: -->
    <!-- Because this bundle transfers IOTA tokens, the seed is used to sign it. Therefore, this seed's addresses must contain at least 1 IOTA token. -->
    <!-- ::: -->

5. テスト IOTA トークンの送信先のアドレスを定義します。
  <!-- 5. Define the address to which you want to send your IOTA token -->

    ```js
    const receivingAddress = "ZLGVEQ9JUZZWCZXLWVNTHBDX9G9KZTJP9VEERIIFHY9SIQKYBVAHIMLHXPQVE9IXFDDXNHQINXJDRPFDXNYVAPLZAW"
    ```

6. 転送するテスト IOTA トークンの量と送信先のアドレスを指定する `transfers` オブジェクトを作成します。
  <!-- 6. Create a `transfers` object that specifies the amount of IOTA tokens you want to transfer and the address to send the tokens to -->

    ```js
    const transfers = [
    {
      value: 1,
      address: receivingAddress
    }
    ]
    ```

7. `transfers` オブジェクトから転送バンドルを作成するには、`transfers` オブジェクトを [`prepareTransfers()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.prepareTransfers) メソッドに渡します。
  <!-- 7. To create a transfer bundle from your `transfers` object, pass it to the [`prepareTransfers()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.prepareTransfers) method -->

    ```js
    const trytes = await iota.prepareTransfers(seed, transfers);
    ```

    このメソッドは、シードのアドレスの残高をチェックするようにノードに要求します。アドレスに転送を完了するのに十分な IOTA トークンがある場合、このメソッドは入力トランザクションを作成して、転送を実行するために十分な全残高をアドレスから取り出します。次に、メソッドはこれらのトランザクションを転送バンドルに追加し、IOTA トークンが取り出されるアドレスの秘密鍵でバンドルに署名します。
    <!-- This method asks the node to check the balance of your seed's addresses. If your addresses have enough IOTA tokens to complete the transfer, the method creates input transactions to withdraw the full balance from enough of your addresses to fulfill the transfer. Then, the method adds those transactions to the transfer bundle and signs the bundle with the private keys of any withdrawn addresses. -->

    :::info:
    シードがデバイスから離れることはありません。ライブラリはアドレスを生成し、アドレスのみをノードに送信します。
    :::
    <!-- :::info: -->
    <!-- Your seed never leaves your device. The library generates addresses and sends them to the node. -->
    <!-- ::: -->

    転送する金額が取り出すアドレスの残高より少ない場合、メソッドは別の出力トランザクションを作成して、残りの IOTA トークンをシードに属する未使用のアドレスに転送します。
    <!-- If the amount you want to transfer is less than the balance of your withdrawn addresses, the method creates another output transaction to transfer the remainder to an unspent address that belongs to your seed. -->

8. [`sendTrytes()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.sendTrytes) メソッドにバンドルトライトを渡します。[`sendTrytes()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.sendTrytes) メソッドは[チップ選択](root://node-software/0.1/iri/concepts/tip-selection.md)、[リモートプルーフオブワーク](root://getting-started/0.1/transactions/proof-of-work.md)を処理し、バンドルを[ノード]に送信します。
  <!-- 8. Pass the bundle trytes to the [`sendTrytes()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.sendTrytes) method, which handles [tip selection](root://node-software/0.1/iri/concepts/tip-selection.md), [remote proof of work](root://getting-started/0.1/transactions/proof-of-work.md), and sending the bundle to the [node](root://getting-started/0.1/network/nodes.md) -->

    ```js
    const response = await iota.sendTrytes(trytes, depth, minimumWeightMagnitude);

    console.log('Bundle sent');
    response.map(tx => console.log(tx));
    ```

    コンソールに、トランザクションに関する情報が表示されます。
    <!-- In the console, you'll see information about the transactions. -->

:::success:おめでとうございます:tada:
最初の転送バンドルを送信しました。トランザクションはタングルにアタッチされ、ネットワークの残りの部分に転送されます。これで、トランザクションが確定し、残高が更新されるのを待つだけです。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just sent your first transfer bundle. Your transactions are attached to the Tangle and will be forwarded to the rest of the network. Now, you just need to wait until the transaction is confirmed for your balance to be updated. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

[REPL.it ツール](https://repl.it)を使用して、ブラウザーでサンプルコードを実行できます。
<!-- We use the [REPL.it tool](https://repl.it) to allow you to run sample code in the browser. -->

このガイドのサンプルコードを実行してウィンドウに結果を表示するには、緑色のボタンをクリックします。
<!-- Click the green button to run the sample code in this guide and see the results in the window. -->

このサンプルコードを実行する前に、シードを自分自身のテストシードに置き換えます。
<!-- Before you run this sample code, replace the seed with your own test seed. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Send-IOTA-tokens-on-the-Devnet?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## 次のステップ
<!-- ## Next steps -->

[アドレスの残高を確認する](../js/check-balance.md)。
<!-- [Check the balance of your address](../js/check-balance.md). -->

このシナリオでは、バンドルの作成と送信にかかる時間の間にアドレスが使用されるかどうかを事前に知ることができません。
<!-- In this scenario, you wouldn't know in advance whether the address is spent during the time it takes to create and send your bundle. -->

たとえば、オンラインショッピングをしていて、チェックアウトには QR コードがあり、IOTA トークンで支払うオプションがあったとします。この QR コードには、トリニティに自動入力されるアドレスが含まれています。
<!-- For example, you are online shopping and the checkout has a QR code that gives you the option to pay in IOTA tokens. This QR code contains an address that is auto-populated in Trinity. -->

チェックアウトを完了して転送バンドルを送信するまでに、Web サイトの所有者が QR コードのアドレスから IOTA トークンを取り出したとします。これで QR コードのアドレスは使用済みアドレスとなり、支払者はこの使用済みアドレスへ IOTA トークンを送信してしまうことになります。
<!-- During the time it takes you to complete the checkout and send your transfer bundle, the website owner withdraws IOTA tokens from the address in the QR code. Now that address is spent, and you have just sent IOTA tokens to it. -->

これを防ぐには、[アカウントモジュール](../../account-module/introduction/overview.md)を使用して、アクティブか期限切れかを指定する条件付き預け入れアドレスを作成することをお勧めします。
<!-- To help stop this from happening, we recommend using the [account module](../../account-module/introduction/overview.md) to create conditional deposit addresses that specify whether they are active or expired. -->
