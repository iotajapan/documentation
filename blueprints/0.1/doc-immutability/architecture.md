# アプリケーションアーキテクチャ
<!-- # Application architecture -->

**ドキュメントの不変性アプリケーションは，React で書かれたグラフィカルユーザーインターフェース（GUI）と NodeJS で書かれたバックエンド API の2つの部分から構成されています．**
<!-- **The Document immutability application consists of two parts: A graphical user interface (GUI), written in React and a back-end API, written in NodeJS.** -->

:::warning:免責事項
人間の努力のように，オープンソースプロジェクトを実行することは，不確実性とトレードオフを伴います．以下に説明するアーキテクチャが，同様のシステムを展開するのに役立つことを願っていますが，間違いが含まれている可能性があり，すべての状況に対処することはできません．あなたのプロジェクトについて何か質問があれば，IOTA 財団はあなたがあなた自身の研究をし，専門家を探し，そして IOTA コミュニティとそれらを話し合うことを奨励します．
:::
<!-- :::warning:Disclaimer -->
<!-- Running an open source project, like any human endeavor, involves uncertainty and trade-offs. We hope the architecture described below helps you to deploy similar systems, but it may include mistakes, and can’t address every situation. If you have any questions about your project, we encourage you to do your own research, seek out experts, and discuss them with the IOTA community. -->
<!-- ::: -->

この設計図は，アプリケーションがクライアントからファイルデータを取得し，データベースに保存し，IOTA ノードを介してタングルにアタッチする次のアーキテクチャを使用します．
<!-- This blueprint uses the following architecture whereby the application takes file data from a client, saves it to a database, and attaches it to the Tangle through an IOTA node. -->

![Document immutability architecture](../images/document-immutability-architecture.png)

## 基本的要素
<!-- ## Building blocks -->

このアプリケーションを使用すると，ユーザーはドキュメントをタングルにアップロードして，変更されていないことを確認できます．
<!-- The application allows users to upload their documents to the Tangle and then verify that they haven't changed. -->

### ドキュメントのアップロード
<!-- ### Uploading a document -->

ユーザーがドキュメントをアップロードすると，アプリケーションは次のことを行います．
<!-- When a user uploads a document, the application does the following: -->

1. ドキュメントをハッシュする
<!-- 1. Hash the document -->
2. データベースにドキュメントを保存する
<!-- 2. Save the document in a database -->
3. ドキュメントのハッシュをタングルにアタッチする
<!-- 3. Attach the hash to the Tangle -->
4. データベースにトランザクションハッシュを保存する
<!-- 4. Save the transaction hash to the database -->

![Document hashing](../images/document-immutability-hashing.png)

#### ドキュメントをハッシュする
<!-- #### Hashing a document -->

ドキュメントは，次のハッシュアルゴリズムのいずれかを使用してハッシュされます．
<!-- The document is hashed, using one of the following hashing algorithms. -->

以下のような少なくとも128ビットのハッシュアルゴリズムを使用することをお勧めします．
<!-- We recommend using at least a 128-bit hashing algorithm such as the following: -->

- SHA256 - 2<sup>128</sup>
- SHA512 - 2<sup>256</sup>
- SHA-3	- Up to 2<sup>512</sup>
- BLAKE2s - 2<sup>128</sup>
- BLAKE2b - 2<sup>256</sup>

#### ドキュメントを保存する
<!-- #### Saving a document -->

ハッシュされた後，ドキュメントはデータベースに格納され，データベースが返すドキュメント ID は別の場所に保存されるため，アプリケーションは再びドキュメントを見つけることができます．
<!-- After being hashed, the document is stored in a database and the document ID that the database returns is saved elsewhere so the application can find it again. -->

#### ドキュメントのハッシュをタングルにアタッチする
<!-- #### Attaching the hash to the Tangle -->

ドキュメントハッシュはトランザクションの `signatureMessageFragment` フィールドに入れられ，IOTA ノードに送信されてタングルにアタッチされます．
<!-- The document hash is put in the `signatureMessageFragment` field of a transaction and sent to the IOTA node to attach it to the Tangle. -->

#### データベースにトランザクションハッシュを保存する
<!-- #### Saving the transaction hash to the database -->

タングル上のトランザクションハッシュはデータベースに保存されているため，アプリケーションは必要に応じてノードにトランザクションハッシュを返すように要求できます．
<!-- The transaction hash on the Tangle is saved in the database so that the application can ask the node to return it when necessary. -->

### ドキュメントを検証する
<!-- ### Verifying a document -->

ユーザーがドキュメントを検証する場合，アプリケーションは次のことを行います．
<!-- When a user wants to verify a document, the application does the following: -->

1. データベースからトランザクションハッシュを取得する
<!-- 1. Get the transaction hash from the database -->
2. データベースからドキュメントをダウンロードする
<!-- 2. Download the document from the database -->
3. タングル上のトランザクションを読み取る
<!-- 3. Read the transaction on the Tangle -->
4. ドキュメントをハッシュし，その結果とタングル上のトランザクションにある内容と比較する
<!-- 4. Hash the document and compare the results -->

#### データベースからトランザクションハッシュを取得する
<!-- #### Getting the transaction hash from the database -->

タングル上のドキュメントハッシュを読み取るには，ドキュメントハッシュが記載されているトランザクションのハッシュが必要です．
<!-- To be able to read the document hash on the Tangle, we need the hash of the transaction it's in. -->

#### ドキュメントをダウンロードする
<!-- #### Downloading the document -->

ドキュメントをハッシュするには，データベースからダウンロードする必要があります．
<!-- To be able to hash the document, we need to download it from the database. -->

#### タングル上のトランザクションを読み取る
<!-- #### Reading the transaction on the Tangle -->

トランザクションハッシュを取得したら，IOTA ノードにトランザクションを返すように依頼できます．トランザクションには，`signatureMessageFragment` フィールドにドキュメントハッシュが含まれています．
<!-- When we have the transaction hash, we can ask the IOTA node to return us the transaction, which contains the document hash in its `signatureMessageFragment` field. -->

#### ドキュメントハッシュを計算して，比較する
<!-- #### Calculating and comparing the document hash -->

これで，元のドキュメントとタングルにアタッチされたそのドキュメントのハッシュが用意できました．元のドキュメントを再度ハッシュします（以前と同じハッシュアルゴリズムを使用します）．
<!-- Now we have the original document and the hash of that document that was attached to the Tangle, we hash the original document again (using the same hashing algorithm as before). -->

2つのハッシュが同じ場合，ファイルは変更されていません．
<!-- If the two hashes match, the file is unchanged. -->

ハッシュが一致しない場合，ファイルが現在とタングルにアタッチされた時間の間に変更されていることがわかります．
<!-- if the hashes do not match, we know that the file has been changed between now and the time it was attached to the Tangle. -->

![Document hashing](../images/document-immutability-verification2.png)
