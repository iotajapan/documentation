# ゼロトークントランザクションのバンドルを送信する
<!-- # Send a bundle of zero-value transactions -->

**トランザクションは、ノードに送信される前にバンドルにまとめられる必要があります。 IOTAクライアントライブラリには、転送オブジェクトからバンドルを作成する組み込み関数があります。**
**Transactions must be packaged in a bundle before being sent to a node. The IOTA client libraries have built-in functions that create bundles from transfer objects.**

:::info:クライアントライブラリを初めて使用されますか？
公式クライアントライブラリを使い始めるための[クイックスタートガイド](root://getting-started/0.1/tutorials/get-started.md)を試してください。
:::
<!-- :::info:First time using a client library? -->
<!-- [Try our quickstart guide](root://getting-started/0.1/tutorials/get-started.md) for getting started with the official client libraries. -->
<!-- ::: -->

:::info:
バンドルまたはトランザクションという用語に慣れていない場合は、[バンドルとトランザクション](../concepts/bundles-and-transactions.md)をお読みください。
:::
<!-- :::info: -->
<!-- If you're unfamiliar with the terms bundle or transaction, we recommend that you [read about bundles and transactions](../concepts/bundles-and-transactions.md). -->
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

## ゼロトークントランザクションのバンドルを送信する
<!-- ## Send a bundle of zero-value transactions -->

この例では、[Devnetノード](root://getting-started/0.1/references/iota-networks.md#devnet)に接続します。 Devnetは、トークンが無料であること以外はMainnetとほぼ同じです。Devnetに送信したトランザクションは、Mainnetのような他のネットワークには存在しません。
<!-- In this example, we create and send a bundle to a [Devnet node](root://getting-started/0.1/references/iota-networks.md#devnet). The Devnet is similar to the Mainnet, except the tokens are free. Any transactions that you send to the Devnet do not exist on other networks such as the Mainnet. -->

1. IOTAライブラリが必要です。
  <!-- 1. Require the IOTA libraries -->

    ```js
    const Iota = require('@iota/core');
    const Converter = require('@iota/converter');
    ```

2. IOTAオブジェクトのインスタンスを作成し、`provider`フィールドを使用してIRIノードに接続します。
  <!-- 2. Create an instance of the IOTA object and use the `provider` field to connect to an IRI node -->

    ```js
    const iota = Iota.composeAPI({
        provider: 'https://nodes.thetangle.org:443'
    });
    ```

3. トランザクションを送信する先の2つのアドレスとシードを格納する変数を作成します。
  <!-- 3. Create the variables to store a seed and two addresses to which you want to send transactions -->

    ```js
    const seed =
    'PUETTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX';

    var recipientAddress1 = "CXDUYK9XGHC9DTSPDMKGGGXAIARSRVAFGHJOCDDHWADLVBBOEHLICHTMGKVDOGRU9TBESJNHAXYPVJ9R9";

    var recipientAddress2 = "CYJV9DRIE9NCQJYLOYOJOGKQGOOELTWXVWUYGQSWCNODHJAHACADUAAHQ9ODUICCESOIVZABA9LTMM9RW";
    ```

    :::info:
    シードを使用するコードはすべてクライアント側で実行されます。シードがご使用中のデバイスから離れることはありません。
    :::
    <!-- :::info: -->
    <!-- Any code that uses a seed is executed on the client side. Your seed never leaves your device. -->
    <!-- ::: -->

4. 送信するトランザクションごとに`transfer`オブジェクトを1つ作成します。`address`フィールドは、送信先のアドレスです。
  <!-- 4. Create one `transfer` object for each transaction that you want to send. The `address` field contains the address to which the transaction will be sent. -->

    ```js
    var transfer1 = {
        'address': recipientAddress1,
        'value': 0,
        'message': Converter.asciiToTrytes('Hello, this is my first message'),
        'tag': 'SENDABUNDLEOFTRANSACTIONS'
    };

    var transfer2 = {
        'address': recipientAddress2,
        'value': 0,
        'message': Converter.asciiToTrytes('Hello, this is my second message'),
        'tag': 'SENDABUNDLEOFTRANSACTIONS'
    };
    ```

    :::info:
    `asciiToTrytes()`メソッドは[基本的なASCII文字](https://en.wikipedia.org/wiki/ASCII#Printable_characters)のみをサポートします。その結果、アクセントやウムラウトなどの発音区別符号やひらがなや漢字などの日本語（2バイト文字）はサポートされておらず、`INVALID_ASCII_CHARS`エラーが発生します。
    :::
    <!-- :::info: -->
    <!-- The `asciiToTrytes()` method supports only [basic ASCII characters](https://en.wikipedia.org/wiki/ASCII#Printable_characters). As a result, diacritical marks such as accents and umlauts aren't supported and result in an `INVALID_ASCII_CHARS` error. -->
    <!-- ::: -->

5. バンドルを作成し、返されたバンドルのトライトを`sendTrytes()`ソッドに渡して、[チップ選択](root://the-tangle/0.1/concepts/tip-selection.md)、[プルーフオブワーク](root://the-tangle/0.1/concepts/proof-of-work.md)、およびバンドルのノードへの送信を行います。
  <!-- 5. Create a bundle and pass the returned bundle trytes to the `sendTrytes()` method to do [tip selection](root://the-tangle/0.1/concepts/tip-selection.md), [proof of work](root://the-tangle/0.1/concepts/proof-of-work.md), and send the bundle to the node. -->

    ```js
    iota.prepareTransfers(seed, [transfer1, transfer2])
      .then(function(trytes){
          return iota.sendTrytes(trytes, 3, 9);
    })

      .then(results => console.log(JSON.stringify(results, ['hash', 'currentIndex', 'lastIndex', 'bundle', 'trunkTransaction', 'branchTransaction'], 1)));
    ```

    コンソールに、送信したバンドルに関する情報が表示されます。`currentIndex`フィールドは、バンドル内のトランザクションの位置です。`lastIndex`フィールドは、バンドル内の最後のトランザクションです。`lastIndex`フィールドは、バンドル内のトランザクションの総数を表します。
    <!-- In the console, you'll see information about the bundle that you sent. The `currentIndex` field is the position of the transaction in the bundle. The `lastIndex` field is the last transaction in the bundle, which indicates the total number of transactions in it. -->

    ```json
    [
    {
    "hash": "9FVWBYVGTMDYPIYGMEHWQSZF9CDWHRADQNYIEJARTMXFSBTSAIFJPM9PNILLLBYIKSMIIDUOVSBWZ9999",
    "currentIndex": 0,
    "lastIndex": 1,
    "bundle": "TKLFNBRZCDUUOYBPHDIKKGSSVKLQINECAZHEKHJBPXZYXVXCDCLXZDQGUXTSZVWJVYABICHESIXXXLZU9",
    "trunkTransaction": "JIKDFORYEREMFYHLJHZGARNECTUUYYKSIVILDMEAPDYYXCVZHPVRJQDHKKWXMYGTUHBRBVYJXKTNA9999",
    "branchTransaction": "IRWPFAQQHSPRL9QBEQRSUSVEAAHQCNILEHJNUYZEQCQBFFLEV9FSGJQH9DZNKCHCOKGMAIXAUDBZZ9999"
    },
    {
    "hash": "JIKDFORYEREMFYHLJHZGARNECTUUYYKSIVILDMEAPDYYXCVZHPVRJQDHKKWXMYGTUHBRBVYJXKTNA9999",
    "currentIndex": 1,
    "lastIndex": 1,
    "bundle": "TKLFNBRZCDUUOYBPHDIKKGSSVKLQINECAZHEKHJBPXZYXVXCDCLXZDQGUXTSZVWJVYABICHESIXXXLZU9",
    "trunkTransaction": "IRWPFAQQHSPRL9QBEQRSUSVEAAHQCNILEHJNUYZEQCQBFFLEV9FSGJQH9DZNKCHCOKGMAIXAUDBZZ9999",
    "branchTransaction": "JPFAFQLMVHJYDWLPZUBKRWQIPYXUJUORQPYKBOLKRLAQKRDKVYWYZRQQEFSARZRPNZTGQANOIATT99999"
    }
    ]
    ```

    :::info:トランクトランザクションとブランチトランザクションは、親トランザクションと呼ばれます。

    [バンドル内のすべてのトランザクションは、それらの`trunkTransaction`フィールドの値によって接続されています](../references/structure-of-a-bundle.md)。トランザクション0の`trunkTransaction`ハッシュ値は、トランザクション1のトランザクションハッシュ値（`hash`）と同じであることがわかります。
    :::
    <!-- :::info:Trunk and branch transactions are called parent transactions. -->
    <!--  -->
    <!-- [All transactions in a bundle are connected through the value of their `trunkTransaction` fields](../references/structure-of-a-bundle.md). You should see that the `trunkTransaction` hash of transaction 0 is the same as the transaction hash (`hash`) of transaction 1. -->
    <!-- ::: -->

6. 1番目のトランザクションの詳細を見るには、1番目のトランザクションのハッシュ値をコピーして[devnet.thetangle.org](https://devnet.thetangle.org/)に貼り付けます。これらの詳細は、Webサイトが接続しているノードから供給されています。
  <!-- 6. To see details about your first transaction, copy the hash of the first transaction and paste it into [devnet.thetangle.org](https://devnet.thetangle.org/). These details have been sourced from the nodes that the website is connected to. -->

    ![Transaction in a Tangle explorer](../images/tangle-explorer.PNG)

7. 2番目のトランザクションの詳細を表示するには、`Parent transactions`までスクロールしてTrunkのハッシュ値をクリックします。
  <!-- 7. To see details about your second transaction, scroll down to 'Parent transactions' and click the Trunk hash -->

    ![Trunk transaction in a Tangle explorer](../images/tangle-explorer-trunk.PNG)

## コードを走らせる
<!-- ## Run the code -->

このガイドのサンプルコードを実行してWebブラウザに結果を表示するには、緑色のボタンをクリックしてください。
<!-- Click the green button to run the sample code in this guide and see the results in the web browser. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Send-bundle?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>
