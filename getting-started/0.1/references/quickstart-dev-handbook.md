# IOTA開発者ハンドブック
<!-- # IOTA developer's handbook -->

このハンドブックでは、IOTAテクノロジーの概要を説明し、あなた自身のアプリケーションでIOTAテクノロジーをどのように活用できるかを特定するのに役立ちます。
<!-- This handbook gives you a walk through the IOTA technology to help you to identify how best you can leverage it in your own applications. -->

## IOTAネットワークとは？
<!-- ## What is an IOTA network? -->

IOTAネットワークは、すべてのノードがタングルのコピーを保存する相互接続されたノードの集まりです。各IOTAネットワークのタングルには異なるトランザクションが含まれているため、IOTAトークンの分布はそれぞれ異なります。
<!-- An IOTA network is a collection of interconnected nodes that each store a copy of the Tangle. In each IOTA network, the Tangle contains different transactions, so the distribution of IOTA tokens is different in each one. -->

IOTA財団は、誰でもがノードを実行でき、ノードに接続でき、トランザクションの読み取り/書き込みができる2つの参加に許可を必要としない（パーミッションレス）ネットワークを維持しています。
<!-- The IOTA Foundation maintains two permissionless networks, where anyone can run a node, connect to them, and read/write transactions. -->

パーミッションレスネットワークは公開されているため、トランザクションで送信するデータは誰でも閲覧できます。ただし、暗号化とMAMチャネルの使用により、トランザクション内のデータをいつでも保護できます。
<!-- Permissionless networks are public, so any data you send in a transaction is open to anyone. But, you can always protect the data in transactions through encryption and the use of MAM channels. -->

|**タイプ**|**ステータス**|**説明**|
|:---------|:-------------|:-------|
|Mainnet|アクティブ/成長中|メインIOTAネットワーク。IOTAトークンには、暗号資産交換所などのプラットフォームで取引できる価値があります。このネットワークは数千のノードで構成され、誰でも利用できます。|
|Devnet|アクティブ|IOTAトークンにテスト目的以外の価値がない開発IOTAネットワーク。このネットワークは、トランザクションを作成して送信するために必要な時間と計算能力が少ないことを除けば、メインネットと同じです。|

<!-- |**Type** |**Status**|**Description**| -->
<!-- :-----|:------|:---------| -->
<!-- :-----|:------|:---------| -->
<!-- |Mainnet|Active and growing|The main IOTA network, where the IOTA token has value that's traded on platforms such as cryptocurrency exchanges. This network consists of thousands of nodes and is open to everybody. -->
<!-- |Devnet|Active|The development IOTA network where the IOTA token has no value except for testing purposes. This network is like the Mainnet except it takes less time and computational power to create and send a transaction. -->

### プライベートIOTAネットワークを実行できるか？
<!-- ### Can I run a private IOTA network? -->

IOTAはオープンソーステクノロジーであるため、誰でも自由に独自のIOTAネットワークを実行して公開できます。プライベートIOTAネットワークを**プライベートタングル**と呼びます。
<!-- IOTA is an open-source technology, so anyone is free to run their own IOTA network and make it public. We call a private IOTA network a **private Tangle**. -->

プライベートタングルは、ユーザーが制御するIOTAネットワークであり、既知のノードのみが含まれます。このタイプのネットワークは、制御することを除いて、パブリックIOTAネットワークと同じテクノロジーを使用します。
<!-- A private Tangle is an IOTA network that you control and that contains only nodes that you know. This type of network uses the same technology as the public IOTA networks, except you control it. -->

プライベートタングルのIOTAトークンはIOTA Mainnetでは無効であるため、価値がありません。
<!-- Any IOTA tokens on a private Tangle are not valid on the IOTA Mainnet, and as such, they do not have any value. -->

### IOTA Mainnetを使用するコストはどのくらいか？
<!-- ### What is the cost of using the IOTA Mainnet? -->

IOTA Mainnetは無料で使用できます。 IOTAノードへのIOTAトランザクションの送信に関連する費用も、処理手数料もありません。IOTAは無料のプロトコルです。
<!-- The IOTA Mainnet is free to use. There are no costs associated for sending IOTA transactions to an IOTA node, nor any processing fee. IOTA is a feeless protocol.  -->

