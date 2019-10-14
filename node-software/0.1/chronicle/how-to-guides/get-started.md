# Linuxサーバーでクロニクルを実行する
<!-- # Run Chronicle on a Linux server -->

**クロニクルを実行すると、1つ以上のIRIノードでトランザクションイベントを購読し、そのトランザクションデータをScyllaDBクラスターに保存します。このようにして、ローカルスナップショット中にIRIノードがいつ台帳からプルーニングするかに関係なく、すべてのトランザクションデータが保存されます。**
<!-- **When you run Chronicle, it subscribes to transaction events on one or more IRI nodes, then saves that transaction data to a ScyllaDB cluster. This way, all transaction data is saved no matter when/if the IRI node prunes it from its ledger during a local snapshot.** -->

:::warning:
このプロジェクトは、実稼働での使用を目的としていません。

これはアルファプロジェクトであり、Rustで引き続き開発および書き換えを行います。Rust版は、リソースおよびメモリ管理の点でElixirよりも高速で効率的です。
:::
<!-- :::warning: -->
<!-- This project is not intended for production use. -->

<!-- This is an alpha project that we will continue to develop and rewrite in Rust, which is faster and more efficient than Elixir in terms of resource and memory management. -->
<!-- ::: -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完了するには、次のものが必要です。
<!-- To complete this guide, you need the following: -->

