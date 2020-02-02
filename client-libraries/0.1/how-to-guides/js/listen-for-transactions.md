# Node.js でライブトランザクションをリッスンする
<!-- # Listen for live transactions in Node.js -->

**このガイドでは，[IRI ノードソフトウェア](root://node-software/0.1/iri/introduction/overview.md)を実行する[ノード](root://getting-started/0.1/network/nodes.md)の[ゼロメッセージキュー（ZMQ）](https://zeromq.org/)にサブスクライブして，タングル上の最近のトランザクションをリッスンします．**
<!-- **In this guide, you listen to the Tangle for recent transactions by subscribing to the [zero message queue (ZMQ)](https://zeromq.org/) on [nodes](root://getting-started/0.1/network/nodes.md) that run the [IRI node software](root://node-software/0.1/iri/introduction/overview.md).** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには，次のパッケージをインストールする必要があります．
<!-- To complete this guide, you need to install the following package: -->

--------------------
### npm
```bash
npm install zeromq
```
---
### Yarn
```bash
yarn add zeromq
```
--------------------

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは，[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します．
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. パッケージをリクワイアし，ZMQ サブスクライブソケットを作成します．
  <!-- 1. Require the package and create a ZMQ subscribe socket -->

    ```js
    const zmq = require('zeromq');
    const sock = zmq.socket('sub');
    ```

2. ソケットをノードの ZMQ ポートに接続します．
  <!-- 2. Connect the socket to a node's ZMQ port -->

    ```js
    sock.connect('tcp://zmq.devnet.iota.org:5556');
    ```

3. スクリプトを追加の引数なしで実行する場合，[`tx`](root://node-software/0.1/iri/references/zmq-events.md) イベントをサブスクライブしてすべてのトランザクションを表示します．それ以外の場合は，アドレスイベントをサブスクライブして，アドレスに送信されたトランザクションのみを表示します．
  <!-- 3. If the script is executed with no additional arguments, subscribe to the [`tx`](root://node-software/0.1/iri/references/zmq-events.md) event to see all transactions, otherwise subscribe to the address event to see only transactions that were sent to that address -->

    ```js
    // コマンドラインの引数をチェックします
    if (!process.argv[2]) {
        //コマンドラインにアドレスを追加するようユーザーに要求します
        console.log('Listening for all transactions')
        console.log('---------------------')
        console.log('If you want to listen for transactions that are sent to a particular address,');
        console.log('pass the address to the `node` command as a command-line argument.');
        console.log('For example: node 6-zmq-listen.js AN...ADDRESS')

        //ノードが受信するすべてのトランザクションをサブスクライブします
        sock.subscribe('tx');
    } else {
        console.log('Listening for transactions sent to this address: ' + process.argv[2])
        console.log(
            'Remember to send a transaction to this address, and be patient: It can take 30seconds for the transaction to appear.')
        // CLI経由で渡されたアドレスをサブスクライブします
        sock.subscribe(process.argv[2])
    }
    ```

    :::info:
    アドレスイベントのトライトを除くすべてのイベントは小文字でなければなりません．アドレスイベントは大文字でなければなりません．
    :::
    <!-- :::info: -->
    <!-- All events must be in lowercase letters except the trytes of the address event, which must be in uppercase letters. -->
    <!-- ::: -->

4. ノードが返すイベントデータを処理します．
  <!-- 4. Process the event data that the node returns -->

    ```js
    sock.on('message', msg => {
    // データを配列に分割します
        const data = msg.toString().split(' ');
        switch (
            // インデックス0を使用してトピックの名前と一致させます
            data[0]
        ) {
            // ノードが受信したすべてのトランザクションを表示します
            case 'tx':
                console.log(`I'm a transaction!`, data)
                break
            // 指定されたアドレスに送信されたトランザクションのみを表示します
            case process.argv[2]:
                console.log(`I'm the transaction you are looking for!`, data);
                break
        }
    });
    ```

    コンソールには，次のようなものが表示されるはずです．

    ```shell
    Listening for all transactions
    ---------------------
    If you want to listen for transactions that are sent to a particular address,
    pass the address to the `node` command as a command-line argument.
    For example: node 6-zmq-listen.js AN...ADDRESS
    I'm a transaction! [ 'tx',
    'ZHSNSJFUYIUGPWLK9JZWWQJFHHPIYMYWMXCQPKMUTAYVDNPWYCKAWBXWVFFXOHBKVAUZOKVUCLMEER999',
    'XMBNQGZLNYMNAHFUNBCTYNKKO9IVVDVGIWYQFONUNYUQWPHBIEULTEN9GNYMNWCVPVFBNJFHIZNKJJAUM',
    '0',
    'SEMARKETMAM9999999999999999',
    '1572612274',
    '4',
    '5',
    'JVLVVTESJLCVKSJSGXXKUTBZLEKNRFB9NNSHVHJXBDIHUJRHBXQJQQBLYER9KQRKFLPZI9EVZFFPTTSCX',
    'S9XVKPZDMI9VHLON9BN9FQLIHWZSNGOYAUH9CWBWZFNTWRZYZODT9UHBHNRXAAGFKTBBZRDNROGPHG999',
    'SB9UKUMXQEDETOCOH9CBKVAPDFHFHFDBUKYQBUULKUUTWBBLARBWHTSRLIZSHLDOBCUOPIHXFNODRO999',
    '1572612275265',
    'SEMARKETMAM9999999999999999' ]
    ```

:::success:おめでとうございます:tada:
受信トランザクションのノードをモニタリングしています．
:::
<!-- :::success:Congratulations :tada: -->
<!-- You're monitoring a node for incoming transactions. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

[REPL.it ツール](https://repl.it)を使用して，ブラウザーでサンプルコードを実行できます．
<!-- We use the [REPL.it tool](https://repl.it) to allow you to run sample code in the browser. -->

このガイドのサンプルコードを実行してウィンドウに結果を表示するには，緑色のボタンをクリックします．
<!-- Click the green button to run the sample code in this guide and see the results in the window. -->

:::info:
ノードからデータを受信するのに1〜2分かかる場合があります．
:::
<!-- :::info: -->
<!-- It may take a minute or two to receive data from the node. -->
<!-- ::: -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/ZMQ-example-Nodejs?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## 次のステップ
<!-- ## Next steps -->

[マスクされた認証済みメッセージングを使用して](../../mam/introduction/overview.md)，他のユーザーがタングルでサブスクライブできるデータの暗号化ストリームを送信します．
<!-- [Start using Masked Authenticated Messaging](../../mam/introduction/overview.md) to send encrypted streams of data that others can subscribe to on the Tangle. -->

インスピレーションについては，[アプリ設計図](root://blueprints/0.1/introduction/overview.md)をご覧ください．
<!-- Take a look at our [app blueprints](root://blueprints/0.1/introduction/overview.md) for inspiration. -->
