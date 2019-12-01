# API

**このリストには、HTTP APIを介してScyllaクラスターと通信するためのコマンドが含まれています。**
<!-- **This list contains the commands for communicating with a Scylla cluster through the HTTP API.** -->

以下のすべてのコマンドには、HTTPヘッダーを含める必要があります。
<!-- All the following commands must include an HTTP header. -->

| **ヘッダー** | **値** | **必須か任意か** |
| :------- | :--- | :------- |
| X-IOTA-API-Version | 1 | 必須 |
| Content-Type | application/json | 任意 |

<!-- | **Header**       | **Value** | **Required or Optional** | -->
<!-- |:---------------|:--------|:--------| -->
<!-- |:---------------|:--------|:--------| -->
<!-- | X-IOTA-API-Version | 1 | Required | -->
<!-- | Content-Type | application/json | Optional | -->

:::warning:
このAPIはベータ版であり、変更される可能性があります。実稼働アプリケーションではこのAPIを使用しないことをお勧めします。
:::
<!-- :::warning: -->
<!-- This API is in beta, and is subject to change. We recommend that you don't use this API in production applications. -->
<!-- ::: -->

## getTrytes

トランザクションのコンテンツを取得します。
<!-- Get a transaction's contents. -->

### パラメーター
<!-- ### Parameters -->

| **パラメーター** | **必須か任意か** | **説明** | **タイプ** |
| :----------- | :----------- | :--- | :----- |
| `hashes` | 必須 | トランザクションハッシュ | 文字列の配列 |

<!-- |**Parameters** |**Required or Optional**|**Description** |**Type** -->
<!-- |--|--|--|--| -->
<!-- | `hashes` |Required| Transaction hashes | array of strings -->

### 例
<!-- ### Examples -->
--------------------
### Python
```python
import urllib2
import json

command = {
  "command": "getTrytes",
  "hashes": [
    "SUOESJCXAIDZHGNEJDCFYABGUIOXIVVAHZCZXYWOXDNMGXKM9PEHV9GAMZJWOCVKXVOZUWGZNLZTZ9999"
  ]
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:4000", data=stringified, headers=headers)
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
    "SUOESJCXAIDZHGNEJDCFYABGUIOXIVVAHZCZXYWOXDNMGXKM9PEHV9GAMZJWOCVKXVOZUWGZNLZTZ9999"
  ]
}

var options = {
  url: 'http://localhost:4000',
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
curl http://localhost:4000 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
"command": "getTrytes",
"hashes": [
  "SUOESJCXAIDZHGNEJDCFYABGUIOXIVVAHZCZXYWOXDNMGXKM9PEHV9GAMZJWOCVKXVOZUWGZNLZTZ9999"
  ]
}'
```
--------------------

### レスポンス例
<!-- ### Response examples -->
--------------------
### 200
```json
{
"trytes": [{"address":"JPYUAV9MBDZG9ZX9BAPBBMYFEVORNBIOOZCYPZDZNRGKQYT9HFEXXXBG9TULULJIOWJWQMXSPLILOJGJG","attachmentTimestamp":1567166602904,"attachmentTimestampLowerBound":0,"attachmentTimestampUpperBound":12,"branchTransaction":"OLZDBGOWXCLNZPJZMFUVYPL9COCBSHPIJGIN9L9SNUMMYVZQNDVOCWOYYGJXKHEJGWANXWRBVELB99999","bundle":"VVCRIZWRJ9GOUJRXRBWDEULYKIBIZNKIWGCWZCWVBTVIBAHKVTWLGYQNIZ9JCZJKVEAXABBAUEIGNGWP9","currentIndex":0,"lastIndex":0,"nonce":"JVF9999999RMF99999999999999","obsoleteTag":"HUIWONTO9999999999999999999","signatureMessageFragment":"ODGAHDLDGDNCGDIDRCRCTCTCSCTCSCGADBZAABZACBCBXAABQAGAHDLDGDNCUCPCXC9DTCSCGADB9BBBABQAGAQCPCSCNCQCFDPCBDRCWCGADBVAUAVAZAQAGAQCPCSCNCHDFDIDBDZCGADBVAUAVAZAQAGAQCPCSCNCHDF...","timestamp":1567166602,"trunkTransaction":"BXZWFMSFBAYWJKJUAKWYTUCZRY9GMNETX9MLN9UKRR9ORGRRIENPERNWCLHBCE9XBMYHAMGFYRRL99999","value":0}]
}
```
--------------------