IOTAデータトランザクションの送信には、クライアントが暗号資産（IOTAトークン）を所有する必要はありません。
<!-- Sending IOTA data transactions does not require clients to own any cryptocurrency. -->

https://thetangle.org などの一部のノード（パーマノード）は、すべてのトランザクションを永続的に保存するオプションをクライアントに提供します。通常、トランザクションは[ローカルスナップショット](root://node-software/0.1/iri/concepts/local-snapshot.md)の結果としてノードの台帳から削除されます。
<!-- Some nodes (permanodes) such as https://thetangle.org might offer clients the option to permanently store all transactions. Transactions are usually deleted from the ledgers of nodes as a result of a [local snapshot](root://node-software/0.1/iri/concepts/local-snapshot.md). -->

通常、パーマノード接続への費用は月額料金として法定通貨で支払われます。
<!-- This costs is usually requested as a monthly fee paid in fiat currency. -->

別のオプションは、ローカルIOTAノードをデプロイし、IOTA Mainnetに接続して（既存のIOTAノードとピアリングすることにより）、独自のトランザクションを送信することです。そのようなノードは、すべてのトランザクションを永続的に（有効にした場合）保存し、他のトランザクションの前に自分のトランザクションを処理できます（他のクライアントが自分のノードにトランザクションを送信するのを止めることもできます）。
<!-- Another option is to deploy a local IOTA node, connect it to the IOTA Mainnet (by peering it with existing IOTA nodes) and send your own transactions to it. Such node will permanently (if enabled to do so) store all your transactions and will process your transactions before any other transaction (you can even stop other clients from sending transactions to your node). -->

このオプションには、ローエンドサーバーのデプロイメント（物理サーバーまたは仮想サーバー）に関連する最小コストが必要です。
<!-- This option will require minimum cost associated with the deployment (physical or virtual) of a low-end server. -->

### アプリでMainnetを使用するにはどうすればよいか？
<!-- ### How do I use the Mainnet in my app? -->

Mainnetでトランザクションを送信する最も簡単な方法は、クライアントライブラリを使用して、アプリケーションのIOTAロジックを構築することです。
<!-- The easiest way to send transactions on the Mainnet is to use our client libraries to build the IOTA logic of your application. -->

アプリケーションまたはサービスは、IOTA Mainnet上のノードに接続する必要があります。 JavaScriptなどのクライアントライブラリでは、通常、`provider`フィールドで接続するノードを定義します。
<!-- Your application or service needs to connect to a node on the IOTA Mainnet. In the client libraries such as JavaScript, you usually define which node to connect to in the `provider` field. -->

```js
// Require the core package
const Iota = require('@iota/core');

// Create a new instance of the IOTA object
// Use the `provider` field to specify which node to connect to
const iota = Iota.composeAPI({
provider: 'https://nodes.thetangle.org:443'
});
```

