# IRI設定オプション
<!-- # IRI configuration options -->

**IRI設定オプションを使用すると、ノードをカスタマイズできます。これらのオプションは、コマンドライン（CLフラグ）または.ini設定ファイル（設定ファイルパラメータ）のどちらでも設定できます。**
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
| <a name="remote"></a> ` --remote ` | `REMOTE` | 任意のホストへのAPIインターフェースを開きます。 | boolean | false | `true`に設定すると、このオプションは`API_HOST`オプションを0.0.0.0に設定するのと同等になります。 |
| <a name="remote-auth"></a> `--remote-auth` | `REMOTE_AUTH` | `username:password`の形式でAPI呼び出しのBasic認証を追加します。 | string | "" | プレーンテキストまたはハッシュパスワードを使用できます。 |
| <a name="remote-limit-api"></a> `--remote-limit-api` | `REMOTE_LIMIT_API` | 特定のAPIエンドポイントへのリクエストを無視します。 | 文字列の配列 | [[addNeighbors](../references/api-reference.md#addNeighbors), [getNeighbors](../references/api-reference.md#getNeighbors), [removeNeighbors](../references/api-reference.md#removeNeighbors), [attachToTangle](../references/api-reference.md#attachToTangle), [interruptAttachToTangle](../references/api-reference.md#interruptAttachToTangle)] | このオプションを使用すると、IRIノードのURLまたはIPアドレスを知っているスパム送信者からノードを保護できます。 |
| <a name="remote-trusted-api-hosts"></a> `--remote-trusted-api-hosts` | `REMOTE_TRUSTED_API_HOSTS` | `REMOTE_LIMIT_API`オプションで設定されているものも含め、任意のAPIエンドポイントを呼び出すことができるホスト。 | コンマ区切りの文字列のリスト | localhost | `REMOTE`オプションを`true`に設定する必要があります。 |

## Network

これらの設定を使用して、ノードが近隣ノードと通信する方法をカスタマイズします。
<!-- Use these settings to customize how your node communicates with neighbors. -->

| **CLフラグ** | **設定ファイルのパラメータ** | **説明** | **受け入れられる値** | **デフォルト値** | **メモ** |
| :----------- | :--------------------------- | :------- | :------------------- | :--------------- | :------- |
| <a name="cache-size"></a> `--cache-size` | `CACHE_SIZE_BYTES` | ネットワークキャッシュの最大サイズをバイト単位で設定します。 | number | 150,000 |
| <a name="dns-refresher"></a> `--dns-refresher` | `DNS_REFRESHER` | 動的IPアドレスを持つ隣接ノードに再接続します。 | boolean | true |
| <a name="dns-resolution"></a> `--dns-resolution` | `DNS_RESOLUTION` | 近隣ノードピアリングに対してDNSを有効にします。 | boolean | true |
| <a name="max-peers"></a> `--max-peers` | `MAX_PEERS` | 相互に繋がっていないピアの最大数を設定します。 | number | 0 | このオプションはIOTA Devnetネットワークでのみ利用可能です。 |
| <a name="neighbors"></a> `-n`, `--neighbors` | `NEIGHBORS` | [隣接ノード](../how-to-guides/find-neighbor-iri-nodes.md)のURLとIPアドレスを設定します。 | 文字列の配列 | [""] |
| <a name="p-drop-cache"></a> `--p-drop-cache` | `P_DROP_CACHE_ENTRY` | ネットワークキャッシュで最近のシーントランザクションを失う確率を設定します。 | 0から1の間の数 | 0.02 |
| <a name="p-remove-request"></a> `--p-remove-request` | `P_REMOVE_REQUEST` | IRIがトランザクションをリクエストするのを止める確率を設定します。 | 0から1の間の数 |0.01 | 存在しないトランザクションハッシュが最終的に削除されるように、この数は0に近いはずです。 |
| <a name="queue-size"></a> `--queue-size` | `Q_SIZE_NODE` | REPLY、BROADCAST、およびRECEIVEネットワークキューの最大サイズを設定します。 | number | 1,000 |
| <a name="send-limit"></a> `--send-limit` | `SEND_LIMIT` | ノードが送信できるデータ制限をMbpsで設定します。 | number | -1 | この数が0未満の場合、制限は設定されません。 |
| <a name="udp-receiver-port"></a> `-u`, `--udp-receiver-port` | `UDP_RECEIVER_PORT` | IRIが隣接IRIノードからUDPデータパケットを受信するポートを設定します。 | string | 14600 |
| <a name="tcp-receiver-port"></a> `-t`, `--tcp-receiver-port` | `TCP_RECEIVER_PORT` | IRIが隣接IRIノードからTCPデータパケットを受信するポートを設定します。 | string | 15600 |

## IXI

Use these settings to customize how your node uses IXI modules.

| **CL flags** |**Configuration file parameters** |  **Description**| **Accepted values** | **Default value**|**Notes** |
| :------------------------ | :--------------- | :--------- | :--------| :------------|:-----|
|<a name="ixi-dir"></a>  `--ixi-dir` |`IXI_DIR`|Folder where IXI modules should be added for automatic discovery |string |ixi |

## Ledger

Use these settings to customize what your node does with its ledger.

| **CL flags** |**Configuration file parameters** |  **Description**| **Accepted values** | **Default value**|**Notes** |
| :------------------------ | :--------------- | :--------- | :--------| :------------|:-----|
|<a name="db"></a>`--db` |`MAIN_DB`|Name the database that's used to store transactions  | string | rocksdb | Currently, the only supported database is RocksDB|
|<a name="db-cache-size"></a>`--db-cache-size` |`DB_CACHE_SIZE`|Set the maximum size of the database cache in kilobytes | number|100,000 |
|<a name="db-log-path"></a>`--db-log-path` |`DB_LOG_PATH`|Set the path to the file where the database logs are saved | string |mainnet.log|
|<a name="db-path"></a> `--db-path`| `DB_PATH`|Set the path to the folder where the database is saved|string |mainnetdb |
|<a name="rescan"></a> `--rescan`|`RESCAN_DB`|Rescan all transaction metadata (approvees, bundles, and tags) |boolean |false |
|<a name="revalidate"></a>`--revalidate` |`REVALIDATE`|Reload data in the database about confirmed transactions, and transaction metadata | boolean| false|

## Protocol

Use these settings to customize which transactions will be accepted by the network, and how they will be propagated to other nodes.

| **CL flags** |**Configuration file parameters** |  **Description**| **Accepted values** | **Default value**|**Notes** |
| :------------------------ | :--------------- | :--------- | :--------| :------------|:-----|
|<a name="p-drop-transaction"></a>  `--p-drop-transaction`|`P_DROP_TRANSACTION`|Set the probability of losing a received transaction |number between 0 and 1 |0.0 | This option is available only for testing purposes
|<a name="p-propagate-request"></a>`--p-propagate-request` |`P_PROPAGATE_REQUEST`|Set the probability of the IRI requesting a missing transaction from a neighbor | number|0.01 | This number should be low to avoid the IRI requesting non-existing transactions that spam the network
|<a name="p-reply-random"></a>`--p-reply-random` |`P_REPLY_RANDOM_TIP`|Set the probability of the IRI replying to a random transaction request, even if it doesn't have anything to send|number between 0 and 1 | 0.66|
|<a name="p-select-milestone"></a>`--p-select-milestone`| `P_SELECT_MILESTONE_CHILD`|Set the probability of the IRI requesting a milestone transaction from a neighbor|number between 0 and 1 | 0.7|This number should be large because it's essential that the IRI finds milestones in order to consider transactions confirmed
|<a name="p-send-milestone"></a>`--p-send-milestone` |`P_SEND_MILESTONE`|Set the probability of the IRI sending a milestone transaction as a random reply to its neighbors | number between 0 and 1 | 0.02|

## ZMQ

Use these settings to customize how clients can [subscribe to your node's ZMQ events](../how-to-guides/subscribe-to-events-in-an-iri-node.md).

| **CL flags** |**Configuration file parameters** |  **Description**| **Accepted values** | **Default value**|**Notes** |
| :------------------------ | :--------------- | :--------- | :--------| :------------|:-----|
|<a name="zmq-enabled"></a>  `--zmq-enable-ipc` | `ZMQ_ENABLE_IPC`|Enable [zero message queue](../concepts/zero-message-queue.md) subscriptions through IPC at the `ipc://iri` address| boolean|false |Set the `ZMQ_IPC` option to change the default address|
|`--zmq-enable-tcp` | `ZMQ_ENABLE_TCP`|Enable [zero message queue](../concepts/zero-message-queue.md) subscriptions through TCP on port 5556|boolean|false |Set the `ZMQ_PORT` option to change the default port|
|<a name="zmq-ipc"></a>`--zmq-ipc` |`ZMQ_IPC`|Set the address that is used to communicate with ZMQ through IPC| string|  ipc://iri|
|<a name="zmq-port"></a> `--zmq-port`|`ZMQ_PORT`|Set the port that is used to connect to the ZMQ through TCP|string | 5556|
|<a name="zmq-threads"></a> `--zmq-threads`|`ZMQ_THREADS`|Set the maximum number of threads that the ZMQ publisher can use|number | 1|

## Tip selection

Use these settings to customize the length and randomness of the weighted random walk during [tip selection](root://the-tangle/0.1/concepts/tip-selection.md).

| **CL flags** |**Configuration file parameters** |  **Description**| **Accepted values** | **Default value**|**Notes** |
| :------------------------ | :--------------- | :--------- | :--------| :------------|:-----|
|<a name="alpha"></a>`--alpha`| `ALPHA`|Set the randomness of the tip selection process             |   number between 0 and infinity  |  0.001     | The number 0 is the most random and infinity is the most deterministic. For an in-depth explanation of this option, [see our blog post](https://blog.iota.org/alpha-d176d7601f1c).|
|<a name="max-analyzed-transactions"></a>`--max-analyzed-transactions` |`MAX_ANALYZED_TXS`|Set the maximum number of unconfirmed transactions that may be analyzed during tip selection to find the latest milestone that references a transaction |number |20,000 |
|  <a name="max-depth"></a>`--max-depth` |`MAX_DEPTH`|Set the maximum number of previous milestones (depth) from where the IRI will start the tip selection |number |15 | This value should be both small enough to allow the weighted random walk to finish in a reasonable amount of time and large enough in include enough new transactions in the [subgraph](root://the-tangle/0.1/concepts/tip-selection.md#subgraph-selection)
|<a name="tip-selection-timeout-sec"></a>`--tip-selection-timeout-sec` |`TIP_SELECTION_TIMEOUT_SEC`|Set the maximum number of seconds that the IRI can spend to complete tip selection |number | 60|This option stops your node from stalling if tip selection takes too long

## Tip solidification

Use these settings to customize how tip transactions become solid.

| **CL flags** |**Configuration file parameters** |  **Description**| **Accepted values** | **Default value**|**Notes** |
| :------------------------ | :--------------- | :--------- | :--------| :------------|:-----|
|<a name="tip-selection-timeout-sec"></a>`--tip-solidfier-enabled` |`TIP_SOLIDIFIER_ENABLED`|Enable your node to solidify tip transactions that aren't milestones |boolean | false| By default, the IRI actively tries to solidify only milestones. Other tip transactions become solid through the gossip protocol. If you set this option to `true`, your node will use more resources.


## Proof of work

Use these settings to customize how your node does [proof of work](root://the-tangle/0.1/concepts/proof-of-work.md).

| **CL flags** |**Configuration file parameters** |  **Description**| **Accepted values** | **Default value**|**Notes** |
| :------------------------ | :--------------- | :--------- | :--------| :------------|:-----|
|<a name="pow-threads"></a>`--pow-threads`|`POW_THREADS`   | Number of threads to use for proof-of-work calculations |number  | 0  |

## Local snapshot

Use these settings to customize how and when your node does [local snapshots](../concepts/local-snapshot.md).

| **CL flags** |**Configuration file parameters** |  **Description**| **Accepted values** | **Default value**|**Notes** |
| :------------------------ | :--------------- | :--------- | :--------| :------------|:-----|
|<a name="local-snapshots-enabled"></a>`--local-snapshots-enabled`|`LOCAL_SNAPSHOTS_ENABLED`   | Enable local snapshots |boolean  | true  | This parameter must be set to `true` for the IRI to read any other `LOCAL_SNAPSHOTS` parameters|
|<a name="local-snapshots-pruning-enabled"></a>`--local-snapshots-pruning-enabled`|`LOCAL_SNAPSHOTS_PRUNING_ENABLED`  |  Enable your node to delete old transactions from its database  | false | Transactions are deleted if they were confirmed by a milestone with an index that is older than the result of the following calculation: current milestone index - (`LOCAL_SNAPSHOTS_DEPTH` + `LOCAL_SNAPSHOTS_PRUNING_DELAY`).  |
|<a name="local-snapshots-depth"></a>`--local-snapshots-depth`|`LOCAL_SNAPSHOTS_DEPTH`  | Amount of seen milestones to record in the snapshot.meta file | number starting from 100 | 100 |
|<a name="local-snapshots-pruning-delay"></a>`--local-snapshots-pruning-delay`|`LOCAL_SNAPSHOTS_PRUNING_DELAY`  | Amount of milestone transactions to keep in the ledger   | number starting from 10,000  | 40,000 | We recommend that you use the default value for this option, which triggers a local snapshot every 28 days  |
|<a name="local-snapshots-interval-synced"></a>`--local-snapshots-interval-synced`|`LOCAL_SNAPSHOTS_INTERVAL_SYNCED`  | Interval, in milestone transactions, at which snapshot files are created if the ledger is fully synchronized  |number| 10   |
|<a name="local-snapshots-interval-unsynced"></a>`--local-snapshots-interval-unsynced`|`LOCAL_SNAPSHOTS_INTERVAL_UNSYNCED`   | Interval, in milestone transactions, at which snapshot files are created if the ledger is not fully synchronized  |number| 1,000  | This value is higher than the `LOCAL_SNAPSHOTS_INTERVAL_SYNCED` configuration option to allow the IRI to focus its resources on synchronizing with its neighbor IRI nodes|
|<a name="local-snapshots-base-path"></a>`--local-snapshots-base-path`|`LOCAL_SNAPSHOTS_BASE_PATH`  |  Path to the snapshot file, without the file extension. |  string |  mainnet   | Prepends the `.snapshot.meta` and `.snapshot.state` files with the value of this parameter. For the default value, the files are named `mainnet.snapshot.meta` and `mainnet.snapshot.state`. You can specify a directory for the files to be added to by doing the following: `<directory name>/<file name>`, which results in `folderpath/filename.snapshot.meta`. |
