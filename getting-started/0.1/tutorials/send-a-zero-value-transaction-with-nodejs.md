# 最初のメッセージをタングルに送信する（Node.js）
<!-- # Send your first message to the Tangle (Node.js) -->

**IOTAを使用すると、IOTAトークンと同様にデータ（ゼロトークン）トランザクションを送信できます。これらのゼロトークントランザクションは、イミュータブルなメッセージをタングルに送信して保存したいアプリケーションに役立ちます。ゼロトークンの[トランザクション](../introduction/what-is-a-transaction.md)のみを送信するためには、IOTAトークンは必要ありません。**
<!-- **IOTA allows you to send data (zero-value) transactions as well as IOTA tokens. These zero-value transactions are useful for applications that want to send and store immutable messages on the Tangle. To send only a zero-value [transaction](../introduction/what-is-a-transaction.md), you don't need any IOTA tokens.** -->

トランザクションを送信するには、ノードに接続してバンドルを作成してからそのバンドルを送信する必要があります。
<!-- To send any transaction, you must connect to a node, create a bundle, then send that bundle to it. -->

## 前提条件
<!-- ## Prerequisites -->

このチュートリアルを完了するには、次のものが必要です。
<!-- To complete this tutorial, you need the following: -->

* Node.js 8、またはNode.js 10以上。[最新のLTS](https://nodejs.org/en/download/)をお勧めします。
<!-- * Node.js 8, or Node.js 10 or higher. We recommend the [latest LTS](https://nodejs.org/en/download/). -->
* [Visual Studio Code](https://code.visualstudio.com/Download)のようなコードエディタ
<!-- * A code editor such as [Visual Studio Code](https://code.visualstudio.com/Download) -->
* コマンドプロンプトへのアクセス
<!-- * Access to a command prompt -->
* インターネット接続
* An Internet connection

## イミュータブルなメッセージをタングルに添付する
<!-- ## Attach an immutable message to the Tangle -->

この例では、[Devnetノード](../references/iota-networks.md#devnet)に接続します。 Devnetは、トークンが無料であること以外はMainnetと似ています。Devnetに送信したトランザクションは、Mainnetのような他のネットワークには存在しません。
<!-- In this example, we connect to a [Devnet node](../references/iota-networks.md#devnet). The Devnet is similar to the Mainnet, except the tokens are free. Any transactions that you send to the Devnet do not exist on other networks such as the Mainnet. -->

1. コマンドプロンプトで、`iota-example`という作業ディレクトリを作成します。
  <!-- 1. In the command prompt, create a working directory called `iota-example` -->

  ```bash
  mkdir iota-example
  ```

2. `iota-example`ディレクトリに移動して、`core`および`converter`IOTAクライアントライブラリをインストールします。
  <!-- 2. Change into the `iota-example` directory and install the `core` and `converter` IOTA client libraries -->

    ```bash
    cd iota-example
    npm install @iota/core @iota/converter --save
    ```

    すべてうまくいけば、標準出力に次のようなものが表示されるはずです。 'npm WARN'メッセージは無視してかまいません。
    <!-- If everything went well, you should see something like the following in the output. You can ignore any 'npm WARN' messages. -->

    ```shell
    + @iota/converter@1.0.0-beta.8
    + @iota/core@1.0.0-beta.8
    added 19 packages from 10 contributors and audited 68 packages in 5.307s
    found 0 vulnerabilities
    ```

    これで、`package.json`ファイルと、IOTAクライアントライブラリとその依存関係を含む`node_modules`ディレクトリができました。
    <!-- You now have a `package.json` file and a `node_modules` directory, which contains the IOTA client libraries and their dependencies. -->

3. `iota-example`ディレクトリに、`data-transaction.js`という新しいファイルを作成します。
  <!-- 3. In the `iota-example` directory, create a new file called `data-transaction.js` -->

4. IOTAクライアントライブラリが必要です。
  <!-- 4. Require the IOTA client libraries -->

    ```js
    // Require the IOTA libraries
    const Iota = require('@iota/core');
    const Converter = require('@iota/converter');
    ```

5. ノードに接続します。
  <!-- 5. Connect to a node -->

    ```js
    // Create a new instance of the IOTA object
    // Use the `provider` field to specify which IRI node to connect to
    const iota = Iota.composeAPI({
      provider: 'https://nodes.devnet.iota.org:443'
    });
    ```

6. メッセージを送信したいアドレスを格納するための変数を作成します。
  <!-- 6. Create a variable to store the address to which you want to send a message -->

    ```js
    const address =
    'HELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDD';
    ```

    :::info:
    今回はIOTAトークンを送信しないので、このアドレスは誰にも属している必要はありません。アドレスが有効であるためには、ただ81[トライト](root://iota-basics/0.1/concepts/trinary.md)で構成されている必要があるだけです。
    :::
    <!-- :::info: -->
    <!-- You aren't sending any IOTA tokens, so this address does not have to belong to anyone. To be valid, the address just needs to consist of 81 [trytes](root://iota-basics/0.1/concepts/trinary.md). -->
    <!-- ::: -->

7. シードを保存するための変数を作成します。
  <!-- 7. Create a variable to store your seed -->

    ```js
    const seed =
    'PUEOTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX';
    ```

    :::info:
    このシードはIOTAトークンを持つアドレスを含む必要がありません。 81文字未満のシードを入力した場合、ライブラリは末尾に9を追加して81文字にします。
    :::
    <!-- :::info: -->
    <!-- This seed doesn't have to contain any addresses with IOTA tokens. If you enter a seed that consists of less than 81 characters, the library will append 9s to the end of it to make 81 characters. -->
    <!-- ::: -->

8. アドレスに送信したいメッセージを作成し、メッセージをトライトに変換します。
  <!-- 8. Create a message that you want to send to the address and convert it to trytes -->

    ```js
    const message = Converter.asciiToTrytes('Hello World!');
    ```

    :::info:
    IOTAネットワークは、[トライトにエンコード](root://iota-basics/0.1/concepts/trinary.md)されたメッセージのみを受け入れます。
    :::
    <!-- :::info: -->
    <!-- IOTA networks accept only [tryte-encoded](root://iota-basics/0.1/concepts/trinary.md) messages. -->
    <!-- ::: -->

    :::info:
    `asciiToTrytes()`メソッドは[基本的なASCII文字](https://en.wikipedia.org/wiki/ASCII#Printable_characters)のみをサポートします。その結果、アクセントやウムラウトなどの発音区別符号はサポートされず、`INVALID_ASCII_CHARS`エラーが発生します。
    :::
    <!-- :::info: -->
    <!-- The `asciiToTrytes()` method supports only [basic ASCII characters](https://en.wikipedia.org/wiki/ASCII#Printable_characters). As a result, diacritical marks such as accents and umlauts aren't supported and result in an `INVALID_ASCII_CHARS` error. -->
    <!-- ::: -->

9. 送信するIOTAトークンの量、送信するメッセージ、および送信先のアドレスを指定する転送オブジェクトを作成します。
  <!-- 9. Create a transfer object that specifies the amount of IOTA tokens you want to send, the message that you want to send, and the address to send it to -->

    ```js
    const transfers = [
    {
        value: 0,
        address: address,
        message: message
    }
    ];
    ```

10. `転送`オブジェクトから[バンドル](../introduction/what-is-a-bundle.md)を作成するには、それを[`prepareTransfers()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.prepareTransfers)メソッドに渡します。次に、返されたバンドルのトライトを`sendTrytes()`メソッドに渡して、[チップ選択](root://the-tangle/0.1/concepts/tip-selection.md)、[プルーフオブワーク](root://the-tangle/0.1/concepts/proof-of-work.md)、および[ノード](../introduction/what-is-a-node.md)へのバンドル送信を行います。
  <!-- 10. To construct a [bundle](../introduction/what-is-a-bundle.md) from your `transfers` object, pass it to the [`prepareTransfers()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.prepareTransfers) method. Then, pass the returned bundle trytes to the `sendTrytes()` method to do [tip selection](root://the-tangle/0.1/concepts/tip-selection.md), [proof of work](root://the-tangle/0.1/concepts/proof-of-work.md), and send the bundle to the [node](../introduction/what-is-a-node.md) -->

    ```js
    iota.prepareTransfers(seed, transfers)
        .then(trytes => {
            return iota.sendTrytes(trytes, 3/*depth*/, 9/*minimum weight magnitude*/)
        })
        .then(bundle => {
        console.log(`Bundle: ${JSON.stringify(bundle, null, 1)}`)
    })
    .catch(err => {
            // Catch any errors
        console.log(err);
    });
    ```

    :::info:Depth
    `depth`引数はチップ選択に影響します。depthが深ければ深いほど、タングルのより奥から重み付きランダムウォークが始まります。
    :::
    <!-- :::info:Depth -->
    <!-- The `depth` argument affects tip selection. The greater the depth, the farther back in the Tangle the weighted random walk starts. -->
    <!-- ::: -->

    :::info:Minimum weight magnitude
    [`minimum weight magnitude`](root://iota-basics/0.1/concepts/minimum-weight-magnitude.md)（MWM）は、フルーフオブワーク（PoW）の困難さに影響を与えます。 MWMが大きいほど、PoWはより困難になります。

    すべてのIOTAネットワークはそれぞれのMWMを強制します。 Devnetでは、MWMは9です。一方、Mainnetでは、MWMは14です。小さすぎるMWMを使用すると、トランザクションは有効にならず、確定もされません。
    :::
    <!-- :::info:Minimum weight magnitude -->
    <!-- The [`minimum weight magnitude`](root://iota-basics/0.1/concepts/minimum-weight-magnitude.md) (MWM) argument affects the difficulty of proof of work (PoW). The greater the MWM, the more difficult the PoW. -->
    <!--  -->
    <!-- Every IOTA network enforces its own MWM. On the Devnet, the MWM is 9. But, on the Mainnet the MWM is 14. If you use a MWM that's too small, your transactions won't be valid and will never be confirmed. -->
    <!-- ::: -->

:::success:おめでとうございます:tada:
初めてのゼロトークントランザクションを送信しました。トランザクションは[タングル](../introduction/what-is-the-tangle.md)に添付されるので、メッセージはイミュータブルになります。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just sent your first zero-value transaction. Your transaction is attached to [the Tangle](../introduction/what-is-the-tangle.md), which makes your message immutable. -->
<!-- ::: -->

コンソールには、送信した[バンドル](../introduction/what-is-a-bundle.md)に関する情報が表示されます。
<!-- In the console, you'll see information about the the [bundle](../introduction/what-is-a-bundle.md) that you sent. -->

バンドル内のトランザクションは、すべてのノードが各々の元帳にトランザクションを書き込むまで、ネットワーク内を伝搬します。
<!-- The transaction in your bundle will propagate through the network until all the nodes have it in their ledgers. -->

## トランザクションがネットワーク上にあることを確認する
<!-- ## Confirm that your transaction is on the network -->

トランザクションが（タングルに接続された）ネットワーク上にあることを確認するには、コンソールから`bundle`フィールドの値をコピーし、[Devnetタングルエクスプローラ](https://devnet.thetangle.org/)を開き、`bundle`フィールドの値を検索バーに貼り付けます。
<!-- To confirm that your transaction is on the network (attached to the Tangle), copy the value of the `bundle` field from the console, open a [Devnet Tangle explorer](https://devnet.thetangle.org/), and paste the value into the search bar. -->

メッセージフィールドにメッセージが表示されます。
<!-- You'll see your message in the Message field. -->

![Immutable message on the Tangle](../images/zero-value-message.png)

:::info:
親トランザクションフィールドを表示して、自分のトランザクションがどのトランザクションに添付されているかを確認することもできます。

親トランザクションはチップ選択中に選択され、トランザクションの [`branchTransaction`と`trunkTransaction`フィールド](root://iota-basics/0.1/references/structure-of-a-transaction.md)に追加されます。
:::
<!-- :::info: -->
<!-- You can also see the Parent transactions field to check which transactions your transaction is attached to in the Tangle. -->
<!--  -->
<!-- These transactions were chosen during tip selection and added to the [`branchTransaction` and `trunkTransaction` fields](root://iota-basics/0.1/references/structure-of-a-transaction.md) of your transaction. -->
<!-- ::: -->

## コードを走らせる
<!-- ## Run the code -->
このチュートリアルのサンプルコードを実行してWebブラウザで結果を確認するには、緑色のボタンをクリックします。
<!-- Click the green button to run the sample code in this tutorial and see the results in the web browser. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/51-Send-ASCII-Data?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

:::info:
[トランザクションフィールドが何を意味するのかを学んでみましょう](root://iota-basics/0.1/references/structure-of-a-transaction.md)。

メッセージは`signatureMessageFragment`フィールドに格納されます。
:::
<!-- :::info: -->
<!-- [Learn what these transaction fields mean](root://iota-basics/0.1/references/structure-of-a-transaction.md). -->
<!--  -->
<!-- Messages are stored in the `signatureMessageFragment` field. -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

[テスト用のIOTAトークンを送信する](../tutorials/send-iota-tokens.md)
<!-- [Send some test IOTA tokens](../tutorials/send-iota-tokens.md) -->

バンドルがどのように構成されているかを知るために、[2つのゼロトークントランザクションのバンドルを送信する](root://iota-basics/0.1/how-to-guides/send-bundle.md)。
<!-- [Send a bundle of two zero-value transactions](root://iota-basics/0.1/how-to-guides/send-bundle.md) to learn how bundles are structured. -->

第三者に頼らずにタングルに直接アクセスするために、[Dockerコンテナ内で自分のノードを実行する](../tutorials/run-your-own-iri-node.md)。
<!-- [Run your own node in a Docker container](../tutorials/run-your-own-iri-node.md) for direct access to the Tangle without relying on third parties. -->