:::info:
[利用可能なMainnetノードの一部分のリスト](https://iota.dance/)を参照してください。
:::
<!-- :::info: -->
<!-- See a [partial list of available Mainnet nodes](https://iota.dance/). -->
<!-- ::: -->

:::warning:
すべてのノードがリモートプルーフオブワーク（PoW）をサポートしているわけではありません。 したがって、PoWをサポートしていないノードにトランザクションを送信する場合は、[別のPoWオプション](root://getting-started/0.1/transactions/proof-of-work.md#options-for-doing-proof-of-work)を使用する必要があります。
:::
<!-- :::warning: -->
<!-- Not all the nodes support remote proof of work (PoW). So, if you want to send a transaction to one of these nodes, you will have to use [another PoW option](root://getting-started/0.1/transactions/proof-of-work.md#options-for-doing-proof-of-work). -->
<!-- ::: -->

ノードに接続したら、送信するデータとアドレスを指定する転送オブジェクトを作成する必要があります。
<!-- When you're connected to a node, you need to build a transfer object that specifies what you want to send and to which address. -->

```js
const transfers = [
    {
        value: 0,
        address: address,
        message: message,
        tag: tag,
        obsoleteTag: obsoleteTag
    }
    ];
```

トランザクションをノードに送信する準備ができたら、正しい最小重量値（MWM）を使用していることを確認してください。メインネット上のMWMは14です。
<!-- When you're ready to send the transaction to your node, make sure that you use the correct minimum weight magnitude. The MWM on the Mainnet is 14. -->

```js
iota.prepareTransfers(seed, transfers)
        .then(trytes => {
            return iota.sendTrytes(trytes, 3/*depth*/, 14/*minimum weight magnitude*/)
        })
        .then(bundle => {
        console.log(`Bundle: ${JSON.stringify(bundle, null, 1)}`)
    })
    .catch(err => {
            // Catch any errors
        console.log(err);
    });
```

### アプリでDevnetを使用するにはどうすればよいか？
<!-- ### How do I use the Devnet in my app? -->

Devnetは、トークンが無料であることを除いて、Mainnetとほぼ同じです。Devnetに送信するトランザクションは、Mainnetなどの他のネットワークには存在しません。
<!-- The Devnet is similar to the Mainnet, except the tokens are free. Any transactions that you send to the Devnet do not exist on other networks such as the Mainnet. -->

Mainnetを使用するのと同じように、Devnetでトランザクションを送信する最も簡単な方法は、クライアントライブラリを使用してアプリケーションのIOTAロジックを構築することです。
<!-- In the same way as you use the Mainnet, the easiest way to send transactions on the Devnet is to use our client libraries to build the IOTA logic of your application. -->

DevnetとMainnetを切り替えるには、`provider`フィールドをhttps://nodes.devnet.iota.org:443 などのDevnetノードのURLに変更し、最小重量値（MWM）を9に変更します。
<!-- To switch between the Devnet and Mainnet, just change the `provider` field to the URL of a Devnet node such as https://nodes.devnet.iota.org:443, and change the minimum weight magnitude to 9. -->

### プライベートタングルを設定するにはどうすればよいか？
<!-- ### How do I set up a private Tangle? -->

プライベートタングルをセットアップするには、ローカルIOTA IRIノードとコンパス（オープンソース版コーディネーター）のインスタンスを実行する必要があります。
<!-- To set up a private Tangle you need to run a local IOTA IRI node and an instance of Compass (our open-source Coordinator). -->

:::info:
[プライベートタングルの設定方法](root://compass/0.1/how-to-guides/set-up-a-private-tangle.md)を参照してください。
:::
<!-- :::info: -->
<!-- [Learn how to set up a private Tangle](root://compass/0.1/how-to-guides/set-up-a-private-tangle.md). -->
<!-- ::: -->

### 役立つリンク
<!-- ### Useful links -->

- [Amazon Web Services（AWS）でノードを実行する](https://gitlab.com/iot.fabian.rami/iota-aws-full-node)。（未検証）
<!-- - [Run a node on Amazon Web Services (AWS)](https://gitlab.com/iot.fabian.rami/iota-aws-full-node) (not tested) -->
- [アプリケーション設計図](root://blueprints/0.1/introduction/overview.md)を参照して、IOTAでアプリケーションを作成する方法を確認してください。
<!-- - [Read our application blueprints](root://blueprints/0.1/introduction/overview.md) to see how you can create an application on IOTA -->

## IOTAとIoT
<!-- ## IOTA and the Internet of Things -->

IOTAの目標は、モノのインターネット（IoT）上のデバイスが相互にデータを転送し、相互に支払いを行えるようにすることです。
<!-- The goal of IOTA is to allow devices on the Internet of Things (IoT) to transfer data and make payments among each other. -->

ほとんどのIoTデバイスはIOTAトランザクションに署名して送信するのに十分な計算能力を備えていますが、一部のローエンドデバイスは必要なプルーフオブワークを計算するのに十分なほど強力ではありません。これらのデバイスについては、プルーフオブワークを計算するための別のオプションを選択することをお勧めします。
<!-- While most IoT devices have enough computational power to sign and send IOTA transactions, some low-end devices are not powerful enough to calculate the necessary proof of work. For these devices, we recommend choosing another option for calculating proof of work. -->

:::info:
プルーフオブワークを行うことができるデバイスの例は[STM X-Cube-IOTA1](https://www.st.com/en/embedded-software/x-cube-iota1.html)です。このデバイスは、IOTA Cクライアントライブラリをミドルウェアとして使用します。
:::
<!-- :::info: -->
<!-- An example of a device that can do proof of work is the [STM X-Cube-IOTA1](https://www.st.com/en/embedded-software/x-cube-iota1.html). This devices uses the IOTA C client library as middleware. -->
<!-- ::: -->

現在、IOTAをクラウドIoT環境（AWS IoTやGoogle Cloud IoTなど）に統合する最善の方法についても取り組んでいます。この統合により、タングルを介して物理デバイス間でIoTデータをシームレスに転送できます。
<!-- We are also currently working on how best to integrate IOTA into cloud IoT environments (such as AWS IoT and Google Cloud IoT). This integration will allow you to seamlessly transfer IoT data among physical devices through the Tangle. -->

## マスクされた認証済みメッセージングに関するメモ
<!-- ## A note on Masked Authenticated Messaging -->

MAM（マスクされた認証済みメッセージング）は、IOTAタングルで暗号化されたデータストリームを作成して読み取ることができるセカンドレイヤーの通信プロトコルです。
<!-- MAM (Masked Authenticated Messaging) is a second layer communication protocol that allows you to create and read encrypted data streams on the IOTA Tangle. -->

MAMは、タングルで機密データをストリーミングするのに特に適しています。
<!-- MAM is particularly suitable for streaming sensitive data on the Tangle. -->

現在、[JavaScript MAMライブラリ](root://client-libraries/0.1/mam/introduction/overview.md)があります。このライブラリを使用するには、次のいずれかが必要です。
Currently, we have a [JavaScript MAM library](root://client-libraries/0.1/mam/introduction/overview.md). To use this library, you need one of the following:

- Raspberry PiなどのNode.jsを実行できるIoTデバイス
<!-- - An IoT device that can run Node.js such as a Raspberry Pi -->
- Node.jsを実行して、デバイスが接続できるMAMプロキシとして機能できるサーバー
<!-- - A server that can run Node.js to act as a MAM proxy to which your device can connect -->

:::info:
[MAMチャネルを介してRaspberry Piでセンサーデータをストリーミングする方法](root://utils/0.1/community/raspberry-pi-pub-sub/overview.md)を参照してください。
:::
<!-- :::info: -->
<!-- [Learn how to stream sensor data on a Raspberry Pi through MAM channels](root://utils/0.1/community/raspberry-pi-pub-sub/overview.md). -->
<!-- ::: -->

IOTAコミュニティは、さまざまな言語でMAMを使用できるようにするいくつかのクライアントライブラリラッパーを維持しています。
<!-- The IOTA community maintains some client library wrappers that allow you to use MAM in different languages: -->

- [Bosch XDKプラットフォーム用のXDK2MAMラッパー](https://xdk2mam.io/)
<!-- - [XDK2MAM wrapper for Bosch XDK platform](https://xdk2mam.io/) -->
- [LoRAデバイスのラッパー](https://github.com/xdk2mam/xdk2mam/tree/Workbench-3.6/lora-sdcard)
<!-- - [Wrapper for LoRA devices](https://github.com/xdk2mam/xdk2mam/tree/Workbench-3.6/lora-sdcard) -->

## スマートコントラクト
<!-- ## Smart contracts -->

キュービックと呼ばれるスマートコントラクトに対するIOTA独自のソリューションを開発しています。IOTAスマートコントラクトは、特定の条件が満たされた場合の資金の単純な譲渡を超えます。実際、キュービックの野望は、スマートコントラクトを許可するだけでなく、IoTデバイスのアウトソーシングクォーラムコンピューティングも提供することです。
<!-- We are developing our own solution to smart contracts, called Qubic. IOTA smart contracts will go beyond the simple transfer of funding when a specific condition is met. In fact, the ambition of Qubic is to not only allow smart contracts, but also provide outsourced quorum computing for IoT devices. -->

:::info:
[キュービックの詳細とロードマップの内容](https://qubic.iota.org/intro)を参照してください。
:::
<!-- :::info: -->
<!-- [Learn more about Qubic and see what's on the roadmap](https://qubic.iota.org/intro). -->
<!-- ::: -->
