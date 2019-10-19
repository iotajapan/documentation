# 存在の証明プロバイダー概要
<!-- # Proof-of-existance provider overview -->

**ファイルは、さまざまな関係者間で契約などの情報を転送するために重要です。時には、ある関係者が他の関係者達が知らないうちにファイルを変更することがあります。したがって、すべての関係者が、ファイルが変更されていないことを確実に証明して、信頼できるようにする方法が必要です。ファイルが変更されていないことを証明するために、存在の証明ユーティリティを使用してファイルの内容をハッシュ化し、タングルに添付できます。このようにして、どの関係者も後でファイルをハッシュ化し、タングル上のイミュータブルなファイルと比較できます。2つのハッシュ値が同じ場合、ファイルは変更されていません。**
<!-- **Files are important for transporting information such as contracts among different parties. Sometimes, one party may change a file without the others knowing. So, all parties need a way to reliably prove that a file has not been changed so they can trust it. To prove that a file is unchanged, you can use the proof-of-existance utility to hash the file's contents and attach it to the Tangle. This way, any party can later hash the file and compare it to the immutable one on the Tangle. If the hashes are the same, the file is unchanged.** -->

## ファイルが変更されていないことを証明する
<!-- ## Prove that a file is unchanged -->

この例では、[存在の証明ライブラリ](https://github.com/iotaledger/iota-poex-tool)を使用して、ファイルが変更されていないことを証明します。
<!-- In this example, we use the [proof-of-existance library](https://github.com/iotaledger/iota-poex-tool) to prove that a file is unchanged. -->

まず、後で変更されていないことを証明したいファイルを作成します。次に、ファイルをハッシュ化し、そのハッシュ値をトランザクションに追加してから、[Devnet](root://getting-started/0.1/references/iota-networks.md#devnet)タングルに添付します。Devnetは、トークンが無料であることを除いて、Mainnetとほぼ同じです。Devnetに送信するトランザクションは、Mainnetなどの他のネットワークには存在しません。
<!-- First, we create a file that we later want to prove is unchanged. Then, we hash the file and add its hash to a transaction before attaching it to the [Devnet](root://getting-started/0.1/references/iota-networks.md#devnet) Tangle. The Devnet is similar to the Mainnet, except the tokens are free. Any transactions that you send to the Devnet do not exist on other networks such as the Mainnet. -->

:::info:
タングルのトランザクションはイミュータブルであるため、トランザクションを真実のソースとして使用できます。
:::
<!-- :::info: -->
<!-- Transactions on the Tangle are immutable, so we can use the transaction as a source of truth. -->
<!-- ::: -->

次に、タングルからトランザクションを読み取り、ファイルハッシュを取得します。ファイルハッシュを取得したら、元のファイルを再度ハッシュ化し、2つハッシュ値を比較します。両方のハッシュ値が同じ場合、元のファイルは変更されていません。
<!-- Then, we read the transaction from the Tangle to retrieve the file hash. When we have the file hash, we hash the original file again and compare the two. If both hashes are the same, the original file is unchanged. -->

### 前提条件
<!-- ### Prerequisites -->

このチュートリアルを完了するには、次のものが必要です。
<!-- To complete this tutorial, you need the following: -->

* Node.js 8、またはNode.js 10以降。 [最新のLTS](https://nodejs.org/en/download/)をお勧めします。
<!-- * Node.js 8, or Node.js 10 or higher. We recommend the [latest LTS](https://nodejs.org/en/download/). -->
* [Visual Studio Code](https://code.visualstudio.com/Download)などのコードエディター
<!-- * A code editor such as [Visual Studio Code](https://code.visualstudio.com/Download) -->
* コマンドプロンプトへのアクセス
<!-- * Access to a command prompt -->
* [`@iota/poex-tool`](https://www.npmjs.com/package/iota-poex-tool)パッケージおよび[`regenerator-runtime`](https://www.npmjs.com/package/regenerator-runtime)パッケージ
<!-- * The [`@iota/poex-tool`](https://www.npmjs.com/package/iota-poex-tool) and the [`regenerator-runtime`](https://www.npmjs.com/package/regenerator-runtime) packages -->

### 手順1. サンプルコントラクトを作成する
<!-- ### Step 1. Create an example contract -->

作業ディレクトリに`contract.txt`という新しいファイルを作成し、次のテキストをコピーします。
<!-- Create a new file called `contract.txt` in your working directory, then copy in the following text -->

  ```bash
  My super secret contract.
  ```

### 手順2. サンプルをセットアップする
<!-- ### Step 2. Set up the sample -->

1. `contract.txt`ファイルと同じディレクトリに`index.js`という新しいファイルを作成し、パッケージを`require`します。
  <!-- 1. Create a new file called `index.js` in the same directory as the `contract.txt` file, then require the packages -->

    ```js
    require('regenerator-runtime');
    const PoEx = require ('@iota/poex-tool');
    ```

2. ハッシュ値をタングルに添付するファイルへのパスを取得する非同期関数を作成します。
  <!-- 2. Create an asynchronous function that takes a path to a file whose hash you want to attach to the Tangle -->

    ```js
    async function publish (file) {
    }
    ```

    :::info:
    この関数のバイナリデータを渡すこともできます。
    :::
    <!-- :::info: -->
    <!-- You can also pass this function binary data. -->
    <!-- ::: -->

3. `publish`関数で、ファイルをハッシュ化し、トランザクションでDevnetノードに送信します。
  <!-- 3. In the `publish` function, hash the file, then send it in a transaction to a Devnet node -->

    ```js
    async function publish (file) {

    const hash = PoEx.hash(file);

        try {
            // Attach the file hash to the Tangle in a bundle
            const bundle = await PoEx.publish({
            provider: 'https://nodes.devnet.iota.org:443',
            data: hash,
            tag: 'POEXTUTORIAL',
            address: 'HELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDD',
            seed: 'PUEOTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX',
            depth: 3,
            minWeightMagnitude: 9
            });
            tailTransaction = bundle[0];
            console.log(`Attached the transaction to the Tangle with hash: ${tailTransaction.hash}`);
            } catch(error) {
            console.log(`Something went wrong: ${error}`);
            }
        }
    }
    ```

4. `publish`関数を実行し、`contract.txt`ファイルへのパスを渡します。
  <!-- 4. Run the `publish` function, and pass it the path to the `contract.txt` file -->

    ```js
    publish('contract.txt');
    ```

    ファイルを実行すると、トランザクションがタングルに添付されていることがわかります。これで、バンドルハッシュを使用して、ファイルが変更されていないことを確認できます。
    <!-- When you execute the file, you should see that the transaction was attached to the Tangle. Now, you can use the bundle hash to verify that the file is unchanged. -->

5. `publish`関数の最後で、ファイルのハッシュ値がタングルに添付されたものとまだ一致していることを検証します。
  <!-- 5. At the end of the `publish` function, verify that the hash of the file still matches the one that was attached to the Tangle -->

    ```js
    // Set the node to check for the transaction
    tailTransaction.provider = 'https://nodes.devnet.iota.org:443';

    // Verifying if the file is unchanged
    const verified = PoEx.verify(tailTransaction, false, file);
    verified? console.log('File verified: The file matches the hash on the Tangle')
    : ('Something has changed. The hash on the Tangle is no longer a match.');
    ```

    :::info:
    ライブラリがDevnetノードからトランザクションをリクエストすることを認識できるように、`provider`フィールドを再度設定します。

    トランザクションはDevnetタングルに接続されているため、Mainnetなどの他のネットワークには存在しません。

    [IOTAネットワークの詳細](root://getting-started/0.1/references/iota-networks.md)をご覧ください。
    :::
    <!-- :::info: -->
    <!-- We set the `provider` field again so that the library knows to request the transaction from a Devnet node. -->
    <!--  -->
    <!-- The transaction is attached to the Devnet Tangle, so it does not exist on any other networks such as the Mainnet. -->
    <!--  -->
    <!-- [Find out more about IOTA networks](root://getting-started/0.1/references/iota-networks.md). -->
    <!-- ::: -->

:::success:おめでとうございます:tada:
これで、タングルを単一の真実のソースとして使用して、ファイルがまだ変更されていないことを確認できました。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You can now use the Tangle as a single source of truth to verify that a file is still unchanged. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

緑色のボタンをクリックして、このチュートリアルのサンプルコードを実行し、Webブラウザーで結果を確認します。
<!-- Click the green button to run the sample code in this tutorial and see the results in the web browser. -->

:::info:
この例では、ファイルを使用しません。代わりに、文字列を`publish`関数に渡し、2番目の引数でバイナリ入力であることを指定します。
:::
<!-- :::info: -->
<!-- In this example, we don't use a file. Instead, we pass a string to the `publish` function and specify that it's a binary input in the second argument. -->
<!-- ::: -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/proof-of-existance-utility?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

次のコマンドを使用して、独自のデバイスでサンプルコードを実行することもできます。
<!-- You can also run the sample code on your own device by using the following command -->

```bash
node index.js
```

ファイルが一致する場合、次の出力が表示されます。
<!-- If the file is a match, you should see the following output: -->

```bash
File verified: The file matches the hash on the Tangle
```

## 次のステップ
<!-- ## Next steps -->

学んだことを使用して、タングル上で署名されたドキュメントを保護するアプリケーションを構築する。参考のために[ドキュメントの不変性の設計図](root://blueprints/0.1/doc-immutability/overview.md)を使用できます。
<!-- Use what you've learned to build an application that secures signed documents on the Tangle. You can use the [document immutability blueprint](root://blueprints/0.1/doc-immutability/overview.md) for inspiration. -->

存在の証明ユーザーインターフェイスを使用して[ファイルハッシュをタングルにアップロード](https://iota-poex.dag.sh)する。
<!-- Use our proof-of-existance user interface to [upload a file hash to the Tangle](https://iota-poex.dag.sh). -->
