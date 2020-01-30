# アプリケーションアーキテクチャ
<!-- # Application architecture -->

**タングルデータストレージアプリケーションは，React で書かれたグラフィカルユーザーインターフェース（GUI）と NodeJS で書かれたバックエンド API の2つの部分から構成されています．**
<!-- **The Tangle data storage application consists of two parts: A graphical user interface (GUI) written in React and a back-end API written in NodeJS.** -->

:::warning:免責事項
人間の努力のように，オープンソースプロジェクトを実行することは，不確実性とトレードオフを伴います．以下に説明するアーキテクチャが，同様のシステムを展開するのに役立つことを願っていますが，間違いが含まれている可能性があり，すべての状況に対処することはできません．あなたのプロジェクトについて何か質問があれば，IOTA 財団はあなたがあなた自身の研究をし，専門家を探し，そしてIOTA コミュニティとそれらを話し合うことを奨励します．
:::
<!-- :::warning:Disclaimer -->
<!-- Running an open source project, like any human endeavor, involves uncertainty and trade-offs. We hope the architecture described below helps you to deploy similar systems, but it may include mistakes, and can’t address every situation. If you have any questions about your project, we encourage you to do your own research, seek out experts, and discuss them with the IOTA community. -->
<!-- ::: -->

この設計図では，クライアントが API サーバーにファイルをアップロードする次のアーキテクチャを使用します．API サーバーは，ファイルを InterPlanetary File System（IPFS）ノードに送信し，IPFS ハッシュをタングルのトランザクションにアタッチします．
<!-- This blueprint uses the following architecture whereby clients upload files to the API server, which sends the files to an InterPlanetary File System (IPFS) node and attaches the IPFS hashes to transactions on the Tangle. -->

![Data Storage PoC - IOTA/IPFS - Architecture](../images/data-storage-ipfs.png)

## 基礎的要素
<!-- ## Building blocks -->

API サーバーは，クライアントに以下の2つのメソッドを公開します．
<!-- The API server exposes two methods to the client: -->

- `storeFile()`
- `retrieveFile()`

## storeFile()

API を使用してファイルを保存するために，クライアントは次のことを行います．
<!-- To store a file using the API, the client does the following: -->

- アップロードするファイルを選択する
<!-- - Select the file to upload -->
- ファイルの内容の SHA256 ハッシュを生成する
<!-- - Generate SHA256 hash of the file content -->
- 追加のファイルメタデータをキャプチャする
<!-- - Capture additional file metadata -->

舞台裏では，API は次のことを行います．
<!-- Behind the scenes, the API does the following: -->

- ファイルの内容を IPFS にアップロードします．IPFS は IPFS ハッシュを返します．
<!-- - Upload the file content to IPFS, which returns the IPFS hash -->
- メタデータ，SHA256，および IPFS ハッシュをトランザクションでタングルにアタッチし，タングルトランザクションハッシュをクライアントに返します．
<!-- - Attach the file metadata, SHA256 hash, and IPFS hash to the Tangle in a transaction, and return the transaction hash to the client -->

![Data Storage PoC - IOTA/IPFS - Store File](../images/data-storage-store.png)

### ファイルコンテンツを IPFS にアップロードする
<!-- ### Uploading file content to the IPFS -->

`storeFile()` メソッドは次の形式の JSON オブジェクトを取ります．
<!-- The `storeFile()` method takes a JSON object in the following format: -->

```javascript
IPFSStoreRequest {
   /**
    * ファイル名
    */
   name: string;

   /**
    * ファイルの説明
    */
   description: string;

   /**
    * ファイルのサイズ
    */
   size: number;

   /**
    * ファイルの変更び
    */
   modified: Date;

   /**
    * ファイルの sha256 ハッシュ
    */
   sha256: string;

   /**
    * base64 でエンコードされたファイルデータ
    */
   data: string;
}
```

JSON オブジェクトを受信すると，ファイルデータは IPFS ノードにアップロードされます．
<!-- On receipt of the JSON object, the file data is uploaded to the IPFS node. -->

```javascript
import ipfsClient from "ipfs-http-client";

const buffer = Buffer.from(request.data, "base64");
const ipfs = ipfsClient(config.ipfs);
const addResponse = await ipfs.add(buffer);
```

### タングルにファイルデータをアタッチする
<!-- ### Attaching the file data to the Tangle -->

`add()` メソッドは IPFS ハッシュを返します．これは，トランザクションでタングルにアタッチされる前に JSON オブジェクト内の他のデータと結合されます．
<!-- The `add()` method returns the IPFS hash, which is combined with the other data in the JSON object before being attached to the Tangle in a transaction. -->

```javascript
// 新しいアドレスを生成する
const nextAddress = generateAddress(config.seed, 0, 2);

// トランザクションに追加するメッセージを定義する
const tanglePayload = {
   name: request.name,
   description: request.description,
   size: request.size,
   modified: request.modified,
   sha256: request.sha256,
   ipfs: addResponse[0].hash
};

// IOTA ノードに接続する
const iota = composeAPI({
        provider: config.provider
    });

// バンドルを作成する
const trytes = await iota.prepareTransfers(
   // ここでは，全てが9で構成されるシードを使用します．
   // これは，シードがトランザクションの署名に使用されないためです．
   "9".repeat(81),
   [
       {
           address: nextAddress,
           value: 0,
           message: TrytesHelper.toTrytes(tanglePayload)
       }
   ]);

// タングルにトランザクションを送信する
const bundle = await iota.sendTrytes(trytes, config.depth, config.mwm);
```

