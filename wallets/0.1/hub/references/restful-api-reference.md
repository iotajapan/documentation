# RESTful APIリファレンス
<!-- # RESTful API reference -->

**Hubには、タングルおよびハブデータベースへの接続を簡素化するRESTful APIがあります。このAPIリファレンスを使用して、エンドポイントを見つけ、それらの機能を学習します。**
<!-- **Hub has a RESTful API that simplifies connections to the Tangle and the Hub database. Use this API reference to find endpoints and learn what they do.** -->

:::info:
RESTful APIを使用する場合は、`--serverType http`コマンドラインフラグでハブを起動する必要があります。
:::
<!-- :::info: -->
<!-- If you want to use the RESTful API, you must start Hub with the `--serverType http` command-line flag. -->
<!-- If you want to use the RESTful API, you must start Hub with the `--serverType http` command-line flag. -->
<!-- ::: -->

以下のコマンドには、HTTPヘッダーを含める必要があります。
<!-- All the following commands must include an HTTP header. -->
<!-- The following commands must include an HTTP header. -->

| **ヘッダー** | **値** | **必須か任意か** |
| :------- | :--- | :---------- |
| X-IOTA-API-Version | 1 | 必須 |
| Content-Type | application/json | 任意 |

<!-- | **Header**       | **Value** | **Required or Optional** | -->
<!-- |:---------------|:--------|:--------| -->
<!-- | X-IOTA-API-Version | 1 | Required | -->
<!-- | Content-Type | application/json | Optional | -->

:::warning:
このAPIはベータ版であり、変更される可能性があります。実稼働アプリケーションではこのAPIを使用しないことをお勧めします。
:::
<!-- :::warning: -->
<!-- This API is in beta, and is subject to change. We recommend that you don't use this API in production applications. -->
<!-- ::: -->

## CreateUser

ハブで新しいユーザーを作成します。
<!-- Create a new user on Hub. -->

### パラメーター
<!-- ### Parameters -->

| **パラメーター** | **必須か任意か** | **説明** | **タイプ** |
| :----------- | :----------- | :--- | :----- |
| `userId` | 必須 | ユーザーの一意のID | string |

<!-- |**Parameter** | **Required or Optional**|**Description** | **Type**| -->
<!-- |--|--|--|--| -->
<!-- | `userId` | Required|A unique ID for the user | string| -->

### 例
<!-- ### Examples -->
--------------------
### Python
```python
import urllib2
import json

command = {
  "command": "CreateUser",
  "userId": "user-1"
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:50051", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
  "command": "CreateUser",
  "userId": "user-1"
};

var options = {
  url: 'http://localhost:50051',
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
### cURL
```bash
curl http://localhost:50051 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
  "command": "CreateUser",
  "userId": "user-1"
}'
```
--------------------

### レスポンス例
<!-- ### Response examples -->
--------------------
### 200
```json
{}
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

空のオブジェクトが成功した結果として返されます。
<!-- An empty object is returned in a successful result. -->

## GetAddressInfo

与えられた預け入れアドレスを所有しているユーザーのIDを取得します。
<!-- Get the ID of the user that owns a given deposit address. -->

### パラメーター
<!-- ### Parameters -->

| **パラメーター** | **必須か任意か** | **説明** | **タイプ** |
| :----------- | :----------- | :--- | :----- |
| `address` | 必須 | 81トライとの預け入れアドレス（チェックサム無し） | string |

<!-- |**Parameter** |**Required or Optional** |**Description** |**Type**| -->
<!-- |--|--|--|--| -->
<!-- | `address` |Required| The 81-tryte deposit address (without checksum) | string| -->

### 例
<!-- ### Examples -->
--------------------
### Python
```python
import urllib2
import json

command = {
"command": "GetAddressInfo",
"address": "PHWYPQECJDVEZYQFIDNMEDFGETLTRUFUERVUYQQLZHOHKQZU9QLLCGLNANXNGGXNTZLBUAALRLH9PIGHF"
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:50051", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
"command": "GetAddressInfo",
"address": "PHWYPQECJDVEZYQFIDNMEDFGETLTRUFUERVUYQQLZHOHKQZU9QLLCGLNANXNGGXNTZLBUAALRLH9PIGHF"
};

var options = {
  url: 'http://localhost:50051',
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
### cURL
```bash
curl http://localhost:50051 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
"command": "GetAddressInfo",
"address": "PHWYPQECJDVEZYQFIDNMEDFGETLTRUFUERVUYQQLZHOHKQZU9QLLCGLNANXNGGXNTZLBUAALRLH9PIGHF"
}
}'
```
--------------------

### レスポンス例
<!-- ### Response examples -->
--------------------
### 200
```json
{
  "userId": "user-1"
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
| :----------------- | :--- |
| `userId` | 預け入れアドレスを所有するユーザーのID |

<!-- |**Return field** | **Description** | -->
<!-- |--|--| -->
<!-- | `userId` | The ID of the user that owns the deposit address| -->

## GetBalance

ユーザーの利用可能な残高を取得します。
<!-- Get a user's available balance. -->

### パラメーター
<!-- ### Parameters -->

| **パラメーター** | **必須か任意か** | **説明** | **タイプ** |
| :----------- | :----------- | :--- | :----- |
| `userId` | 必須 | ユーザーのID | string |

<!-- |**Parameters** |**Required or Optional** |**Description** |**Type** -->
<!-- |--|--|--|--| -->
<!-- | `userId` |Required| The ID of the user | string -->

### 例
<!-- ### Examples -->
--------------------
### Python
```python
import urllib2
import json

command = {
  "command": "GetBalance",
  "userId": "user-1"
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:50051", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
  "command": "GetBalance",
  "userId": "user-1"
  }

var options = {
  url: 'http://localhost:50051',
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
### cURL
```bash
curl http://localhost:50051 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
  "command": "GetBalance",
  "userId": "user-1"
  }'
