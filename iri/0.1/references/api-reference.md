# APIリファレンス
<!-- # API reference -->

**この一覧には、HTTP APIを介してノードと通信するためのコマンドが含まれています。**
<!-- **This list contains the commands for communicating with a node through the HTTP API.** -->

以下のすべてのコマンドにHTTPヘッダーを含める必要があります。
<!-- All the following commands must include an HTTP header. -->

| ヘッダー | 値 | 必須か任意か |
|:---------------|:--------|:--------|
| X-IOTA-API-Version | 1 | 必須 |
| Content-Type | application/json | 任意 |
| Authorization | Bearer {token} | 任意 |

**重要：** このAPIはベータ版であり、変更される可能性があります。運用版アプリケーションではこのAPIを使用しないことをお勧めします。
<!-- **Important:** This API is in beta, and is subject to change. We recommend that you don't use this API in production applications. -->

## addNeighbors

ノードに一時的な隣接ノードのリストを追加します。
<!-- Add a list of temporary neighbors to a node. -->

:::info:
ノードが再起動すると、隣接ノードは削除されます。ノードに恒久的に隣接ノードを追加したいのなら、[`NEIGHBORS`](../references/iri-configuration-options.md#neighbors)設定オプションに隣接ノードのURIを追加してください。
:::
<!-- :::info: -->
<!-- The neighbors are removed if the node restarts. If you want to permanently add the neighbors to your own node, add their URIs to the [`NEIGHBORS`](../references/iri-configuration-options.md#neighbors) configuration option. -->
<!-- ::: -->

### パラメータ

隣接ノードを追加するためのURI（unique resource identification）フォーマットは、`"udp：// IPADDRESS：PORT"`です。
<!--  The URI (unique resource identification) format for adding neighbors is `"udp://IPADDRESS:PORT"`. -->

| パラメータ | 必須か任意か | 説明 | 型 |
|--|--|--|--|
| `uris` | 必須 | 追加する隣接ノードのURIの文字列 | 文字列の配列 |

### 例
--------------------
### Python
```python
import urllib2
import json

command = {
  "command": "addNeighbors",
  "uris": [
    "udp://8.8.8.8:14265",
    "udp://8.8.8.8:14265"
  ]
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:14265", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
  "command": "addNeighbors",
  "uris": [
    "udp://8.8.8.8:14265",
    "udp://8.8.8.8:14265"
  ]
}

var options = {
  url: 'http://localhost:14265',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'X-IOTA-API-Version': '1',
    'Content-Length': Buffer.byteLength(JSON.stringify(command))
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
### Curl
```bash
curl http://localhost:14265 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
  "command": "addNeighbors",
  "uris": [
    "udp://8.8.8.8:14265",
    "udp://8.8.8.8:14265"
  ]
}'
```
--------------------

### レスポンス例
--------------------
### 200
```json
{
  "addedNeighbors": 2,
  "duration": 125
}
```
---
### 400
```json
{
  "error": "'command' parameter has not been specified"
}
```
--------------------

## 結果
<!-- ### Results -->

| リターンフィールド | 説明 |
|--|--|
| `addedNeighbors` | 追加した隣接ノードの総数 |
| `duration` | リクエストを完了するのにかかったミリ秒数 |

## attachToTangle

与えられたトランザクショントライトについてノード上で[プルーフオブワーク](root://the-tangle/0.1/concepts/proof-of-work.md)を行います。
<!-- Do [proof of work](root://the-tangle/0.1/concepts/proof-of-work.md) on a node for the given transaction trytes. -->

 ### パラメータ

 `branchTransaction`および`trunkTransaction`パラメーターは、[`getTransactionsToApprove`](#getTransactionsToApprove)エンドポイントから返されます。
<!--  The `branchTransaction` and  `trunkTransaction` parameters are returned from the [`getTransactionsToApprove`](#getTransactionsToApprove) endpoint. -->

| パラメータ | 必須か任意か | 説明 | 型 |
|--|--|--|--|
| `trunkTransaction` | 必須 | [トランクトランザクション](root://iota-basics/0.1/references/structure-of-a-transaction.md)ハッシュ | 文字列型 |
| `branchTransaction` | 必須 | [ブランチトランザクション](root://iota-basics/0.1/references/structure-of-a-transaction.md)ハッシュ | 文字列型 |
| `minWeightMagnitude` | 必須 | [最小重量値](root://iota-basics/0.1/concepts/minimum-weight-magnitude.md) | 整数型 |
| `trytes` | 必須 | トランザクショントライトの文字列 | 文字列の配列 |

### 例
--------------------
### Python
```python
import urllib2
import json

command = {
"command": "attachToTangle",
"trunkTransaction": "VDJJSJVAIQXAUIZOWYLFXVTKFXHNZOGYFRIKBYWD9ZI9NNKYVOLWRJKCXXF9DOXFEGGFWSRVLHVLVADJI",
"branchTransaction": "WXQWVSAJVZLEHQTNFRUBEECZDOJGBRCTUBNDEKDFHKPMTVAQILPTQNG9EEPNEB9PLQZWZAZAKSIJBPG9P",
"minWeightMagnitude": 14,
"trytes": [
  "HOHZUBAFSGNYMOOYGPCKANKOR ...",
  "IOELDJYWAZBKWBTQZYLPTPLIT ..."
  ]
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:14265", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
"command": "attachToTangle",
"trunkTransaction": "VDJJSJVAIQXAUIZOWYLFXVTKFXHNZOGYFRIKBYWD9ZI9NNKYVOLWRJKCXXF9DOXFEGGFWSRVLHVLVADJI",
"branchTransaction": "WXQWVSAJVZLEHQTNFRUBEECZDOJGBRCTUBNDEKDFHKPMTVAQILPTQNG9EEPNEB9PLQZWZAZAKSIJBPG9P",
"minWeightMagnitude": 14,
"trytes": [
  "HOHZUBAFSGNYMOOYGPCKANKOR ...",
  "IOELDJYWAZBKWBTQZYLPTPLIT ..."
  ]
};

var options = {
  url: 'http://localhost:14265',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'X-IOTA-API-Version': '1',
    'Content-Length': Buffer.byteLength(JSON.stringify(command))
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
### Curl
```bash
curl http://localhost:14265 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
"command": "attachToTangle",
"trunkTransaction": "VDJJSJVAIQXAUIZOWYLFXVTKFXHNZOGYFRIKBYWD9ZI9NNKYVOLWRJKCXXF9DOXFEGGFWSRVLHVLVADJI",
"branchTransaction": "WXQWVSAJVZLEHQTNFRUBEECZDOJGBRCTUBNDEKDFHKPMTVAQILPTQNG9EEPNEB9PLQZWZAZAKSIJBPG9P",
"minWeightMagnitude": 14,
"trytes": [
  "HOHZUBAFSGNYMOOYGPCKANKOR ...",
  "IOELDJYWAZBKWBTQZYLPTPLIT ..."
  ]
}'
```
--------------------

### レスポンス例
--------------------
### 200
```json
{
  "trytes": ["OQBOQQOUAWPFCRKELBAS9DHKZ ...", "RGQKNQPXHC9QAVSFDPPFBSKTS ..."],
  "duration": 59
}
```
---
### 400
```json
{
  "error": "'command' parameter has not been specified"
}
```
--------------------

### 結果

戻り値の最後の243トライトは、以下のもので構成されています。
<!-- The last 243 trytes of the return value consist of the following: -->

`trunkTransaction` + `branchTransaction` + `nonce`.

| リターンフィールド | 説明 |
|--|--|
| `trytes` | 有効な`nance`フィールドを含むトランザクショントライト |

## broadcastTransactions

トランザクショントライトをノードへブロードキャストします。
<!-- Broadcast transaction trytes to a node. -->

### パラメータ

このエンドポイントの`trytes`パラメータには、[`attachToTangle`](#attachToTangle)エンドポイントによって行われるプルーフオブワークを含める必要があります。
<!-- The `trytes` parameter for this endpoint must include proof of work, which is done by the [`attachToTangle`](#attachToTangle) endpoint. -->

| パラメータ | 必須か任意か | 説明 | 型 |
|--|--|--|--|
| `trytes` | 必須 | 有効なトランザクショントライト | 文字列の配列 |

### 例
--------------------
### Python
```python
import urllib2
import json

command = {
  "command": "broadcastTransactions",
  "trytes": ["P9KFSJVGSPLXAEBJSHWFZLGP ..."]
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:14265", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
  "command": "broadcastTransactions",
  "trytes": ["P9KFSJVGSPLXAEBJSHWFZLGP ..."]
  }

var options = {
  url: 'http://localhost:14265',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'X-IOTA-API-Version': '1',
    'Content-Length': Buffer.byteLength(JSON.stringify(command))
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
### Curl
```bash
curl http://localhost:14265 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
  "command": "broadcastTransactions",
  "trytes": ["P9KFSJVGSPLXAEBJSHWFZLGP ..."]
  }'
```
--------------------

### レスポンス例
--------------------
### 200
```json
{
  "duration": 567
}
```
---
### 400
```json
{
  "error": "'command' parameter has not been specified"
}
```
--------------------

### 結果

| リターンフィールド | 説明 |
|--|--|
| `duration` | リクエストを完了するのにかかったミリ秒数 |

## checkConsistency

トランザクションの整合性を確認します。整合性のあるトランザクションとは、次のことが当てはまるトランザクションです。
<!-- Check the consistency of transactions. A consistent transaction is one where the following statements are true: -->
* トランザクションの参照トランザクションが欠落していない。
<!-- * The transaction isn't missing a reference transaction -->
* トランザクションのバンドルが有効。
<!-- * The transaction's bundle is valid -->
* トランザクションの参照トランザクションが有効。
<!-- * The transaction's reference transactions are valid -->

### パラメータ

| パラメータ | 必須か任意か | 説明 | 型 |
|--|--|--|--|
| `tails` | 必須 | 確認するトランザクションハッシュ | 文字列の配列 |

### 例
--------------------
### Python
```python
import urllib2
import json

command = {
  "command": "checkConsistency",
  "tails": [
    "SHHXOGUUYSCCFVMCZYCNBJIMGEEIBEPCCEUXKXF9ROYQNJFFGEHOOHDLNDN9XAWXYBVYYARTPRAFFOJN9",
    "QMMDUXSUOSITO9JVPCJWHIQRVDBPKKZGTSYOKLUNMSM9WIXLLJLFEMKUPEO9MOFDYRDC9GMRRETRGAWJD"
  ]
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:14265", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
  "command": "checkConsistency",
  "tails": [
    "SHHXOGUUYSCCFVMCZYCNBJIMGEEIBEPCCEUXKXF9ROYQNJFFGEHOOHDLNDN9XAWXYBVYYARTPRAFFOJN9",
    "QMMDUXSUOSITO9JVPCJWHIQRVDBPKKZGTSYOKLUNMSM9WIXLLJLFEMKUPEO9MOFDYRDC9GMRRETRGAWJD"
  ]
};

var options = {
  url: 'http://localhost:14265',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'X-IOTA-API-Version': '1',
    'Content-Length': Buffer.byteLength(JSON.stringify(command))
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
### Curl
```bash
curl http://localhost:14265 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
"command": "checkConsistency",
"tails": [
  "SHHXOGUUYSCCFVMCZYCNBJIMGEEIBEPCCEUXKXF9ROYQNJFFGEHOOHDLNDN9XAWXYBVYYARTPRAFFOJN9", 
  "QMMDUXSUOSITO9JVPCJWHIQRVDBPKKZGTSYOKLUNMSM9WIXLLJLFEMKUPEO9MOFDYRDC9GMRRETRGAWJD"
  ]
}'
```
--------------------

### レスポンス例
--------------------
### 200
```json
{
"state":true,
"info":"",
"duration":982
}
```
---
### 400
```json
{
  "error": "'command' parameter has not been specified"
}
```
--------------------

### 結果

| リターンフィールド | 説明 |
|--|--|
| `state` | `tails`パラメータ内の与えられたトランザクションの状態。`true`は、与えられたすべてのトランザクションに整合性があることを意味します。`false`は、与えられたトランザクションの一つ以上に整合性がないことを意味します。 |
| `info` | `state`フィールドが`false`の場合、このフィールドにはトランザクションに整合性がない理由に関する情報が含まれます。 |
| `duration` | リクエストを完了するのにかかったミリ秒数 |

## findTransactions

Find transactions that contain the given values in their transaction fields.
The parameters define the transaction fields to search for, including `bundles`, `addresses`, `tags`, and `approvees`.

**Using multiple transaction fields, returns transactions hashes at the intersection of those values.** 

### Parameters
	
|Parameters |Description | Type
|--|--|--|
| `bundles` | Bundle hashes to search for | array of strings |
| `addresses` | Addresses to search for (do not include the checksum) | array of strings |
| `tags` | Tags to search for | array of strings |
| `approvees` | Child transactions to search for | array of strings |

### Examples
--------------------
### Python
```python
import urllib2
import json

command = {
  "command": "findTransactions",
  "addresses": [
    "RVORZ9SIIP9RCYMREUIXXVPQIPHVCNPQ9HZWYKFWYWZRE9JQKG9REPKIASHUUECPSQO9JT9XNMVKWYGVA"
  ]
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:14265", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
  "command": "findTransactions",
  "addresses": [
    "RVORZ9SIIP9RCYMREUIXXVPQIPHVCNPQ9HZWYKFWYWZRE9JQKG9REPKIASHUUECPSQO9JT9XNMVKWYGVA"
  ]
};


var options = {
  url: 'http://localhost:14265',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
		'X-IOTA-API-Version': '1',
    'Content-Length': Buffer.byteLength(JSON.stringify(command))
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
### Curl
```bash
curl http://localhost:14265 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
  "command": "findTransactions",
  "addresses": [
    "RVORZ9SIIP9RCYMREUIXXVPQIPHVCNPQ9HZWYKFWYWZRE9JQKG9REPKIASHUUECPSQO9JT9XNMVKWYGVA"
  ]
}'
```
--------------------

### Response examples
--------------------
### 200
```json
{
  "hashes": [
    "P9KFSJVGSPLXAEBJSHWFZLGP9GGJTIO9YITDEHATDTGAFLPLBZ9FOFWWTKMAZXZHFGQHUOXLXUALY9999", "P9KFSJVGSPLXAEBJSHWFZLGP9GGJTIO9YITDEHATDTGAFLPLBZ9FOFWWTKMAZXZHFGQHUOXLXUALY9999"
  ],
  "duration": 567
}
```
---
### 400
```json
{
  "error": "'command' parameter has not been specified"
}
```
--------------------

### Results

An array of transaction hashes, is returned in the same order for all individual elements.

|Return field | Description |
|--|--|
| `hashes` | The transaction hashes which are returned depend on your input. `bundles`: returns an array of transaction hashes that contain the given bundle hash. `addresses`: returns an array of transaction hashes that contain the given address in the `address` field. `tags`: returns an array of transaction hashes that contain the given value in the `tag` field. `approvees`: returns an array of transaction hashes that contain the given transactions in their `branchTransaction` or `trunkTransaction` fields. |
| `duration` | Number of milliseconds it took to complete the request |

## getBalances

Get the confirmed balance of an address.

If the `tips` parameter is missing, the returned balance is correct as of the latest confirmed milestone.

 ### Parameters
	
|Parameters | Required or Optional|Description |Type
|--|--|--|--|
| `addresses` |Required| Address for which to get the balance (do not include the checksum) |array of strings|
| `threshold` |Required| Confirmation threshold between 0 and 100 | integer|
| `tips` |Optional| Tips whose history of transactions to traverse to find the balance |array of strings|

### Examples
--------------------
### Python
```python
import urllib2
import json

command = {
  "command": "getBalances",
  "addresses": [
    "DE9DVSOWIIIKEBAAHCKBWNXGXTOKVLZPLRAGKZG9GXKFRFWERKBFYMPRLAGVZTRVYPEPHBMUPDMRQ9DPZ"
  ],
  "threshold": 100
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:14265", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
  "command": "getBalances",
  "addresses": [
    "DE9DVSOWIIIKEBAAHCKBWNXGXTOKVLZPLRAGKZG9GXKFRFWERKBFYMPRLAGVZTRVYPEPHBMUPDMRQ9DPZ"
  ],
  "threshold": 100
};

var options = {
  url: 'http://localhost:14265',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
		'X-IOTA-API-Version': '1',
    'Content-Length': Buffer.byteLength(JSON.stringify(command))
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
### Curl
```bash
curl http://localhost:14265 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
  "command": "getBalances",
  "addresses": [
    "DE9DVSOWIIIKEBAAHCKBWNXGXTOKVLZPLRAGKZG9GXKFRFWERKBFYMPRLAGVZTRVYPEPHBMUPDMRQ9DPZ"
  ],
  "threshold": 100
}'
```
--------------------

### Response examples
--------------------
### 200
```json
{
  "balances":["197"],
  "references":[
    "GSBROIMQWTOQTFGJHHJPMCZR9DIRN9CQGUBKTGSOQLZRGKFBJFMRIGNGWZDNWKADGMNR9TMLRMLIUZ999"
  ],
  "milestoneIndex":1084812,
  "duration":0
}
```
---
### 400
```json
{
  "error": "'command' parameter has not been specified"
}
```
--------------------

### Results

|Return field | Description |
|--|--|
| `balances` | Array of balances in the same order as the `addresses` parameters were passed to the endpoint|
| `references` | The referencing tips. If no `tips` parameter was passed to the endpoint, this field contains the hash of the latest milestone that confirmed the balance |
| `milestoneIndex` | The index of the milestone that confirmed the most recent balance |
| `duration` | Number of milliseconds it took to process the request |

## getInclusionStates

Get the inclusion states of a set of transactions. 
This endpoint determines if a transaction is confirmed by the network (referenced by a valid milestone). 
You can search for multiple tips (and thus, milestones) to get past inclusion states of transactions.

 ### Parameters
	
|Parameter | Required or Optional|Description | Type|
|--|--|--|--|
| `transactions` |Required| List of transaction hashes for which you want to get the inclusion state|array of strings
| `tips` | Optional|List of tip transaction hashes (including milestones) you want to search for | array of strings

### Examples
--------------------
### Python
```python
import urllib2
import json

command = {
  "command": "getInclusionStates",
  "transactions": [
    "P9KFSJVGSPLXAEBJSHWFZLGP9GGJTIO9YITDEHATDTGAFLPLBZ9FOFWWTKMAZXZHFGQHUOXLXUALY9999", 
    "P9KFSJVGSPLXAEBJSHWFZLGP9GGJTIO9YITDEHATDTGAFLPLBZ9FOFWWTKMAZXZHFGQHUOXLXUALY9999"
  ],
  "tips": [
    "P9KFSJVGSPLXAEBJSHWFZLGP9GGJTIO9YITDEHATDTGAFLPLBZ9FOFWWTKMAZXZHFGQHUOXLXUALY9999", 
    "P9KFSJVGSPLXAEBJSHWFZLGP9GGJTIO9YITDEHATDTGAFLPLBZ9FOFWWTKMAZXZHFGQHUOXLXUALY9999"
  ]
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:14265", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
  "command": "getInclusionStates",
  "transactions": [
    "P9KFSJVGSPLXAEBJSHWFZLGP9GGJTIO9YITDEHATDTGAFLPLBZ9FOFWWTKMAZXZHFGQHUOXLXUALY9999", 
    "P9KFSJVGSPLXAEBJSHWFZLGP9GGJTIO9YITDEHATDTGAFLPLBZ9FOFWWTKMAZXZHFGQHUOXLXUALY9999"
  ],
  "tips": [
    "P9KFSJVGSPLXAEBJSHWFZLGP9GGJTIO9YITDEHATDTGAFLPLBZ9FOFWWTKMAZXZHFGQHUOXLXUALY9999", 
    "P9KFSJVGSPLXAEBJSHWFZLGP9GGJTIO9YITDEHATDTGAFLPLBZ9FOFWWTKMAZXZHFGQHUOXLXUALY9999"
  ]
}

var options = {
  url: 'http://localhost:14265',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
		'X-IOTA-API-Version': '1',
    'Content-Length': Buffer.byteLength(JSON.stringify(command))
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
### Curl
```bash
curl http://localhost:14265 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
  "command": "getInclusionStates",
  "transactions": [
    "P9KFSJVGSPLXAEBJSHWFZLGP9GGJTIO9YITDEHATDTGAFLPLBZ9FOFWWTKMAZXZHFGQHUOXLXUALY9999", 
    "P9KFSJVGSPLXAEBJSHWFZLGP9GGJTIO9YITDEHATDTGAFLPLBZ9FOFWWTKMAZXZHFGQHUOXLXUALY9999"
  ],
  "tips": [
    "P9KFSJVGSPLXAEBJSHWFZLGP9GGJTIO9YITDEHATDTGAFLPLBZ9FOFWWTKMAZXZHFGQHUOXLXUALY9999", 
    "P9KFSJVGSPLXAEBJSHWFZLGP9GGJTIO9YITDEHATDTGAFLPLBZ9FOFWWTKMAZXZHFGQHUOXLXUALY9999"
  ]
}'
```
--------------------

### Response examples
--------------------
### 200
```json
{
  "states": [true, true],
  "duration": 726
}
```
---
### 400
```json
{
  "error": "'command' parameter has not been specified"
}
```
--------------------

### Results

|Return field | Description |
|--|--|
| `states` | List of boolean values in the same order as the `transactions` parameters. A `true` value means the transaction was confirmed |
| `duration` | Number of milliseconds it took to complete the request |

## getNeighbors

Get a node's neighbors and their activity.

### Examples
--------------------
### Python
```python
import urllib2
import json

command = {"command": "getNeighbors"}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:14265", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {"command": "getNeighbors"}

var options = {
  url: 'http://localhost:14265',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
		'X-IOTA-API-Version': '1',
    'Content-Length': Buffer.byteLength(JSON.stringify(command))
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
### Curl
```bash
curl http://localhost:14265 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{"command": "getNeighbors"}'
```
--------------------

### Response examples
--------------------
### 200
```json
{
  "neighbors": [{ 
    "address": "/8.8.8.8:14265", 
    "numberOfAllTransactions": 158, 
    "numberOfRandomTransactionRequests": 271,
    "numberOfNewTransactions": 956,
    "numberOfInvalidTransactions": 539, 
    "numberOfStaleTransactions": 663, 
    "numberOfSentTransactions": 672, 
    "connectiontype": "TCP" 
  }],
  "duration": 735
}
```
---
### 400
```json
{
  "error": "'command' parameter has not been specified"
}
```
--------------------

### Results

:::info:
The activity accumulates until the node restarts.
:::

|Return field| Description |
|--|--|
| `neighbors` | Array of objects, including the following fields: address, connectionType, numberOfAllTransactions, numberOfRandomTransactionRequests, numberOfNewTransactions, numberOfInvalidTransactions, numberOfStaleTransactions, numberOfSentTransactions, connectiontype |
| `duration` | Number of milliseconds it took to complete the request |

## getNodeInfo

Get information about a node.

### Examples
--------------------
### Python
```python
import urllib2
import json

command = {"command": "getNodeInfo"}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:14265", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {"command": "getNodeInfo"}

var options = {
  url: 'http://localhost:14265',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
		'X-IOTA-API-Version': '1',
    'Content-Length': Buffer.byteLength(JSON.stringify(command))
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
### Curl
```bash
curl http://localhost:14265 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{"command": "getNodeInfo"}'
```
--------------------

### Response examples
--------------------
### 200
```json
{
 "appName": "IRI",
 "appVersion": "1.7.0-RELEASE",
 "jreAvailableProcessors": 8,
 "jreFreeMemory": 2115085674,
 "jreVersion": "1.8.0_191",
 "jreMaxMemory": 20997734400,
 "jreTotalMemory": 4860129502,
 "latestMilestone": "CUOENIPTRCNECMVOXSWKOONGZJICAPH9FIG9F9KYXF9VYXFUKTNDCCLLWRZNUHZIGLJZFWPOVCIZA9999",
 "latestMilestoneIndex": 1050373,
 "latestSolidSubtangleMilestone": "CUOENIPTRCNECMVOXSWKOONGZJICAPH9FIG9F9KYXF9VYXFUKTNDCCLLWRZNUHZIGLJZFWPOVCIZA9999",
 "latestSolidSubtangleMilestoneIndex": 1050373,
 "milestoneStartIndex": 1050101,
 "lastSnapshottedMilestoneIndex": 1039138,
 "neighbors": 7,
 "packetsQueueSize": 0,
 "time": 1554970558971,
 "tips": 9018,
 "transactionsToRequest": 0,
 "features": [
  "snapshotPruning",
  "dnsRefresher",
  "tipSolidification"
 ],
 "coordinatorAddress": "EQSAUZXULTTYZCLNJNTXQTQHOMOFZERHTCGTXOLTVAHKSA9OGAZDEKECURBRIXIJWNPFCQIOVFVVXJVD9",
 "duration": 0
}
```
---
### 400
```json
{
  "error": "'command' parameter has not been specified"
}
```
--------------------

### Results

|**Return field** | **Description** |
|--|--|
| `appName` | Name of the IRI network |
| `appVersion` | Version of the IRI |
| `jreAvailableProcessors` | Available CPU cores on the node |
| `jreFreeMemory` | Amount of free memory in the Java virtual machine |
| `jreMaxMemory` | Maximum amount of memory that the Java virtual machine can use |
| `jreTotalMemory` | Total amount of memory in the Java virtual machine|
| `jreVersion` | The version of the Java runtime environment|
| `latestMilestone` | Transaction hash of the latest milestone |
| `latestMilestoneIndex` | Index of the latest milestone |
| `latestSolidSubtangleMilestone` | Transaction hash of the latest solid milestone |
| `latestSolidSubtangleMilestoneIndex` | Index of the latest solid milestone |
| `milestoneStartIndex` | Start milestone for the current version of the IRI |
|`lastSnapshottedMilestoneIndex`|Index of the last milestone that triggered a [local snapshot](../concepts/local-snapshot.md) on the node |
| `neighbors` | Total number of connected neighbor nodes  |
| `packetsQueueSize` | Size of the packet queue |
| `time` | Current UNIX timestamp |
| `tips` | Number of tips in the network |
| `transactionsToRequest` | Total number of transactions that the node is missing in its ledger|
| `features` | Enabled configuration options|
| `coordinatorAddress` | Address (Merkle root) of the Coordinator|
| `duration` | Number of milliseconds it took to complete the request |

## getTips

Get tip transaction hashes from a node.

### Examples
--------------------
### Python
```python
import urllib2
import json

command = {"command": "getTips"}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:14265", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {"command": "getTips"}

var options = {
  url: 'http://localhost:14265',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
		'X-IOTA-API-Version': '1',
    'Content-Length': Buffer.byteLength(JSON.stringify(command))
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
### Curl
```bash
curl http://localhost:14265 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{"command": "getTips"}'
```
--------------------

### Response examples
--------------------
### 200
```json
{
  "hashes": [
    "P9KFSJVGSPLXAEBJSHWFZLGP9GGJTIO9YITDEHATDTGAFLPLBZ9FOFWWTKMAZXZHFGQHUOXLXUALY9999", 
    "P9KFSJVGSPLXAEBJSHWFZLGP9GGJTIO9YITDEHATDTGAFLPLBZ9FOFWWTKMAZXZHFGQHUOXLXUALY9999"
  ],
  "duration": 17
}
```
---
### 400
```json
{
  "error": "'command' parameter has not been specified"
}
```
--------------------

### Results

|Return field| Description |
|--|--|
| `hashes` | Array of tip transaction hashes |
| `duration` | Number of milliseconds it took to complete the request |

## getTransactionsToApprove

Get two consistent tip transaction hashes to use as branch/trunk transactions.

### Parameters

|Parameter|Required or Optional| Description |Type|
|--|--|--|--|
| `depth` |Required| Number of bundles to go back to determine the transactions for approval. |integer|
| `reference` |Optional| Transaction hash from which to start the weighted random walk. Use this parameter to make sure the returned tip transaction hashes approve a given reference transaction. |string

### Examples
--------------------
### Python
```python
import urllib2
import json

command = {
  "command": "getTransactionsToApprove",
  "depth": 4,
  "reference": "P9KFSJVGSPLXAEBJSHWFZLGP9GGJTIO9YITDEHATDTGAFLPLBZ9FOFWWTKMAZXZHFGQHUOXLXUALY9999"
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:14265", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
  "command": "getTransactionsToApprove",
  "depth": 4,
  "reference": "P9KFSJVGSPLXAEBJSHWFZLGP9GGJTIO9YITDEHATDTGAFLPLBZ9FOFWWTKMAZXZHFGQHUOXLXUALY999"
}

var options = {
  url: 'http://localhost:14265',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
		'X-IOTA-API-Version': '1',
    'Content-Length': Buffer.byteLength(JSON.stringify(command))
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
### Curl
```bash
curl http://localhost:14265 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
  "command": "getTransactionsToApprove",
  "depth": 4,
  "reference": "P9KFSJVGSPLXAEBJSHWFZLGP9GGJTIO9YITDEHATDTGAFLPLBZ9FOFWWTKMAZXZHFGQHUOXLXUALY9999"
}'
```
--------------------

### Response examples
--------------------
### 200
```json
{
"trunkTransaction":"YXQWAVOYFGGPGAIMVLGMWBPSWLAGDBAVWUXBBTPAUHANQQAKEUAOKOMFHKHCFEGAIG9JPMMGTFUTZ9999",
"branchTransaction":"PHKTCBHQFZGMPJT9ZBCKMPIBZJXF9JYKXKJUHHRJTEIIPFVNNCIGAZUQVOMMFJZKULLQMOYYFEVIZ9999",
"duration":982
}
```
---
### 400
```json
{"error": "'command' parameter has not been specified"}
```
--------------------

### Results

|Return field | Description |
|--|--|
| `trunkTransaction` | Valid trunk transaction hash |
| `branchTransaction` | Valid branch transaction hash |
| `duration` | The time it took to process the request in milliseconds |

## getTrytes

Get a transaction's contents in trytes.

### Parameters

|Parameter | Required or Optional|Description |Type
|--|--|--|--|
| `hashes` |Required| Transaction hashes | array of strings

### Examples
--------------------
### Python
```python
import urllib2
import json

command = {
  "command": "getTrytes",
  "hashes": [
    "NGDFRIHOOS9J9YBQCDSFJZJKNSAJTNFKSXXEZWPZQSLRTYQDNX9UCGJHU9OZGFATCCQSGSFUZLLET9LID", 
    "MUIYDLYHCAYGYK9IPVQX9GIHIWWCATAJ9BNFPVKZHZOSXAWVHEHHMSVEVTNRJVGCGEMSNI9ATUXFKPZRQ"
  ]
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:14265", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
  "command": "getTrytes",
  "hashes": [
    "NGDFRIHOOS9J9YBQCDSFJZJKNSAJTNFKSXXEZWPZQSLRTYQDNX9UCGJHU9OZGFATCCQSGSFUZLLET9LID", 
    "MUIYDLYHCAYGYK9IPVQX9GIHIWWCATAJ9BNFPVKZHZOSXAWVHEHHMSVEVTNRJVGCGEMSNI9ATUXFKPZRQ"
  ]
}

var options = {
  url: 'http://localhost:14265',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
		'X-IOTA-API-Version': '1',
    'Content-Length': Buffer.byteLength(JSON.stringify(command))
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
### Curl
```bash
curl http://localhost:14265 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{ 
"command": "getTrytes",
"hashes": [
  "NGDFRIHOOS9J9YBQCDSFJZJKNSAJTNFKSXXEZWPZQSLRTYQDNX9UCGJHU9OZGFATCCQSGSFUZLLET9LID", 
  "MUIYDLYHCAYGYK9IPVQX9GIHIWWCATAJ9BNFPVKZHZOSXAWVHEHHMSVEVTNRJVGCGEMSNI9ATUXFKPZRQ"
  ]
}'
```
--------------------

### Response examples
--------------------
### 200
```json
{
"trytes": ["JJSLJFJD9HMHHMKAJNRODFHUN ..."],
"duration":982
}
```
---
### 400
```json
{
  "error": "'command' parameter has not been specified"
}
```
--------------------

### Results

You can convert the returned trytes to ASCII characters by using the client libraries.

|Return field | Description |
|--|--|
| `trytes` | Array of transaction trytes for the given transaction hashes (in the same order as the parameters) |
| `duration` | Number of milliseconds it took to complete the request |

:::info:
If a node doesn't have the trytes for a given transaction hash in its ledger, the value at the index of that transaction hash is either `null` or a string of 9s.
:::

## interruptAttachingToTangle

Abort the process that's started by the [`attachToTangle`](#attachToTangle) endpoint.

### Examples
--------------------
### Python
```python
import urllib2
import json

command = {"command": "interruptAttachingToTangle"}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:14265", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {"command": "interruptAttachingToTangle"}

var options = {
  url: 'http://localhost:14265',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
		'X-IOTA-API-Version': '1',
    'Content-Length': Buffer.byteLength(JSON.stringify(command))
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
### Curl
```bash
curl http://localhost:14265 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{"command": "interruptAttachingToTangle"}'
```
--------------------

### Response examples
--------------------
### 200
```json
{
"duration":982
}
```
---
### 400
```json
{
  "error": "'command' parameter has not been specified"
}
```
--------------------

### Results

|Return field | Description |
|--|--|
| `duration` | Number of milliseconds it took to complete the request |

## removeNeighbors

Temporarily removes a list of neighbors from a node.

:::info:
The neighbors are added again if the node restarts. If you want to permanently remove the neighbors from your own node, remove their URIs from the [`NEIGHBORS`](../references/iri-configuration-options.md#neighbors) configuration option. 
:::

### Parameters

The URI (unique resource identification) format for removing neighbors is `"udp://IPADDRESS:PORT"`.

|Parameter | Required or Optional|Description | Type|
|--|--|--|--|
| `uris` | Required|Strings of neighbor URIs to remove | array of strings|

### Examples
--------------------
### Python
```python
import urllib2
import json

command = {"command": "removeNeighbors", "uris": ["udp://8.8.8.8:14265", "udp://8.8.8.8:14265"]}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:14265", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {"command": "removeNeighbors", "uris": ["udp://8.8.8.8:14265", "udp://8.8.8.8:14265"]}

var options = {
  url: 'http://localhost:14265',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
		'X-IOTA-API-Version': '1',
    'Content-Length': Buffer.byteLength(JSON.stringify(command))
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
### Curl
```bash
curl http://localhost:14265 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{"command": "removeNeighbors", "uris": ["udp://8.8.8.8:14265", "udp://8.8.8.8:14265"]}'
```
--------------------

### Response examples
--------------------
### 200
```json
{
"removedNeighbors": 2,
"duration":982
}
```
---
### 400
```json
{"error": "'command' parameter has not been specified"}
```
--------------------

### Results

|Return field | Description |
|--|--|
| `removedNeighbors` | Total number of removed neighbors |
| `duration` | Number of milliseconds it took to complete the request |

## storeTransactions

Store transactions in a node's local storage.

### Parameters

The value of the `trytes` parameter must be valid. Valid trytes are returned by the [`attachToTangle`](#attachToTangle) endpoint.

|Parameter | Required or Optional|Description |Type
|--|--|--|--|
| `trytes` |Required| Transaction trytes | array of strings

### Examples
--------------------
### Python
```python
import urllib2
import json

command = {
  "command": "storeTransactions",
  "trytes": ["RKDQGFBD9W9VKDEJDEXUNJBAG ..."]
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:14265", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
  "command": "storeTransactions",
  "trytes": ["RKDQGFBD9W9VKDEJDEXUNJBAG ..."]
}

var options = {
  url: 'http://localhost:14265',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
		'X-IOTA-API-Version': '1',
    'Content-Length': Buffer.byteLength(JSON.stringify(command))
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
### Curl
```bash
curl http://localhost:14265 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{ 
  "command": "storeTransactions",
  "trytes": ["RKDQGFBD9W9VKDEJDEXUNJBAG ..."]
}'
```
--------------------

### Response examples
--------------------
### 200
```json
{
"trytes": ["JJSLJFJD9HMHHMKAJNRODFHUN ..."],
"duration": 982
}
```
---
### 400
```json
{
  "error": "'command' parameter has not been specified"
}
```
--------------------

### Results

|Return field | Description |
|--|--|
| `duration` | Number of milliseconds it took to complete the request |

## wereAddressesSpentFrom

Check if an address was ever withdrawn from, either in the current epoch or in any previous epochs.

If an address has a pending transaction, it's also considered 'spent'.

### Parameters

|Parameter | Required or Optional|Description |Type
|--|--|--|--|
| `addresses` |Required| addresses to check (do not include the checksum) | array of strings

### Examples
--------------------
### Python
```python
import urllib2
import json

command = {
  "command": "wereAddressesSpentFrom",
  "addresses": [
    "BKDEARVZVOWC9LZKTAB9AUSJSHCGVDQQGJUVNWHV9XNICMDFHEZOVLYRJYMHXKZZXSNRZRPYFSUFAFIP9", 
    "JKPNBVXIFLISXOXLSGHFCYIY9WJHHMORXAOWUXTLGCCHCCKEBHVBWSEEMBIYXMIEZ9FCRHFOHJRANSGB9"
  ]
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:14265", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
  "command": "wereAddressesSpentFrom",
  "addresses": [
    "BKDEARVZVOWC9LZKTAB9AUSJSHCGVDQQGJUVNWHV9XNICMDFHEZOVLYRJYMHXKZZXSNRZRPYFSUFAFIP9",
    "JKPNBVXIFLISXOXLSGHFCYIY9WJHHMORXAOWUXTLGCCHCCKEBHVBWSEEMBIYXMIEZ9FCRHFOHJRANSGB9"
  ]
};

var options = {
  url: 'http://localhost:14265',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
		'X-IOTA-API-Version': '1',
    'Content-Length': Buffer.byteLength(JSON.stringify(command))
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
### Curl
```bash
curl http://localhost:14265 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
  "command": "wereAddressesSpentFrom",
  "addresses": [
    "BKDEARVZVOWC9LZKTAB9AUSJSHCGVDQQGJUVNWHV9XNICMDFHEZOVLYRJYMHXKZZXSNRZRPYFSUFAFIP9", 
    "JKPNBVXIFLISXOXLSGHFCYIY9WJHHMORXAOWUXTLGCCHCCKEBHVBWSEEMBIYXMIEZ9FCRHFOHJRANSGB9"
  ]
}'
```
--------------------

### Response examples
--------------------
### 200
```json
{
"states": [true, false],
"duration": 982
}
```
---
### 400
```json
{
  "error": "'command' parameter has not been specified"
}
```
--------------------

### Results

|Return field | Description |
|--|--|
| `states` | States of the specified addresses in the same order as the values in the `addresses` parameter. A `true` value means that the address has been spent from. |
| `duration` | Number of milliseconds it took to complete the request |
