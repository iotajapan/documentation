# アドレスを作成する
<!-- # Create an address -->

**IOTAトークンを受け取るには、送信者に自分のアドレスの1つを渡す必要があります。このアドレスは自分のシードから導出されるものです。インデックスをインクリメントしたり、別のセキュリティレベルを使用したりすることで、シードから新しいアドレスを導出することができます。**
<!-- **To receive IOTA tokens, you must give the sender one of your addresses. These addresses are derived from your seed. You can derive a new address from a seed by either incrementing the index and/or using a different security level.** -->

:::info:クライアントライブラリを初めて使用されますか？
公式クライアントライブラリを使い始めるための[クイックスタートガイド](root://getting-started/0.1/tutorials/get-started.md)をお試してください。
:::
<!-- :::info:First time using a client library? -->
<!-- [Try our quickstart guide](root://getting-started/0.1/tutorials/get-started.md) for getting started with the official client libraries. -->
<!-- ::: -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完成するには、次のものが必要です。
<!-- To complete this guide, you need the following: -->

* Node.js 8、またはNode.js 10以上。[最新のLTS](https://nodejs.org/en/download/)をお勧めします。
<!-- * Node.js 8, or Node.js 10 or higher. We recommend the [latest LTS](https://nodejs.org/en/download/). -->
* [Visual Studio Code](https://code.visualstudio.com/Download)のようなコードエディタ
<!-- * A code editor such as [Visual Studio Code](https://code.visualstudio.com/Download) -->
* インターネット接続
<!-- * An Internet connection -->

## 新しいアドレスを作成する
<!-- ## Create a new address -->

この例では、[Devnetノード](root://getting-started/0.1/references/iota-networks.md#devnet)に接続します。 Devnetは、トークンが無料であること以外はMainnetとほぼ同じです。Devnetに送信したトランザクションは、Mainnetのような他のネットワークには存在しません。
<!-- In this example, we connect to a [Devnet node](root://getting-started/0.1/references/iota-networks.md#devnet). The Devnet is similar to the Mainnet, except the tokens are free. Any transactions that you send to the Devnet do not exist on other networks such as the Mainnet. -->

1. IOTAライブラリが必要です。
  <!-- 1. Require the IOTA libraries -->

    ```js
    const Iota = require('@iota/core');
    ```

2. IOTAオブジェクトのインスタンスを作成し、`provider`フィールドを使用してノードに接続します。
  <!-- 2. Create an instance of the IOTA object and use the `provider` field to connect to a node -->

    ```js
    const iota = Iota.composeAPI({
        provider: 'https://nodes.devnet.iota.org:443'
    });
    ```

3. シードを格納するための変数を作成します。
  <!-- 3. Create a variable to store a seed -->

    ```js
    const seed =
    'PUETTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX';
    ```

    :::info:
    シードを使用するコードはすべてクライアント側で実行されます。シードがデバイスを離れることはありません。
    :::
    <!-- :::info: -->
    <!-- Any code that uses a seed is executed on the client side. Your seed never leaves your device. -->
    <!-- ::: -->

4. アドレスを作成するために、`seed`変数を`getNewAddress()`メソッドに渡します。
  <!-- 4. Pass the `seed` variable to the `getNewAddress()` method to create an address -->

    ```js
    iota.getNewAddress(seed, {index: 0, security: 2})
    .then(address => console.log(address));
    ```

    ファイルを実行すると、アドレスが表示されます。スクリプトをもう一度実行すると、同じシード、インデックス、およびセキュリティレベルから導出しているため、同じアドレスが表示されます。
    <!-- When you execute the file, you should see an address. If you execute the script again, you'll see the same address because its derived from the same seed, index and security level. -->

`getNewAddress()`メソッドのインデックスとセキュリティレベルの引数を変更して、別のアドレスを導出してください。
<!-- Try changing the index and security level arguments in the `getNewAddress()` method to create a different address. -->

## コードを実行する
<!-- ## Run the code -->

このガイドのサンプルコードを実行してWebブラウザに結果を表示するには、緑色のボタンをクリックしてください。
<!-- Click the green button to run the sample code in this guide and see the results in the web browser. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Create-an-address?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

:::warning:
インデックスまたはセキュリティレベルを指定せずに`getNewAddress()`メソッドを呼び出すと、APIはまだIOTAトークンが取り出されていないアドレスを返します。

これを行うために、APIは、そのアドレスを使用している入力トランザクションを見つけるようにノードにリクエストします。

一部のノードは台帳からのトランザクションを刈り取るため、インデックスやセキュリティレベルを省略することはお勧めしません。その結果、アドレスが使われているかどうかをAPIが常に知ることはできません。
:::
<!-- :::warning: -->
<!-- If you call the `getNewAddress()` method without the index or security level, the API will return an address from which you haven't yet withdrawn (unspent). -->
<!--  -->
<!-- To do this, the API asks the node to find input transactions that use the address. -->
<!--  -->
<!-- We don't recommend omitting the index or security level because some nodes prune transactions from their ledgers. As a result, the API won't always know if an address is spent. -->
<!-- ::: -->
