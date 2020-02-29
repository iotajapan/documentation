# アプリケーションアーキテクチャ
<!-- # Application architecture -->

**P2P エネルギーグリッドアプリケーションは，エネルギー源，生産者，消費者，グリッドから IOTA ネットワークにデータを公開します．**
<!-- **The P2P energy grid application publishes data to an IOTA network from sources, producers, consumers, and the grid.** -->

:::warning:免責事項
人間の努力のように，オープンソースプロジェクトを実行することは，不確実性とトレードオフを伴います．以下に説明するアーキテクチャが，同様のシステムを展開するのに役立つことを願っていますが，間違いが含まれている可能性があり，すべての状況に対処することはできません．あなたのプロジェクトについて何か質問があれば，IOTA 財団はあなたがあなた自身の研究をし，専門家を探し，そして IOTA コミュニティとそれらを話し合うことを奨励します．
:::
<!-- :::warning:Disclaimer -->
<!-- Running an open source project, like any human endeavor, involves uncertainty and trade-offs. We hope the architecture described below helps you to deploy similar systems, but it may include mistakes, and can’t address every situation. If you have any questions about your project, we encourage you to do your own research, seek out experts, and discuss them with the IOTA community. -->
<!-- ::: -->

この設計図では，エネルギー源，生産者，消費者，およびグリッドが IOTA ネットワークを介して相互接続される次のアーキテクチャを使用します．
<!-- This blueprint uses the following architecture whereby the source, producer, consumer, and grid are interconnected through an IOTA network. -->

![P2P Energy PoC - Sequence Diagram](../images/p2p_sequence.png)

## 基本的要素
<!-- ## Building blocks -->

すべてのエンティティは，MAM チャネルと Web API 呼び出しの組み合わせを使用して通信します．
<!-- All entities communicate, using a combination of MAM channels and web API calls. -->

![P2P energy PoC - communication](../images/p2p_communication.png)

### エネルギー源の MAM チャネル
<!-- ### Source MAM channel -->

エネルギー源は MAM チャネルを作成し，次のペイロードを使用して，一定期間にどれだけのエネルギーを貢献したかを生産者に伝えます．
<!-- Sources creates a MAM channel to tell the producer how much energy they have contributed over a period of time, using the following payload: -->

```json
{
   "command": "output",        /* The MAM command */
   "startTime": 1542267468229, /* Unix Epoch in milliseconds */
   "endTime": 1542267469229,   /* Unix Epoch in milliseconds */
   "output": 1.234             /* kWh */
}
```

生産者が正しいチャネルにサブスクライブしていることを検証できるようにするために，エネルギー源は新しいチャネルに `hello` コマンドを発行します．
<!-- To allow the producer to validate that it's subscribed to the correct channel, the source publishes a `hello` command to any new channel. -->

```json
{
   "command": "hello"
}
```

チャネルが使用されなくなったことを知らせるために，ソースは `goodbye` コマンドを発行します．
<!-- To signal that a channel is no longer in use, the source publishes a `goodbye` command to it. -->

```json
{
   "command": "goodbye"
}
```

