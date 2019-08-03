# ノードのイベントを購読する
<!-- # Subscribe to events on a node -->

**ノードのZMQポートが有効になっている場合は、トランザクション確定などのイベントを購読できます。これらのイベントにより、IOTAネットワークからのリアルタイムデータが得られます。**
<!-- **If a node has the ZMQ port enabled, you can subscribe to its events such as transaction confirmations. These events give you real-time data from an IOTA network.** -->

以下を実行するために、[ゼロメッセージキュー（ZMQ）](../concepts/zero-message-queue.md)のイベントを購読することができます。
<!-- You may want to subscribe to events in the [zero message queue (ZMQ)](../concepts/zero-message-queue.md) to do the following: -->

* あるアドレスにトランザクションが送信されて確定したことを監視する。
<!-- * Monitor an address for when a transaction is sent to it and confirmed -->
* [thetangle.org](https://thetangle.org/)などのタングルビジュアライゼーションWebサイトを作成する。
<!-- * Create a Tangle visualisation website, such as [thetangle.org](https://thetangle.org/) -->

次のようにして、ZMQのイベントを購読することができます。
<!-- You can subscribe to events in the ZMQ by doing the following: -->

1. オープンソースのZMQライブラリをインストールする。
<!-- 1. Install the open-source ZMQ library -->

2. ZMQソケットを作成し、それを[ZMQ対応設定パラメーター](../references/iri-configuration-options.md#zmq-enabled)が`true`に設定されているIRIノードに接続する。
<!-- 2. Create a ZMQ socket and connect it to an IRI node that has the [`ZMQ-enabled` configuration parameter](../references/iri-configuration-options.md#zmq-enabled) set to `true` -->

3. ZMQからのイベントを購読する。
<!-- 3. Subscribe to events from the ZMQ -->

以下のハウツーガイドでは、Node.jsとPythonを使用していますが、[ZMQライブラリでサポートされている任意のプログラミング言語](http://zguide.zeromq.org/page:all)も使用できます。
<!-- In the following how-to guide we use Node.js and Python, but you could use any [programming language that is supported by the ZMQ library](http://zguide.zeromq.org/page:all). -->

## 前提条件
<!-- ## Prerequisites -->

このガイドのコードサンプルを使用するには、次のものが必要です。
<!-- To use the code samples in this guide, you must have the following: -->

* [Node.js (8+)](https://nodejs.org/en/)または[Python (3+)](https://www.python.org/downloads/)と[PIP](https://pip.pypa.io/en/stable/installing/)
<!-- * [Node.js (8+)](https://nodejs.org/en/) or [Python (3+)](https://www.python.org/downloads/) and [PIP](https://pip.pypa.io/en/stable/installing/) -->
* [Visual Studio Code](https://code.visualstudio.com/Download)のようなコードエディタ
<!-- * A code editor such as [Visual Studio Code](https://code.visualstudio.com/Download) -->
* コマンドプロンプトへのアクセス
<!-- * Access to a command prompt -->
* インターネットへの接続
<!-- * An Internet connection -->

## 最近確定されたトランザクションをリッスンする
<!-- ## Listen for recently confirmed transactions -->

ノードのZMQで`sn`イベントを購読して、最近確定されたトランザクションをリッスンすることができます。`sn`イベントから次のデータが返されます。
<!-- You can subscribe to the `sn` event on the ZMQ of a node to listen for recently confirmed transactions. The following data is returned from the `sn` event: -->

* トランザクションを参照した最初のマイルストーンのインデックス
<!-- * The index of the first milestone that referenced the transaction -->
* トランザクションハッシュ
<!-- * The transaction hash -->

1\. zeromqライブラリをインストールします。
<!-- 1\. Install the zeromq library -->

--------------------
### Node.js

```bash
npm install zeromq --save
```
---
### Python

```bash
pip install pyzmq
```
--------------------

2\. ライブラリをインポートしてZMQサブスクライブソケットを作成します。
<!-- 2\. Import the libraries and create a ZMQ subscribe socket -->

--------------------
### Node.js

```js
const zmq = require('zeromq');
const sock = zmq.socket('sub');
```
---
### Python

```python
import zmq

context = zmq.Context()
socket = context.socket(zmq.SUB)
```
--------------------

3\. ソケットをノードのアドレスに接続します。別のノードに接続したい場合は、`tcp://zmq.devnet.iota.org:5556`を別のノードのURLに置き換えてください。
<!-- 3\. Connect the socket to the node's address. If you want to connect do a different node, replace the `tcp://zmq.devnet.iota.org:5556` URL with the URL of your node. -->

--------------------
### Node.js

```js
sock.connect('tcp://zmq.devnet.iota.org:5556');
```
---
### Python

```python
socket.connect('tcp://zmq.devnet.iota.org:5556')
```
--------------------

4\. `sn`イベントを購読します。このイベントは確定済みトランザクション用です。
<!-- 4\. Subscribe to the `sn` event. This event is for confirmed transactions. -->

--------------------
### Node.js

```js
sock.subscribe('sn');
console.log("Socket connected");
```
---
### Python
```python
socket.subscribe('sn')
print ("Socket connected")
```
--------------------

5\. ノードが返すイベントデータを処理します。
<!-- 5\. Process the event data that the node returns -->

--------------------
### Node.js

```js
sock.on('message', msg => {
    //Split the data into an array
    const data = msg.toString().split(' ');
    console.log(`Transaction confirmed by milestone index: ${data[1]}` );
    console.log(`Transaction hash: ${data[2]}` );
});
```
---
### Python
```python
while True:
    print ("Waiting for events from the node")
    message = socket.recv()
    data = message.split()
    print ("Transaction confirmed by milestone index: ", data[1])
    print ("Transaction hash: ", data[2])
```
--------------------

出力は次のようになります。
<!-- The output should display something like the following: -->
```shell
Transaction confirmed by milestone index: 964091
Transaction hash: QUU9NXGQBKF9XVIVOGAPEMELTEKANNJPUFCEEFWHQKRASFGDUQNSFMRXULPDSLXUZU9NVQQEBAQLVG999
Transaction confirmed by milestone index: 964091
Transaction hash: DXFNIOMKEOETZXSMGEDUIY9JFWCFQTGSVJHIUWMQWKCUMCTYZRWAMVURZYJPYGUBZPUELKVZSALNNU999
Transaction confirmed by milestone index: 964091
Transaction hash: OHRNZFLVXJVHBT9HNOQWIOQHICJ9NVTLKAPYLBUVVGIRTYGUSZKWINSUTSJJGPBBFLNCGUFTVYFNNF999
Transaction confirmed by milestone index: 964091
Transaction hash: QNCPDSSMPISSVXBENGGNNBTRBSLCBXTVBLTZLH9DFNXUWWPQNAIFJPAQENDUYL9XTWOMNURAGRFNWN999
```

## コードを実行する
<!-- ## Run the code -->

このガイドのサンプルコードを実行してWebブラウザに結果を表示するには、緑色のボタンをクリックします。
<!-- Click the green button to run the sample code in this guide and see the results in the web browser. -->

:::info:
ZMQがノードからデータを受信するのに数分かかることがあります。

緑色のテキストは無視してかまいません。
:::
<!-- :::info: -->
<!-- It may take a minute or so for the ZMQ to receive data from the node. -->
<!--  -->
<!-- You can ignore any green text. -->
<!-- ::: -->

### Node.js

<iframe height="600px" width="100%" src="https://repl.it/@jake91/ZMQ-example-Nodejs?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

### Python

<iframe height="600px" width="100%" src="https://repl.it/@jake91/ZMQ-example-Python?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## 次のステップ
<!-- ## Next steps -->

ZMQに関する知識を活用して、他の[イベント](../references/zmq-events.md)についてIRIを監視するアプリケーションを構築してみてください。
<!-- Use your knowledge of the ZMQ to build an application that monitors the IRI for other [events](../references/zmq-events.md). -->

:::info:
トランザクションがなかなか確定しないとき、トランザクションが確定する可能性を高めるために[トランザクションを促進するか再添付する](root://dev-essentials/0.1/how-to-guides/confirm-pending-bundle.md)ことができます。
:::
<!-- :::info: -->
<!-- If your transactions aren't being confirmed, you can [promote or reattach them](root://dev-essentials/0.1/how-to-guides/confirm-pending-bundle.md) to increase the likelihood of this happening. -->
<!-- ::: -->
