# IRI設定オプション
<!-- # IRI configuration options -->

**IRI設定オプションを使用すると、ノードの動作をカスタマイズできます。これらのオプションは、コマンドライン（CLフラグ）または.ini設定ファイル（設定ファイルパラメータ）のどちらでも設定できます。**
<!-- **The IRI configuration options allow you to customize your node. You can choose to set these options either in the command line (CL flags) or in a .ini configuration file (config file parameters).** -->

変更したいオプションを見つけやすくするために、次のカテゴリに分類しました。
<!-- To make it easier to find the options you want to change, we've separated them into the following categories: -->

* **API：** APIがどのように応答し、どのホストがAPIにアクセスできるか
<!-- * **API:** How the API responds and which hosts can access it -->
* **Network：** ノードが隣接ノードと通信する方法
<!-- * **Network:** How your node communicates with neighbors -->
* **IXI：** ノードがIXIモジュールを使用する方法
<!-- * **IXI:** How your node uses IXI modules -->
* **Ledger：** ノードが台帳で何をするのか
<!-- * **Ledger:** What your node does with its ledger -->
* **Protocol：** どのトランザクションがネットワークによって受け入れられるのか、そしてトランザクションが他のノードにどのように伝播されるのか
<!-- * **Protocol:** What transactions will be accepted by the network, and how they will be propagated to other nodes -->
* **ZMQ：** クライアントが[ノードのZMQイベントを購読する](../how-to-guides/subscribe-to-events-in-an-iri-node.md)方法
<!-- * **ZMQ:** How clients can [subscribe to your node's ZMQ events](../how-to-guides/subscribe-to-events-in-an-iri-node.md) -->
* **Tip selection：** [チップ選択](root://the-tangle/0.1/concepts/tip-selection.md)中の重み付きランダムウォークの長さとランダムさ
<!-- * **Tip selection:** The length and randomness of the weighted random walk during [tip selection](root://the-tangle/0.1/concepts/tip-selection.md) -->
* **Tip solidification：** チップトランザクションが凝固になる方法
<!-- * **Tip solidification:** How tip transactions become solid. -->
* **Proof of work：** ノードが[プルーフオブワーク](root://the-tangle/0.1/concepts/proof-of-work.md)を行う方法
<!-- * **Proof of work:** How your node does [proof of work](root://the-tangle/0.1/concepts/proof-of-work.md) -->
* **Local snapshots：** ノードが[ローカルスナップショット](../concepts/local-snapshot.md)を実行する方法とタイミング
<!-- * **Local snapshots:** How and when your node does [local snapshots](../concepts/local-snapshot.md) -->

:::info:
IRIをダウンロードした場合は、 `-help`フラグを付けてIRIを実行して、すべての設定オプションの一覧を見ることもできます。
:::
<!-- :::info: -->
<!-- If you've downloaded the IRI, you can also run it with the `--help` flag to see a list of all the configuration options. -->
<!-- ::: -->

:::info:
`--remote=true`または`--remote=false`のように、すべてのブールフラグにはパラメータが必要です。
:::
<!-- :::info: -->
<!-- All boolean flags require a parameter, for example `--remote=true` or `--remote=false`. -->
<!-- ::: -->

## API

これらの設定を使用して、APIの動作方法とアクセスできるホストをカスタマイズします。
<!-- Use these settings to customize how the API behaves and which hosts can access it. -->

| **CLフラグ** | **設定ファイルのパラメータ** | **説明** | **受け入れられる値** | **デフォルト値** | **メモ** |
| :----------- | :--------------------------- | :------- | :------------------- | :--------------- |:---------- |
| <a name="api-host"></a> `--api-host` | `API_HOST` | APIをリッスンするホストを設定します。 | string | localhost | 任意のホストを受け入れるには、このオプションを0.0.0.0に設定するか、`REMOTE`オプションを`true`に設定します。 |
| <a name="port"></a> `--port, -p`（**必須**） | `API_PORT` | APIをリッスンするポートを設定します。 | string | 14265 |
| <a name="max-body-length"></a> `--max-body-length` | `MAX_BODY_LENGTH` | API呼び出しのボディに含めることができる最大文字数を設定します。 | number | 1,000,000 | リクエストボディの長さがこの数を超えると、エラーが返されます。 |
| <a name="max-find-transactions"></a> `--max-find-transactions` | `MAX_FIND_TRANSACTIONS` | [findTransactionsエンドポイント](../references/api-reference.md#findTransactions)から返される可能性があるトランザクションの最大数を設定します。 | number | 100,000 | トランザクション数がこの数を超えると、エラーが返されます。 |
| <a name="max-requests-list"></a> `--max-requests-list` | `MAX_REQUESTS_LIST` | API呼び出しパラメーター数の最大数を設定します。 | number | 1,000 | パラメータ数がこの数を超えると、エラーが返されます。 |
| <a name="max-get-trytes"></a> `--max-get-trytes` | `MAX_GET_TRYTES` | [getTrytesエンドポイント](../references/api-reference.md#getTrytes)から返される可能性のあるトライト数の最大数を設定します。 | number | 10,000 | トライト数がこの数を超えると、エラーが返されます。 |
| <a name="remote"></a> ` --remote ` | - | 任意のホストへのAPIインターフェースを開きます。 | boolean | false | `true`に設定すると、このオプションは`API_HOST`オプションを0.0.0.0に設定するのと同等になります。このフラグの後に `true`または`false`の値を追加しなければなりません。 |
| <a name="remote-auth"></a> `--remote-auth` | `REMOTE_AUTH` | `username:password`の形式でAPI呼び出しのBasic認証を追加します。 | string | "" | プレーンテキストまたはハッシュパスワードを使用できます。 |
| <a name="remote-limit-api"></a> `--remote-limit-api` | `REMOTE_LIMIT_API` | 特定のAPIエンドポイントへのリクエストを無視します。 | 文字列の配列 | [[addNeighbors](../references/api-reference.md#addNeighbors), [getNeighbors](../references/api-reference.md#getNeighbors), [removeNeighbors](../references/api-reference.md#removeNeighbors), [attachToTangle](../references/api-reference.md#attachToTangle), [interruptAttachToTangle](../references/api-reference.md#interruptAttachToTangle)] | このオプションを使用すると、IRIノードのURLまたはIPアドレスを知っているスパム送信者からノードを保護できます。 |
| <a name="remote-trusted-api-hosts"></a> `--remote-trusted-api-hosts` | `REMOTE_TRUSTED_API_HOSTS` | `REMOTE_LIMIT_API`オプションで設定されているものも含め、任意のAPIエンドポイントを呼び出すことができるホスト。 | コンマ区切りの文字列のリスト | localhost | `REMOTE`オプションを`true`に設定する必要があります。 |

## Network

これらの設定を使用して、ノードが近隣ノードと通信する方法をカスタマイズします。
<!-- Use these settings to customize how your node communicates with neighbors. -->

| **CLフラグ** | **設定ファイルのパラメータ** | **説明** | **受け入れられる値** | **デフォルト値** | **メモ** |
| :----------- | :--------------------------- | :------- | :------------------- | :--------------- | :------- |
| <a name="auto-tethering-enabled"></a>`--auto-tethering-enabled` | `AUTO_TETHERING_ENABLED` | 未知の隣接ノードからの新しい接続を有効にします。 | boolean | false | 未知の隣接ノードとは、 `NEIGHBORS`オプションで定義されておらず、`addNeighbors`APIエンドポイントを通して追加されてもいないものです。あなたのノードに接続できる隣接ノードの最大数を制限するには `MAX_NEIGHBORS`オプションを使います。 |
| <a name="cache-size"></a> `--cache-size` | `CACHE_SIZE_BYTES` | ネットワークキャッシュの最大サイズをバイト単位で設定します。 | number | 150,000 |
| <a name="dns-refresher"></a> `--dns-refresher` | `DNS_REFRESHER` | 動的IPアドレスを持つ隣接ノードに再接続します。 | boolean | true |
| <a name="dns-resolution"></a> `--dns-resolution` | `DNS_RESOLUTION` | 近隣ノードピアリングに対してDNSを有効にします。 | boolean | true |
| <a name="max-neighbors"></a> `--max-neighbors` | `MAX_NEIGHBORS` | 最大隣接ノード数を設定します。 | number | 5 | このオプションは少なくとも `NEIGHBORS`オプションにあるのと同じ数のURLに設定されるべきです。 |
| <a name="neighbors"></a> `-n`, `--neighbors` | `NEIGHBORS` | [隣接ノード](../how-to-guides/find-neighbor-iri-nodes.md)のURLとIPアドレスを設定します。 | 文字列の配列 | "" |
| <a name="neighboring-socket-address"></a> `--neighboring-socket-address` | `NEIGHBORING_SOCKET_ADDRESS` | ゴシッププロトコル用のTCPサーバソケットをアドレスにバインドします。 | string | 0.0.0.0 | デフォルトアドレスは、TCPサーバソケットをすべてのネットワークインタフェースにバインドします。ノードに複数のIPアドレスがあり、特定のIPアドレスにのみバインドしたい場合は、このオプションを変更してください。 |
| <a name="neighboring-socket-port"></a> `--neighboring-socket-port` | `NEIGHBORING_SOCKET_PORT` | ゴシッププロトコル用のTCPサーバソケットをポートにバインドします。 | string | 15600 |
| <a name="p-drop-cache"></a> `--p-drop-cache` | `P_DROP_CACHE_ENTRY` | ネットワークキャッシュで最近のシーントランザクションを失う確率を設定します。 | 0から1の間の数 | 0.02 |
| <a name="p-remove-request"></a> `--p-remove-request` | `P_REMOVE_REQUEST` | IRIがトランザクションをリクエストするのを止める確率を設定します。 | 0から1の間の数 |0.01 | 存在しないトランザクションハッシュが最終的に削除されるように、この数は0に近いはずです。 |
| <a name="queue-size"></a> `--queue-size` | `Q_SIZE_NODE` | REPLY、BROADCAST、およびRECEIVEネットワークキューの最大サイズを設定します。 | number | 1,000 |
| <a name="reconnect-attempt-interval-seconds"></a> `--reconnect-attempt-interval-seconds` | `RECONNECT_ATTEMPT_INTERVAL_SECONDS` | 切断された隣接ノードに再接続しようとする前に待機する秒数を設定します。 | number | 60 |

## IXI

これらの設定を使用して、ノードがIXIモジュールを使用する方法をカスタマイズします。
<!-- Use these settings to customize how your node uses IXI modules. -->

| **CLフラグ** | **設定ファイルのパラメータ** | **説明** | **受け入れられる値** | **デフォルト値** | **メモ** |
| :----------- | :--------------------------- | :------- | :------------------- | :--------------- | :------- |
| <a name="ixi-dir"></a> `--ixi-dir` | `IXI_DIR` | 自動検出用にIXIモジュールを追加するフォルダ | string | ixi |

## Ledger

これらの設定を使用して、ノードが自身の台帳に対して実行する動作をカスタマイズします。
<!-- Use these settings to customize what your node does with its ledger. -->

| **CLフラグ** | **設定ファイルのパラメータ** | **説明** | **受け入れられる値** | **デフォルト値** | **メモ** |
| :----------- | :--------------------------- | :------- | :------------------- | :--------------- | :------- |
| <a name="db"></a> `--db` | `MAIN_DB` | トランザクションを保管するために使用されるデータベースの名前 | string | rocksdb | 現在、サポートしているデータベースはRocksDBだけです。 |
| <a name="db-cache-size"></a> `--db-cache-size` | `DB_CACHE_SIZE` | データベースキャッシュの最大サイズをキロバイト単位で設定します。 | number | 100,000 |
| <a name="db-log-path"></a> `--db-log-path` | `DB_LOG_PATH` | データベースログが保存されているファイルへのパスを設定します。 | string | mainnet.log |
| <a name="db-path"></a> `--db-path` | `DB_PATH` | データベースが保存されているフォルダへのパスを設定します。 | string | mainnetdb |
| <a name="rescan"></a> `--rescan` | `RESCAN_DB` | すべてのトランザクションメタデータ（承認トランザクション、バンドル、およびタグ）を再スキャンします。 | boolean | false |
| <a name="revalidate"></a> `--revalidate` | `REVALIDATE` | データベース内の確定済みトランザクションに関するデータとトランザクションメタデータをリロードします。 | boolean | false |

## Protocol

これらの設定を使用して、どのトランザクションがネットワークによって受け入れられるか、およびそのトランザクションが他のノードにどのように伝搬されるかをカスタマイズします。
<!-- Use these settings to customize which transactions will be accepted by the network, and how they will be propagated to other nodes. -->

| **CLフラグ** | **設定ファイルのパラメータ** | **説明** | **受け入れられる値** | **デフォルト値** | **メモ** |
| :----------- | :--------------------------- | :------- | :------------------- | :--------------- | :------- |
| <a name="p-drop-transaction"></a> `--p-drop-transaction` | `P_DROP_TRANSACTION` | 受け取ったトランザクションを失う確率を設定します。 | 0から1の間の数 | 0.0 | このオプションはテスト目的でのみ利用可能です。 |
| <a name="p-propagate-request"></a> `--p-propagate-request` | `P_PROPAGATE_REQUEST` | IRIが隣接ノードからの紛失トランザクションをリクエストする確率を設定します。 | number | 0.01 | この数は、スパムトランザクションをIRIがリクエストするのを避けるために、低くする必要があります。 |
| <a name="p-reply-random"></a> `--p-reply-random` | `P_REPLY_RANDOM_TIP` | 送信するトランザクションがない場合でも、IRIがランダムなトランザクションリクエストに応答する確率を設定します。 |  | 0.66 |
| <a name="p-select-milestone"></a> `--p-select-milestone`| `P_SELECT_MILESTONE_CHILD` | IRIが隣接ノードからマイルストーントランザクションをリクエストする確率を設定します。 | 0から1の間の数 | 0.7 | トランザクションが確定したと見なされるには、IRIが順番にマイルストーンを見つけることが不可欠であるため、この数は大きくする必要があります。 |
| <a name="p-send-milestone"></a> `--p-send-milestone` | `P_SEND_MILESTONE` | IRIがマイルストーントランザクションを隣接ノードへのランダム返信として送信する確率を設定します。 | 0から1の間の数 | 0.02 |

## ZMQ

これらの設定を使用して、クライアントが[ノードのZMQイベントを購読する](../how-to-guides/subscribe-to-events-in-an-iri-node.md)方法をカスタマイズします。
<!-- Use these settings to customize how clients can [subscribe to your node's ZMQ events](../how-to-guides/subscribe-to-events-in-an-iri-node.md). -->

| **CLフラグ** | **設定ファイルのパラメータ** | **説明** | **受け入れられる値** | **デフォルト値** | **メモ** |
| :----------- | :--------------------------- | :------- | :------------------- | :--------------- | :------- |
| <a name="zmq-enabled"></a> `--zmq-enable-ipc` | `ZMQ_ENABLE_IPC`| `ipc://iri`アドレスのIPCを介した[ゼロメッセージキュー](../concepts/zero-message-queue.md)サブスクリプションを有効にします。 | boolean | false | デフォルトアドレスを変更するには、`ZMQ_IPC`オプションを設定します。 |
| `--zmq-enable-tcp` | `ZMQ_ENABLE_TCP` | ポート5556でTCPを介した[ゼロメッセージキュー](../concepts/zero-message-queue.md)サブスクリプションを有効にします。 | boolean | false | デフォルトポートを変更するには`ZMQ_PORT`オプションを設定します。 |
| <a name="zmq-ipc"></a>`--zmq-ipc` | `ZMQ_IPC` | IPCを介してZMQと通信するために使用されるアドレスを設定します。 | string| ipc://iri |
| <a name="zmq-port"></a> `--zmq-port` | `ZMQ_PORT` | TCP経由でZMQに接続するために使用されるポートを設定します。 | string | 5556 |
| <a name="zmq-threads"></a> `--zmq-threads` | `ZMQ_THREADS` | ZMQ発行者が使用できるスレッドの最大数を設定します。 | number | 1 |

## Tip selection

これらの設定を使用して、[チップ選択](root://the-tangle/0.1/concepts/tip-selection.md)中の重み付きランダムウォークの長さとランダムさをカスタマイズします。
<!-- Use these settings to customize the length and randomness of the weighted random walk during [tip selection](root://the-tangle/0.1/concepts/tip-selection.md). -->

| **CLフラグ** | **設定ファイルのパラメータ** | **説明** | **受け入れられる値** | **デフォルト値** | **メモ** |
| :----------- | :--------------------------- | :------- | :------------------- | :--------------- | :------- |
| <a name="alpha"></a> `--alpha` | `ALPHA` | チップ選択プロセスのランダムさを設定します。 | 0から無限大の間の数 | 0.001 | 数値0が最もランダムで、無限大が最も決定的です。このオプションの詳細な説明については、[ブログの投稿](https://blog.iota.org/alpha-d176d7601f1c)を参照してください。 |
| <a name="max-analyzed-transactions"></a> `--max-analyzed-transactions` | `MAX_ANALYZED_TXS` | チップ選択中に分析される可能性のある未確定トランザクションの最大数を設定して、未確定トランザクションを参照する最新のマイルストーンを見つけます。 | number | 20,000 |
| <a name="max-depth"></a> `--max-depth` | `MAX_DEPTH` | IRIがチップ選択を開始する場所からの過去のマイルストーン数（深さ）の最大数を設定します。 | number | 15 | この値は、重み付きランダムウォークが妥当な時間内に終了するのに十分なほど小さく、かつ[部分グラフ](root://the-tangle/0.1/concepts/tip-selection.md#subgraph-selection)に十分な新しいトランザクションが含まれるのに十分なほど大きい必要があります。 |
| <a name="tip-selection-timeout-sec"></a> `--tip-selection-timeout-sec` | `TIP_SELECTION_TIMEOUT_SEC` | IRIがチップ選択を完了するために費やすことができる最大秒数を設定します。 | number | 60 | このオプションは、チップ選択に時間がかかりすぎる場合に、ノードが停止するのを防ぎます。 |

## Tip solidification

これらの設定を使用して、チップトランザクションが凝固する方法をカスタマイズします。
<!-- Use these settings to customize how tip transactions become solid. -->

| **CLフラグ** | **設定ファイルのパラメータ** | **説明** | **受け入れられる値** | **デフォルト値** | **メモ** |
| :----------- | :--------------------------- | :------- | :------------------- | :--------------- | :------- |
| <a name="tip-selection-timeout-sec"></a> `--tip-solidfier-enabled` | `TIP_SOLIDIFIER_ENABLED` | マイルストーンではないチップトランザクションをノードが凝固させることを有効にします。 | boolean | false | デフォルトでは、IRIは積極的にマイルストーンのみを凝固しようとします。他のチップトランザクションは、ゴシッププロトコルを通じて凝固になります。このオプションを`true`に設定すると、ノードはより多くのリソースを使用します。 |


## Proof of work

これらの設定を使用して、ノードがどのように[プルーフオブワーク](root://the-tangle/0.1/concepts/proof-of-work.md)するかをカスタマイズします。
<!-- Use these settings to customize how your node does [proof of work](root://the-tangle/0.1/concepts/proof-of-work.md). -->

| **CLフラグ** | **設定ファイルのパラメータ** | **説明** | **受け入れられる値** | **デフォルト値** | **メモ** |
| :----------- | :--------------------------- | :------- | :------------------- | :--------------- | :------- |
| <a name="pow-threads"></a> `--pow-threads` | `POW_THREADS` | プルーフオブワークの計算に使用するスレッド数 | number | 0 |

## Local snapshot

これらの設定を使用して、ノードが[ローカルスナップショット](../concepts/local-snapshot.md)を実行する方法とタイミングをカスタマイズします。
<!-- Use these settings to customize how and when your node does [local snapshots](../concepts/local-snapshot.md). -->

| **CLフラグ** | **設定ファイルのパラメータ** | **説明** | **受け入れられる値** | **デフォルト値** | **メモ** |
| :----------- | :--------------------------- | :------- | :------------------- | :--------------- | :------- |
| <a name="local-snapshots-enabled"></a> `--local-snapshots-enabled` | `LOCAL_SNAPSHOTS_ENABLED` | ローカルスナップショットを有効にします。 | boolean  | true  | IRIが他の`LOCAL_SNAPSHOTS`パラメータを読み取るには、このパラメータを`true`に設定する必要があります。|
| <a name="local-snapshots-pruning-enabled"></a> `--local-snapshots-pruning-enabled` | `LOCAL_SNAPSHOTS_PRUNING_ENABLED` | ノードが自身のデータベースから古いトランザクションを削除できるようにします。 | false | 次の計算の結果よりも古いインデックスを持つマイルストーンによって確定されたトランザクションは削除されます。`現在のマイルストーンインデックス` - (`LOCAL_SNAPSHOTS_DEPTH` + ` LOCAL_SNAPSHOTS_PRUNING_DELAY`) |
| <a name="local-snapshots-depth"></a> `--local-snapshots-depth` | `LOCAL_SNAPSHOTS_DEPTH` | snapshot.metaファイルに記録するシーンマイルストーンの量 | 100から始まる数 | 100 |
| <a name="local-snapshots-pruning-delay"></a> `--local-snapshots-pruning-delay` | `LOCAL_SNAPSHOTS_PRUNING_DELAY` | 台帳に保持するマイルストーントランザクションの量 | 10,000から始まる数 | 40,000 | このオプションにはデフォルト値を使用することをお勧めします。これにより、28日ごとにローカルスナップショットが行われます。 |
| <a name="local-snapshots-interval-synced"></a> `--local-snapshots-interval-synced` | `LOCAL_SNAPSHOTS_INTERVAL_SYNCED` | 台帳が完全に同期している場合にスナップショットファイルが作成される間隔（マイルストーントランザクションの数） | number | 10 |
| <a name="local-snapshots-interval-unsynced"></a> `--local-snapshots-interval-unsynced` | `LOCAL_SNAPSHOTS_INTERVAL_UNSYNCED` | 台帳が完全に同期していない場合にスナップショットファイルが作成される間隔（マイルストーントランザクションの数） | number | 1,000 | この値は、IRIが隣接IRIノードとの同期にリソースを集中できるようにするために`LOCAL_SNAPSHOTS_INTERVAL_SYNCED`設定オプションよりも大きいです。 |
| <a name="local-snapshots-base-path"></a> `--local-snapshots-base-path` | `LOCAL_SNAPSHOTS_BASE_PATH` | ファイル拡張子を除いたスナップショットファイルへのパス。 | string | mainnet | このパラメーターの値を`.snapshot.meta`および`.snapshot.state`ファイルの先頭に追加します。デフォルト値の場合、ファイルの名前は`mainnet.snapshot.meta`および`mainnet.snapshot.state`です。追加するファイルのディレクトリを指定するには、次のようにします。`&lt;directory name&gt;/&lt;file name&gt;`が、`folderpath/filename.snapshot.meta`となります。 |
