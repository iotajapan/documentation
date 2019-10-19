# データをトライトへ変換する
<!-- # Convert data to trytes -->

**トランザクションフィールドの値は、トライトで表現する必要があります。データ変換を容易にするために、IOTAクライアントライブラリには、トライト、トリット、およびASCII文字との間でデータを変換するための組み込みコンバータがあります。**
<!-- **The values of transaction fields must be represented in trytes. To facilitate data conversion, the IOTA client libraries have built-in functions to convert data to/from trytes, trits, and ASCII characters.** -->

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
* [Visual Studio Code](https://code.visualstudio.com/Download)などのコードエディタ
<!-- * A code editor such as [Visual Studio Code](https://code.visualstudio.com/Download) -->

## メッセージをトライトへ変換する
<!-- ## Convert a message to trytes -->

この例では、メッセージをトライトに変換してから、iota.jsライブラリの[`converter`パッケージ](https://github.com/iotaledger/iota.js/tree/next/packages/converter)を使用してASCII文字に変換し直します。
<!-- In this example, we convert a message to trytes, then convert it back to ASCII characters using the [`converter` package](https://github.com/iotaledger/iota.js/tree/next/packages/converter) of the iota.js library. -->

1. `converter`パッケージを`requrie`します。
  <!-- 1. Require the `converter` package -->

    ```js
    const Converter = require('@iota/converter');
    ```

2. メッセージを保持するための変数を作成する。
  <!-- 2. Create a variable to hold a message -->

    ```js
    var data = "Hello World!";
    ```

3. `data`変数を`asciiToTrytes()`メソッドに渡して、メッセージをトライトに変換します。
  <!-- 3. Pass the `data` variable to the `asciiToTrytes()` method to convert the message to trytes -->

    ```js
    var trytes = Converter.asciiToTrytes(data);

    console.log(`${data} converted to trytes: ${trytes}`);
    ```

4. メッセージをASCII文字に変換するために、`trytesToAscii()`メソッドにトライトを渡します。
  <!-- 4. Pass the returned trytes to the `trytesToAscii()` method to convert them to ASCII characters -->

    ```js
    var message = Converter.trytesToAscii(trytes);

    console.log(`${trytes} converted back to ASCII: ${message}`);
    ```

    ファイルを実行すると、変換された文字列が標準出力に表示されます。
    <!-- When you execute the file, you should see the converted strings in the output: -->

    ```console
    Hello World! converted to trytes: RBTC9D9DCDEAFCCDFD9DSCFA
    RBTC9D9DCDEAFCCDFD9DSCFA converted back to ASCII: Hello World!
    ```

:::info:
`asciiToTrytes()`メソッドは[基本的なASCII文字](https://en.wikipedia.org/wiki/ASCII#Printable_characters)のみをサポートします。その結果、アクセントやウムラウトなどの発音区別符号やひらがなや漢字などの日本語（2バイト文字）はサポートされておらず、`INVALID_ASCII_CHARS`エラーが発生します。
:::
<!-- :::info: -->
<!-- The `asciiToTrytes()` method supports only [basic ASCII characters](https://en.wikipedia.org/wiki/ASCII#Printable_characters). As a result, diacritical marks such as accents and umlauts aren't supported and result in an `INVALID_ASCII_CHARS` error. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

このガイドのサンプルコードを実行してWebブラウザに結果を表示するには、緑色のボタンをクリックしてください。
<!-- Click the green button to run the sample code in this guide and see the results in the web browser. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Convert-data-to-trytes?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>