### 結果
<!-- ### Results -->

| **リターンフィールド** | **説明** |
| :----------------- | :--- |
| `trytes` | 指定されたトランザクションハッシュのトランザクショントライトの配列（パラメーターと同じ順序） |

<!-- |**Return field** |**Description**| -->
<!-- |--|--| -->
<!-- | `trytes` | Array of transaction trytes for the given transaction hashes (in the same order as the parameters) | -->

## findTransactions

トランザクションフィールドに指定された値を含むトランザクションを検索します。パラメータは、`bundles`、`addresses`、`tags`、`approvees`、`hints`など、検索するトランザクションフィールドを定義します。
<!-- Find transactions that contain the given values in their transaction fields. -->
<!-- The parameters define the transaction fields to search for, including `bundles`, `addresses`, `tags`, `approvees`, and `hints`. -->

:::info:
各呼び出しに渡すことができるパラメーターは1つだけです。
:::
<!-- :::info: -->
<!-- You can pass only one parameter to each call. -->
<!-- ::: -->

### パラメーター
<!-- ### Parameters -->

| **パラメーター** | **説明** | **タイプ** |
| :----------- | :--- | :----- |
| `addresses` | 検索するアドレス（チェックサムを含めないでください） | 文字列の配列 |
| `approvees` | 検索する子トランザクションハッシュ | 文字列の配列 |
| `bundles` | 検索するバンドルハッシュ | 文字列の配列 |
|`hints` | トランザクションの添付タイムスタンプで検索するタグの一部（少なくとも4トライ）またはアドレス（81トライ）。`month`プロパティと`year`プロパティで検索する添付ファイルのタイムスタンプを指定できます。`page_size`プロパティを使用して、返されるトランザクションハッシュの最大数を指定できます。結果の残りは`paging_state`配列で返されます。 | オブジェクトの配列 |
| `tags` | 検索するタグ | 文字列の配列 |

<!-- |**Parameters** |**Description** | **Type** -->
<!-- |--|--|--| -->
<!-- |--|--|--| -->
<!-- | `addresses` | Addresses to search for (do not include the checksum) | array of strings | -->
<!-- | `approvees` | Child transaction hashes to search for | array of strings | -->
<!-- | `bundles` | Bundle hashes to search for | array of strings | -->
<!-- |`hints`|Part of a tag (at least 4 trytes) or an address (81 trytes) to search for by the transaction's attachment timestamp. You can specify the attachment timestamp to search for in the `month` and `year` properties. You can use the `page_size` property to specify the maximum number of transaction hashes to return. The rest of the results will be returned in a `paging_state` array.|array of objects| -->
<!-- | `tags` | Tags to search for | array of strings | -->


### 例
<!-- ### Examples -->
--------------------
### Python
```python
import urllib2
import json

command = {
  "command": "findTransactions",
  "hints": [
  {"tag":"POWSRV","month":8,"year":2019, "page_size": 500}
  ]
}

stringified = json.dumps(command)

headers = {
    'content-type': 'application/json',
    'X-IOTA-API-Version': '1'
}

request = urllib2.Request(url="http://localhost:4000", data=stringified, headers=headers)
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
  "hints": [
  {"tag":"POWSRV","month":8,"year":2019, "page_size": 500}
  ]
};


var options = {
  url: 'http://localhost:4000',
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
curl http://localhost:4000 \
-X POST \
-H 'Content-Type: application/json' \
-H 'X-IOTA-API-Version: 1' \
-d '{
  "command": "findTransactions",
  "hints": [
  {"tag":"POWSRV","month":8,"year":2019, "page_size": 500}
  ]
}'
```
--------------------

