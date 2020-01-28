# IRI 構成オプション
<!-- # IRI configuration options -->

**IRI 構成オプションを使用すると、ノードの動作をカスタマイズできます。これらのオプションは、コマンドライン（CL フラグ）または`.ini` 構成ファイル（構成ファイルパラメーター）で設定できます。**
<!-- **The IRI configuration options allow you to customize how your node behaves. You can choose to set these options either in the command line (CL flags) or in a .ini configuration file (config file parameters).** -->

変更したいオプションを見つけやすくするために、次のカテゴリに分類しています。
<!-- To make it easier to find the options you want to change, we've separated them into the following categories: -->

- **API：**API がどのように応答し、どのホストが API にアクセスできるか
<!-- - **API:** How the API responds and which hosts can access it -->
- **IXI：**ノードが IXI モジュールを使用する方法
<!-- - **IXI:** How your node uses IXI modules -->
- **Database：**ノードが台帳で何をするのか
<!-- - **Database:** What your node does with its ledger -->
- **Local snapshots：**ノードが[ローカルスナップショット](root://getting-started/0.1/network/nodes.md#local-snapshots)を実行する方法とタイミング
<!-- - **Local snapshots:** How and when your node does [local snapshots](root://getting-started/0.1/network/nodes.md#local-snapshots) -->
- **Network：**ノードが隣接ノードと通信する方法
<!-- - **Network:** How your node communicates with neighbors -->
- **Proof of work：**ノードが[プルーフオブワーク](root://getting-started/0.1/transactions/proof-of-work.md)を行う方法
<!-- - **Proof of work:** How your node does [proof of work](root://getting-started/0.1/transactions/proof-of-work.md) -->
- **Protocol：**どのトランザクションがネットワークによって受け入れられるのか、そしてトランザクションが他のノードにどのように伝播されるのか
<!-- - **Protocol:** What transactions will be accepted by the network, and how they will be propagated to other nodes -->
- **Testnet：**メインネットで実行していないときにノードが従うべきコーディネーター
<!-- - **Testnet:** Which Coordinator your node should follow when it's not running on the Mainnet -->
- **Tip selection：**[チップ選択](../concepts/tip-selection.md)中の重み付きランダムウォークの長さとランダムさ
<!-- - **Tip selection:** The length and randomness of the weighted random walk during [tip selection](../concepts/tip-selection.md) -->
- **ZMQ：**クライアントがノードの ZMQ イベントをサブスクライブする方法
<!-- - **ZMQ:** How clients can subscribe to your node's ZMQ events -->

:::info:
IRI をダウンロードした場合は、`-help` フラグを付けて IRI を実行して、すべての設定オプションの一覧を見ることもできます。
:::
<!-- :::info: -->
<!-- If you've downloaded the IRI, you can also run it with the `--help` flag to see a list of all the configuration options. -->
<!-- ::: -->

:::info:
`--remote=true` または `--remote=false` のように、すべてのブールフラグにはパラメータが必要です。
:::
<!-- :::info: -->
<!-- All boolean flags require a parameter, for example `--remote=true` or `--remote=false`. -->
<!-- ::: -->

## API

これらの設定を使用して、API の動作方法とアクセスできるホストをカスタマイズします。
<!-- Use these settings to customize how the API behaves and which hosts can access it. -->

|**コマンドラインフラグ**|**構成ファイルパラメーター**|**説明**|**有効な値**|**デフォルト値**|**メモ**|
|:-----------------------|:-------------------------|:-------|:-----------|:---------------|:-------|
|<a name="api-host"></a>`--api-host`|`API_HOST`|API をリッスンするホストを設定します。|string|localhost|任意のホストを受け入れるには、このオプションを`0.0.0.0`に設定するか、`REMOTE` オプションを `true` に設定します。|
|<a name="port"></a>`--port, -p`（**必須**）|`API_PORT`|API をリッスンするポートを設定します。|string|14265|
|<a name="max-body-length"></a>`--max-body-length`|`MAX_BODY_LENGTH`|API 呼び出しのボディに含めることができる最大文字数を設定します。|number|1,000,000|リクエストボディの長さがこの数を超えると、エラーが返されます。|
|<a name="max-find-transactions"></a>`--max-find-transactions`|`MAX_FIND_TRANSACTIONS`|[`findTransactions` エンドポイント](../references/api-reference.md#findTransactions)から返される可能性があるトランザクションの最大数を設定します。|number|100,000|トランザクション数がこの数を超えると、エラーが返されます。|
|<a name="max-requests-list"></a>`--max-requests-list`|`MAX_REQUESTS_LIST`|API 呼び出しパラメーター数の最大数を設定します。|number|1,000|パラメータ数がこの数を超えると、エラーが返されます。|
|<a name="max-get-trytes"></a>`--max-get-trytes`|`MAX_GET_TRYTES`|[getTrytes エンドポイント](../references/api-reference.md#getTrytes)から返される可能性のあるトライト数の最大数を設定します。|number|10,000|トライト数がこの数を超えると、エラーが返されます。|
| <a name="mwm"></a>`--mwm`|`MWM`|[最小重量値](root://getting-started/0.1/network/minimum-weight-magnitude.md)を設定します．|number|9|
|<a name="remote"></a>`--remote`|-|任意のホストへの API インターフェースを開きます。|boolean|false|`true` に設定すると、このオプションは `API_HOST` オプションを`0.0.0.0`に設定するのと同等になります。このフラグの後に `true` または `false` の値を追加しなければなりません。|
|<a name="remote-auth"></a>`--remote-auth`|`REMOTE_AUTH`|`username:password` の形式で API 呼び出しの Basic 認証を追加します。|string|""|プレーンテキストまたはハッシュパスワードを使用できます。|
|<a name="remote-limit-api"></a>`--remote-limit-api`|`REMOTE_LIMIT_API`|特定の API エンドポイントへのリクエストを無視します。|文字列の配列|[[addNeighbors](../references/api-reference.md#addNeighbors), [getNeighbors](../references/api-reference.md#getNeighbors), [removeNeighbors](../references/api-reference.md#removeNeighbors), [attachToTangle](../references/api-reference.md#attachToTangle), [interruptAttachToTangle](../references/api-reference.md#interruptAttachToTangle)]|このオプションを使用すると、IRI ノードの URL または IP アドレスを知っているスパム送信者からノードを保護できます。|
|<a name="remote-trusted-api-hosts"></a>`--remote-trusted-api-hosts`|`REMOTE_TRUSTED_API_HOSTS`|`REMOTE_LIMIT_API` オプションで設定されているものも含め、任意の API エンドポイントを呼び出すことができるホスト。|コンマ区切りの文字列のリスト|localhost|また、 `--remote` オプションを `true` に設定するか、`API_HOST` オプションを`0.0.0.0`に設定する必要があります．|

## Database

これらの設定を使用して、ノードが台帳で実行することをカスタマイズします。
<!-- Use these settings to customize what your node does with its ledger. -->

|**コマンドラインフラグ**|**構成ファイルパラメーター**|**説明**|**有効な値**|**デフォルト値**|**メモ**|
|:-----------------------|:-------------------------|:-------|:-----------|:---------------|:-------|
|<a name="db"></a>`--db`|`MAIN_DB`|トランザクションの保存に使用されるデータベースに名前を付けます。|string|rocksdb|現在、サポートされているデータベースは RocksDB のみです。|
|<a name="db-cache-size"></a>`--db-cache-size`|`DB_CACHE_SIZE`|データベースキャッシュの最大サイズをキロバイト単位で設定します。|number|100,000|
|<a name="db-log-path"></a>`--db-log-path`|`DB_LOG_PATH`|データベースログが保存されるファイルへのパスを設定します。|string|mainnet.log|
|<a name="db-path"></a>`--db-path`|`DB_PATH`|データベースが保存されているディレクトリへのパスを設定します。|string|mainnetdb|
|<a name="rescan"></a>`--rescan`|`RESCAN_DB`|すべてのトランザクションメタデータ（承認者、バンドル、タグ）を再スキャンします。|boolean|false|
|<a name="revalidate"></a>`--revalidate`|`REVALIDATE`|確定済みのトランザクションとトランザクションメタデータに関するデータベースのデータを再読み込みします。|boolean|false|
|`--spent-addresses-db-path`|`SPENT_ADDRESSES_DB_PATH`|使用済みアドレスデータベースが保存されるディレクトリへのパスを設定します。|string|"spent-addresses-db"|
|`--spent-addresses-db-log-path`|`SPENT_ADDRESSES_DB_LOG_PATH`|使用済みアドレスデータベースのログが保存されるディレクトリへのパスを設定します。|string|"spent-addresses-log"|

## IXI

これらの設定を使用して、ノードが IXI モジュールを使用する方法をカスタマイズします。
<!-- Use these settings to customize how your node uses IXI modules. -->

|**コマンドラインフラグ**|**設定ファイルパラメータ**|**説明**|**有効な値**|**デフォルト値**|**メモ**|
|:-----------------------|:-------------------------|:-------|:-----------|:---------------|:-------|
|<a name="ixi-dir"></a>`--ixi-dir`|`IXI_DIR`|自動検出のために IXI モジュールを追加するディレクトリ|string|ixi|

## Local snapshot

これらの設定を使用して、ノードが[ローカルスナップショット](root://getting-started/0.1/network/nodes.md#local-snapshots)を実行する方法とタイミングをカスタマイズします。
<!-- Use these settings to customize how and when your node does [local snapshots](root://getting-started/0.1/network/nodes.md#local-snapshots). -->

|**コマンドラインフラグ**|**構成ファイルパラメーター**|**説明**|**有効な値**|**デフォルト値**|**メモ**|
|:-----------------------|:-------------------------|:-------|:-----------|:---------------|:-------|
|<a name="local-snapshots-enabled"></a>`--local-snapshots-enabled`|`LOCAL_SNAPSHOTS_ENABLED`|ローカルスナップショットを有効にします。|boolean|true|IRI が他の `LOCAL_SNAPSHOTS` パラメーターを読み取るには、このパラメーターを `true` に設定する必要があります。|
|<a name="local-snapshots-pruning-enabled"></a>`--local-snapshots-pruning-enabled`|`LOCAL_SNAPSHOTS_PRUNING_ENABLED`|ノードがデータベースから古いトランザクションを削除できるようにします。|false|トランザクションは、次の計算の結果よりも古いインデックスを持つマイルストーンによって確定された場合に削除されます：現在のマイルストーンインデックス-（`LOCAL_SNAPSHOTS_DEPTH`+`LOCAL_SNAPSHOTS_PRUNING_DELAY`）。|
|<a name="local-snapshots-depth"></a>`--local-snapshots-depth`|`LOCAL_SNAPSHOTS_DEPTH`|`snapshot.meta` ファイルに記録する見られるマイルストーンの量|100から始まる数|100|
|<a name="local-snapshots-pruning-delay"></a>`--local-snapshots-pruning-delay`|`LOCAL_SNAPSHOTS_PRUNING_DELAY`|台帳に保持するマイルストーントランザクションの量|10,000から始まる数|40,000|28日ごとにローカルスナップショットをトリガーするこのオプションのデフォルト値を使用することをお勧めします。|
|<a name="local-snapshots-interval-synced"></a>`--local-snapshots-interval-synced`|`LOCAL_SNAPSHOTS_INTERVAL_SYNCED`|台帳が完全に同期している場合にスナップショットファイルが作成されるマイルストーントランザクションの間隔|number|10|
|<a name="local-snapshots-interval-unsynced"></a>`--local-snapshots-interval-unsynced`|`LOCAL_SNAPSHOTS_INTERVAL_UNSYNCED`|台帳が完全に同期していない場合にスナップショットファイルが作成されるマイルストーントランザクションの間隔|number|1,000|この値は、IRI が隣接ノードとの同期にリソースを集中できるようにするための設定オプション `LOCAL_SNAPSHOTS_INTERVAL_SYNCED` よりも高い。|
|<a name="local-snapshots-base-path"></a>`--local-snapshots-base-path`|`LOCAL_SNAPSHOTS_BASE_PATH`|ファイル拡張子なしのスナップショットファイルへのパス。|string|mainnet|`.snapshot.meta` および `.snapshot.state` ファイルにこのパラメーターの値を付加します。デフォルト値の場合、ファイルの名前は `mainnet.snapshot.meta` および `mainnet.snapshot.state` です。追加するファイルのディレクトリを指定するには、`<directory>/<filename>` を実行します。これにより、`directory/filename.snapshot.meta` が作成されます。|

## Network

これらの設定を使用して、ノードが近隣ノードと通信する方法をカスタマイズします。
<!-- Use these settings to customize how your node communicates with neighbors. -->

|**コマンドラインフラグ**|**構成ファイルパラメーター**|**説明**|**有効な値**|**デフォルト値**|**メモ**|
|:-----------------------|:-------------------------|:-------|:-----------|:---------------|:-------|
|<a name="auto-tethering-enabled"></a>`--auto-tethering`|`AUTO_TETHERING`|不明な隣接ノードからの新しい接続を有効にします。|boolean|false|不明な隣接ノードは、`NEIGHBORS` オプションで定義されていないか、`addNeighbors` API エンドポイントを介して追加されていない隣接ノードです。ノードに接続できる隣接ノードの最大数を制限するには、`MAX_NEIGHBORS` オプションを使用します。|
|<a name="cache-size"></a>`--cache-size`|`CACHE_SIZE_BYTES`|ネットワークキャッシュの最大サイズをバイト単位で設定します。|number|150,000|
|<a name="dns-refresher"></a>`--dns-refresher`|`DNS_REFRESHER`|動的 IP アドレスを持つ隣接ノードに再接続します。|boolean|true|
|<a name="dns-resolution"></a>`--dns-resolution`|`DNS_RESOLUTION`|隣接ノードピアリングの DNS を有効にします。|boolean|true|
|<a name="max-neighbors"></a>`--max-neighbors`|`MAX_NEIGHBORS`|隣接ノードの最大数を設定します。|number|5|このオプションは、少なくとも `NEIGHBORS` オプションにある URL と同じ数に設定する必要があります。8つ以上の隣接ノードに接続しないことをお勧めします。そうしないと、ノードの同期が困難になります。|
|<a name="neighbors"></a>`-n`, `--neighbors`|`NEIGHBORS`|[隣接ノード](../how-to-guides/find-neighbor-iri-nodes.md)の URL と IP アドレスを設定します。|array of strings |[""]|
|<a name="neighboring-socket-address"></a>`--neighboring-socket-address`|`NEIGHBORING_SOCKET_ADDRESS`|ゴシッププロトコルの TCP サーバーソケットをアドレスにバインドします。|string|0.0.0.0|デフォルトアドレスは、TCP サーバーソケットをすべてのネットワークインターフェイスにバインドします。ノードに複数の IP アドレスがあり、特定の IP アドレスのみにバインドする場合は、このオプションを変更します。|
|<a name="neighboring-socket-port"></a>`--neighboring-socket-port`|`NEIGHBORING_SOCKET_PORT`|ゴシッププロトコル用の TCP サーバーソケットをポートにバインドします。|string|15600|
|<a name="p-drop-cache"></a>`--p-drop-cache`|`P_DROP_CACHE_ENTRY`|ネットワークキャッシュで最近見たトランザクションを失う可能性を設定します。|0から1の間の数|0.02|
|<a name="p-remove-request"></a>`--p-remove-request`|`P_REMOVE_REQUEST`|IRI がトランザクションのリクエストを停止する確率を設定します。|0から1の間の数|0.01|存在しないトランザクションハッシュが最終的に削除されるように、この数は0に近い必要があります。|
|<a name="queue-size"></a>`--queue-size`|`Q_SIZE_NODE`|REPLY、BROADCAST、および RECEIVE ネットワークキューの最大サイズを設定します。|number|1,000|
|<a name="reconnect-attempt-interval-seconds"></a>`--reconnect-attempt-interval-seconds`|`RECONNECT_ATTEMPT_INTERVAL_SECONDS`|切断した隣接ノードへの再接続を試みる前に待機する秒数を設定します。|number|60|

## Proof of work

これらの設定を使用して、ノードがどのように[プルーフオブワーク](root://getting-started/0.1/transactions/proof-of-work.md)するかをカスタマイズします。
<!-- Use these settings to customize how your node does [proof of work](root://getting-started/0.1/transactions/proof-of-work.md). -->

|**コマンドラインフラグ**|**構成ファイルパラメーター**|**説明**|**有効な値**|**デフォルト値**|**メモ**|
|:-----------------------|:-------------------------|:-------|:-----------|:---------------|:-------|
|<a name="pow-threads"></a>`--pow-threads`|`POW_THREADS`|プルーフオブワークの計算に使用するスレッドの数|number|0|このパラメーターが0に設定されている場合、PoW スレッドの数は、デバイスのコアの数に応じて異なります。|

## Protocol

これらの設定を使用して、どのトランザクションがネットワークによって受け入れられるか、およびそのトランザクションが他のノードにどのように伝搬されるかをカスタマイズします。
<!-- Use these settings to customize which transactions will be accepted by the network, and how they will be propagated to other nodes. -->

|**コマンドラインフラグ**|**構成ファイルパラメーター**|**説明**|**有効な値**|**デフォルト値**|**メモ**|
|:-----------------------|:-------------------------|:-------|:-----------|:---------------|:-------|
|<a name="p-drop-transaction"></a>`--p-drop-transaction`|`P_DROP_TRANSACTION`|受け取ったトランザクションを失う確率を設定します。|0から1の間の数|0.0|このオプションはテスト目的でのみ利用可能です。|
|<a name="p-propagate-request"></a>`--p-propagate-request`|`P_PROPAGATE_REQUEST`|IRI が隣接ノードからの紛失トランザクションをリクエストする確率を設定します。| number|0.01|この数は、スパムトランザクションをIRIがリクエストするのを避けるために、低くする必要があります。|
|<a name="p-reply-random"></a>`--p-reply-random`|`P_REPLY_RANDOM_TIP`|送信するトランザクションがない場合でも、IRI がランダムなトランザクションリクエストに応答する確率を設定します。|number|0.66|
|<a name="p-select-milestone"></a>`--p-select-milestone`|`P_SELECT_MILESTONE_CHILD`|IRI が隣接ノードからマイルストーントランザクションをリクエストする確率を設定します。|0から1の間の数|0.7|トランザクションが確定したと見なされるには、IRI が順番にマイルストーンを見つけることが不可欠であるため、この数は大きくする必要があります。|
|<a name="p-send-milestone"></a>`--p-send-milestone`|`P_SEND_MILESTONE`|IRI がマイルストーントランザクションを隣接ノードへのランダム返信として送信する確率を設定します。|0から1の間の数|0.02|

## Testnet

これらの設定を使用して、ノードがテストネット（メインネット以外のネットワーク）上のトランザクションを検証および確定する方法をカスタマイズします。
<!-- Use these settings to customize how your node validates and confirms transactions on a testnet (any network other than the Mainnet). -->

|**コマンドラインフラグ**|**構成ファイルパラメーター**|**説明**|**有効な値**|**デフォルト値**|**メモ**|
|:-----------------------|:-------------------------|:-------|:-----------|:---------------|:-------|
|<a name="testnet"></a>`--testnet`|`TESTNET`|メインネット以外のネットワークでノードを実行できるようにします。|boolean|false|IRI が他のテストネットパラメーターを読み取るには、このパラメーターを `true` に設定する必要があります。このフラグのみを含める場合、デフォルトのテストネット構成オプションはデブネット用です。|
|`--testnet-coordinator`|`COORDINATOR`|テストネットのコーディネーターの81トライアドレスを設定します。|81トライト文字列|"EQQFCZBIHRHWPXKMTOLMYUYPCN9XLMJPYZVFJSAY9FQHCCLWTOLLUGKKMXYFDBOOYFBLBI9WUEILGECYM"|このアドレスはコーディネーターのマークルルートです。|
|`--testnet-no-coo-validation`|`DONT_VALIDATE_TESTNET_MILESTONE_SIG`|マイルストーンの検証を無効にします。|boolean|false|
|`--testnet-coordinator-security-level`|`COORDINATOR_SECURITY_LEVEL`|コーディネーターのアドレスと秘密鍵のセキュリティレベルを設定します。|number|1|
|`--testnet-coordinator-signature-mode`|`COORDINATOR_SIGNATURE_MODE`|テストネットのコーディネーターがマイルストーンに署名するために使用する署名モードを設定します。|string(KERL, CURLP27, または CURLP81)|CURLP27|
|`--milestone-keys`|`NUMBER_OF_KEYS_IN_A_MILESTONE`|コーディネーターのマークル木の深さを設定します。|number|22|
||`LOCAL_SNAPSHOTS_BASE_PATH`|スナップショットファイルのベースパスを設定します。|string|"testnet"|
|`--snapshot`|`SNAPSHOT_FILE`|スナップショットの `.txt` ファイルへのパスを設定します。|string|"/snapshotTestnet.txt"|このパスは、ローカルスナップショットのベースパスに相対的である必要があります。|
|`--snapshot-sig`|`SNAPSHOT_SIG`|スナップショットファイルの署名へのパスを設定します。|string|"/snapshotTestnet.sig"|このパスは、ローカルスナップショットのベースパスに相対的である必要があります。|
|`--snapshot-timestamp`|`SNAPSHOT_TIME`|ノードが新しいトランザクションを検証できるように、新しいトランザクションが持つことができる最も古い接続時間を設定します。|Unix時間|1522306500|
|`----milestone-start`|`MILESTONE_START_INDEX`|ノードがトランザクションの検証と確認に使用する開始マイルストーンインデックスを設定します。|number|434525|
||`DB_PATH`|ノードが台帳を保存するために使用するパスを設定します。|string|"testnetdb"|
||`DB_LOG_PATH`|ログファイルの保存にノードが使用するパスを設定します。|string|"testnetdb.log"|

## Tip selection

これらの設定を使用して、[チップ選択](../concepts/tip-selection.md)中の重み付きランダムウォークの長さとランダムさをカスタマイズします。
<!-- Use these settings to customize the length and randomness of the weighted random walk during [tip selection](../concepts/tip-selection.md). -->

|**コマンドラインフラグ**|**構成ファイルパラメーター**|**説明**|**有効な値**|**デフォルト値**|**メモ**|
|:-----------------------|:-------------------------|:-------|:-----------|:---------------|:-------|
|<a name="alpha"></a>`--alpha`|`ALPHA`|チップ選択プロセスのランダムさを設定します。|0から無限大の間の数|0.001|数値0が最もランダムで、無限大が最も決定的です。このオプションの詳細な説明については、[ブログの投稿](https://blog.iota.org/alpha-d176d7601f1c)を参照してください。|
|<a name="max-analyzed-transactions"></a>`--max-analyzed-transactions`|`MAX_ANALYZED_TXS`|チップ選択中に分析される可能性のある未確定トランザクションの最大数を設定して、未確定トランザクションを参照する最新のマイルストーンを見つけます。|number|20,000|
|<a name="max-depth"></a>`--max-depth`|`MAX_DEPTH`|IRI がチップ選択を開始する場所からの過去のマイルストーン数（深さ）の最大数を設定します。|number|15|この値は、重み付きランダムウォークが妥当な時間内に終了するのに十分なほど小さく、かつ[部分グラフ](../concepts/tip-selection.md#subgraph-selection)に十分な新しいトランザクションが含まれるのに十分なほど大きい必要があります。|
|<a name="tip-selection-timeout-sec"></a>`--tip-selection-timeout-sec`|`TIP_SELECTION_TIMEOUT_SEC`|IRI がチップ選択を完了するために費やすことができる最大秒数を設定します。|number|60|このオプションは、チップ選択に時間がかかりすぎる場合に、ノードが停止するのを防ぎます。|

## ZMQ

これらの設定を使用して、クライアントがノードの ZMQ イベントをサブスクライブする方法をカスタマイズします。
<!-- Use these settings to customize how clients can subscribe to your node's ZMQ events. -->

| **コマンドラインフラグ** | **構成ファイルパラメーター** | **説明** | **有効な値** | **デフォルト値** | **メモ** |
| :------------------- | :--------------------- | :--- | :------- | :----------- | :--- |
| <a name="zmq-enabled"></a>`--zmq-enable-ipc` | `ZMQ_ENABLE_IPC` | `ipc://iri` アドレスの IPC を介した ZMQ サブスクリプションを有効にします。 | boolean | false | デフォルトアドレスを変更するには、`ZMQ_IPC` オプションを設定します。 |
| `--zmq-enable-tcp` | `ZMQ_ENABLE_TCP` | ポート5556で TCP を介した ZMQ サブスクリプションを有効にします。 | boolean | false | デフォルトポートを変更するには `ZMQ_PORT` オプションを設定します。 |
| <a name="zmq-ipc"></a>`--zmq-ipc` | `ZMQ_IPC` | IPC を介して ZMQ と通信するために使用されるアドレスを設定します。 | string | ipc://iri |
| <a name="zmq-port"></a>`--zmq-port` | `ZMQ_PORT` | TCP 経由で ZMQ に接続するために使用されるポートを設定します。 | string | 5556 |
| <a name="zmq-threads"></a>`--zmq-threads` | `ZMQ_THREADS` | ZMQ 発行者が使用できるスレッドの最大数を設定します。 | number | 1 |