- [単一のデータセンターにScyllaクラスターを作成](https://docs.scylladb.com/operating-scylla/procedures/cluster-management/create_cluster/)（Scyllaバージョン3.0.6以降）します。2019年現時点では、クロニクルは単一のデータセンター内のクラスターのみをサポートしています。
<!-- - [Create a Scylla cluster on a single data center](https://docs.scylladb.com/operating-scylla/procedures/cluster-management/create_cluster/) (Scylla version 3.0.6 or later). At the moment, Chronicle supports only clusters in a single data center. -->

- [Elixir](https://elixir-lang.org/install.html)バージョン1.8.1以降およびErlang VMバージョン22以降（Elixirに付属）をインストールします。パッケージマネージャーがこれらのバージョンを提供していない場合は、[このツール](https://github.com/asdf-vm/asdf)を使用してください。
<!-- - [Install Elixir](https://elixir-lang.org/install.html) version 1.8.1 or later and Erlang VM version 22 or later (included with Elixir). If your package manager doesn't offer these versions, use [this tool](https://github.com/asdf-vm/asdf ). -->

- [Phoenix](https://hexdocs.pm/phoenix/installation.html)をインストールします。
<!-- - [Install Phoenix](https://hexdocs.pm/phoenix/installation.html) -->

- [Bazel](https://docs.bazel.build/versions/master/install.html)をインストールします。
<!-- - [Install Bazel](https://docs.bazel.build/versions/master/install.html) -->

Scyllaクラスターをセットアップするときは、次のことを考慮してください。
<!-- When setting up your Scylla cluster, consider the following: -->

- **データ送信：** ScyllaDBとChronicle間の通信には、少なくとも9000バイトのMTU（最大送信単位）を持つフレームを推奨します。
<!-- - **Data transmission:** We recommend frames with an MTU (maximum transmission unit) of at least 9000 bytes for communications between ScyllaDB and Chronicle. -->

- **停電：** ChronicleおよびScyllaDBを実行しているデバイスには、バックアップ電源とインターネット接続が必要です。少なくとも1つのアクティブノードが同じクエリを作成している場合、多数のノードの停電はデータの一貫性に影響しません。
<!-- - **Power outage:** Devices running Chronicle and ScyllaDB should have a backup power supply and Internet connection. A power outage for a number of nodes will not affect data consistency if you have at least one active node writing the same queries. -->

- **データセキュリティ：** IOTAトランザクションは、信頼できるデータと価値の記録を提供するため、このデータをクロニクルで保護することが重要です。クロニクルデータはScyllaデータベースに保存されるため、[公式の指示](https://docs.scylladb.com/operating-scylla/security/security_checklist/)に従って、承認、認証、暗号化、およびセキュリティ監査を設定できます。
<!-- - **Data security:** IOTA transactions provide a trustworthy record of data and value, so securing this data in Chronicle is important. Because Chronicle data is stored in a Scylla database, you can follow the [official instructions](https://docs.scylladb.com/operating-scylla/security/security_checklist/) for setting up authorization, authentication, encryption, and security audits. -->

## 手順1. クロニクルを設定する
<!-- ## Step 1. Configure Chronicle -->

1. リポジトリのクローンを作成し、`chronicle`ディレクトリに移動します。
  <!-- 1. Clone the repo and change into the `chronicle` directory -->

    ```bash
    git clone https://github.com/iotaledger/chronicle.git
    cd chronicle
    ```

2. `core`ディレクトリ内の`config.exs`ファイルを開きます。
  <!-- 2. Open the core `config.exs` file -->

    ```bash
    sudo nano apps/core/config/config.exs
    ```

3. `__ DATA_CENTERS__`オブジェクトで、IPアドレスを`127.0.0.1`からScyllaクラスターのIPアドレスに変更します。
  <!-- 3. In the `__ DATA_CENTERS__` object, change the IP address from `127.0.0.1` to the IP address of your Scylla cluster -->

4. `broker`ディレクトリ内の`config.exs`ファイルを開きます。
  <!-- 4. Open the broker `config.exs` file -->

    ```bash
    sudo nano apps/broker/config/config.exs
    ```

5. `tx_trytes`配列で、ZMQが有効になっている1つ以上のノードのURLを追加します。
  <!-- 5. In the `tx_trytes` array, add the URL of one or more nodes that have ZMQ enabled -->

    ```bash
    {'zmq.iota.org',5556}
    ```

6. 2016年11月から2019年7月までの[過去のタングルデータ](http://u204324-sub2.your-storagebox.de/)をダウンロードします。
  <!-- 6. [Download the historical Tangle data](http://u204324-sub2.your-storagebox.de/) from November 2016 to July 2019 -->

    **Username:** u204324-sub2
    **Password:** Ldtd22LiXaztAPUg

7. ダウンロードしたファイルを `/historical/data`ディレクトリに移動します。
  <!-- 7. Move the downloaded files to the `/historical/data` directory -->

8. `historical`ディレクトリに移動します。
  <!-- 8. Change into the `historical` directory -->

    ```bash
    cd ..
    ```

9. `dmps.txt`ファイルに、`/historical/ data`ディレクトリに移動したファイルの名前（`.dmp`ファイル拡張子なし）がすべて含まれていることを確認してから、ファイルを閉じます。
  <!-- 9. Make sure that the `dmps.txt` file has all the names of the files that you moved to the `/historical/data` directory (without the `.dmp` file extension), then close the file -->

## 手順2. クロニクルをコンパイルする
<!-- ## Step 2. Compile Chronicle -->

1. 依存関係をインストールします。
  <!-- 1. Install the dependencies -->

    ```bash
    mix deps.get
    ```

2. フェニックスシークレットを生成し、クリップボードにコピーします。
  <!-- 2. Generate the phoenix secret and copy it to the clipboard -->

    ```bash
    mix phx.gen.secret
    ```

3. プロジェクトをコンパイルします。
  <!-- 3. Compile the project -->

    ```bash
    mix deps.compile
    mix compile
    ```

## 手順3. クロニクルを実行します。
<!-- ## Step 3. Run Chronicle -->

Chronicleを実行するには、次のコマンドを実行します。
<!-- To run Chronicle, execute the following command: -->

```bash
SECRET_KEY_BASE=theGenerated64-byteSecretString PORT=4000 HOST=localhost MIX_ENV=prod elixir --name app@hostname --cookie "MySecretChronicleCookie" -S mix run --no-halt
```

* `SECRET_KEY_BASE`：手順2.2のフェニックスシークレット
<!-- * `SECRET_KEY_BASE`: Your Phoenix secret from step 2.2 -->
* `PORT`：APIサーバーがリッスンするポート
<!-- * `PORT`: The port that you want the API server to listen to -->
* `HOST`：APIがリッスンするホスト名またはIPアドレス
<!-- * `HOST`: The hostname or IP address that you want the API to listen to -->

:::info:
ホストがlocalhostの場合、`--name`フラグの値は`Chronicle@localhost`になります。それ以外の場合は、localhostをホスト名に置き換えます。

`--cookie`フラグの値は秘密にします。
:::
<!-- :::info: -->
<!-- If the host is localhost, then the value of the `--name` flag can be `Chronicle@localhost`. Otherwise, replace localhost with your hostname. -->

<!-- Keep the value of the `--cookie` flag secret. -->
<!-- ::: -->

次のようなものが表示されるはずです。
<!-- You should see something like the following: -->

```bash
20:57:18.560 [info] TxFeeder is ready, topic: tx_trytes, host: zmq.iota.org
20:57:18.807 [info] Running ExtendedApiWeb.Endpoint with cowboy 2.6.3 at :::4000 (http)
20:57:18.822 [info] Access ExtendedApiWeb.Endpoint at http://localhost:4000
```

Chronicleが起動すると、`imported the last dmp 'milestone' file`メッセージが表示されます。このメッセージが表示されたら、`dmps.txt`ファイルを開いて空であることを確認することにより、インポートが成功したことを確認できます。
<!-- When Chronicle starts, you should see the `imported the last dmp 'milestone' file` message. When you see this message, you can check that the import was successful by opening the `dmps.txt` file and seeing that it's empty. -->

:::success: おめでとうございます:tada:
クロニクルはすべてのトランザクションデータをScyllaクラスターに保存しています。
:::
<!-- :::success: Congratulations :tada: -->
<!-- Chronicle is now saving all transaction data to your Scylla cluster. -->
<!-- ::: -->

## 手順4. クロニクルをテストする
<!-- ## Step 4. Test Chronicle -->

クロニクルがトランザクションデータを受信していることを確認するには、APIを使用してScyllaクラスターをクエリします。
<!-- To make sure that Chronicle is receiving transaction data, use the API to query your Scylla cluster. -->

1. [thetangle.org](https://thetangle.org/)にアクセスし、トランザクションハッシュをクリップボードにコピーします。
  <!-- 1. Go to [thetangle.org](https://thetangle.org/) and copy a transaction hash to your clipboard -->

2. コマンドプロンプトで、クロニクルAPIの`getTrytes`エンドポイントへのcURLリクエストを作成します。トランザクションハッシュを`hashes`配列に貼り付け、URLを`http://localhost:4000/api`などのクロニクルノードのURLに置き換えます。
  <!-- 2. In the command prompt create a cURL request to the `getTrytes` endpoint of the Chronicle API. Paste your transaction hash into the `hashes` array, and replace the URL with the URL of your Chronicle node such as http://localhost:4000/api -->

    ```bash
    curl http://host:port/api \
    -X POST \
    -H 'Content-Type: application/json' \
    -H 'X-IOTA-API-Version: 1' \
    -d '{
    "command": "getTrytes",
    "hashes": [""]
    }'
    ```

次のようなものが表示されるはずです。
<!-- You should see something like the following: -->

```json
{
"trytes": [{"address":"JPYUAV9MBDZG9ZX9BAPBBMYFEVORNBIOOZCYPZDZNRGKQYT9HFEXXXBG9TULULJIOWJWQMXSPLILOJGJG","attachmentTimestamp":1567166602904,"attachmentTimestampLowerBound":0,"attachmentTimestampUpperBound":12,"branchTransaction":"OLZDBGOWXCLNZPJZMFUVYPL9COCBSHPIJGIN9L9SNUMMYVZQNDVOCWOYYGJXKHEJGWANXWRBVELB99999","bundle":"VVCRIZWRJ9GOUJRXRBWDEULYKIBIZNKIWGCWZCWVBTVIBAHKVTWLGYQNIZ9JCZJKVEAXABBAUEIGNGWP9","currentIndex":0,"lastIndex":0,"nonce":"JVF9999999RMF99999999999999","obsoleteTag":"HUIWONTO9999999999999999999","signatureMessageFragment":"ODGAHDLDGDNCGDIDRCRCTCTCSCTCSCGADBZAABZACBCBXAABQAGAHDLDGDNCUCPCXC9DTCSCGADB9BBBABQAGAQCPCSCNCQCFDPCBDRCWCGADBVAUAVAZAQAGAQCPCSCNCHDFDIDBDZCGADBVAUAVAZAQAGAQCPCSCNCHDF...","snapshotIndex":null,"tag":"999GOPOW9ATTACHTOTANGLE9ZIG","timestamp":1567166602,"trunkTransaction":"BXZWFMSFBAYWJKJUAKWYTUCZRY9GMNETX9MLN9UKRR9ORGRRIENPERNWCLHBCE9XBMYHAMGFYRRL99999","value":0}]
}
```

:::info:
`snapshotIndex`フィールドは、トランザクションを確定したマイルストーンのインデックスです。

このフィールドが`null`の場合、トランザクションはペンディング中です。
:::
<!-- :::info: -->
<!-- The `snapshotIndex` field is the index of the milestone that confirmed the transaction. -->

<!-- If this field is `null`, the transaction is pending. -->
<!-- ::: -->

:::warning:
現時点では、トランザクションがいつ確定されたかはクロニクルにはわかりません。

この問題は、IRIの次のバージョンが[このプルリクエスト](https://github.com/iotaledger/iri/pull/1551)と一緒にリリースされるときに解決されます。
:::
<!-- :::warning: -->
<!-- At the moment, Chronicle does not know when a transaction is confirmed. -->

<!-- This issue will be solved when the next version of the IRI is released with [this  pull request](https://github.com/iotaledger/iri/pull/1551). -->
<!-- ::: -->

:::success: おめでとうございます:tada:
クロニクルデータベースのデータを正常に照会できます。
:::
<!-- :::success: Congratulations :tada: -->
<!-- You can successfully query the data in the Chronicle database. -->
<!-- ::: -->

## 手順5. プロジェクトに貢献する
<!-- ## Step 5. Contribute to the project -->

クロニクルを実行しているので、[GitHubリポジトリ](https://github.com/iotaledger/chronicle)を参照して、問題に貢献し、開発の最新情報を入手してください。
<!-- Now that you've got Chronicle running, see the [GitHub repository](https://github.com/iotaledger/chronicle) to contribute to issues and to keep up to date with development. -->

これはアルファプロジェクトであり、Rustで引き続き開発および書き換えを行います。Rust版は、リソースおよびメモリ管理の点でElixirよりも高速で効率的です。
<!-- This is an alpha project that we will continue to develop and rewrite in Rust, which is faster and more efficient than Elixir in terms of resource and memory management. -->