### レスポンス例
<!-- ### Response examples -->
--------------------
### 200
```json
{
  "hashes":["YBOCSXAPQ9ZRKLPZTPUAHIEYZMM9WILR9ELGKMJ9DPKEQLVWHPBPXHDBXVDNBOTHSCLHSKMV9VXWZ9999","Y9CPOZPBICEGGYZUG9ORWDODJLWSFFFMKGVUQWTTWVHAYXO9TTLFLZIPTPVONUGMCVUWCVNM9EQJA9999","..."
  ],
  "hints":[
    {"month":8,"page_size":500,"paging_state":[0,0,0,0,20,1,0,0,32,0,0,0,4,0,0,0,2,0,0,0,73,79,2,0,0,0,84,65,2,0,0,0,7,227,2,0,0,0,0,8,1,136,0,0,0,5,0,0,0,2,0,0,0,73,83,2,0,0,0,65,83,19,0,0,0,67,65,77,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,4,0,0,0,93,105,39,120,81,0,0,0,89,66,79,67,83,88,65,80,81,57,90,82,75,76,80,90,84,80,85,65,72,73,69,89,90,77,77,57,87,73,76,82,57,69,76,71,75,77,74,57,68,80,75,69,81,76,86,87,72,80,66,80,88,72,68,66,88,86,68,78,66,79,84,72,83,67,76,72,83,75,77,86,57,86,88,87,90,57,57,57,57,11,254,255,127,64,75,209,157,39,39,44,67,150,117,165,64,16,45,245,135,1,0,0,0,57,0,0,0,1,25,0,0,0,20,0,0,0,1,0,0,0,8,0,0,0,210,169,0,71,3,23,125,133,1,1,25,0,0,0,20,0,0,0,1,0,0,0,8,0,0,0,210,169,0,71,3,23,125,133,1,1,1,0,0,0,177,75,216,214,96,152,70,193,205,202,198,226,80,81,213,128,1,2],"tag":"POWSRV","year":2019
    }
  ]
}
```
--------------------

### 結果
<!-- ### Results -->

トランザクションハッシュの配列は、個々の要素すべてに対して同じ順序で返されます。
<!-- An array of transaction hashes is returned in the same order for all individual elements. -->

| **リターンフィールド** | **説明** |
| :----------------- | :--- |
| `hashes` | 返されるトランザクションハッシュは、入力によって異なります。`bundles`：指定されたバンドルハッシュを含むトランザクションハッシュの配列を返します。`addresses`：` address`フィールドに指定されたアドレスを含むトランザクションハッシュの配列を返します。`tags`：` tag`フィールドに指定された値を含むトランザクションハッシュの配列を返します。`approvees`：` branchTransaction`または`trunkTransaction`フィールドに指定されたトランザクションを含むトランザクションハッシュの配列を返します。`hints`：ヒントオブジェクトの配列を返します。`paging_state`配列を`findTransactions`エンドポイントに渡して、`paging_size`プロパティの値までのトランザクションハッシュをさらに確認します。 |

<!-- |**Return field** | **Description** | -->
<!-- |--|--| -->
<!-- | `hashes` | The transaction hashes which are returned depend on your input. `bundles`: returns an array of transaction hashes that contain the given bundle hash. `addresses`: returns an array of transaction hashes that contain the given address in the `address` field. `tags`: returns an array of transaction hashes that contain the given value in the `tag` field. `approvees`: returns an array of transaction hashes that contain the given transactions in their `branchTransaction` or `trunkTransaction` fields. `hints`: returns an array of hint objects. Pass the `paging_state` array to the `findTransactions` endpoint to see more of the resulting transaction hashes up to the value of the `paging_size` property. | -->
