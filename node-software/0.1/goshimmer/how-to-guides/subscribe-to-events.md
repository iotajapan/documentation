# ノードの着信トランザクションを監視する
<!-- # Monitor your node for incoming transactions -->

**ノードがトランザクションを受信すると、ノードは受信したトランザクションをトランザクションイベントとしてゼロメッセージキュー（ZMQ）と呼ばれるサービスに発行します。クライアントとして、このイベントを購読して、ノードの着信トランザクションを監視することができます。**
<!-- **When your node receives transactions, it publishes them as a transaction event to a service called zero message queue (ZMQ). As a client, you can subscribe to this event to monitor your node for incoming transactions.** -->

次のようにして、トランザクションデータをノードから受信することができます。
<!-- You can receive this transaction data from a node by doing the following: -->

1. オープンソースのZMQライブラリをインストールします。
<!-- 1. Install an open-source ZMQ library -->

2. ZMQソケットを作成してノードに接続します。
<!-- 2. Create a ZMQ socket and connect it to a node -->

3. ZMQからのイベントを購読します。
<!-- 3. Subscribe to events from the ZMQ -->

以下のハウツーガイドではNode.jsとPythonを使用していますが、任意の[ZMQライブラリがサポートしているプログラミング言語](http://zguide.zeromq.org/page:all)を使用できます。
<!-- In the following how-to guide we use Node.js and Python, but you could use any [programming language that the ZMQ library supports](http://zguide.zeromq.org/page:all). -->

## 前提条件
<!-- ## Prerequisites -->

このガイドのサンプルコードを使用するには、次のものが必要です。
<!-- To use the sample code in this guide, you must have the following: -->

* [Node.js 8以上](https://nodejs.org/en/)か[Python（3以上）](https://www.python.org/downloads/)と[PIP](https://pip.pypa.io/en/stable/installing/)
<!-- * [Node.js (8+)](https://nodejs.org/en/) or [Python (3+)](https://www.python.org/downloads/) and [PIP](https://pip.pypa.io/en/stable/installing/) -->
* [Visual Studio Code](https://code.visualstudio.com/Download)などのコードエディタ
<!-- * A code editor such as [Visual Studio Code](https://code.visualstudio.com/Download) -->
* コマンドプロンプトへのアクセス
<!-- * Access to a command prompt -->
* インターネット接続
<!-- * An Internet connection -->

## トランザクションイベントを購読する
<!-- ## Subscribe to the transaction event -->

ノードが最近受信したトランザクションを監視するために`tx`イベントを購読することができます。
<!-- You can subscribe to the `tx` event to monitor a node for recently received transactions. -->

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

3\. ソケットをノードのアドレスに接続します。
  <!-- 3\. Connect the socket to your node's address -->

--------------------
### Node.js

```js
sock.connect('tcp://localhost:5556');
```
---
### Python

```python
socket.connect('tcp://localhost:5556')
```
--------------------

4\. `tx`イベントを購読します。このイベントは受信トランザクション用です。
  <!-- 4\. Subscribe to the `tx` event. This event is for received transactions. -->

--------------------
### Node.js

```js
sock.subscribe('tx');
console.log("Socket connected");
```
---
### Python
```python
socket.subscribe('tx')
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
    for(var i = 0; i < data.length; i++){
        console.log(data[i]);
    }
});
```
---
### Python
```python
while True:
    print ("Waiting for events from the node")
    message = socket.recv()
    data = message.split()
    print (data)
```
--------------------

出力は次のようになります。
<!-- The output should display something like the following: -->
```shell
tx
YMTMRYBLFPTYCLHAWJVDEZNXITKOW9YMOICXPZVHNHMVLPWLDFYLVAO9XFWICBJCUZAHVQPHINBDXD9NE
999999999999999999999999999999999999999999999999999999999999999999999999999999999
125204
999999999999999999999999999
0
0
0
YMTMRYBLFPTYCLHAWJVDEZNXITKOW9YMOICXPZVHNHMVLPWLDFYLVAO9XFWICBJCUZAHVQPHINBDXD9NE
BLFZJUOBAPWCXTGOCSBVJSXIYOGHN9SUGQEMSUOCUPRK9FXDOONJIOCCSKTBZC9LLBLVSC9BOXEDRE9HY
FXTZC9KKRBWSBYKCOGUDZOZUWHTQWDNMZPZ9SCVYTWVBQNYIXHREHCTP9DEJCR9LHUEHMBIXXGSDQJUUW
1562247720
999999999999999999999999999
```

このデータをランダムな例とすると、以下に対応します。
<!-- If we take this data as a random example, it corresponds to the following: -->

| **データ** | **説明** |
| :--------- | :------- |
| YMTMRYBLFPTYCLHAWJVDE... | トランザクションハッシュ |
| 999999999999999999999... | アドレス |
| 125204 | トークン量 |
| 999999999999999999999... | 痕跡タグ |
| 0 | タイムスタンプ |
| 0 | バンドル内のこのトランザクションのインデックス |
| 0 | バンドル内の最後のトランザクションのインデックス |
| YMTMRYBLFPTYCLHAWJVDE... | バンドルハッシュ |
| BLFZJUOBAPWCXTGOCSBVJ... | トランクトランザクションハッシュ |
| FXTZC9KKRBWSBYKCOGUDZ... | ブランチトランザクションハッシュ |
| 1562247720 | ノードがトランザクションを受信したときのUnixタイムスタンプ |
| 99999999999999999999... | 空のタグ |

<!-- | **Data**| **Description**| -->
<!-- |:--------|:---------------| -->
<!-- |tx|Name of the ZMQ event| -->
<!-- |YMTMRYBLFPTYCLHAWJVDE...|Transaction hash| -->
<!-- |999999999999999999999...|Address| -->
<!-- |125204|Value| -->
<!-- |999999999999999999999...|Obsolete tag| -->
<!-- |0|Timestamp| -->
<!-- |0|Current index of this transaction in the bundle| -->
<!-- |0|Index of the head transaction in the bundle| -->
<!-- |YMTMRYBLFPTYCLHAWJVDE...|Bundle hash| -->
<!-- |BLFZJUOBAPWCXTGOCSBVJ...|Trunk transaction hash| -->
<!-- |FXTZC9KKRBWSBYKCOGUDZ...|Branch transaction hash| -->
<!-- |1562247720|Unix timestamp of when the node received the transaction| -->
<!-- |99999999999999999999...|Empty tag| -->

:::info:
`トークン量`フィールドは、トランザクションハッシュが各トランザクションに対して一意であるようにゼロではない値を有し、IOTAトークンは転送されません。現時点では、ZMQエンドポイントは`tx`イベントのみをサポートしています。
:::
<!-- :::info: -->
<!-- The `value` field has a non-zero value so that the transaction hash is unique for each transaction. No IOTA tokens are transferred. -->
<!-- At the moment, the ZMQ endpoint only supports the `tx` event. -->
<!-- ::: -->