MAM チャネルを作成した後，エネルギー源は[生産者の登録 API](#producer-registration-api) を使用して生産者に登録します．
<!-- After creating a MAM channel, sources register with producers, using the [producer's registration API](#producer-registration-api). -->

![P2P energy PoC - source workflow](../images/p2p_source.png)

### 消費者の MAM チャネル
<!-- ### Consumer MAM channel -->

消費者は，MAM チャネルを作成して，次のペイロードを使用して，グリッドを最新の状態に保ちます．
<!-- Consumers create a MAM channel to keep the grid up to date with how much they owe, using the following payload: -->

```json
{
   "startTime": 1542267468229,         /* ms since 1900 */
   "endTime": 1542267469229,           /* ms since 1900 */
   "usage": 1.234                      /* kWh */
}
```

MAM チャネルを作成した後，消費者は[グリッドの登録 API](#grid-registration-api) を使用してグリッドに登録します．]
<!-- After creating a MAM channel, consumers register with the grid, using the [grid's registration API](#grid-registration-api). -->

![P2P energy PoC - consumer workflow](../images/p2p_consumer.png)

### 生産者の MAM チャネル
<!-- ### Producer MAM channel -->

生産者は MAM チャネルを作成して，グリッドに次のことを伝えます．
<!-- The producer creates a MAM channel to tell the grid the following: -->

- 一定期間にすべてのエネルギー源が貢献したエネルギー量
<!-- How much energy all of its sources have contributed over a period of time -->
- エネルギーの提案価格
<!-- A proposed price for the energy -->
- IOTA トークンで支払いを送信するアドレス
<!-- An address to which to send the payment in IOTA tokens -->

```json
{
   "command": "output",                    /* The MAM command */
   "startTime": 1542267468229,             /* Unix Epoch in milliseconds */
   "endTime": 1542267469229,               /* Unix Epoch in milliseconds */
   "output": 1.234,                        /* kWh */
   "producerPrice": 56789,                 /* IOTA tokens per kWh */
   "paymentIdOrAddress": "PPPPPP...QQQQQQ" /* Method of receiving payment */
}
```

`producerPrice` フィールドはグリッドによって満たされることを保証されていません．代わりに，グリッドはすべての生産者の価格を監視および記録することで自身の価格を計算できます．たとえば，グリッドはすべての `producerPrice` フィールドの値の平均，または生産者がグリッドに貢献した量に基づいた加重平均で支払うことができます．
<!-- The `producerPrice` field is not guaranteed to be met by the grid, instead the grid may calculate its own price by monitoring and recording the price for all producers. For example, the grid may pay an average of the values of all `producerPrice` fields or a weighted average based on how much the producer contributed to the grid. -->

`paymentIdOrAddress` は，グリッドが支払いを行う別の方法を持つように，IOTA アドレスまたは参照 ID にすることができます．
<!-- The `paymentIdOrAddress` can be an IOTA address or a reference ID so that the grid has another way of making payments. -->

MAM チャネルを作成した後，生産者は[グリッドの登録 API](#grid-registration-api) を使用してグリッドに登録します．このようにして，グリッドは生産者が生成するエネルギー量をモニタリングできます．
<!-- After creating a MAM channel, producers register with the grid, using the [grid's registration API](#grid-registration-api). This way, the grid can monitor the amount of energy that the producers generate. -->

![P2P energy PoC - producer workflow](../images/p2p_producer.png)

<a name="producer-registration-api"></a>
### 生産者の登録 API
<!-- ### Producer registration API -->

生産者は，エネルギー源が API を使用して生産者の MAM チャネルに登録できるようにして，生産者がエネルギー源をモニタリングできるようにします．
<!-- Producers allow sources to register their MAM channels, using an API, so that producers can monitor them. -->

#### 登録
<!-- #### Register -->

新しいエネルギー源を登録します．
<!-- Registers a new source. -->

```bash
PUT https://producer/registration/:registrationId
```

##### パスパラメーター
<!-- ##### Path parameters -->

|**パラメーター**|**必須か任意か**|**説明**|**型**|
|----------------|----------------|--------|------|
| `registrationId` | 必須|エネルギー源を識別する文字列．自分自身のランダム ID を生成し，将来の API 呼び出しのために保持する必要があります．| string|

<!-- |**Parameter** | **Required or Optional**|**Description** | **Type**| -->
<!-- |--|--|--|--| -->
<!-- | `registrationId` | Required|A string of characters that identifies a source. You must generate your own random ID and keep it for future API calls. | string| -->

##### ボディパラメーター
<!-- ##### Body parameters -->

|**パラメーター**|**必須か任意か**|**説明**|**型**|
|----------------|----------------|--------|------|
| `itemName` | 必須|エネルギー源の名前．任意の名前を入力できます．| string|
| `itemType` | 必須|エネルギー源が提供するエネルギーのタイプ．任意のタイプを入力できます．| string|
| `root` | 任意|MAM チャネルのルート．| string|
| `sideKey` | 任意|`root`パラメーターで指定された MAM チャネルのサイドキー．MAM チャネルが制限モードにある場合にのみ，サイドキーが必要です．| string|

<!-- |**Parameter** | **Required or Optional**|**Description** | **Type**| -->
<!-- |--|--|--|--| -->
<!-- | `itemName` | Required|The name of your source. You can enter any name. | string| -->
<!-- | `itemType` | Required|The type of energy that the source provides. You can enter any type. | string| -->
<!-- | `root` | Optional|The root of the MAM channel | string| -->
<!-- | `sideKey` | Optional|The side key for the MAM channel that's given in the `root` parameter. You need a side key only if the MAM channel is in restricted mode. | string| -->

##### 例
<!-- ##### Examples -->
--------------------
### Python
```python
import urllib2
import json

command = {
   "itemName": "My Solar Source",
   "itemType": "solar",
   "root": "CCCCCC...DDDDDD",
   "sideKey": "AAA....ZZZZZZ"
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json'
}

request = urllib2.Request(url="https://producer/registration/:registrationId", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
   "itemName": "My Solar Source",
   "itemType": "solar",
   "root": "CCCCCC...DDDDDD",
   "sideKey": "AAA....ZZZZZZ"
}

var options = {
  url: 'https://producer/registration/:registrationId',
  method: 'PUT',
  headers: {
    'Content-Type': 'application/json'
  },
  json: command
};

request(options, function (error, response, data) {
  if (!error && response.statusCode == 200) {
    console.log(data);
  }
});
```
---
### cURL
```bash
curl https://producer/registration/:registrationId \
-X PUT \
-H 'Content-Type: application/json' \
-d '{
   "itemName": "My Solar Source",
   "itemType": "solar",
   "root": "CCCCCC...DDDDDD",
   "sideKey": "AAA....ZZZZZZ"
}'
```
--------------------

##### レスポンス例
<!-- ##### Response examples -->
--------------------
### 200
```json
{
   "success": true,
   "message": "OK"
}
```
--------------------

##### 結果
<!-- ##### Results -->

|**リターンフィールド**|**説明**|
|----------------------|--------|
| `success` |リクエストが成功したかどうか|
| `message` |サーバーからのメッセージ|

<!-- |**Return field** | **Description** | -->
<!-- |--|--| -->
<!-- | `success` | Whether the request was successful | -->
<!-- | `message` | A message from the server| -->

#### 登録解除
<!-- #### Deregister -->

既存のエネルギー源を登録解除します．
<!-- Deregisters an existing source. -->

エネルギー源が登録解除されると，生産者はその MAM チャネルのモニタリングを停止します．
<!-- When a source is dereistered, the producer stops monitoring its MAM channel. -->

```bash
DELETE https://producer/registration/:registrationId/:sideKey
```

##### パスパラメーター
<!-- ##### Path parameters -->

|**パラメーター**|**必須か任意か**|**説明**|**型**|
|----------------|----------------|--------|------|
| `registrationId` | 必須|エネルギー源の登録に使用された登録 ID| string|
| `sideKey` | 任意|エネルギー源がサイドキーと一緒に登録された場合，このパラメーターを渡す必要があります| string|

<!-- |**Parameter** | **Required or Optional**|**Description** | **Type**| -->
<!-- |--|--|--|--| -->
<!-- | `registrationId` | Required|The registration ID that was used to register the source | string| -->
<!-- | `sideKey` | Optional|If the source was registered with a side key, you must pass this parameter | string| -->

##### 例
<!-- ##### Examples -->
--------------------
### Python
```python
import urllib2
import json

headers = {
    'content-type': 'application/json'
}

request = urllib2.Request(url="https://producer/registration/:registrationId/:sideKey", headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var options = {
  url: 'https://producer/registration/:registrationId/:sideKey',
  method: 'DELETE',
  headers: {
    'Content-Type': 'application/json'
  }
};

request(options, function (error, response, data) {
  if (!error && response.statusCode == 200) {
    console.log(data);
  }
});
```
---
### cURL
```bash
curl https://producer/registration/:registrationId/:sideKey \
-X DELETE \
-H 'Content-Type: application/json'
```
--------------------

##### レスポンス例
<!-- ##### Response examples -->
--------------------
### 200
```json
{
   "success": true,
   "message": "OK"
}
```
--------------------

##### 結果
<!-- ##### Results -->

|**リターンフィールド**|**説明**|
|----------------------|--------|
| `success` |リクエストが成功したかどうか|
| `message` |サーバーからのメッセージ|

<!-- |**Return field** | **Description** | -->
<!-- |--|--| -->
<!-- | `success` | Whether the request was successful | -->
<!-- | `message` | A message from the server| -->

### グリッド MAM チャネル
<!-- ### Grid MAM channel -->

消費者がグリッドに登録すると，グリッドは MAM チャネルを作成して，以下のペイロードを使用して，消費者が一定期間に借りている金額を通知します．
<!-- When consumers register with the grid, it creates a MAM channel to tell them how much they owe over a period of time, using the following payload: -->

```json
{
   "command": "payment-request",           /* the command */
   "owed": 345,                            /* amount in IOTA owed
                                              excludes pending transactions */
   "usage": 123,                           /* the amount of kWh the payment is for */
   "paymentIdOrAddress": "WWWWWW...XXXXXX" /* payment address for owed
                                              balance payment to grid */
}
```

`paymentIdOrAddress` は IOTA アドレスまたは参照 ID にすることができるため，消費者は別の支払い方法を利用できます．
<!-- The `paymentIdOrAddress` can be an IOTA address or a reference ID so that the consumer has another way of making payments. -->

グリッドが自身の IOTA トークンの取り分を取得した後，残りはエネルギーの生産者に支払われます．
<!-- After the grid has taken its cut of the IOTA tokens, the rest is paid to the producers for their energy. -->

生産者と消費者が正しいチャネルにサブスクライブしていることを検証できるように，グリッドは新しいチャネルに `hello` コマンドを発行します．
<!-- To allow the producers and consumers to validate that they're subscribed to the correct channel, the grid publishes a `hello` command to any new channel. -->

```json
{
   "command": "hello"
}
```

チャネルが使用されなくなったことを知らせるために，エネルギー源は `goodbye` コマンドを発行します．
<!-- To signal that a channel is no longer in use, the source publishes a `goodbye` command to it. -->

```json
{
   "command": "goodbye"
}
```

<a name="grid-registration-api"></a>
### グリッドの登録 API
<!-- ### Grid registration API -->

グリッドを使用すると，API を使用して，生産者と消費者が自分で登録できます．
<!-- Grids allow producers and consumers to register themselves, using an API. -->

#### 登録
<!-- #### Register -->

新しい生産者や消費者を登録します．
<!-- Registers a new producer or consumer. -->

```bash
PUT https://producer/registration/:registrationId
```

##### パスパラメーター
<!-- ##### Path parameters -->

|**パラメーター**|**必須か任意か**|**説明**|**型**|
|----------------|----------------|--------|------|
| `registrationId` | 必須|生産者または消費者を識別する文字列．自分自身のランダム ID を生成し，将来の API 呼び出しのために保持する必要があります．| string|

<!-- |**Parameter** | **Required or Optional**|**Description** | **Type**| -->
<!-- |--|--|--|--| -->
<!-- | `registrationId` | Required|A string of characters that identifies a producer or consumer. You must generate your own random ID and keep it for future API calls. | string| -->

##### ボディパラメーター
<!-- ##### Body parameters -->

|**パラメーター**|**必須か任意か**|**説明**|**型**|
|----------------|----------------|--------|------|
| `itemName` | 必須|生産者または消費者の名前．任意の名前を入力できます．| string|
| `itemType` | 必須|`producer` または `consumer`| string|
| `root` | 任意|登録する MAM チャネルのルート| string|
| `sideKey` | 任意|`root` パラメーターで指定された MAM チャネルのサイドキー． MAM チャネルが制限モードにある場合にのみ，サイドキーが必要です．| string|

<!-- |**Parameter** | **Required or Optional**|**Description** | **Type**| -->
<!-- |--|--|--|--| -->
<!-- | `itemName` | Required|The name of your producer or consumer. You can enter any name. | string| -->
<!-- | `itemType` | Required|`producer` or `consumer`| string| -->
<!-- | `root` | Optional|The root of the MAM channel to register | string| -->
<!-- | `sideKey` | Optional|The side key for the MAM channel that's given in the `root` parameter. You need a side key only if the MAM channel is in restricted mode. | string| -->

##### 例
<!-- ##### Examples -->
--------------------
### Python
```python
import urllib2
import json

command = {
   {
   "itemName": "Jake",
   "itemType": "consumer",
   "root": "CCCCCC...DDDDDD",
   "sideKey": "AAA....ZZZZZZ"
}
```

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json'
}

request = urllib2.Request(url="https://grid/registration/:registrationId", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
   "itemName": "Jake",
   "itemType": "consumer",
   "root": "CCCCCC...DDDDDD",
   "sideKey": "AAA....ZZZZZZ"
}

var options = {
  url: 'https://grid/registration/:registrationId',
  method: 'PUT',
  headers: {
    'Content-Type': 'application/json'
  },
  json: command
};

request(options, function (error, response, data) {
  if (!error && response.statusCode == 200) {
    console.log(data);
  }
});
```
---
### cURL
```bash
curl https://grid/registration/:registrationId \
-X PUT \
-H 'Content-Type: application/json' \
-d '{
   "itemName": "Jake",
   "itemType": "consumer",
   "root": "CCCCCC...DDDDDD",
   "sideKey": "AAA....ZZZZZZ"
}'
```
--------------------

##### レスポンス例
<!-- ##### Response examples -->
--------------------
### 200
```json
{
   {
   "success": true,
   "message": "OK",
   "root": "JJJJJJ...KKKKKK",
   "sideKey": "CCC...DDDD"
}
}
```
--------------------

##### 結果
<!-- ##### Results -->

|**リターンフィールド**|**説明**|
|----------------------|--------|
| `success` |リクエストが成功したかどうか|
| `message` |サーバーからのメッセージ|
|`root`|消費者が支払い情報をモニタリングする必要がある MAM チャネルのルート|
|`sideKey`|MAM チャネルが制限モードの場合，これはメッセージを復号化するためのサイドキーです|

<!-- |**Return field** | **Description** | -->
<!-- |--|--| -->
<!-- | `success` | Whether the request was successful | -->
<!-- | `message` | A message from the server| -->
<!-- |`root`| The root of the MAM channel that the consumer should monitor for payment information| -->
<!-- |`sideKey`| If the MAM channel is in restricted mode, this is the side key to decrypt the message| -->

#### 登録解除
<!-- #### Deregister -->

既存の MAM チャネルを登録解除します．
<!-- Deregisters an existing MAM channel. -->

MAM チャネルの登録が解除されると，グリッドはモニタリングを停止します．
<!-- When a MAM channel is deregistered, the grid stops monitoring it. -->

```bash
DELETE https://grid/registration/:registrationId/:sideKey
```

##### パスパラメーター
<!-- ##### Path parameters -->

|**パラメーター**|**必須か任意か**|**説明**|**型**|
|----------------|----------------|--------|------|
| `registrationId` | 必須|エネルギー源の登録に使用された登録 ID| string|
| `sideKey` | 任意|エネルギー源がサイドキーで登録された場合，このパラメーターを渡す必要があります| string|

<!-- |**Parameter** | **Required or Optional**|**Description** | **Type**| -->
<!-- |--|--|--|--| -->
<!-- | `registrationId` | Required|The registration ID that was used to register the source | string| -->
<!-- | `sideKey` | Optional|If the source was registered with a side key, you must pass this parameter | string| -->

##### 例
<!-- ##### Examples -->
--------------------
### Python
```python
import urllib2
import json

headers = {
    'content-type': 'application/json'
}

request = urllib2.Request(url="https://grid/registration/:registrationId/:sideKey", headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var options = {
  url: 'https://grid/registration/:registrationId/:sideKey',
  method: 'DELETE',
  headers: {
    'Content-Type': 'application/json'
  }
};

request(options, function (error, response, data) {
  if (!error && response.statusCode == 200) {
    console.log(data);
  }
});
```
---
### cURL
```bash
curl https://grid/registration/:registrationId/:sideKey \
-X DELETE \
-H 'Content-Type: application/json'
```
--------------------

##### レスポンス例
<!-- ##### Response examples -->
--------------------
### 200
```json
{
   "success": true,
   "message": "OK"
}
```
--------------------

##### 結果
<!-- ##### Results -->

|**リターンフィールド**|**説明**|
|----------------------|--------|
| `success` |リクエストが成功したかどうか|
| `message` |サーバーからのメッセージ|

<!-- |**Return field** | **Description** | -->
<!-- |--|--| -->
<!-- | `success` | Whether the request was successful | -->
<!-- | `message` | A message from the server| -->

<a name="grid-storage-api"></a>
### グリッドストレージ API
<!-- ### Grid storage API -->

グリッドストレージ API は，情報を永続的に保存する必要があるアーキテクチャの任意の部分で使用できます．
<!-- The grid storage API is available to any part of the architecture that needs to store information on a permanent basis. -->

#### 登録
<!-- #### Register -->

データベースにアイテムを保存します．
<!-- Saves an item to the database. -->

```bash
PUT https://grid/storage/item/:registration-id/:context/:id
```

##### パスパラメーター
<!-- ##### Path parameters -->

|**パラメーター**|**必須か任意か**|**説明**|**型**|
|----------------|----------------|--------|------|
| `registration-id` | 必須|生産者または消費者の登録に使用された登録 ID| string|
| `context` | 必須|コンテンツをグループ化するために使用される名前．このパラメーターは，データのディレクトリと考えることができます．| string|
|`id`| 必須|データを識別する名前．たとえば，`file.txt`| string|

<!-- |**Parameter** | **Required or Optional**|**Description** | **Type**| -->
<!-- |--|--|--|--| -->
<!-- | `registration-id` | Required|The registration ID that was used to register the producer or consumer | string| -->
<!-- | `context` | Required|A name that's used to group your content. You can think of this parameter as a directory for your data. -->
<!--  | string| -->
<!-- |`id`| Required|A name that identifies your data. For example `file.txt`| string| -->

##### ボディパラメーター
<!-- ##### Body parameters -->

|**パラメーター**|**必須か任意か**|**説明**|**型**|
|----------------|----------------|--------|------|
| `{}` | 必須|JSON オブジェクト| JSON|

<!-- |**Parameter** | **Required or Optional**|**Description** | **Type**| -->
<!-- |--|--|--|--| -->
<!-- | `{}` | Required|A JSON object | JSON| -->

##### 例
<!-- ##### Examples -->
--------------------
### Python
```python
import urllib2
import json

headers = {
    'content-type': 'application/json'
    }

command = {
   "myCustomData": "Some data"
}

stringified = json.dumps(command)

request = urllib2.Request(url="https://grid/storage/item/:registration-id/:context/:id", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
   "myCustomData": "Some data"
}

var options = {
  url: 'https://grid/storage/item/:registration-id/:context/:id',
  method: 'PUT',
  headers: {
    'Content-Type': 'application/json'
  },
  json: command
};

request(options, function (error, response, data) {
  if (!error && response.statusCode == 200) {
    console.log(data);
  }
});
```
---
### cURL
```bash
curl https://grid/storage/item/:registration-id/:context/:id \
-X PUT \
-H 'Content-Type: application/json' \
-d '{
   "myCustomData": "Some data"
}'
```
--------------------

##### レスポンス例
<!-- ##### Response examples -->
--------------------
### 200
```json
{
   "success": true,
   "message": "OK"
}
```
--------------------

##### 結果
<!-- ##### Results -->

|**Return field** | **Description** |
|--|--|
| `success` | Whether the request was successful |
| `message` | A message from the server|

#### アイテムを取得する
<!-- #### Get item -->

データベースから既存のデータを返します．
<!-- Returns existing data from the database. -->

```bash
GET https://grid/storage/:registration-id/:context/:id
```

##### パスパラメーター
<!-- ##### Path parameters -->

|**パラメーター**|**必須か任意か**|**説明**|**型**|
|----------------|----------------|--------|------|
| `registration-id` | 必須|データの登録に使用された登録 ID| string|
| `context` | 必須|データが含まれるコンテキストの名前| string|
|`id`| 必須|読み取りたいデータの名前| string|

<!-- |**Parameter** | **Required or Optional**|**Description** | **Type**| -->
<!-- |--|--|--|--| -->
<!-- | `registration-id` | Required|The registration ID that was used to register the data | string| -->
<!-- | `context` | Required|The name of the context that the data is in | string| -->
<!-- |`id`| Required|The name of the data that you want to read| string| -->

##### 例
<!-- ##### Examples -->
--------------------
### Python
```python
import urllib2
import json

headers = {
    'content-type': 'application/json'
}

request = urllib2.Request(url="https://grid/registration/:registration-id/:context/:id", headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var options = {
  url: 'https://grid/registration/:registration-id/:context/:id',
  method: 'GET',
  headers: {
    'Content-Type': 'application/json'
  }
};

request(options, function (error, response, data) {
  if (!error && response.statusCode == 200) {
    console.log(data);
  }
});
```
---
### cURL
```bash
curl https://grid/registration/:registration-id/:context/:id \
-X GET \
-H 'Content-Type: application/json'
```
--------------------

##### レスポンス例
<!-- ##### Response examples -->
--------------------
### 200
```json
{
   "success": true,
   "message": "OK",
   "item": {
      "myCustomData": "Some data"
   }
}
```
--------------------

##### 結果
<!-- ##### Results -->

|**リターンフィールド**|**説明**|
|----------------------|--------|
| `success` |リクエストが成功したかどうか|
| `message` |サーバーからのメッセージ|
|`item`|見つかった JSON データ|

<!-- |**Return field** | **Description** | -->
<!-- |--|--| -->
<!-- | `success` | Whether the request was successful | -->
<!-- | `message` | A message from the server| -->
<!-- |`item`|The JSON data that was found| -->

#### アイテムを削除する
<!-- #### Delete item -->

既存のアイテムを削除します．
<!-- Deletes existing items. -->

```bash
DELETE https://grid/storage/:registration-id/:context?/:id?
```

##### パスパラメーター
<!-- ##### Path parameters -->

|**パラメーター**|**必須か任意か**|**説明**|**型**|
|----------------|----------------|--------|------|
| `registration-id` | 必須|データの登録に使用された登録 ID．`context` および `id` パラメーターを省略すると，この ID 用に保存されているすべてのデータが削除されます．| string|
| `context` | 任意|削除するコンテキストの名前．`id` パラメータを省略すると，このコンテキストのすべてのデータが削除されます．| string|
|`id`| 任意|削除するデータの名前| string|

<!-- |**Parameter** | **Required or Optional**|**Description** | **Type**| -->
<!-- |--|--|--|--| -->
<!-- | `registration-id` | Required|The registration ID that was used to register the data. If you omit the `context` and `id` parameters, all the data that is saved for this ID will be deleted. | string| -->
<!-- | `context` | Optional|The name of the context that you want to delete. If you omit the `id` parameter, all data in this context will be deleted. | string| -->
<!-- |`id`| Optional|The name of the data that you want to delete| string| -->

##### 例
<!-- ##### Examples -->
--------------------
### Python
```python
import urllib2
import json

headers = {
    'content-type': 'application/json'
  }

request = urllib2.Request(url="https://grid/registration/:registration-id", headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var options = {
  url: 'https://grid/registration/:registration-id',
  method: 'DELETE',
  headers: {
    'Content-Type': 'application/json'
  }
};

request(options, function (error, response, data) {
  if (!error && response.statusCode == 200) {
    console.log(data);
  }
});
```
---
### cURL
```bash
curl https://grid/registration/:registration-id \
-X DELETE \
-H 'Content-Type: application/json'
```
--------------------

##### レスポンス例
<!-- ##### Response examples -->
--------------------
### 200
```json
{
   "success": true,
   "message": "OK"
}
```
--------------------

##### 結果
<!-- ##### Results -->

|**リターンフィールド**|**説明**|
|----------------------|--------|
| `success` |リクエストが成功したかどうか|
| `message` |サーバーからのメッセージ|

<!-- |**Return field** | **Description** | -->
<!-- |--|--| -->
<!-- | `success` | Whether the request was successful | -->
<!-- | `message` | A message from the server| -->

#### 全てのアイテムの読み取り
<!-- #### Read all items -->

ページング方式でストレージからアイテムを取得することができます．`page` と `page-size` はどちらもオプションであり，デフォルトはそれぞれ0と10です．
<!-- Allows you to get items from the storage in a paged manner. Both page and page-size are optional and will default to 0 and 10 respectively. -->

```bash
GET https://grid/storage/:registration-id/:context?page?&page-size?
```

**レスポンス**
<!-- **Response** -->

```json
{
   "success": true,          /* true or false */
   "message": "OK",          /* Or error message if fail */
   "ids": [...],             /* array of the ids */
   "items": [...],           /* array of the items */
   "totalPages": 6,          /* calculated from page-size and num items */
   "totalItems": 45,         /* total number of items available */
   "pageSize": 10            /* The page size used in calculations */
}
```

##### 例
<!-- #### Examples -->

ABC123 の登録 ID を持つ生産者は，バックアップのためにローカルだけではなく安全な場所に，生産者のエネルギー源の詳細を保存したいと思うかもしれません．
<!-- A producer with a registration ID of ABC123 might want to store the details of its sources in a safe place instead of just locally for backup. -->

https://grid/storage/ABC123/sources/XXX123 に PUT を付けてアイテムを作成または更新します．
<!-- It would create or update an item with a PUT to https://grid/storage/ABC123/sources/XXX123. -->

https://grid/storage/ABC123/sources/XXX123 への GET でアイテムを取得できます．
<!-- The item could be retrieved with a GET to https://grid/storage/ABC123/sources/XXX123 -->

https://grid/storage/ABC123/sources/XXX123 への DELETE でアイテムを削除できます．
<!-- The item could be deleted with a DELETE to https://grid/storage/ABC123/sources/XXX123 -->

またはABC123のすべてのアイテムは https://grid/storage/ABC123/ で削除できます．
<!-- Or all the items for ABC123 could be deleted with -->
<!-- https://grid/storage/ABC123/ -->

生産者は，https://grid/storage/ABC123/sources?page=5&pageSize=10 への GET 呼び出しを使用して，ページサイズ10でページ5のすべてのアイテムを取得できます．
<!-- The producer could get all its items on page 5 with a page size of 10 with a GET call to https://grid/storage/ABC123/sources?page=5&pageSize=10 -->
