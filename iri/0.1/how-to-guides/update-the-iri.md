# IRIを更新する
<!-- # Update the IRI -->

**IRIはベータ開発段階にあるため、変更される可能性があります。つまり、更新する必要があります。**
<!-- **The IRI is in a beta development stage, so it's likely to change, meaning that you'll need to update it.** -->

新しいバージョンのIRIが[GitHubでリリース](https://github.com/iotaledger/iri/releases)されたら、ローカルのIRIを新しいバージョンに更新することをお勧めします。
<!-- When a new version of the [IRI is released on GitHub](https://github.com/iotaledger/iri/releases), we recommend that you update your local IRI to the new version. -->

1. IRIを止めます。
  <!-- 1. Stop the IRI -->

2. `mainnetdb`ディレクトリ、`mainnet.log`ファイル、および現在のIRIの`.jar`ファイルを削除します。
  <!-- 2. Delete the `mainnetdb` directory, the `mainnet.log` file, and your current IRI .jar file -->

    :::warning:
    `spent-address-db`ディレクトリは削除しないでください。
    :::
    <!-- :::warning: -->
    <!-- Do not delete your `spent-address-db` directory. -->
    <!-- ::: -->

3. [最新のIRIをダウンロードします](../how-to-guides/run-an-iri-node-on-linux.md#ビルド済みのIRI-Javaファイルをダウンロードする)。
  <!-- 4. [Download the latest IRI](../how-to-guides/run-an-iri-node-on-linux.md#download-the-pre-built-iri-java-file) -->

4. [IRIを実行します](../how-to-guides/run-an-iri-node-on-linux.md#IRIを実行する)。
  <!-- 5. [Run the IRI](../how-to-guides/run-an-iri-node-on-linux.md#run-the-iri) -->
