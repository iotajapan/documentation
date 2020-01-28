# IRI を更新する
<!-- # Update IRI -->

**IRI はベータ開発段階にあるため，変更される可能性があります．つまり，更新する必要があります．このガイドでは，IRI を最新バージョンに更新します．**
<!-- **IRI is in a beta development stage, so it's likely to change, meaning that you'll need to update it. In this guide, you update IRI to the latest version.** -->

新しいバージョンの IRI が [GitHub でリリース](https://github.com/iotaledger/iri/releases)されたら，ローカルの IRI を新しいバージョンに更新することをお勧めします．
<!-- When a new version of [IRI is released on GitHub](https://github.com/iotaledger/iri/releases), we recommend that you update your local IRI to the new version. -->

1. IRI を止めます．
  <!-- 1. Stop IRI -->

2. `mainnetdb` ディレクトリ，`mainnet.log` ファイル，および IRI の `.jar` ファイルを削除します．
  <!-- 2. Delete the `mainnetdb` directory, the `mainnet.log` file, and the IRI `.jar` file -->

    :::warning:
    `spent-address-db` ディレクトリは削除しないでください．
    :::
    <!-- :::warning: -->
    <!-- Do not delete your `spent-address-db` directory. -->
    <!-- ::: -->

3. [最新バージョンの IRI をダウンロードし，実行します](../how-to-guides/install-iri.md#run-the-iri)．
<!-- 3. [Download the latest version of IRI and run it](../how-to-guides/install-iri.md) -->

## 次のステップ
<!-- ## Next steps -->

[IRI を実行する](../how-to-guides/install-iri.md)．
<!-- [run IRI](../how-to-guides/install-iri.md) -->
