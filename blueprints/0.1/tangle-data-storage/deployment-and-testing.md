# タングルデータストレージアプリをデプロイしてテストする
<!-- # Deploy and test the Tangle data storage app -->

**IPFSノードとタングルにデータの保存を開始するには、アプリケーションをデプロイし、IOTA財団のグラフィカルユーザーインターフェースを使用してファイルデータをアップロードし、ファイルデータを取得し、ハッシュを比較して内容を確認します。**
<!-- **To start storing data in the IPFS node and the Tangle, deploy the application and use our graphical user interface to upload file data, retrieve file data, and compare hashes to verify the contents.** -->

## タングルデータストレージアプリをデプロイする
<!-- ## Deploy the Tangle data storage app -->

デプロイの手順は、GitHubの以下のマークダウンファイルでホストされています。
<!-- The deployment instructions are hosted on GitHub in the following markdown files: -->

- [PoCのソースコード](https://github.com/iotaledger/poc-ipfs/blob/master/README.md) - Reactで書かれたフロントエンドGUIとNode.js APIとして書かれたバックエンドの2つの部分から成ります。このPoCを再現するために、専用のハードウェアを配置する必要はありません。
<!-- - [PoC source code](https://github.com/iotaledger/poc-ipfs/blob/master/README.md) -  -->
<!-- Consists of two parts a front-end GUI written in React and a back-end written as a Node.js API. -->
<!-- In order to reproduce this PoC there is no requirement to deploy dedicated hardware. -->
- [フロントエンドのデプロイ方法](https://github.com/iotaledger/poc-ipfs/blob/master/client/DEPLOYMENT.md)
<!-- - [Front-end deployment instructions](https://github.com/iotaledger/poc-ipfs/blob/master/client/DEPLOYMENT.md) -->
- [Node.js APIのデプロイ方法](https://github.com/iotaledger/poc-ipfs/blob/master/api/DEPLOYMENT.md)
<!-- - [Node.js API deployment instructions](https://github.com/iotaledger/poc-ipfs/blob/master/api/DEPLOYMENT.md) -->

## タングルデータストレージアプリをテストする
<!-- ## Test the Tangle data storage app -->

1. アプリケーションがロードされると、次の画像が表示されます。
  <!-- 1. After the application loads, the following image is displayed: -->

    ![Upload File for IOTA IPFS Data Storage PoC](../images/data-storage-upload.png)

    :::info:
    ファイルサイズの制限は、この設計図の目的のためだけです。実際の実装では任意のファイルサイズを許可できます。
    :::
    <!-- :::info: -->
    <!-- The file size limitation is only for the purposes of this blueprint. Real-world implementations can allow any file size. -->
    <!-- ::: -->

2. ファイルを選択すると、他のフィールドには自動的にメタデータが入力されていることがわかります。
  <!-- 2. Select a file, and see that the other fields are automatically populated with metadata -->

    ![Upload File for IOTA IPFS Data Storage PoC - Populated](../images/data-storage-upload2.png)

3. **Upload**をクリックして、メタデータをIPFSノードとタングルに保存します。すべてうまくいけば、確認メッセージが表示されます。
  <!-- 3. Click **Upload** to store the metadata on the IPFS node and the Tangle. If everything went well, you should see a confirmation message. -->

    ![Uploaded File for IOTA IPFS Data Storage PoC](../images/data-storage-upload-finished.png)

4. ファイルを取得して検証するには、**Retrieve File**ページに移動します。
  <!-- 4. To retrieve and validate a file, go to the Retrieve File page. -->

    ![Retrieve File for IOTA IPFS Data Storage PoC](../images/data-storage-upload-retrieve.png)

5. トランザクションハッシュを入力して**RETRIEVE**をクリックします。トランザクションハッシュはAPIに渡され、タングル上のトランザクションのメッセージからIPFSハッシュやSHA256ハッシュなどが取得されます。IPFSハッシュはファイルのダウンロードに使用され、SHA256ハッシュはダウンロードしたファイルのSHA256ハッシュの検証に使用されます。
  <!-- 5. Enter a transaction hash and click **RETRIEVE**. The transaction hash will be passed to the API, which will retrieve the IPFS hash from the transaction's message, which is used to download the file and validate its SHA256 hash against the one stored in the transaction. -->

    ![Retrieve File for IOTA IPFS Data Storage PoC](../images/data-storage-upload-validated.png)

これで、ファイルの保存、取得、検証の全ライフサイクルが完了しました。タングル上のトランザクションに格納されているファイルメタデータ、SHA256ハッシュ、およびIPFSハッシュはイミュータブルです。IPFSノードから取得したファイルの内容がタングルの内容と一致しない場合、そのファイルの内容は信頼できなくなります。
<!-- This completes the full lifecycle for storing, retrieving, and validating a file. The file metadata, SHA256 hash and IPFS hash that are stored in a transaction on the Tangle are immutable. If the file contents that you retrieve from the IPFS node don't match the ones on the Tangle, then the contents of that file can no longer be trusted. -->
