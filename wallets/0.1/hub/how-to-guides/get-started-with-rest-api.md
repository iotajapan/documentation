# RESTful API入門
<!-- # Get started with the RESTful API -->

**ハブのRESTful APIサーバーを公開すると、HTTPエンドポイントを介してハブと対話できます。これらのエンドポイントを使用すると、ハブデータベースおよびIOTA[ノード](root://getting-started/0.1/network/nodes.md)と対話することにより、ユーザーのIOTAトークンを管理できます。このガイドでは、RESTful APIの基本を学習して、新しい預け入れアドレスを持つ新しいユーザーを作成します。**
<!-- **When you expose Hub's RESTful API server, you can interact with it through HTTP endpoints. These endpoints allow you to manage users' IOTA tokens by interfacing with the Hub database and a [node](root://getting-started/0.1/network/nodes.md). In this guide, you learn the basics of the RESTful API to create a new user with some new deposit addresses.** -->

## 前提条件
<!-- ## Prerequisites -->

このガイドのコードサンプルを使用するには、次のものが必要です。
<!-- To use the code samples in this guide, you must have the following: -->

- [ハブのインスタンス](../how-to-guides/install-hub.md)
<!-- - An [instance of Hub](../how-to-guides/install-hub.md) -->
- [Node.js (8以上)](https://nodejs.org/en/)、[Python (3以上)](https://www.python.org/downloads/)と[PIP](https://pip.pypa.io/en/stable/installing/)もしくは[cURL](https://curl.haxx.se/download.html)
<!-- - [Node.js (8+)](https://nodejs.org/en/), [Python (3+)](https://www.python.org/downloads/) and [PIP](https://pip.pypa.io/en/stable/installing/), or [cURL](https://curl.haxx.se/download.html) -->
- [Visual Studio Code](https://code.visualstudio.com/Download)などのコードエディター
<!-- - A code editor such as [Visual Studio Code](https://code.visualstudio.com/Download) -->
- コマンドラインインターフェースへのアクセス
<!-- - Access to a command-line interface -->

---

1\. 新しいユーザーを作成します。`http://localhost:50051` URLを、ハブのセットアップ時に使用した`--listenAddress`フラグの値に置き換えます。
<!-- 1\. Create a new user. Replace the `http://localhost:50051` URL with value of the `--listenAddress` flag that you used when you set up Hub. -->

--------------------
### Python

```python
import urllib2
import json

command = {
	"command": "CreateUser",
	"userId": "Jake"
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

command = {
  "command": "CreateUser",
  "userId": "Jake"
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
  "command": "CreateUser",
  "userId": "Jake"
}'
```
--------------------

コンソールに空のオブジェクトが表示されます。これは、ユーザーが作成されたことを意味します。
<!-- You should see an empty object in the console, which means that the user was created. -->

:::info:
このユーザーは、[`user_account`テーブルのクエリ](../how-to-guides/query-the-database.md)によってハブデータベースで確認できます。
:::
<!-- :::info: -->
<!-- You can see this user in the Hub database by [querying the `user_account` table](../how-to-guides/query-the-database.md). -->
<!-- ::: -->

2\. [チェックサム](root://getting-started/0.1/clients/checksums.md)を付随して新しい預け入れアドレスを作成します。
<!-- 2\. Create a new deposit address with a [checksum](root://getting-started/0.1/clients/checksums.md) -->

--------------------
### Python

```python
import urllib2
import json

command = {
	"Command": "GetDepositAddress",
	"userId": "Jake",
	"includeChecksum": "true"
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

command = {
  "command": "GetDepositAddress",
  "userId": "Jake",
  "includeChecksum": "true"
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
  "command": "GetDepositAddress",
  "userId": "Jake",
  "includeChecksum": "true"
}'
```
--------------------

標準出力には、90トライトの預け入れアドレスが表示されます。
<!-- In the output, you should see a 90-tryte deposit address. -->

```bash
"address": "RDZVDZKRBX9T9L9XXONXDVJDRKYPAABWMQLORGCDCWHDDTSOPRZPCQB9AIZZWZAQ9NBZNVUUUSPQHRGWDYZUVP9WSC"
```

:::info:
データベースでは、アドレスは常にチェックサムなしで保存されます。
:::
<!-- :::info: -->
<!-- In the database, addresses are always saved without the checksum. -->
<!-- ::: -->

3\. ユーザーの預け入れアドレスにIOTAトークンを送信します。
<!-- 3\. Send some IOTA tokens to the user's deposit addresses -->

:::info:
[公式トリニティウォレット](root://wallets/0.1/trinity/introduction/overview.md)を使用してIOTAトークンを送信できます。
:::
<!-- :::info: -->
<!-- You can use the [official Trinity wallet](root://wallets/0.1/trinity/introduction/overview.md) to send IOTA tokens. -->
<!-- ::: -->

4\. ユーザーの残高と履歴を取得します。
<!-- 4\. Get the balance and history for the user -->

--------------------
### Python

```python
import urllib2
import json

command = {
	"command": "GetBalance",
	"userId": "Jake"
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

command = {
  "command": "GetBalance",
  "userId": "Jake"
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
  "userId": "Jake"
}'
```
--------------------

IOTAトークンを預け入れアドレスに送信した場合、標準出力には次のように表示されます。
<!-- If you sent IOTA tokens to the deposit address, the output should display something like the following: -->

```shell
10 i available for 'Jake'
History:
events {
    timestamp: 1540856214000
    type: DEPOSIT
    amount: 10
}
```

[thetangle.org](https://thetangle.org/)などのタングルエクスプローラーで預け入れアドレスの履歴を見ると、ハブが預け入れアドレスから別のアドレス（ユーザーが取り出しを要求するまで資金が集められるハブ所有者のアドレス）に資金を移動したことが分かります。このプロセスは[スウィープ](../concepts/sweeps.md)と呼ばれます。
<!-- If you look at the deposit address history in a Tangle explorer such as [thetangle.org](https://thetangle.org/), you will see that Hub moved the funds away from the deposit address and into another address (Hub owner's address where funds are aggregated until a user requests a withdrawal). This process is called a [sweep](../concepts/sweeps.md). -->

:::success:おめでとうございます:tada:
新しいユーザーを正常に作成し、ハブが預け入れを処理する方法をテストしました。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've successfully created a new user and tested how Hub handles deposits. -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

[デモ取引所のセットアップ](../how-to-guides/create-a-demo-exchange.md)を行い、ハブの統合をテストする。
<!-- [Set up a demo exchange](../how-to-guides/create-a-demo-exchange.md) to test an integration of Hub. -->

[統合オプションをご覧ください](../how-to-guides/integrate-hub.md)。
<!-- [See our integration options](../how-to-guides/integrate-hub.md). -->