```
--------------------

### レスポンス例
<!-- ### Response examples -->
--------------------
### 200
```json
{
  "available": 1000
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
| :----------------- | :--- |
| `available` | ユーザーがハブで利用できるIOTAトークンの合計量 |

<!-- |**Return field**  |**Description** | -->
<!-- |--|--| -->
<!-- | `available` | The total amount of IOTA tokens that a user has available on Hub | -->

## GetDepositAddress

与えられたユーザーの新しい預け入れアドレスを作成します。
<!-- Create a new deposit address for a given user. -->

### パラメーター
<!-- ### Parameters -->

| **パラメーター** | **必須か任意か** | **説明** | **タイプ** |
| :----------- | :----------- | :--- | :----- |
| `userId` | 必須 | ユーザーのID | string |
|`includeChecksum`| 任意 | アドレスの最後に9トライとのチェックサムを含めるかどうか | boolean |

<!-- |**Parameter** | **Required or Optional**|**Description** |**Type** -->
<!-- |--|--|--|--| -->
<!-- | `userId` |Required| The ID of the user | string| -->
<!-- |`includeChecksum`|Optional|Whether to include the 9-tryte checksum at the end of the address|boolean -->

### 例
<!-- ### Examples -->
--------------------
### Python
```python
import urllib2
import json

command = {
  "command": "GetDepositAddress",
  "userId": "user-1"
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:50051", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
  "command": "GetDepositAddress",
  "userId": "user-1"
};

var options = {
  url: 'http://localhost:50051',
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
### cURL
```bash
curl http://localhost:50051 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
"command": "GetDepositAddress",
"userId": "user-1"
}'
```
--------------------

### レスポンス例
<!-- ### Response examples -->
--------------------
### 200
```json
{
"address": "PHWYPQECJDVEZYQFIDNMEDFGETLTRUFUERVUYQQLZHOHKQZU9QLLCGLNANXNGGXNTZLBUAALRLH9PIGHF"
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
| :----------------- | :--- |
| `address` | 新しい81トライトの預け入れアドレス（チェックサム無し） |

<!-- |**Return field** | **Description** | -->
<!-- |--|--| -->
<!-- | `address` | A new 81-tryte deposit address (without checksum) | -->

## GetStats

ハブに保存されているIOTAトークンの合計量を取得します。
<!-- Get the total amount of IOTA tokens that are stored in Hub. -->

### 例
<!-- ### Examples -->
--------------------
### Python
```python
import urllib2
import json

command = {
  "command": "GetStats"
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:50051", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
  "command": "GetStats"
};


var options = {
  url: 'http://localhost:50051',
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
### cURL
```bash
curl http://localhost:50051 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
  "command": "GetStats"
}'
```
--------------------

### レスポンス例
<!-- ### Response examples -->
--------------------
### 200
```json
{
  "totalBalance": 10000
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
| :----------------- | :--- |
| `totalBalance` | ハブ所有者のアカウントに保存されているIOTAトークンの合計量 |

<!-- |**Return field** | **Description** | -->
<!-- |--|--| -->
<!-- | `totalBalance` | The total amount of IOTA tokens that are stored in the Hub owner's account| -->

## GetUserHistory

ユーザーの残高の履歴を取得します。
<!-- Get the history of a user's balance. -->

### パラメーター
<!-- ### Parameters -->

| **パラメーター** | **必須か任意か** | **説明** | **タイプ** |
| :----------- | :----------- | :--- | :----- |
| `userId` | 必須 | ユーザーのID | string |
| `newerThan` | 任意 | ユーザーの履歴を取得する日時。値が`0`の場合、ハブは履歴全体を取得します。 | Unix timestamp |

<!-- |**Parameter** | **Required or Optional**|**Description** |**Type** -->
<!-- |--|--|--|--| -->
<!-- | `userId` |Required| The ID of the user | string| -->
<!-- | `newerThan` |Optional| The time and date from which to get the user's history. A `0` value means that Hub gets the entire history.|Unix timestamp -->

### 例
<!-- ### Examples -->
--------------------
### Python
```python
import urllib2
import json

command = {
    "command": "GetUserHistory",
    "userId": "user-1"
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:50051", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
    "command": "GetUserHistory",
    "userId": "user-1"
}

var options = {
  url: 'http://localhost:50051',
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
### cURL
```bash
curl http://localhost:50051 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{"command": "GetUserHistory",
    "userId": "user-1"
}'
```
--------------------

### レスポンス例
<!-- ### Response examples -->
--------------------
### 200
```json
{
   "event_0": {
        "userID": "user-1",
        "timestamp": "1563796442000",
        "amount": "1000",
        "reason": "DEPOSIT",
        "sweepBundleHash": "EWLCW9FFLSBUGZZOOLFQLTRJFKNGPUVCIOCQYTSDOSZLBCBJIIJZGPALGAKKANBTDYOBVQFOJHA9OVFOY",
        "withdrawalUuid": ""
    },
    "event_1": {
        "userID": "user-1",
        "timestamp": "1563796562000",
        "amount": "-1",
        "reason": "WITHDRAWAL",
        "sweepBundleHash": "",
        "withdrawalUuid": "4782e7d5-9ce4-477d-8fd0-32f5f3385db2"
    },
    "event_2": {
        "userID": "user-1",
        "timestamp": "1563796604000",
        "amount": "1",
        "reason": "WITHDRAWAL_CANCELED",
        "sweepBundleHash": "",
        "withdrawalUuid": "4782e7d5-9ce4-477d-8fd0-32f5f3385db2"
    }
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
| :----------------- | :--- |
| `userId` | アカウントの残高が変更されたユーザーのID |
| `timestamp` | エポックからの残高変更が発生したミリ秒単位の時間 |
| `reason` | [アカウント残高の変更を引き起こしたイベント](#balance-change-events-for-user-accounts)の種類 |
| amount | 残高へと変更した量 |
| `sweepBundleHash` | `DEPOSIT`イベントのバンドルハッシュが含まれます。 |
| `withdrawalUuid` | `WITHDRAWAL`または`WITHDRAWAL_CANCELED`イベントの取り出しUUIDが含まれます。 |

<!-- |**Return field** | **Description** | -->
<!-- | `userId`          | ID of the user whose account's balance changed | -->
<!-- | `timestamp`       | Time since epoch in milliseconds that the balance change occured| -->
<!-- |`reason`|  The type of [event that caused a change to the account balance](#balance-change-events-for-user-accounts)    | -->
<!-- | amount          | Amount that changed to the balance     | -->
<!-- | `sweepBundleHash` | Contains either the bundle hash for a `DEPOSIT` event| -->
<!-- |`withdrawalUuid`| Contains a withdrawal UUID for a `WITHDRAWAL` or `WITHDRAWAL_CANCELED` event -->

## ProcessTransferBatch

取引所からの購入/販売のバッチを処理します。
<!-- Process a batch of buys/sells from the exchange. -->

:::info:
バッチの合計額は0になる必要があります。
:::
<!-- :::info: -->
<!-- The total amount of a batch must sum to 0. -->
<!-- ::: -->

:::info:
このエンドポイントは、ハブデータベース内のユーザーの残高に影響します。トランザクションはタングルに送信されません。
:::
<!-- :::info: -->
<!-- This endpoint affects users' balances in the Hub database. No transactions are sent to the Tangle. -->
<!-- ::: -->

### パラメーター
<!-- ### Parameters -->

| **パラメーター** | **必須か任意か** | **説明** | **タイプ** |
| :----------- | :----------- | :--- | :----- |
| `userId` | 必須 | 転送中に残高を更新するユーザーのID | string |
| `amount` | 必須 | ユーザーの残高に加算または減算するIOTAトークンの量 | integer |

<!-- |**Parameters** |**Required or Optional**|**Description** |**Type** -->
<!-- |--|--|--|--| -->
<!-- | `userId` |Required| The ID of the user whose balance you want to update during the transfer |string| -->
<!-- | `amount` |Required| The amount of IOTA tokens to add or subtract from the user's balance  | integer| -->

### 例
<!-- ### Examples -->
--------------------
### Python
```python
import urllib2
import json

command = {
 "command": "ProcessTransferBatch",
 "transfers": [{"userId": "user-1","amount": -1},{"userId": "user-2","amount": 1}]
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:50051", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
    "command": "ProcessTransferBatch",
    "transfers": [{"userId": "user-1","amount": -1},{"userId": "user-2","amount": 1}]
};

var options = {
  url: 'http://localhost:50051',
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
### cURL
```bash
curl http://localhost:50051 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
  "command": "ProcessTransferBatch",
 "transfers": [{"userId": "user-1","amount": -1},{"userId": "user-2","amount": 1}]
}'
```
--------------------

### レスポンス例
<!-- ### Response examples -->
--------------------
### 200
```json
{}
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

空のオブジェクトが成功した結果で返されます。
<!-- An empty object is returned in a successful result. -->

## BalanceSubscription

特定の時間以降の残高の変化のストリームを監視します。
<!-- Monitor a stream of balance changes since a given time. -->

### パラメーター
<!-- ### Parameters -->

| **パラメーター** | **必須か任意か** | **説明** | **タイプ** |
| :----------- | :----------- | :--- | :----- |
| `newerThan` | 必須 | バランスの変更を監視する開始日時。`0`値は、ハブがすべての残高変更を取得することを意味します。 | Unix timestamp |

<!-- |**Parameters** |**Required or Optional**|**Description** |**Type**| -->
<!-- |--|--|--|--| -->
<!-- | `newerThan` |Required| The time and date from which to start monitoring balance changes. A `0` value means that Hub gets all balance changes.|Unix timestamp -->

### 例
<!-- ### Examples -->
--------------------
### Python
```python
import urllib2
import json

command = {
  "command": "BalanceSubscription",
  "newerThan": 1563796441000
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:50051", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
  "command": "BalanceSubscription",
  "newerThan": 1563796441000
}

var options = {
  url: 'http://localhost:50051',
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
### cURL
```bash
curl http://localhost:50051 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
  "command": "BalanceSubscription",
  "newerThan": 1563796441000
}'
```
--------------------

### レスポンス例
<!-- ### Response examples -->
--------------------
### 200
```json
{
    "event_0": {
        "type": "USER_ACCOUNT",
        "reason": "DEPOSIT",
        "userId": "user-1",
        "timestamp": "1563796442000",
        "sweepBundleHash": "EWLCW9FFLSBUGZZOOLFQLTRJFKNGPUVCIOCQYTSDOSZLBCBJIIJZGPALGAKKANBTDYOBVQFOJHA9OVFOY",
        "withdrawalUuid": "",
	"amount":1000
    },
    "event_1": {
        "type": "USER_ACCOUNT",
        "reason": "WITHDRAWAL",
        "userId": "user-1",
        "timestamp": "1563796562000",
        "sweepBundleHash": "",
        "withdrawalUuid": "4782e7d5-9ce4-477d-8fd0-32f5f3385db2",
	"amount":1000
    },
    "event_2": {
        "type": "USER_ACCOUNT",
        "reason": "WITHDRAWAL_CANCELED",
        "userId": "user-1",
        "timestamp": "1563796604000",
        "sweepBundleHash": "",
        "withdrawalUuid": "4782e7d5-9ce4-477d-8fd0-32f5f3385db2",
	"amount":1000
    },
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

`type`フィールドの値に応じて、次のデータが返されます：
<!-- Depending on the value of the `type` field, the following data is returned: -->

#### USER_ACCOUNT

| **リターンフィールド** | **説明** |
| :----------------- | :--- |
| `type` | `USER_ACCOUNT`タイプは、ユーザーのアカウントの残高を変更するためのものです。 |
| `reason` | [アカウント残高の変更を引き起こしたイベント](#balance-change-events-for-user-accounts)の種類 |
| `userId` | アカウントの残高が変更されたユーザーのID |
| `timestamp` | エポックからの残高変更が発生したミリ秒単位の時間 |
| amount | 残高に変更した量 |
| `sweepBundleHash` | `DEPOSIT`イベントのバンドルハッシュが含まれます。 |
| `withdrawalUuid` | `WITHDRAWAL`または`WITHDRAWAL_CANCELED`イベントの取り出しUUIDが含まれます。 |

<!-- |**Return field** | **Description** | -->
<!-- | ----------- | ------------------------------------------------------------ | -->
<!-- | ----------- | ------------------------------------------------------------ | -->
<!-- |`type`|The `USER_ACCOUNT` type is for changes to the balance of a user's account| -->
<!-- |`reason`|  The type of [event that caused a change to the account balance](#balance-change-events-for-user-accounts)    | -->
<!-- | `userId`          | ID of the user whose account's balance changed | -->
<!-- | `timestamp`       | Time since epoch in milliseconds that the balance change occured| -->
<!-- | amount          | Amount that changed to the balance     | -->
<!-- | `sweepBundleHash` | Contains either the bundle hash for a `DEPOSIT` event| -->
<!-- |`withdrawalUuid`| Contains a withdrawal UUID for a `WITHDRAWAL` or `WITHDRAWAL_CANCELED` event| -->

#### USER_ADDRESS

| **リターンフィールド** | **説明** |
| :----------------- | :--- |
| `type` |`USER_ADDRESS`タイプは、ユーザーの預け入れアドレスの1つを変更するためのものです。 |
| `userId` | アドレスの残高が変更されたユーザーのID |
| `userAddress` | 残高が変更されたアドレス |
| `amount` | 残高に変更した量 |
| `reason` | [ユーザーのアドレスの残高の変更を引き起こしたイベント](#balance-change-events-for-user-addresses)のタイプ |
| `hash` | `DEPOSIT`理由の末尾トランザクションハッシュまたは`SWEEP`理由のバンドルハッシュが含まれます。 |
| timestamp | エポックからの残高変更が発生したミリ秒単位の時間 |

<!-- |**Return Field**|**Description**| -->
<!-- | ----------- | ------------------------------------------------------------ | -->
<!-- | ----------- | ------------------------------------------------------------ | -->
<!-- |`type`|The `USER_ADDRESS` type is for changes to one of a user's deposit address| -->
<!-- | `userId`      | The ID of the user whose address's balance changed  | -->
<!-- | `userAddress` | Address whose balance was changed | -->
<!-- | `amount`      |Amount that changed to the balance | -->
<!-- | `reason`      | The type of [event that caused a change to the balance of the user's address](#balance-change-events-for-user-addresses) | -->
<!-- | `hash`       | Contains either a tail transaction hash for a `DEPOSIT` reason or a bundle hash for a `SWEEP` reason | -->
<!-- | timestamp   | Time since epoch in milliseconds that the balance change occured| -->

#### HUB_ADDRESS

| **リターンフィールド** | **説明** |
| :----------------- | :--- |
| `type` | `HUB_ADDRESS`タイプは、ハブの所有者のアドレスの1つを変更するためのものです。 |
| `hubAddress`      | ハブ所有者の81トライトのアドレス（チェックサムなし） |
| `amount`          | 残高に変更した量 |
| `reason`          | [ハブアドレスの残高の変化を引き起こしたイベント](#balance-change-events-for-hub-addresses)のタイプ |
| `sweepBundleHash` | 更新された残高になったスイープのバンドルハッシュ |
| `timestamp`       | エポックからの残高変更が発生したミリ秒単位の時間 |

<!-- |**Field**|**Description**| -->
<!-- | --------------- | ----------------------------------------------------------- | -->
<!-- | `type` |The `HUB_ADDRESS` type is for changes to one of the Hub owner's addresses| -->
<!-- | `hubAddress`      | Hub owner's 81-tryte address  (without checksum)         | -->
<!-- | `amount`          | Amount that changed to the balance       | -->
<!-- | `reason`          | The type of [event that caused a change to the balance of the Hub address balance](#balance-change-events-for-hub-addresses)         | -->
<!-- | `sweepBundleHash` | Bundle hash of the sweep that resulted in the updated balance          | -->
<!-- | `timestamp`       | Time since epoch in milliseconds that the balance change occured          | -->

## RecoverFunds

IOTAトークンを使用済みのアドレスから未使用のアドレスに転送します。
<!-- Transfer IOTA tokens from a spent address to an unspent one. -->

:::info:
このエンドポイントを使用するには、[`--RecoverFunds_enabled`フラグ](../references/command-line-options.md#recoverFunds)を指定してハブを実行する必要があります。
:::
<!-- :::info: -->
<!-- To use this endpoint, you must run Hub with the [`--RecoverFunds_enabled` flag](../references/command-line-options.md#recoverFunds). -->
<!-- ::: -->

:::info:
トークンの転送先をさらに制御したい場合は、[`SignBundle`エンドポイント](#SignBundle)を使用します。
:::
<!-- :::info: -->
<!-- If you want more control over where the tokens are transferred, use the [`SignBundle` endpoint](#SignBundle). -->
<!-- ::: -->

### パラメーター
<!-- ### Parameters -->

| **パラメーター** | **必須か任意か** | **説明** | **タイプ** |
| :----------- | :----------- | :--- | :----- |
| `userId` | 必須 | 使用済みアドレスから資金をリカバーしたいユーザーのID | string |
| `address` | 必須| ユーザーの使用済みアドレス（チェックサムなし） | string |
| `validateChecksum` | 必須 | アドレスを検証するかどうか。`payoutAddress`フィールドが90トライアドレス（チェックサム付き）の場合、このフィールドを`true`に設定します。 | boolean |
| `payoutAddress` | 必須 | `address`フィールドのアドレスの合計残高の転送先アドレス（チェックサムを含む場合があります） | string |

<!-- |**Parameters** |**Required or Optional**|**Description** |**Type**| -->
<!-- |--|--|--|--| -->
<!-- | `userId` | Required | The ID of the user whose spent address you want to recover the funds from          | string| -->
<!-- | `address` | Required| The user's spent address (without checksum)   |string| -->
<!-- | `validateChecksum` | Required| Whether to validate the address. Set this field to `true` if the `payoutAddress` field is a 90-tryte address (with checksum) |boolean| -->
<!-- | `payoutAddress` | Required|Address to which to transfer the total balance of the address in the `address` field (may include checksum)       |string -->

### 例
<!-- ### Examples -->
--------------------
### Python
```python
import urllib2
import json

command = {
    "command": "RecoverFunds",
    "userId" : "user-1",
    "address":"PHWYPQECJDVEZYQFIDNMEDFGETLTRUFUERVUYQQLZHOHKQZU9QLLCGLNANXNGGXNTZLBUAALRLH9PIGHF",
    "payoutAddress": "LEYNSIMADMXAUYRGXKKEXPHDMZLRISZBSRZXUMCIKP9JQDOXSCIUGKYFFNPPVPGCHEJAWWSDHCKGOORPCX9WQZZEHY",
    "validateChecksum": "true"
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:50051", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
    "command": "RecoverFunds",
    "userId" : "user-1",
    "address":"PHWYPQECJDVEZYQFIDNMEDFGETLTRUFUERVUYQQLZHOHKQZU9QLLCGLNANXNGGXNTZLBUAALRLH9PIGHF",
    "payoutAddress": "LEYNSIMADMXAUYRGXKKEXPHDMZLRISZBSRZXUMCIKP9JQDOXSCIUGKYFFNPPVPGCHEJAWWSDHCKGOORPCX9WQZZEHY",
    "validateChecksum": "true"
}

var options = {
  url: 'http://localhost:50051',
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
### cURL
```bash
curl http://localhost:50051 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{"command": "RecoverFunds",
    "userId" : "user-1",
    "address":"PHWYPQECJDVEZYQFIDNMEDFGETLTRUFUERVUYQQLZHOHKQZU9QLLCGLNANXNGGXNTZLBUAALRLH9PIGHF",
    "payoutAddress": "LEYNSIMADMXAUYRGXKKEXPHDMZLRISZBSRZXUMCIKP9JQDOXSCIUGKYFFNPPVPGCHEJAWWSDHCKGOORPCX9WQZZEHY",
    "validateChecksum": "true"
}'
```
--------------------

### レスポンス例
<!-- ### Response examples -->
--------------------
### 200
```json
{}
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

空のオブジェクトが成功した結果で返されます。
<!-- An empty object is returned in a successful result. -->

## SignBundle

署名を取得して、未署名のバンドルに追加します。
<!-- Get a signature to add to an unsigned bundle. -->

このエンドポイントは、使用済みの預け入れアドレスから2つ以上の未使用アドレスにIOTAトークンを転送するビルド済みバンドルに署名するのに役立ちます。
<!-- This endpoint is useful for signing pre-built bundles that transfer IOTA tokens from a spent deposit address to two or more unspent addresses. -->

使用済みアドレスのすべてのIOTAトークンを単一のアドレスに転送する場合は、[`RecoverFunds`エンドポイント](#RecoverFunds)を使用します。
<!-- If you want to transfer all the IOTA tokens of a spent address into a single address, use the [`RecoverFunds` endpoint](#RecoverFunds). -->

このエンドポイントの使用方法の例については、[バンドルを構築し、ハブでバンドルに署名する](../how-to-guides/recover-tokens.md)を参照してください。
<!-- For an example of how to use this endpoint, follow our guide to [build a bundle and sign it with Hub](../how-to-guides/recover-tokens.md). -->

:::info:
このエンドポイントを使用するには、[`--SignBundle_enabled`フラグ](../references/command-line-options.md#signBundle)を指定してハブを実行する必要があります。
:::
<!-- :::info: -->
<!-- To use this endpoint, you must run Hub with the [`--SignBundle_enabled` flag](../references/command-line-options.md#signBundle). -->
<!-- ::: -->

### パラメーター
<!-- ### Parameters -->

| **パラメーター** | **必須か任意か** | **説明** | **タイプ** |
| :----------- | :----------- | :--- | :----- |
| `address`          | 必須 | 取り出したいユーザーの預け入れアドレス（チェックサムを含む場合があります） | string |
| `bundleHash`       | 必須 | 署名が必要なバンドルハッシュ | string |
| `authentication`   | 任意 | エンドポイントのHMACキー | string |
| `validateChecksum` | 任意 | アドレスを検証するかどうか。`address`フィールドが90トライアドレス（チェックサム付き）の場合、このフィールドを`true`に設定します。 |

<!-- |**Parameters** |**Required or Optional**|**Description** |**Type**| -->
<!-- |--|--|--|--| -->
<!-- | `address`          |The user's deposit address that you want to withdraw from (may include a checksum) |string| -->
<!-- | `bundleHash`       | The bundle hash that needs signing   |string| -->
<!-- | `authentication`   |Optional |HMAC key for the endpoint |string| -->
<!-- | `validateChecksum` |Whether to validate the address. Set this field to `true` if the `address` field is a 90-tryte address (with checksum) -->

### 例
<!-- ### Examples -->

--------------------
### Python
```python
import urllib2
import json

command = {
    "command": "SignBundle" ,
    "address" : "LIQJBJRBSTGYWHYRPCLLCZUMP9SLHCBBWGQ9YRFWYDFF9FMXIAELYLTTBXCPVIDWWZYIOJIFLUFYVZIBD",
    "bundleHash": "EGEDXKAOPIDYOZRFZWNH9VWKYULBQWAUDFHDZE9YFIXRZARLUIUGACCPYVWUYIRYKGIBLJYEDXBFUNKAW"
    "validateChecksum": "true"
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:50051", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
    "command": "SignBundle" ,
    "address" : "LIQJBJRBSTGYWHYRPCLLCZUMP9SLHCBBWGQ9YRFWYDFF9FMXIAELYLTTBXCPVIDWWZYIOJIFLUFYVZIBD",
    "bundleHash": "EGEDXKAOPIDYOZRFZWNH9VWKYULBQWAUDFHDZE9YFIXRZARLUIUGACCPYVWUYIRYKGIBLJYEDXBFUNKAW"
    "validateChecksum": "true"
}

var options = {
  url: 'http://localhost:50051',
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
### cURL
```bash
curl http://localhost:50051 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
    "command": "SignBundle" ,
    "address" : "LIQJBJRBSTGYWHYRPCLLCZUMP9SLHCBBWGQ9YRFWYDFF9FMXIAELYLTTBXCPVIDWWZYIOJIFLUFYVZIBD",
    "bundleHash": "EGEDXKAOPIDYOZRFZWNH9VWKYULBQWAUDFHDZE9YFIXRZARLUIUGACCPYVWUYIRYKGIBLJYEDXBFUNKAW"
    "validateChecksum": "true"
}'
```
--------------------

### レスポンス例
<!-- ### Response examples -->
--------------------
### 200
```json
{
  "signature": "DRFFHLSWJBLEBNHXMSLIGUKBQJIJRNHGMGVDFKKPF9DYABMQXDVLWPHXT9LEPSXVBIYKZEWCT9U99NJ9DQJVJJMBGCUHTZQBCYWEIN9DCSDPJXRKGQQBEHPBZZWRFNBIENYV9UNSSNXZHGATAWRTHEW9FQVTXUJLFCQEPLVWATFRXFAUSNFKVVYFISDULSVIQNEQVPNIKZSBJQHWAZMGOIPZSKY9QDVMCFXUS9UGUDK9BOZFAWXQFHFBEYUJBESIFMBWNXVFCKHTGXTWKLSMZ9FHYFUZKE9GTLFWXPCLFPQKCJQAZFSTGWJILTACIXLRRJWDGIGJOCTUCEBFLWFQBQKAHTJMAEWZSDMOTQ9GPALHYYRGTFYJCDRFKZFUPECKUEVZJPVQDYIDJAIHSFSWZSQLQIJSCKJPJPLFVHXLUMLZKUHJWRVPBIOESDIFCUCKZAGANIATOUCSF9IOUA9EQWCSOFMETZEADESOADJGLCDKUQIBEPMXBWNIHJMJHCYUSM9LXMEBTHORZDGYST9VJLUUSXTSAURNUZAUPNWIUSRVL9KGFCEIAIWTNIH9BIRCSLWDAPXYKSZAOFERYFGPBWYUIEEENGMMYVPRRRIXULRJWJCLUBBNMNLFMNPYDHVBFZUFRCD9PCZAQLDBJFFKGQJW9LFUUPIZPKUCBAJFUEUTMXKWUKFXKQISDXYIHEJSQTCEHVABAUWGGANWZCNBOLGZDCLXSZIUKRMFG9THEZTULVZ9CFGNUWCTQVSACVYSZASONJIGUSXZAZNVCBIWRHLOKCXPXFONAWNMWRMGAXXJNPYRNDKAKIEWNSDRSNTSTMDPUYNPSIJTWERECFKSNTWOKCMDIUEKEYXORVIQVQXKWOWNMVBLWVJIWBWKAHHECGLNZAADFVSATZDMLYXBGRCEATLTCMQACYQQDQOXXMSUWSZVZKEXQUYM9DZMMOMBIVCPJU9UUKAXOSQQX9QRPTFXSZPBZFFJLGOBVZZOT9LNWMA9LTRUZUJBDGH9GILESQLNYDYPCHHYICVMAPDVDILEOZDNZMVSERSDWLZ9ZXFKJXISY9BLXZZ9DJYIRPXNLGAWMDZOXOWBUDZRUROHKPREZJTXNSUNC9LDHGFLGYOXBAQVGYUZXECCKUUFOPSLPH9QRLAZUOWGBQBVEJBAZKUEWSUIVGLEULZKDXFQSGQNZYXXZCYPRZUSBPMVPLWMYDWEGXANNXLOTME9HQGQW9POKCSQWHLHUUUWNFFHXZYDYUYBMSJVWSAKMEFJDJNJZHIYP9NBLFWVKBSDIUXLAVLMYLLCRX9BZZZQEESERVDIHKKPVAM9UEOXR9CFW9OAXLCLMJHWEOSTCYYHPWHJZEKAXPBGNGVMCGNHOUWMJX9GEV9BBGYVNEG9AXSNPFEGOZNZWXMNXVV9CVJ9Z9DGLYOUWZKFJYIIWRUEYPGDFSJGRXZDS9RKFELFVMASCAPXVCPLNQRJHADACOOGGHOVTTG9DCOCMHTMTMNEVASXHKLBAWKNFTFDAEEHATXGUKBRLDXNLSHZEPO9OZQBSHJRLGLNPKHAX9WRMYARVJETYAPO9SUWZQHAKJAUWBIWQDSQ9FWIEWSJWKQGNIFGGVKZLWZZOVVBGPNRTONMZDLJHGWQUSCILYOBG9YUUUZDXILKBIQ9ZEKXEHURJLFMDB9CUZI9TEJSQZYHMNTX9F9GJBZ9UJNCKZVYAROOVZYZURRWKLYHJEMZDQLH9FWODFZUEZRV9DWNZCOMXNXFWCPCZQDGFP9DKJYUNXIYIRUVWRSWWIGKZWITSYIVEOPHBJGSZNYJSFTXAKLENHCPGOYQSGEZQMRVNFGAGZXFG9GYFIBJRVU9MYDSZSTJQVVODQJJEHYJYCHOHTCOPZJVGMBEIIGYFMNAPZATAHQGFVWSGYODTOHRE9FVYXSLESASDMMAPBZZVPLIWGIRORAGWNFA9JPSUWJCTNBYRDJANSYMNDGQJNSFQRSVMRIAXDWQONWSAYUYFXJUTWYLPXVV9YGJBMQUXODSEFAEGYXSYYEQTVRAWNCKKSWPRYYINMBOBNZJSANFLKDFMGTNUDU9SFQRBUJM9XDNWIECTJPEICLVXTMCMGQJT9BTRZ9HQCHHBYXSPJBGTSTJDIOUQ9SLRVHTENSCLLGBFMXOYFKYB9SAKWTPGHEXPU9ERFYGEMEPSLJUPRJNRRWQLA9XVSWBEGJHLLDXGDTESDPRBRQHLCIOPWBDYJVCLOHHDYFNQSXBOVUVICSVBMEZYJJZHGDKAHFRJHBDWUSFSGC9UCLVH9ZNVMPYMRMMJTSWAFDRGNALYLRRVPLGMMKGZR9LXEV9ZXRPFKYDBLIUEVEJKVDANOBKFLRSWQPMOMTEJA9WVHFGBNTAJIPWNUWKJMQAOKBCACLYGRCBZRVFKIOMCXYXGUTNODHMLOZRETEPDONTAPVQEQAWCAH9CSTPYRFUXPKBDVONWFNOZHA9N9YUTEIGAHAPEKNWGCXNAMVJVLPCAPDPNTVVSKROYWXDKVLOAHPCCJBGBPSMQEDFDXSGNMIMLEZPBVCKYKVZFFIZVECYQKQWDHXRVEAOL9QIDZK9JFBRYL9JOSAWMBZAXJWNBTLKRSIYHZEILOPRZVLOFACJGIPABPFZILVQMUAYMPABXEBDYOSYBZKUXCFVXUEQJJOUDMBZZGOBVBF9N9ENHSCTDVFASKXHOLLFYMMWSDFHJKHODXYPWMEOQZKIECIBHYXF9PXZOCBMIBI9PLVDVBGALAWBT9FLIZWRNENVLSUCSOATNVRUKTTWZNTZRCMTLALKYGOUKVSO9BBUDVUCFCIKQZWJIUBMXTSNQSFRETWAHPQ9MTYOUKX9BLA9QUUHPNLNINNIPWQVUPUCXHTMURNWYSUGOKWPYMZ9ILDJWEOD9D99YWKQTKKOOWQOLEZMTZ99AYQPWUVVZUNVORK9XRAE9GQSHLVIZLCEDWHSWYNVZKELOI9AVGUWFSJYHA9WZNVNZYFEZYJAXKUOPINHR9OEN9NVLNIDEJLPLVEMIANSGFXXXL9IKQAFLFPOPWO9SAEMNETZMUM9NEHAA9JOGLHOBHYHXASFWHGNKCRNWZSCDPIFGFHOFLWCOMNCPRBECDEVSNXGKNF9IOYIVZRBJJHCREKOFHMTXXALIWOGOZRIJITCWGKQKCVUPUOFNMEUGSVPYAKRORXQHUXJVCAEWJBLAPDDSCIYMODOMM9GWQSVQUBGBUUDVGSSHAONRHVMILHPMBHKKHUTRZFWNAEEUHCQUGHLSVXAMOGVTXELPGKXBHELPCGYRCLMYGIZYLANN9LSXCQVQPBLIZXJUVRTA9CQAFSWWTIBILERJDTYHJWDPOYHNCHJEWSVXEJCPZVNVLWWOAZHJLLTS9WAEXXSMWHITJYJLFHGGDFNYIGDKJFUZGSJDCXSVNZQYUSAPVJRHSRNLNQDLDBEFPLRXJ9MIZBPNBZVHHLOYC9VYLDKNJAHLNPRCSOZKHWHQKXYWXABCPHYNQYRZJEARZCOLQMBCJCHNNSFGORUOHFOFJMF9MJHHFFVBCS9QITDPGXTJKDDJKHGBOLEVMXRAWUMCXUABILEYVHRTZFFMAM9REMUPNEPUHKWFHOJEFBLXXYHI99UDFMXLXEFWQLPTKJJQEQAXYCQNCZMMOTSKGQCJNFCWGUISHWZLDWJBNNAFDDOMOJ9QRVDEXXNHLKVP9CQJSGZRT9CHDTUJIXWM9WB9RTYQ9EEIJWEVSGKQPFLKG9HKNWSGWZUAGOBENKKT9NUMUIAPWZ9UESGRDITJSOMJAPHIN9CJSSUJCAKCLTQPJOUEMKVLSZZ9MAEROPNSBHPHOFBVVOSNTWPSCRTAYQJQSG9YXPRMAHREBCQKMRFISYDXXNRMGEHCXFNH9SAGFOYAHFCQRNQFRXDIJAIBIRPBNVQZLZNJHBVCZHOIHJURHCFFVHFCPBSKGLQOCXKWXSFINGLYQAH9YGPFWJGQDOUIMFJJOFGNDREMEIMDKOEQONGHTVYWVXWHQQOJGCM9YEHBCVWNJLJXK9HQB9BDYTUJVTHYU9R9DPMUHTLB9NGFEEVKIUANTFHUQRRK9LSSFMUBZLKJTJLPQLBEUSVAFJURBSGFNMBZCDKEUTCNYHBZPUCZGQDAP9JISCORGZVTMLCFRDHKJBCEOYOHEHUDTHMGHMFDTPKGNQPCIBTSOCISWKFSMPDVUKPAWVACALATUSWJTJPXHJWUFDEICUNUDSHGYEGRPMDRABLMFCHXNUA9LHNUMDKXH99XGJKU9XMUXFOXKWLI9AFNDD9CSETSFB9MLEMEY9UMHQQYROBZZEBGHRQRERMHILEVEGNOBSFUIIIJZHRFPOBXHGRSXPYEC"
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
| :----------------- | :--- |
| `signature` | バンドル内の入力トランザクションの`signatureMessageFragment`フィールドに含める署名 |

<!-- |**Return field**| **Description** | -->
<!-- |--|--| -->
<!-- | `signature` | The signature to include in the `signatureMessageFragment` field of the input transaction in the bundle| -->

## SweepDetail

スイープに関する情報（確定ステータス、トランザクショントライト、および再添付）を取得します。
<!-- Get information about a sweep (confirmation status, transaction trytes, and reattachments). -->

### パラメーター

| **パラメーター** | **必須か任意か** | **説明** | **タイプ** |
| :----------- | :----------- | :--- | :----- |
| `bundleHash` | スウィープのバンドルハッシュ | string |

<!-- |**Parameters** |**Required or Optional**|**Description** |**Type**| -->
<!-- |--|--|--|--| -->
<!-- | `bundleHash`       | The bundle hash of the sweep   |string| -->

### 例
<!-- ### Examples -->
--------------------
### Python
```python
import urllib2
import json

command = {
    "command": "SweepDetail",
    "bundleHash" : "EWLCW9FFLSBUGZZOOLFQLTRJFKNGPUVCIOCQYTSDOSZLBCBJIIJZGPALGAKKANBTDYOBVQFOJHA9OVFOY"
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:50051", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
    "command": "SweepDetail",
    "bundleHash" : "EWLCW9FFLSBUGZZOOLFQLTRJFKNGPUVCIOCQYTSDOSZLBCBJIIJZGPALGAKKANBTDYOBVQFOJHA9OVFOY"
}

var options = {
  url: 'http://localhost:50051',
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
### cURL
```bash
curl http://localhost:50051 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
    "command": "SweepDetail",
    "bundleHash" : "EWLCW9FFLSBUGZZOOLFQLTRJFKNGPUVCIOCQYTSDOSZLBCBJIIJZGPALGAKKANBTDYOBVQFOJHA9OVFOY"
}'
```
--------------------

### レスポンス例
<!-- ### Response examples -->
--------------------
### 200
```json
{
    "confirmed": "false",
    "trytes": "...SOMETRANSACTIONTRYTES...",
    "trytes": "...SOMETRANSACTIONTRYTESFORREATTACHMENTTRANSACTION...",
    "tailHash": "G9POQLBHRQZOZUV9XEROHSANPAIMHUQORSFQFRJM9JGUXHOHORDFWNTNUDWDCKXPUPJKXZDEBHXDDN999",
    "tailHash": "EOBRJDZTYTAXIMSEPXUWPZJBRMPJENHCFXLJGSHGIESTCQOGSGQLNOKTDYE9VKCAAGGINGKTEDYWYD999",
    "tailHash": "ATVRPYELRHPUHBAL9CSTQEJFEQCSENDNGF9AXHEFCYFJTAMQFUPPPPSVFXXXHPTQCAJJCEYJGVBARW999"
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
| :----------------- | :--- |
| `confirmed` | スウィープの確定ステータス |
| `trytes`    | 最初のバンドルから始まり、最新の再添付バンドルで終わるトランザクショントライト。 |
| `tailHash`  | スウィープの末尾トランザクションハッシュ（再添付された各スウィープは、新しい末尾トランザクションハッシュになります） |

<!-- |**Return field** | **Description** | -->
<!-- |--|--| -->
<!-- | `confirmed` | The sweep's confirmation status | -->
<!-- | `trytes`    | The transactions trytes, starting from the first bundle and ending with the latest reattachment bundle                            | -->
<!-- | `tailHash`  | The sweep's tail transaction hashes (each reattached sweep results in a new tail transaction hash) | -->

## SweepInfo

スウィープに含まれていた取り出しUUIDのリストを取得するには、次のパラメーターを使用します。
<!-- To get a list of withdrawal UUIDs that were included in a sweep, use the following parameters: -->

| **パラメーター** | **必須か任意か** | **説明** | **タイプ** |
| :----------- | :----------- | :--- | :----- |
| `requestByUuid` | 必須 | 取り出しUUIDでこのエンドポイントを呼び出すかどうか | boolean |
| `withdrawalUuid` | 必須 | スウィープへの包含を確認するための取り出しUUID | string |

<!-- |**Parameters** |**Required or Optional**|**Description** |**Type**| -->
<!-- |--|--|--|--| -->
<!-- |`requestByUuid`|Required|Whether you are calling this endpoint with a withdrawal UUID|boolean| -->
<!-- | `withdrawalUuid`       | Required|The withdrawal UUID to check for inclusion in a sweep   |string| -->

特定の取り出しを実行したスウィープのバンドルハッシュを取得するには、次のパラメーターを使用します。
<!-- To get the bundle hash of the sweep that actioned a given withdrawal, use the following parameters: -->

| **パラメーター** | **必須か任意か** | **説明** | **タイプ** |
| :----------- | :----------- | :--- | :----- |
|`requestByUuid`| 必須 | 取り出しUUIDでこのエンドポイントを呼び出すかどうか | boolean |
| `bundleHash`     | 必須 | 取り出しを確認するスイープのバンドルハッシュ | string |

<!-- |**Parameters** |**Required or Optional**|**Description** |**Type**| -->
<!-- |--|--|--|--| -->
<!-- |`requestByUuid`|Required|Whether you are calling this endpoint with a withdrawal UUID|boolean| -->
<!-- | `bundleHash`     |Required| The bundle hash of the sweep to check for withdrawals| string -->

### 例
<!-- ### Examples -->
--------------------
### Python
```python
import urllib2
import json

command = {
    "command": "SweepInfo",
    "requestByUuid": "false",
    "bundleHash" : "EWLCW9FFLSBUGZZOOLFQLTRJFKNGPUVCIOCQYTSDOSZLBCBJIIJZGPALGAKKANBTDYOBVQFOJHA9OVFOY"
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:50051", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
    "command": "SweepInfo",
    "requestByUuid": "false",
    "bundleHash" : "EWLCW9FFLSBUGZZOOLFQLTRJFKNGPUVCIOCQYTSDOSZLBCBJIIJZGPALGAKKANBTDYOBVQFOJHA9OVFOY"
}

var options = {
  url: 'http://localhost:50051',
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
### cURL
```bash
curl http://localhost:50051 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
    "command": "SweepInfo",
    "requestByUuid": "false",
    "bundleHash" : "EWLCW9FFLSBUGZZOOLFQLTRJFKNGPUVCIOCQYTSDOSZLBCBJIIJZGPALGAKKANBTDYOBVQFOJHA9OVFOY"
}'
```
--------------------

### レスポンス例
<!-- ### Response examples -->
--------------------
### 200
```json
{
    "bundleHash": "EWLCW9FFLSBUGZZOOLFQLTRJFKNGPUVCIOCQYTSDOSZLBCBJIIJZGPALGAKKANBTDYOBVQFOJHA9OVFOY",
    "timestamp": "1567577007000",
    "withdrawalUuid": "4782e7d5-9ce4-477d-8fd0-32f5f3385db2",
    "withdrawalUuid": "6784e7d5-9fe4-477d-8fd0-32f5f3785de2"
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
| :----------------- | :--- |
| `bundleHash` | スウィープのバンドルハッシュ |
| `timestamp` | スウィープが作成されたときのUNIXタイムスタンプ |
| `withdrawalUuid` | スウィープ内で実行された取り出しのUUID |

<!-- |**Return field**| **Description** | -->
<!-- |--|--| -->
<!-- | `bundleHash` | The bundle hash of the sweep| -->
<!-- | `timestamp` | The UNIX timestamp of when the sweep was created | -->
<!-- |`withdrawalUuid`|The UUIDs of the withdrawals that were actioned in the sweep| -->

## SweepSubscription

与えられた時間以降のすべてのスウィープのストリームを監視します。
<!-- Monitor a stream of all sweeps since a given time. -->

### パラメーター
<!-- ### Parameters -->

| **パラメーター** | **必須か任意か** | **説明** | **タイプ** |
| :----------- | :----------- | :--- | :----- |
| `newerThan` | 必須 | スウィープの監視を開始する日時。値が`0`の場合、ハブはすべてのスウィープイベントを取得します。 | Unix timestamp |

<!-- |**Parameters** |**Required or Optional**|**Description** |**Type**| -->
<!-- |--|--|--|--| -->
<!-- | `newerThan` |Required| The time and date from which to start monitoring sweeps . A `0` value means that Hub gets all sweep events.|Unix timestamp -->

### 例
<!-- ### Examples -->
--------------------
### Python
```python
import urllib2
import json

command = {
  "command": "SweepSubscription",
  "newerThan": 1563796441000
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:50051", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
  "command": "SweepSubscription",
  "newerThan": 1563796441000
}

var options = {
  url: 'http://localhost:50051',
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
### cURL
```bash
curl http://localhost:50051 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
  "command": "SweepSubscription",
  "newerThan": 1563796441000
}'
```
--------------------

### レスポンス例
<!-- ### Response examples -->
--------------------
### 200
```json
{
    "event_0": {
        "bundleHash": "EWLCW9FFLSBUGZZOOLFQLTRJFKNGPUVCIOCQYTSDOSZLBCBJIIJZGPALGAKKANBTDYOBVQFOJHA9OVFOY",
        "timestamp": "1563796442000",
        "withdrawalUuid": []
    },
    "event_1": {
        "bundleHash": "AJINYQCLKFYOCFWFLPESXAQGXYSZCHILJ9ZZCTNQOUGOFGTIOAXYZBCEWEXWDGAFFXBOXZJAPAUHVAZEC",
        "timestamp": "1567537268000",
        "withdrawalUuid": []

    },
    "event_2": {
        "bundleHash": "GOHZXSDAFYDJTJ9GZKKCBAFFKDCTFGFIYDXADGUH9SJGFYPGIOWXEOJXOYSIGYANNWXEII9KSKUZZCHGX",
        "timestamp": "1567537470000",
        "withdrawalUuid": []
    }
}
```
---
### 400
```json
{"error": "'command' parameter has not been specified"}
```
--------------------

### 結果
<!-- ### Results -->

| **リターンフィールド** | **説明** |
| :----------------- | :--- |
| `bundleHash` | スウィープのバンドルハッシュ |
| `timestamp` | スウィープが作成されたときのUNIXタイムスタンプ |
|`withdrawalUuid`| スウィープで実行された取り出しのUUID |

<!-- |**Return field**| **Description** | -->
<!-- |--|--| -->
<!-- | `bundleHash` | The bundle hash of the sweep| -->
<!-- | `timestamp` | The UNIX timestamp of when the sweep was created | -->
<!-- |`withdrawalUuid`|The UUIDs of the withdrawals that were actioned in the sweep| -->

## UserWithdraw

与えられたユーザーのアカウントから取り出しリクエストを送信します。リクエストが成功した場合、ハブは次のスウィープに取り出しを含めます。
<!-- Submit a withdrawal request from a given user's account. If request is successful, Hub includes the withdrawal in the next sweep. -->

### パラメーター
<!-- ### Parameters -->

| **パラメーター** | **必須か任意か** | **説明** | **タイプ** |
| :----------- | :----------- | :--- | :----- |
| `userId` | 必須 | IOTAトークンを取り出したいユーザーのID | string|
| `amount` | 必須 | ユーザーのアカウントから取り出す量 | integer|
| `payoutAddress` | 必須 | IOTAトークンの転送先アドレス（チェックサムを含む場合があります） | string|
| `validateChecksum` | 必須 | アドレスを検証するかどうか。`payoutAddress`フィールドが90トライトアドレス（チェックサム付き）の場合、このフィールドを`true`に設定します。 | boolean |
| `tag` | 任意 | バンドル内の入力トランザクションの`tag`フィールドに含める値 | string |

<!-- |**Parameters** |**Required or Optional**|**Description** |**Type** -->
<!-- |--|--|--|--| -->
<!-- | `userId` |Required| The ID of the user that wants to withdraw IOTA tokens | string| -->
<!-- | `amount` |Required| The amount to withdraw from the user's account | integer| -->
<!-- | `payoutAddress` |Required| Address to which to transfer the IOTA tokens (may include checksum) | string| -->
<!-- | `validateChecksum` |Required|Whether to validate the address. Set this field to `true` if the `payoutAddress` field is a 90-tryte address (with checksum) |boolean| -->
<!-- | `tag` |Optional| The value to include in the `tag` field of the input transaction in the bundle | string| -->

### 例
<!-- ### Examples -->
--------------------
### Python
```python
import urllib2
import json

command = {
  "command": "UserWithdraw" ,
  "userId" : "user-1",
  "amount": 1,
  "payoutAddress": "LFABJNKAKJVXYH9OPVZ9HJFOPOHDAGKOHZSRWHSNXYBHCYWQDHGRVKPFBLSGRZUOBL9DUBCKI9DWSPEJC"
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:50051", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
  "command": "UserWithdraw" ,
  "userId" : "user-1",
  "amount": 1,
  "payoutAddress": "LFABJNKAKJVXYH9OPVZ9HJFOPOHDAGKOHZSRWHSNXYBHCYWQDHGRVKPFBLSGRZUOBL9DUBCKI9DWSPEJC"
}

var options = {
  url: 'http://localhost:50051',
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
### cURL
```bash
curl http://localhost:50051 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
    "command": "UserWithdraw" ,
    "userId" : "user-1",
    "amount": 1,
    "payoutAddress": "LFABJNKAKJVXYH9OPVZ9HJFOPOHDAGKOHZSRWHSNXYBHCYWQDHGRVKPFBLSGRZUOBL9DUBCKI9DWSPEJC"
}'
```
--------------------

### レスポンス例
<!-- ### Response examples -->
--------------------
### 200
```json
{
    "uuid": "50485062-f5f0-4cac-bad6-bb0362ae5138"
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
| :----------------- | :--- |
| `uuid` | この取り出しリクエストの取り出しUUID |

<!-- |**Return field** |**Description**| -->
<!-- |--|--| -->
<!-- | `uuid` | The withdrawal UUID for this withdrawal request | -->

## UserWithdrawCancel

取り出しをキャンセルするリクエストを送信します。
<!-- Submit a request to cancel a withdrawal. -->

:::info:
取り消しは、取り出しがスウィープに含まれていない場合にのみ可能です。
:::
<!-- :::info: -->
<!-- A cancelation is possible only if the withdrawal isn't already included in a sweep. -->
<!-- ::: -->

### パラメーター
<!-- ### Parameters -->

| **パラメーター** | **必須か任意か** | **説明** | **タイプ** |
| :----------- | :----------- | :--- | :----- |
| `uuid` | 必須 | キャンセルする取り出しUUID | string |

<!-- |**Parameters** |**Required or Optional**|**Description** |**Type** -->
<!-- |--|--|--|--| -->
<!-- | `uuid` |Required| Withdrawal UUID that you want to cancel | string| -->

### 例
<!-- ### Examples -->
--------------------
### Python
```python
import urllib2
import json

command = {
    "command": "UserWithdrawCancel" ,
    "uuid": "50485062-f5f0-4cac-bad6-bb0362ae5138"
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:50051", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
    "command": "UserWithdrawCancel" ,
    "uuid": "50485062-f5f0-4cac-bad6-bb0362ae5138"
}

var options = {
  url: 'http://localhost:50051',
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
### cURL
```bash
curl http://localhost:50051 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
    "command": "UserWithdrawCancel" ,
    "uuid": "50485062-f5f0-4cac-bad6-bb0362ae5138"
}'
```
--------------------

### レスポンス例
<!-- ### Response examples -->
--------------------
### 200
```json
{
"success":true
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
| :----------------- | :--- |
| `success` | 取り出しがキャンセルされたかどうか |

<!-- |**Return field** |**Description**| -->
<!-- |--|--| -->
<!-- | `success` | Whether the withdrawal was canceled | -->

## WasAddressSpentFrom

預け入れアドレスがすでに取り出されているどうかを調べます。
<!-- Find out if a deposit address has already been withdrawn from. -->

このエンドポイントが`true`を返す場合、これ以上IOTAトークンを預け入れないでください。
<!-- If this endpoint returns true, you should not deposit any more IOTA tokens into it. -->

### パラメーター
<!-- ### Parameters -->

| **パラメーター** | **必須か任意か** | **説明** | **タイプ** |
| :----------- | :----------- | :--- | :----- |
| `address`          | 必須 | 使用済みステータスを確認するユーザーの預け入れアドレス（チェックサムを含めることができます） | string |
| `validateChecksum` | 任意 | アドレスを検証するかどうか。`address`フィールドが90トライトのアドレス（チェックサム付き）の場合、このフィールドを`true`に設定します。 | boolean |

<!-- |**Parameters** |**Required or Optional**|**Description** |**Type** -->
<!-- |--|--|--|--| -->
<!-- | `address`          |Required|The user's deposit address whose spent status you want to check (may include a checksum) |string| -->
<!-- | `validateChecksum` |Optional|Whether to validate the address. Set this field to `true` if the `address` field is a 90-tryte address (with checksum) -->

### 例
<!-- ### Examples -->
--------------------
### Python
```python
import urllib2
import json

command = {
    "command": "WasAddressSpentFrom",
    "address" : "LIQJBJRBSTGYWHYRPCLLCZUMP9SLHCBBWGQ9YRFWYDFF9FMXIAELYLTTBXCPVIDWWZYIOJIFLUFYVZIBDXKSTGPYPA",
    "validateChecksum": "true"
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:50051", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
    "command": "WasAddressSpentFrom",
    "address" : "LIQJBJRBSTGYWHYRPCLLCZUMP9SLHCBBWGQ9YRFWYDFF9FMXIAELYLTTBXCPVIDWWZYIOJIFLUFYVZIBDXKSTGPYPA",
    "validateChecksum": "true"
}

var options = {
  url: 'http://localhost:50051',
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
### cURL
```bash
curl http://localhost:50051 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
    "command": "WasAddressSpentFrom",
    "address" : "LIQJBJRBSTGYWHYRPCLLCZUMP9SLHCBBWGQ9YRFWYDFF9FMXIAELYLTTBXCPVIDWWZYIOJIFLUFYVZIBDXKSTGPYPA",
    "validateChecksum": "true"
}'
```
--------------------

### レスポンス例
<!-- ### Response examples -->
--------------------
### 200
```json
{
"wasAddressSpentFrom": "true"
}
```
---
### 400
```json
{"error": "'command' parameter has not been specified"}
```
--------------------

### 結果
<!-- ### Results -->

| **リターンフィールド** | **説明** |
| :----------------- | :--- |
| `WasAddressSpentFrom` | アドレスが使用済みかどうか |

<!-- |**Return field** |**Description**| -->
<!-- |--|--| -->
<!-- | `WasAddressSpentFrom` | Whether the address is spent| -->

## WasWithdrawalCancelled

キャンセルされた取り出しのステータスを取得します。
<!-- Get the status of a canceled withdrawal. -->

### パラメーター
<!-- ### Parameters -->

| **パラメーター** | **必須か任意か** | **説明** | **タイプ** |
| :----------- | :----------- | :--- | :----- |
| `uuid` | 必須 | キャンセルステータスを確認する取り出しUUID | string |

<!-- |**Parameters** |**Required or Optional**|**Description** |**Type** -->
<!-- |--|--|--|--| -->
<!-- | `uuid` |Required| Withdrawal UUID whose cancelation status you want to check | string| -->

### 例
<!-- ### Examples -->
--------------------
### Python
```python
import urllib2
import json

command = {
    "command": "WasWithdrawalCancelled",
    "uuid": "c10e6d8f-1f7c-4fdc-b21d-2e533870be6e"
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:50051", data=stringified, headers=headers)
returnData = urllib2.urlopen(request).read()

jsonData = json.loads(returnData)

print jsonData
```
---
### Node.js
```js
var request = require('request');

var command = {
    "command": "WasWithdrawalCancelled",
    "uuid": "c10e6d8f-1f7c-4fdc-b21d-2e533870be6e"
}

var options = {
  url: 'http://localhost:50051',
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
### cURL
```bash
curl http://localhost:50051 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
    "command": "WasWithdrawalCancelled",
    "uuid": "c10e6d8f-1f7c-4fdc-b21d-2e533870be6e"
}'
```
--------------------

### レスポンス例
<!-- ### Response examples -->
--------------------
### 200
```json
{
"wasCancelled": true
}
```
---
### 400
```json
{"error": "'command' parameter has not been specified"}
```
--------------------

### 結果
<!-- ### Results -->

| **リターンフィールド** | **説明** |
| :----------------- | :--- |
| `wasCancelled` | 取り出しがキャンセルされたかどうか |

<!-- |**Return field** |**Description**| -->
<!-- |--|--| -->
<!-- | `wasCancelled` | Whether the withdrawal was canceled| -->

## ユーザーアカウントの残高変更イベント
<!-- ## Balance change events for user accounts -->

ユーザーの残高履歴に関するデータをリクエストする場合、残高変更の理由は次のいずれかのイベントになります。
<!-- When requesting data about a user's balance history, the reason for the balance change will be one of the following events. -->

| **名前** | **説明** |
| :--- | :--- |
| `DEPOSIT`             | ユーザーは預け入れイベントを通じてトークンを受け取りました。 |
| `BUY`                 | ユーザーはバッチ転送の一部としてトークンを取得しました。|
| `WITHDRAWAL`          | ユーザーがトークンを取り出しました。 |
| `WITHDRAWAL_CANCELED` | ユーザーが取り出しリクエストをキャンセルしました。 |
| `SELL`                | ユーザーはバッチ転送の一部としてトークンを失いました。 |

<!-- |**Name**|**Description**  | -->
<!-- | :------------------- | :------------------------------------------------------------ | -->
<!-- | :------------------- | :------------------------------------------------------------ | -->
<!-- | `DEPOSIT`              | User received tokens through a deposit event | -->
<!-- | `BUY`                  | User gained tokens as part of a batch transfer| -->
<!-- | `WITHDRAWAL`           | User withdrew tokens                    | -->
<!-- | `WITHDRAWAL_CANCELED`   | User canceled a withdrawal request          | -->
<!-- | `SELL`                 | User lost tokens as part of a batch transfer| -->

## ユーザーアドレスの残高変更イベント
<!-- ## Balance change events for user addresses -->

| **名前** | **説明** |
| :--- | :--- |
| `UADD_UNKNOWN` | 不明 |
| `UA_DEPOSIT`   | ユーザーのアドレスに新しい預け入れを受け取りました。 |
| `UA_SWEEP`     | スウィープ中に、ユーザーのアドレスの合計残高がハブ所有者のアドレスの1つに転送されました。 |

<!-- |**Name**| **Description**             | -->
<!-- | ------------ | ------ | ------------------------ | -->
<!-- | ------------ | ------ | ------------------------ | -->
<!-- | `UADD_UNKNOWN` |    Unknown                      | -->
<!-- | `UA_DEPOSIT`   |  User's address received a new deposit | -->
<!-- | `UA_SWEEP`     | Total balance of the user's address was transferred to one of the Hub owner's addresses during a sweep   | -->

## ハブアドレスの残高変更イベント
<!-- ## Balance change events for Hub addresses -->

| **名前** | **説明** |
| :--- | :--- |
| `HUB_UNKNOWN` | 不明 |
| `INBOUND`     | IOTAトークンは、スウィープ中にハブアドレスに預け入れらました。 |
| `OUTBOUND`    | IOTAトークンは、スウィープ中にハブアドレスから取り出されました。 |

<!-- |**Name**| **Description**             | -->
<!-- | ------------ | ------ | -->
<!-- | `HUB_UNKNOWN` |    Unknown                      | -->
<!-- | `INBOUND`   |  IOTA tokens were deposited into the Hub address during a sweep | -->
<!-- | `OUTBOUND`     | IOTA tokens were withdrawn from the Hub address during a sweep| -->
