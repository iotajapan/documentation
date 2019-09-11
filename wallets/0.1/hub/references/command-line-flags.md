# コマンドラインフラグ
<!-- # Command line flags -->

**ハブまたは署名サーバーを起動するとき、runコマンドに次のフラグを1つ以上渡すことで、ハブまたは署名サーバーの機能をカスタマイズできます。**
<!-- **When you start Hub or the signing server, you can customize how they function by passing the run command one or more of the following flags.** -->

## データベースフラグ
<!-- ## Database flags -->

| **フラグ** | **説明** | **デフォルト値** |
| :--------- | :------- | :--------------- |
| `--db` | データベースの名前 | "hub" |
| `--dbDebug` | データベースサーバー接続のデバッグモードを有効にします。 | false |
| `--dbHost` | データベースサーバーのURLまたはIPアドレス | "127.0.0.1" |
| `--dbPassword` | データベースのユーザーパスワード | "password" |
| `--dbPort` | データベースサーバーのポート | 3306 |
| `--dbType` | データベースのタイプ | "mariadb" |
| `--dbUser` | データベースのユーザー名 | "user" |
| `-fetchTransactionMessages` | ハブが預け入れトランザクションのメッセージをデータベースに保存するかどうか | false |

<!-- | **Flag** |   **Description**| **Default values**| -->
<!-- | :------------------------ | :--------------- | :--------| -->
<!-- |`--db` |Name of the database | "hub"| -->
<!-- |`--dbDebug` |Enable debug mode for the database server connection| false| -->
<!-- |`--dbHost`| URL or IP address of the database server|"127.0.0.1"| -->
<!-- |`--dbPassword`| Password for the database server| "password"| -->
<!-- |`--dbPort`|Database server port| 3306| -->
<!-- |`--dbType`|Type of database| "mariadb"| -->
<!-- |`--dbUser` |Database username| "user"| -->
<!-- |`-fetchTransactionMessages`| Whether Hub stores the message of a deposit transaction in the database |false| -->

## ハブセキュリティタグ
<!-- ## Hub security flags -->

