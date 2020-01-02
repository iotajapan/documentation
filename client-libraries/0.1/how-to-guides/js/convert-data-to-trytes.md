# データをトライトへ、トライトをデータへ変換する
<!-- # Convert data to/from ternary -->

**トランザクションフィールドの値は[トライト](root://getting-started/0.1/introduction/ternary.md)で表す必要があります。データ変換を容易にするために、IOTA クライアントライブラリには、データをトライト、トリット、およびASCII文字との間で変換するための組み込み関数があります。**
<!-- **The values of transaction fields must be represented in [trytes](root://getting-started/0.1/introduction/ternary.md). To facilitate data conversion, the IOTA client libraries have built-in functions to convert data to/from trytes, trits, and ASCII characters.** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには、次のパッケージをインストールする必要があります。
<!-- To complete this guide, you need to install the following package: -->

--------------------
### npm
```bash
npm install @iota/converter
```
---
### Yarn
```bash
yarn add @iota/converter
```
--------------------

## コードウォークスルー
<!-- ## Code walkthrough -->

1. ASCII 文字のメッセージを定義します。
  <!-- 1. Define a message in ASCII characters -->

    ```js
    var data = "Hello World!";
    ```

2. メッセージをトライトへ変換します。
  <!-- 2. Convert the message to trytes -->

    ```js
    var trytes = Converter.asciiToTrytes(data);

    console.log(`${data} converted to trytes: ${trytes}`);
    ```

    :::info:
    `asciiToTrytes()` メソッドは[基本 ASCII 文字]のみをサポートします。その結果、アクセントやウムラウトなどの発音区別記号や日本語はサポートされず、`INVALID_ASCII_CHARS` エラーが発生します。
    :::
    <!-- :::info: -->
    <!-- The `asciiToTrytes()` method supports only [basic ASCII characters](https://en.wikipedia.org/wiki/ASCII#Printable_characters). As a result, diacritical marks such as accents and umlauts aren't supported and result in an `INVALID_ASCII_CHARS` error. -->
    <!-- ::: -->

3. トライトを ASCII 文字に戻します。
  <!-- 3. Convert the trytes back to ASCII characters -->

    ```js
    var message = Converter.trytesToAscii(trytes);

    console.log(`${trytes} converted back to ASCII: ${message}`);
    ```

    ファイルを実行すると、変換された文字列が出力に表示されるはずです。
    <!-- When you execute the file, you should see the converted strings in the output: -->

    ```console
    Hello World! converted to trytes: RBTC9D9DCDEAFCCDFD9DSCFA
    RBTC9D9DCDEAFCCDFD9DSCFA converted back to ASCII: Hello World!
    ```

## コードを実行する
<!-- ## Run the code -->

[REPL.it ツール](https://repl.it)を使用して、ブラウザーで JavaScript クライアントライブラリからサンプルコードを実行できます。
<!-- We use the [REPL.it tool](https://repl.it) to allow you to run sample code from the JavaScript client library in the browser. -->

このガイドのサンプルコードを実行してウィンドウに結果を表示するには、緑色のボタンをクリックします。
<!-- Click the green button to run the sample code in this guide and see the results in the window. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Convert-data-to-trytes?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>
