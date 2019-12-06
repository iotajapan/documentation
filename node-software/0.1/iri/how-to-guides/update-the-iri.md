# IRIを更新する
<!-- # Update IRI -->

**IRIはベータ開発段階にあるため、変更される可能性があります。つまり、更新する必要があります。**
<!-- **IRI is in a beta development stage, so it's likely to change, meaning that you'll need to update it.** -->

新しいバージョンのIRIが[GitHubでリリース](https://github.com/iotaledger/iri/releases)されたら、ローカルのIRIを新しいバージョンに更新することをお勧めします。
<!-- When a new version of [IRI is released on GitHub](https://github.com/iotaledger/iri/releases), we recommend that you update your local IRI to the new version. -->

1. IRIを止めます。
  <!-- 1. Stop IRI -->

2. `mainnetdb`ディレクトリ、`mainnet.log`ファイル、および現在のIRIの`.jar`ファイルを削除します。
  <!-- 2. Delete the `mainnetdb` directory, the `mainnet.log` file, and your current IRI `.jar` file -->

    :::warning:
    `spent-address-db`ディレクトリは削除しないでください。
    :::
    <!-- :::warning: -->
    <!-- Do not delete your `spent-address-db` directory. -->
    <!-- ::: -->

3. [最新のIRIをダウンロードし、実行します](../how-to-guides/install-iri.md#run-the-iri)。
<!-- 3. [Download the latest IRI and run it](../how-to-guides/install-iri.md#run-the-iri) -->
