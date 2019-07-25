# トラブルシューティング
<!-- # Troubleshooting -->

**このリファレンスガイドを使用して、IRIのインストール、設定、または実行に関連する問題を解決してください。**
<!-- **Use this reference guide to resolve issues related to installing, configuring, or running the IRI.** -->

問題の解決策が見つからない場合は、[IOTA StackExchange](https://iota.stackexchange.com/)で検索してください。
<!-- If you can't find the solution to your issue, search the [IOTA StackExchange](https://iota.stackexchange.com/). -->

## エラー - trustAnchorsパラメータは空でない必要があります
<!-- ## Error - trustAnchors parameter must be non-empty -->

Ubuntu 18でIRIをコンパイルすると、次のエラーが表示されることがあります。
<!-- When you compile the IRI on Ubuntu 18, you may see the following error: -->
```
java.security.InvalidAlgorithmParameterException: the trustAnchors parameter must be non-empty
```

このエラーと解決策の詳細については、この[Stack Overflowの回答](https://stackoverflow.com/questions/6784463/error-trustanchors-parameter-must-be-non-empty)を参照してください。
<!-- See some details about this error and the solution in this [Stack Overflow answer](https://stackoverflow.com/questions/6784463/error-trustanchors-parameter-must-be-non-empty). -->

## IRIが隣接ノードと同期しない
<!-- ## The IRI won't synchronize with its neighbors -->

IRIが同期するまでには時間がかかることがあります。IRIが自動的に同期するかどうかを確認するためにしばらくお待ちください。
<!-- It may take some time for the IRI to synchronize. Wait a while to see if the IRI synchronizes by itself. -->

* [6か7の隣接ノードがいることを確認してください](../how-to-guides/find-neighbor-iri-nodes.md)。
<!-- * [Make sure that you have 6 or 7 neighbors](../how-to-guides/find-neighbor-iri-nodes.md) -->

* [最新版のIRI](https://github.com/iotaledger/iri/releases)を実行していることを確認してください。
<!-- * Make sure that you're running the [latest version of the IRI](https://github.com/iotaledger/iri/releases) -->

* IRIと隣接ノードが互いにデータを送信しあっていることを確認してください。getNeighbors APIメソッドを呼び出して、着信トランザクション（`numberOfAllTransactions`）と発信トランザクション数（`numberOfSentTransactions`）の両方を確認します。隣接ノードがIRIにデータを送っていない場合は、接続する新しい隣接ノードを見つけてください。
  <!-- * Make sure that the IRI and its neighbors are sending data among each other. Call the getNeighbors API method to see both the incoming transactions (`numberOfAllTransactions`) and the number of outgoing transactions (`numberOfSentTransactions`). If your neighbors aren't sending you data, find new neighbors to connect to. -->

    ```bash
    curl http://localhost:14265 -X POST -H 'Content-Type: application/json' -H 'X-IOTA-API-Version: 1' -d '{"command": "getNeighbors"}'
    ```

* [Discord](https://discord.iota.org)の#helpおよび#fullnodesチャンネルでサポートを求めてください。
<!-- * Ask for more support on [Discord](https://discord.iota.org) in our #help and #fullnodes channels -->

:::info:
最新のデータベースファイルは[dbfiles.iota.org](https://dbfiles.iota.org/?prefix=)からダウンロードできます。最新のデータベースファイルをダウンロードして解凍することで、ノードは隣接ノードとより速く同期することができます。
:::
<!-- :::info: -->
<!-- You can download the latest database files from [dbfiles.iota.org](https://dbfiles.iota.org/?prefix=). -->
<!-- By downloading and extracting the latest database files, your node can synchronize faster with its neighbors. -->
<!-- ::: -->