`sendTrytes()` メソッドから返されたバンドルにはトランザクションハッシュが含まれています．これはクライアントに返され，タングル上のデータの読み取りに使用されます．
<!-- The bundle returned from the `sendTrytes()` method contains the transaction hash, which is returned to the client to use for reading the data on the Tangle. -->

## retrieveFile()

ファイルを取得してその内容を検証するために，クライアントは次のことを行います．
<!-- To retrieve a file and validate its contents, the client does the following: -->

- トランザクションハッシュを使用して，タングルからファイルデータを取得する
<!-- - Get the file data from the Tangle, using the transaction hash -->
- 返された IPFS ハッシュを使用して IPFS からファイルの内容を取得する
<!-- - Get the file contents from IPFS using the returned IPFS hash -->
- ファイルデータに対して SHA256 ハッシュを実行し，計算された SHA256 とタングルから返された SHA256 を比較する
<!-- - Perform a SHA256 hash on the file data, and compare the calculated SHA256 with the one returned from the Tangle -->

![Data Storage PoC - IOTA/IPFS - Retrieve File](../images/data-storage-retrieve.png)

### タングルからファイルデータを取得する
<!-- ### Getting the file data from the Tangle -->

タングルからファイルデータを取得するには，トランザクションハッシュを使用して IOTA ノードからトランザクションをリクエストします．
<!-- To get the file data from the Tangle, we request the transaction from the IOTA node, using the transaction hash. -->

```javascript
// IOTA ノードに接続する
const iota = composeAPI({
        provider: config.provider
    });

// 指定されたハッシュを使用して，トランザクショントライトを取得する
const transactions = await iota.getTrytes([request.transactionHash]);
// トランザクショントライトをオブジェクトに変換する
const txObject = asTransactionObject(transactions[0]);
// メッセージを取得し，ASCII 文字列に変換する
const ascii = trytesToAscii(txObject.signatureMessageFragment);
// JSONメッセージをパースする
const payload = JSON.parse(ascii)
```

### IPFS からファイルデータを取得する
<!-- ### Getting the file data from the IPFS -->

トランザクションハッシュは，[Cloudflare](https://cloudflare-ipfs.com/ipfs/) などのパブリック IPFS ゲートウェイを使用して，IPFS ノードからファイルをリクエストするために使用されます．
<!-- The transaction hash is used to request the file from the IPFS node, using a public IPFS gateway such as [Cloudflare](https://cloudflare-ipfs.com/ipfs/). -->

### データを比較する
<!-- ### Comparing the data -->

ファイルが IPFS からバッファに返されたと仮定すると，ファイルは SHA256 アルゴリズムを使用してハッシュされ，その結果のハッシュはトランザクションの `signatureMessageFragment` フィールドからのハッシュと比較されます．
<!-- Assuming the file was returned from the IPFS into a buffer, the file is hashed using a SHA256 algorithm and the resulting hash is compared to the one from the transaction's `signatureMessageFragment` field. -->

```javascript
const sha256 = crypto.createHash("sha256");
sha256.update(fileBuffer);
const ipfsSha256 = sha256.digest("hex");
if (ipfsSha256 === payload.sha256) {
   console.log("All Is Well");
} else {
   console.log("Oh no, the hash does not match");
}
```

## カスタマイズの考慮事項
<!-- ## Customization considerations -->

自分自身のシステムでこの設計図を使用する場合は，次のことを考慮する必要があります．
<!-- If you want to use this blueprint in your own system, you should consider the following. -->

### データセキュリティ
<!-- ### Data security -->

IPFS は分散 Web であるため，IPFS ハッシュを持っている人なら誰でもファイルの内容をダウンロードして読むことができます．
<!-- Because the IPFS is a distributed web, anyone who has the IPFS hash can download and read the contents of the file. -->

許可されていないエンティティがデータを読み取らないようにするには，データを IPFS ノードにアップロードする前に暗号化します．
<!-- To prevent unauthorized entities from reading the data, you could encrypt it before uploading it to the IPFS node. -->

### 代替データストレージソリューション
<!-- ### Alternative data storage solutions -->

このアプリケーションでは，データは IPFS ノードにアップロードされますが，代替データストレージソリューションにアップロードする場合も同じ原則が適用されます．
<!-- In this application, data is uploaded to an IPFS node, however the same principles apply if you were to upload to an alternative data storage solution. -->

Amazon S3 や Azure Storage などの代替ストレージソリューションを使用するには，一意のハッシュ（たとえばファイルの SHA256 ハッシュ）を使用してデータを代替ストレージソリューションにアップロードするだけです．
<!-- To use alternative storage solutions such as Amazon S3 or Azure Storage, you just need to upload the data to it with a unique hash (for example the SHA256 hash of the file). -->