| **フラグ** | **説明** | **デフォルト値** |
| :--------- | :------- | :--------------- |
| `--argon2MCost` | Argon2のメモリコスト（バイト単位）。このフラグについては、[Stack Exchangeのこのトピック](https://crypto.stackexchange.com/questions/48954/questions-about-the-argon2-options)を参照してください。 | 1 << 17 |
| `--argon2TCost` | Argon2の時間コスト。このフラグについては、[Stack Exchangeのこのトピック](https://crypto.stackexchange.com/questions/48954/questions-about-the-argon2-options)を参照してください。 | 4 |
| `--argon2Mode` | 使用するArgon2モード：1=Argon2i、2=Argon2id | 2 |
| `--argon2Parallelism` | Argon2で並列に使用するスレッド数。このフラグについては、[Stack Exchangeのこのトピック](https://crypto.stackexchange.com/questions/48954/questions-about-the-argon2-options)を参照してください。 | 1 |
| `--authMode` | ハブが使用する接続暗号化の種類。このフラグは、値として "none"または"ssl"を受け入れます。"ssl"に設定すると、 `--sslKey`、 `--sslCert`、および`sslCA`フラグも使用する必要があります。 | "none" |
| <a name="keySec"></a>`--keySecLevel` | IOTA[秘密鍵](root://dev-essentials/0.1/concepts/addresses-and-signatures.md)に使用する[セキュリティレベル](root://dev-essentials/0.1/references/security-levels.md) | 2 |
| `--listenAddress` | ハブAPIが待機するホスト。デフォルトのホストは、任意のIPアドレスがAPIにアクセスすることを許可します。 | "0.0.0.0:50051" |
| `--maxConcurrentArgon2Hash` | 並行Argon2ハッシュプロセスの最大数 | 4 |
| `--salt` | 各預け入れアドレスのシードを作成するために`seed_uuid`フィールドと一緒にハッシュされる文字列。この値は20文字以上にする必要があります。 | "" |
| `--sslCert` | SSL証明書へのパス | "/dev/null" |
| `--sslCA` | 認証局（CA）ルートへのパス | "/dev/null" |
| `--sslKey` | SSL証明書キーへのパス | "/dev/null" |

<!-- | **Flag** |   **Description**| **Default values**| -->
<!-- | :------------------------ | :--------------- | :--------| -->
<!-- |`--argon2MCost`|Memory cost of Argon2 in bytes. [See this Stack Exchange topic](https://crypto.stackexchange.com/questions/48954/questions-about-the-argon2-options) for a discussion on this flag.| 1 << 17| -->
<!-- |`--argon2TCost` |Time cost of Argon2. [See this Stack Exchange topic](https://crypto.stackexchange.com/questions/48954/questions-about-the-argon2-options) for a discussion on this flag.|4| -->
<!-- |`--argon2Mode` |Argon2 mode to use: 1=argon2i, 2=argon2id|2| -->
<!-- |`--argon2Parallelism`|Number of threads to use in parallel for Argon2. [See this Stack Exchange topic](https://crypto.stackexchange.com/questions/48954/questions-about-the-argon2-options) for a discussion on this flag.|1| -->
<!-- |`--authMode`|Type of connection encryption that Hub uses. This flag accepts "none" or "ssl" as the value. When set to "ssl", you must also use the `--sslKey`, `--sslCert`, and `sslCA` flags. and |"none"| -->
<!-- |<a name="keySec"></a>`--keySecLevel` |[Security level](root://dev-essentials/0.1/references/security-levels.md) to use for IOTA [addresses](root://dev-essentials/0.1/concepts/addresses-and-signatures.md)|2| -->
<!-- |`--listenAddress`| Host to which the Hub API will listen. The default host allows any IP address to access the API.| "0.0.0.0:50051"| -->
<!-- |`--maxConcurrentArgon2Hash`|Maximum number of concurrent Argon2 hash processes| 4| -->
<!-- |`--salt`| Characters that are hashed together with the `seed_uuid` field to create seeds for each deposit address. This value should be at least 20 characters long.   |   ""    | -->
<!-- |`--sslCert` |Path to the SSL certificate |"/dev/null"| -->
<!-- |`--sslCA` |Path to the certificate authority (CA) root |"/dev/null"| -->
<!-- |`--sslKey` |Path to the SSL certificate key |"/dev/null"| -->

## IOTAプロトコルフラグ
<!-- ## IOTA protocol flags -->

| **フラグ** | **説明** | **デフォルト値** |
| :--------- | :------- | :--------------- |
| `--apiAddress` | ハブが接続するIRIノードのURLまたはIPアドレス。悪意のあるノードに接続しないようにするには、[自分のノードを実行](root://node-software/0.1/iri/introduction/overview.md)して接続することをお勧めします。ハブはHTTPSプロトコルを使用するノードに接続できません。 | "127.0.0.1:14265" |
| `--attachmentInterval` | [トランザクションの再添付と促進](root://dev-essentials/0.1/concepts/reattach-rebroadcast-promote.md)の間にHubが待機する間隔（ミリ秒）。0=無効。 | 240000 |
| `--depth` | [`getTransactionsToApprove`（GTTA）](root://node-software/0.1/iri/references/api-reference.md#getTransactionsToApprove)エンドポイントの`depth`パラメーターに使用する値 | 3 |
| `--minWeightMagnitude` | プルーフオブワークに使用する[最小重量値（MWM）](root://dev-essentials/0.1/concepts/minimum-weight-magnitude.md)。 Mainnetでハブを使用するには、MWMは14を使用する必要があります。 | 9 |
| `--powMode` | [プルーフオブワーク](root://dev-essentials/0.1/concepts/minimum-weight-magnitude.md)のモード。`local`か`remote`か。 | local |
| `--useHttpsIRI` | HTTPSプロトコルを介してIRIノードと通信します（`--apiAddress`フラグで指定されたノードがhttpsをサポートしていることを確認します）。 | `--nouseHttpsIRI` |

<!-- | **Flag** |   **Description**| **Default values**| -->
<!-- | :------------------------ | :--------------- | :--------| -->
<!-- |`--apiAddress`| URL or IP address of the IRI node that Hub connects to. To avoid connecting to a malicious node, we recommend [running your own node](root://node-software/0.1/iri/introduction/overview.md) and connecting to it.| "127.0.0.1:14265"| -->
<!-- |`--attachmentInterval`|Interval in milliseconds that Hub waits between [reattaching and promoting transactions](root://dev-essentials/0.1/concepts/reattach-rebroadcast-promote.md). 0=disabled.|240000| -->
<!-- |`--depth`|Value to use for the `depth` parameter of the [`getTransactionsToApprove` (GTTA)](root://node-software/0.1/iri/references/api-reference.md#getTransactionsToApprove) endpoint| 3| -->
<!-- |`--minWeightMagnitude`| [Minimum weight magnitude (MWM)](root://dev-essentials/0.1/concepts/minimum-weight-magnitude.md) to use for proof of work. To use Hub on the Mainnet, you must use a MWM of 14.| 9| -->
<!-- |`--powMode`|[Proof of work](root://dev-essentials/0.1/concepts/minimum-weight-magnitude.md) mode, local or remote| -->
<!-- |`--useHttpsIRI`| Communicate with the IRI node through the HTTPS protocol (make sure that the node that's specified in the `--apiAddress` flag supports https)|`--nouseHttpsIRI`| -->

## 署名フラグ
<!-- ## Signing flags -->

| **フラグ** | **説明** | **デフォルト値** |
| :--------- | :------- | :--------------- |
| `--authProvider` | [`signBundle`](../references/api-reference.md#hub.rpc.SignBundleRequest)メソッドを認証するために使用するプロバイダ。この値は "non"または"hmac"になります。 | "none" |
| `--hmacKeyPath` | [`signBundle`](../references/api-reference.md#hub.rpc.SignBundleRequest)メソッドの認証に使用されるHMACキーへのパス | "/dev/null" |
| <a name="recoverFunds"></a>`--RecoverFunds_enabled` | `RecoverFunds`エンドポイントを使用できるかどうか | `--noRecoverFunds_enabled` |
| <a name="signBundle"></a>`--SignBundle_enabled` | [`SignBundle`エンドポイント](../references/api-reference.md#hub.rpc.SignBundleRequest)を使用できるかどうか | false |
| `--signingMode` | バンドルに署名するために使用するメソッド。この値は"local"または"remote"です（署名サーバーを実行している場合）。 | "local" |
| `--signingProviderAddress` | 署名サーバーのURLまたはIPアドレス。`--signingMode`に "remote"値がある場合は、このフラグも使用する必要があります。 | "0.0.0.0:50052" |
| `--signingServerChainCert` | SSL暗号化を使用するようにハブを設定し（`--authMode="ssl"`）、[署名サーバー](../how-to-guides/install-the-signing-server.md)がある場合は、SSL証明書チェーンへのパスをこのフラグに渡す必要があります。 | "/dev/null" |
| `--signingServerKeyCert` | SSL暗号化を使用するようにハブを設定し（`--authMode="ssl"`）、[署名サーバー](../how-to-guides/install-the-signing-server.md)がある場合は、SSL証明書キーへのパスをこのフラグに渡す必要があります。| "/dev/null" |
| `--signingServerSslCert` | SSL暗号化を使用するようにハブを設定し（`--authMode="ssl"`）、[署名サーバー](../how-to-guides/install-the-signing-server.md)がある場合は、SSL証明書へのパスをこのフラグに渡す必要があります。 | "/dev/null" |

<!-- | **Flag** |   **Description**| **Default values**| -->
<!-- | :------------------------ | :--------------- | :--------| -->
<!-- |`--authProvider`| Provider to use to authenticate the [`signBundle`](../references/api-reference.md#hub.rpc.SignBundleRequest) method. This value can be "none" or "hmac"| "none"| -->
<!-- |`--hmacKeyPath` |Path to the HMAC key used to authenticate the [`signBundle`](../references/api-reference.md#hub.rpc.SignBundleRequest) method|"/dev/null"| -->
<!-- |<a name="recoverFunds"></a>`--RecoverFunds_enabled`|Whether you can use the `RecoverFunds` endpoint|`--noRecoverFunds_enabled`| -->
<!-- |<a name="signBundle"></a>`--SignBundle_enabled`|Whether you can use the [`SignBundle` endpoint](../references/api-reference.md#hub.rpc.SignBundleRequest)|false| -->
<!-- |`--signingMode`|Method to use to sign bundles. This value can be "local" or "remote" (if you run a signing server) |"local"| -->
<!-- |`--signingProviderAddress`|URL or IP address of the signing server. Where you have the "remote" value for the `--signingMode`, you must also use this flag.|"0.0.0.0:50052"| -->
<!-- |`--signingServerChainCert`|If you configure Hub to use SSL encryption (`--authMode="ssl"`) and you have a [signing server](../how-to-guides/install-the-signing-server.md), you must pass the path to the SSL certificate chain to this flag|"/dev/null"| -->
<!-- |`--signingServerKeyCert`|If you configure Hub to use SSL encryption (`--authMode="ssl"`) and you have a [signing server](../how-to-guides/install-the-signing-server.md), you must pass the path to the SSL certificate key to this flag|"/dev/null"| -->
<!-- |`--signingServerSslCert`|If you configure Hub to use SSL encryption (`--authMode="ssl"`) and you have a [signing server](../how-to-guides/install-the-signing-server.md), you must pass the path to the SSL certificate to this flag|"/dev/null"| -->

## スウィープフラグ
<!-- ## Sweep flags -->

| **フラグ** | **説明** | **デフォルト値** |
| :--------- | :------- | :--------------- |
| <a name="monitorInterval"></a> `--monitorInterval` | ハブが預け入れアドレスをチェックし、[`user_address`テーブルと`user_address_balance`テーブル](../references/database-tables.md)を更新する間隔（ミリ秒）。トークンを含むものは、次のスウィープに含まれます（スウィープを無効にするには、このフラグを0に設定します）。 | 60000 |
| `--sweepInterval` | ハブがスウィープ間で待機する間隔（ミリ秒）。0=無効。 | 600000 |
| <a name="sweepLimits"></a> `--sweep_max_deposit` | スウィープに含めるユーザー預け入れの最大数 | 5 |
| `--sweep_max_withdraw` | スウィープに含める取り出しリクエストの最大数 | 7 |

<!-- | **Flag** |   **Description**| **Default values**| -->
<!-- | :------------------------ | :--------------- | :--------| -->
<!-- |<a name="monitorInterval"></a>`--monitorInterval`|Interval in milliseconds that Hub checks deposit addresses and updates the [`user_address` and `user_address_balance` tables](../references/database-tables.md). Those that contain tokens are included in the next sweep. 0=disabled.|60000| -->
<!-- |`--sweepInterval` |Interval in milliseconds that Hub waits between sweeps (set this flag to 0 to disable sweeps)|600000| -->
<!-- |<a name="sweepLimits"></a>`--sweep_max_deposit`|Maximum number of user deposits to include in a sweep|5| -->
<!-- |`--sweep_max_withdraw`|Maximum number of withdraw requests to include in a sweep|7| -->
