# APIリファレンス
<!-- # API reference -->

**この一覧には、HTTP APIを介してノードと通信するためのコマンドが含まれています。**
<!-- **This list contains the commands for communicating with a node through the HTTP API.** -->

以下のすべてのコマンドにHTTPヘッダーを含める必要があります。
<!-- All the following commands must include an HTTP header. -->

| **ヘッダー** | **値** | **必須または任意** |
|:---------------|:--------|:--------|
| X-IOTA-API-Version | 1 | 必須 |
| Content-Type | application/json | 任意 |
| Authorization | Bearer {token} | 任意 |

:::warning:
このAPIはベータ版であり、変更される可能性があります。運用版アプリケーションではこのAPIを使用しないことをお勧めします。
:::
<!-- :::warning: -->
<!-- This API is in beta, and is subject to change. We recommend that you don't use this API in production applications. -->
<!-- ::: -->

## addNeighbors

ノードに一時的な隣接ノードのリストを追加します。
<!-- Add a list of temporary neighbors to a node. -->

:::info:
ノードが再起動すると、隣接ノードは削除されます。ノードに恒久的に隣接ノードを追加したい場合は、[`NEIGHBORS`](../references/iri-configuration-options.md#neighbors)設定オプションに隣接ノードのURIを追加してください。
:::
<!-- :::info: -->
<!-- The neighbors are removed if the node restarts. If you want to permanently add the neighbors to your own node, add their URIs to the [`NEIGHBORS`](../references/iri-configuration-options.md#neighbors) configuration option. -->
<!-- ::: -->

### パラメータ

隣接ノードを追加するためのURIフォーマットは、`"tcp://IPADDRESS:PORT"`です。
<!-- The URI format for adding neighbors is `"tcp://IPADDRESS:PORT"`. -->

| **パラメータ** | **必須または任意** | **説明** | **タイプ** |
|--|--|--|--|
| `uris` | 必須 | 追加する隣接ノードのURIの文字列 | stringの配列 |

### 例
--------------------
### Python
```python
import urllib2
import json

command = {
  "command": "addNeighbors",
  "uris": [
    "tcp://8.8.8.8:14265",
    "tcp://8.8.8.8:14265"
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
    "tcp://8.8.8.8:14265",
    "tcp://8.8.8.8:14265"
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
    "tcp://8.8.8.8:14265",
    "tcp://8.8.8.8:14265"
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

### 結果
<!-- ### Results -->

| **リターンフィールド** | **説明** |
|--|--|
| `addedNeighbors` | 追加した隣接ノードの総数 |
| `duration` | リクエストを完了するのにかかったミリ秒数 |

## attachToTangle

与えられたトランザクショントライトについてノード上で[プルーフオブワーク](root://the-tangle/0.1/concepts/proof-of-work.md)を行います。
<!-- Do [proof of work](root://the-tangle/0.1/concepts/proof-of-work.md) on a node for the given transaction trytes. -->

 ### パラメータ

 `branchTransaction`および`trunkTransaction`パラメーターは、[`getTransactionsToApprove`](#getTransactionsToApprove)エンドポイントから返されます。
<!--  The `branchTransaction` and  `trunkTransaction` parameters are returned from the [`getTransactionsToApprove`](#getTransactionsToApprove) endpoint. -->

| **パラメータ** | **必須または任意** | **説明** | **タイプ** |
|--|--|--|--|
| `trunkTransaction` | 必須 | [トランクトランザクション](root://iota-basics/0.1/references/structure-of-a-transaction.md)ハッシュ | string |
| `branchTransaction` | 必須 | [ブランチトランザクション](root://iota-basics/0.1/references/structure-of-a-transaction.md)ハッシュ | string |
| `minWeightMagnitude` | 必須 | [最小重量値](root://iota-basics/0.1/concepts/minimum-weight-magnitude.md) | integer |
| `trytes` | 必須 | トランザクショントライトの文字列 | stringの配列 |

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

| **リターンフィールド** | **説明** |
|--|--|
| `trytes` | 有効な`nance`フィールドを含むトランザクショントライト |

## broadcastTransactions

トランザクショントライトをノードへブロードキャストします。
<!-- Broadcast transaction trytes to a node. -->

### パラメータ

このエンドポイントの`trytes`パラメータには、[`attachToTangle`](#attachToTangle)エンドポイントによって行われるプルーフオブワークを含める必要があります。
<!-- The `trytes` parameter for this endpoint must include proof of work, which is done by the [`attachToTangle`](#attachToTangle) endpoint. -->

| **パラメータ** | **必須または任意** | **説明** | **タイプ** |
|--|--|--|--|
| `trytes` | 必須 | 有効なトランザクショントライト | stringの配列 |

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

| **リターンフィールド** | **説明** |
|--|--|
| `duration` | リクエストを完了するのにかかったミリ秒数 |

## checkConsistency

トランザクションの整合性を確認します。整合性のあるトランザクションとは、次のことが当てはまるトランザクションです。
<!-- Check the consistency of transactions. A consistent transaction is one where the following statements are true: -->
* ノードにトランザクションのブランチまたはトランクトランザクションが欠落していない。
<!-- * The node isn't missing the transaction's branch or trunk transactions -->
* トランザクションのバンドルが有効。
<!-- * The transaction's bundle is valid -->
* トランザクションのブランチおよびトランクトランザクションが有効。
<!-- * The transaction's branch and trunk transactions are valid -->

### パラメータ

| **パラメータ** | **必須または任意** | **説明** | **タイプ** |
|--|--|--|--|
| `tails` | 必須 | 確認するトランザクションハッシュ | stringの配列 |

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

| **リターンフィールド** | **説明** |
|--|--|
| `state` | `tails`パラメータ内で与えられたトランザクションの状態。`true`は、与えられたすべてのトランザクションに整合性があることを意味します。`false`は、与えられたトランザクションの一つ以上に整合性がないことを意味します。 |
| `info` | `state`フィールドが`false`の場合、このフィールドにはトランザクションに整合性がない理由に関する情報が含まれます。 |
| `duration` | リクエストを完了するのにかかったミリ秒数 |

## findTransactions

トランザクションフィールドに与えられた値を含むトランザクションを見つけます。パラメータは、`bundles`、`addresses`、`tags`、`approvees`などの検索するトランザクションフィールドを定義します。
<!-- Find transactions that contain the given values in their transaction fields. -->
<!-- The parameters define the transaction fields to search for, including `bundles`, `addresses`, `tags`, and `approvees`. -->

**複数のトランザクションフィールドを使用して、複数のトランザクションフィールドの値の交点部分でトランザクションハッシュを返します。**
<!-- **Using multiple transaction fields, returns transactions hashes at the intersection of those values.** -->

### パラメータ

| **パラメータ** | **説明** | **タイプ** |
|--|--|--|
| `bundles` | 検索するバンドルハッシュ | stringの配列 |
| `addresses` | 検索するアドレス（チェックサムを含まない） | stringの配列 |
| `tags` | 検索するタグ | stringの配列 |
| `approvees` | 検索する子トランザクション | stringの配列 |

### 例
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

### レスポンス例
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

### 結果

トランザクションハッシュの配列は、すべての個々の要素に対して同じ順序で返されます。
<!-- An array of transaction hashes, is returned in the same order for all individual elements. -->

| **リターンフィールド** | **説明** |
|--|--|
| `hashes` | 返されるトランザクションハッシュは入力によって異なります。`bundles`：与えられたバンドルハッシュを含むトランザクションハッシュの配列を返します。`addresses`：`address`フィールドに与えられたアドレスを含むトランザクションハッシュの配列を返します。`tags`：`tag`フィールドに与えられた値を含むトランザクションハッシュの配列を返します。`approvees`：`branchTransaction`フィールドまたは`trunkTransaction`フィールドに与えられたトランザクションを含むトランザクションハッシュの配列を返します。 |
| `duration` | リクエストを完了するのにかかったミリ秒数 |

## getBalances

確定済みのアドレスの残高を取得します。
<!-- Get the confirmed balance of an address. -->

`tips`パラメータがない場合、返された残高は最新の確定済みマイルストーンの時点で正しいです。
<!-- If the `tips` parameter is missing, the returned balance is correct as of the latest confirmed milestone. -->

 ### パラメータ

| **パラメータ** | **必須または任意** | **説明** | **タイプ** |
|--|--|--|--|
| `addresses` | 必須 | 残高を取得するアドレス（チェックサムを含まない） | stringの配列 |
| `threshold` | 必須 | 0から100までの確認しきい値 | integer |
| `tips` | 任意 | 残高を見つけるためにトランザクションの履歴をたどるチップ | stringの配列 |

### 例
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

### レスポンス例
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

### 結果

| **リターンフィールド** | **説明** |
|--|--|
| `balances` | `addresses`パラメーターがエンドポイントに渡されたのと同じ順序での残高の配列 |
| `references` | 参照しているチップ。`tips`パラメータがエンドポイントに渡されなかった場合、このフィールドには残高を確定した最新のマイルストーンのハッシュが含まれます。 |
| `milestoneIndex` | 最新の残高を確定したマイルストーンのインデックス |
| `duration` | リクエストの処理にかかったミリ秒数 |

## getInclusionStates

一連のトランザクションの包含状態を取得します。
<!-- Get the inclusion states of a set of transactions. -->

このエンドポイントは、トランザクションがネットワークによって確定された（有効なマイルストーンによって参照された）かどうかを判別します。
<!-- This endpoint determines if a transaction is confirmed by the network (referenced by a valid milestone). -->

過去の包含状態のトランザクションを取得するために、複数のチップ（つまりマイルストーン）を検索できます。
<!-- You can search for multiple tips (and thus, milestones) to get past inclusion states of transactions. -->

### パラメータ

| **パラメータ** | **必須または任意** | **説明** | **タイプ** |
|--|--|--|--|
| `transactions` | 必須 | 包含状態を取得したいトランザクションハッシュのリスト | stringの配列 |
| `tips` | 必須(空も可) | 検索したいチップトランザクションハッシュ（マイルストーンを含む）のリスト | stringの配列 |

### 例
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

### レスポンス例
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

### 結果

| **リターンフィールド** | **説明** |
|--|--|
| `states` | `transactions`パラメータと同じ順序のブール値のリスト。`true`はトランザクションが確定されたことを意味します。 |
| `duration` | リクエストを完了するのにかかったミリ秒数 |

## getNeighbors

ノードの隣接ノードとその活動状況を取得します。
<!-- Get a node's neighbors and their activity. -->

### 例
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

### レスポンス例
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

### 結果

:::info:
ノードが再起動するまで活動状況は蓄積されます。
:::

| **リターンフィールド** | **説明** |
|--|--|
| `neighbors` | 次のフィールドを含むオブジェクトの配列。address、connectionType、numberOfAllTransactions、numberOfRandomTransactionRequests、numberOfNewTransactions、numberOfInvalidTransactions、numberOfStaleTransactions、numberOfSentTransactions、およびconnectiontype。 |
| `duration` | リクエストを完了するのにかかったミリ秒数 |

## getNodeInfo

ノードに関する情報を取得します。
<!-- Get information about a node. -->

### 例
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

### レスポンス例
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

### 結果

| リターンフィールド | 説明 |
|--|--|
| `appName` | IRIネットワークの名前 |
| `appVersion` | IRIのバージョン |
| `jreAvailableProcessors` | ノードで使用可能なCPUコア |
| `jreFreeMemory` | Java仮想マシンの空きメモリ量 |
| `jreMaxMemory` | Java仮想マシンが使用できる最大メモリ量 |
| `jreTotalMemory` | Java仮想マシンの総メモリ量 |
| `jreVersion` | Java実行環境のバージョン |
| `latestMilestone` | 最新のマイルストーンのトランザクションハッシュ |
| `latestMilestoneIndex` | 最新のマイルストーンのインデックス |
| `latestSolidSubtangleMilestone` | 最新の凝固マイルストーンのトランザクションハッシュ |
| `latestSolidSubtangleMilestoneIndex` | 最新の凝固マイルストーンのインデックス |
| `milestoneStartIndex` | 現在のバージョンのIRIが開始したマイルストーンのインデックス |
| `lastSnapshottedMilestoneIndex` | ノードで[ローカルスナップショット](../concepts/local-snapshot.md)を引き起こした最後のマイルストーンのインデックス |
| `neighbors` | 繋がっている隣接ノードの総数 |
| `packetsQueueSize` | パケットキューのサイズ |
| `time` | 現在のUNIXタイムスタンプ |
| `tips` | ネットワーク内のチップ数 |
| `transactionsToRequest` | ノードの台帳に欠落しているトランザクションの総数 |
| `features` | 有効な設定オプション |
| `coordinatorAddress` | コーディネーターのアドレス（マークルルート） |
| `duration` | リクエストを完了するのにかかったミリ秒数 |

## getTips

ノードからチップトランザクションハッシュを取得します。
<!-- Get tip transaction hashes from a node. -->

### 例
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

### 結果

| **リターンフィールド** | **説明** |
|--|--|
| `hashes` | チップトランザクションハッシュの配列 |
| `duration` | リクエストを完了するのにかかったミリ秒数 |

## getTransactionsToApprove

ブランチ/トランクトランザクションとして使用する2つの矛盾しないチップトランザクションハッシュを取得します。
<!-- Get two consistent tip transaction hashes to use as branch/trunk transactions. -->

### パラメータ

| **パラメータ** | **必須または任意** | **説明** | **タイプ** |
|--|--|--|--|
| `depth` | 必須 | 承認のためのトランザクションを決定するために戻るバンドルの数。 | integer |
| `reference` | 任意 | 重み付きランダムウォークを開始するトランザクションハッシュ。このパラメータを使用して、返されたチップトランザクションハッシュが与えられた参照トランザクションを確実に承認するようにします。 | string |

### 例
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

### レスポンス例
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

### 結果

| **リターンフィールド** | **説明** |
|--|--|
| `trunkTransaction` | 有効なトランクトランザクションハッシュ |
| `branchTransaction` | 有効なブランチトランザクションハッシュ |
| `duration` | リクエストの処理に要した時間（ミリ秒） |

## getTrytes

トライトでトランザクション内容を取得します。
<!-- Get a transaction's contents in trytes. -->

### パラメータ

| **パラメータ** | **必須または任意** | **説明** | **タイプ** |
|--|--|--|--|
| `hashes` | 必須 | トランザクションハッシュ | stringの配列 |

### 例
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

### レスポンス例
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

### 結果

クライアントライブラリを使用して、返されたトライトをASCII文字に変換できます。
<!-- You can convert the returned trytes to ASCII characters by using the client libraries. -->

| **リターンフィールド** | **説明** |
|--|--|
| `trytes` | 与えられたトランザクションハッシュに対するトランザクショントライトの配列（パラメータと同じ順序で） |
| `duration` | リクエストを完了するのにかかったミリ秒数 |

:::info:
ノードの台帳に与えられたトランザクションハッシュのトライトがない場合、そのトランザクションハッシュのインデックスの値は`null`または9の文字列です。
:::

## interruptAttachingToTangle

[`attachToTangle`](#attachToTangle)エンドポイントによって開始されたプロセスを中止します。
<!-- Abort the process that's started by the [`attachToTangle`](#attachToTangle) endpoint. -->

### 例
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

### レスポンス例
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

### 結果

| **リターンフィールド** | **説明** |
|--|--|
| `duration` | リクエストを完了するのにかかったミリ秒数 |

## removeNeighbors

ノードから隣接ノードのリストを一時的に削除します。
<!-- Temporarily removes a list of neighbors from a node. -->

:::info:
ノードが再起動すると、隣接ノードが再び追加されます。ノードから恒久的に隣接ノードを削除したい場合は、 [`NEIGHBORS`](../references/iri-configuration-options.md#neighbors)設定オプションから隣接ノードのURIを削除してください。
:::

### パラメータ

隣接ノードを削除するためのURIフォーマットは`"tcp://IPADDRESS:PORT"`です。
<!-- The URI format for removing neighbors is `"tcp://IPADDRESS:PORT"`. -->

| **パラメータ** | **必須または任意** | **説明** | **タイプ** |
|--|--|--|--|
| `uris` | 必須 | 削除する隣接ノードのURIの文字列 | stringの配列 |

### 例
--------------------
### Python
```python
import urllib2
import json

command = {"command": "removeNeighbors", "uris": ["tcp://8.8.8.8:14265", "tcp://8.8.8.8:14265"]}

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

var command = {"command": "removeNeighbors", "uris": ["tcp://8.8.8.8:14265", "tcp://8.8.8.8:14265"]}

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
-d '{"command": "removeNeighbors", "uris": ["tcp://8.8.8.8:14265", "tcp://8.8.8.8:14265"]}'
```
--------------------

### レスポンス例
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

### 結果

| **リターンフィールド** | **説明** |
|--|--|
| `removedNeighbors` | 削除された隣接ノードの総数 |
| `duration` | リクエストを完了するのにかかったミリ秒数 |

## storeTransactions

トランザクションをノードのローカルストレージに格納します。
<!-- Store transactions in a node's local storage. -->

### パラメータ

`trytes`パラメータの値は有効でなければなりません。有効なトライトは[`attachToTangle`](#attachToTangle)エンドポイントによって返されます。
<!-- The value of the `trytes` parameter must be valid. Valid trytes are returned by the [`attachToTangle`](#attachToTangle) endpoint. -->

| **パラメータ** | **必須または任意** | **説明** | **タイプ** |
|--|--|--|--|
| `trytes` | 必須 | トランザクショントライト | stringの配列 |

### 例
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

### レスポンス例
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

### 結果

| **リターンフィールド** | **説明** |
|--|--|
| `duration` | リクエストを完了するのにかかったミリ秒数 |

## wereAddressesSpentFrom

現在のエポックまたは以前のエポックのいずれかで、与えられたアドレスからIOTAトークンが取り出されたことがあるかどうかを確認します。
<!-- Check if an address was ever withdrawn from, either in the current epoch or in any previous epochs. -->

アドレスにペンディング中のトランザクションがある場合も「使用済み（署名済み）」と見なされます。
<!-- If an address has a pending transaction, it's also considered 'spent'. -->

### パラメータ

| **パラメータ** | **必須または任意** | **説明** | **タイプ** |
|--|--|--|--|
| `addresses` | 必須 | チェックするアドレス（チェックサムを含まない） | stringの配列 |

### 例
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

### レスポンス例
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

### 結果

| **リターンフィールド** | **説明** |
|--|--|
| `states` | `addresses`パラメータの値と同じ順序で指定されたアドレスの状態。`true`は、アドレスからIOTAトークンが取り出されたことがあることを意味します。 |
| `duration` | リクエストを完了するのにかかったミリ秒数 |
