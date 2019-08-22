# コマンドラインフラグ
<!-- # Command line flags -->

**ハブまたは署名サーバーを起動するとき、runコマンドに次のフラグを1つ以上渡すことで、ハブまたは署名サーバーの機能をカスタマイズできます。**
<!-- **When you start Hub or the signing server, you can customize how they function by passing the run command one or more of the following flags.** -->

## common/flags/flags.hからのフラグ（ハブ/署名サーバー両方のバイナリ用）
<!-- ## Flags from `common/flags/flags.h` (for both hub/signing_server binaries) -->

| **フラグ** | **説明** | **デフォルト値** |
| :--------- | :------- | :--------------- |
| `--salt` | 各預け入れアドレスのシードを作成するために`seed_uuid`フィールドと一緒にハッシュされる文字列。この値は20文字以上にする必要があります。 | "" |
| `--listenAddress` | ハブAPIが待機するホスト。デフォルトのホストは、任意のIPアドレスがAPIにアクセスすることを許可します。 | "0.0.0.0:50051" |
| `--authMode` | ハブが使用する接続暗号化の種類。このフラグは、値として "none"または"ssl"を受け入れます。"ssl"に設定すると、 `--sslKey`、 `--sslCert`、および`sslCA`フラグも使用する必要があります。 | "none" |
| `--sslKey` | SSL証明書キーへのパス | "/dev/null" |
| `--sslCert` | SSL証明書へのパス | "/dev/null" |
| `--sslCA` | 認証局（CA）ルートへのパス | "/dev/null" |
| `--maxConcurrentArgon2Hash` | 並行Argon2ハッシュプロセスの最大数 | 4 |
| `--argon2TCost` | Argon2の時間コスト。このフラグについては、[Stack Exchangeのこのトピック](https://crypto.stackexchange.com/questions/48954/questions-about-the-argon2-options)を参照してください。 | 4 |
| `--argon2MCost` | Argon2のメモリコスト（バイト単位）。このフラグについては、[Stack Exchangeのこのトピック](https://crypto.stackexchange.com/questions/48954/questions-about-the-argon2-options)を参照してください。 | 1 << 17 |
| `--argon2Parallelism` | Argon2で並列に使用するスレッド数。このフラグについては、[Stack Exchangeのこのトピック](https://crypto.stackexchange.com/questions/48954/questions-about-the-argon2-options)を参照してください。 | 1 |
| `--argon2Mode` | 使用するArgon2モード：1=Argon2i、2=Argon2id | 2 |
| <a name="keySec"></a>`--keySecLevel` | IOTA[秘密鍵](root://dev-essentials/0.1/concepts/addresses-and-signatures.md)に使用する[セキュリティレベル](root://dev-essentials/0.1/references/security-levels.md) | 2 |

## hub/db/db.ccからのフラグ
<!-- ## Flags from hub/db/db.cc -->

| **フラグ** | **説明** | **デフォルト値** |
| :--------- | :------- | :--------------- |
| `--dbPassword` | データベースサーバーのパスワード | "password" |
| `--db` | データベースの名前 | "hub" |
| `--dbDebug` | データベースサーバー接続のデバッグモードを有効にします。 | false |
| `--dbHost` | データベースサーバーのURLまたはIPアドレス | "127.0.0.1" |
| `--dbPassword` | データベースのユーザーパスワード | "password" |
| `--dbPort` | データベースサーバーのポート | 3306 |
| `--dbType` | データベースのタイプ | "mariadb" |
| `--dbUser` | データベースのユーザー名 | "user" |

## hub/server/server.ccからのフラグ
<!-- ## Flags from hub/server/server.cc -->

| **フラグ** | **説明** | **デフォルト値** |
| :--------- | :------- | :--------------- |
| `--apiAddress` | ハブが接続するIRIノードのURLまたはIPアドレス。悪意のあるノードに接続しないようにするには、[自分のノードを実行](root://node-software/0.1/iri/introduction/overview.md)して接続することをお勧めします。ハブはHTTPSプロトコルを使用するノードに接続できません。 | "127.0.0.1:14265" |
| `--attachmentInterval` | [トランザクションの再添付と促進](root://dev-essentials/0.1/concepts/reattach-rebroadcast-promote.md)の間にHubが待機する間隔（ミリ秒）。0=無効。 | 240000 |
| `--authProvider` | [`signBundle`](../references/api-reference.md#hub.rpc.SignBundleRequest)メソッドを認証するために使用するプロバイダ。この値は "non"または"hmac"になります。 | "none" |
| `--depth` | [`getTransactionsToApprove`（GTTA）](root://node-software/0.1/iri/references/api-reference.md#getTransactionsToApprove)エンドポイントの`depth`パラメーターに使用する値 | 3 |
| `--hmacKeyPath` | [`signBundle`]メソッドの認証に使用されるHMACキーへのパス | "/dev/null" |
| `--minWeightMagnitude` | プルーフオブワークに使用する[最小重量値（MWM）](root://dev-essentials/0.1/concepts/minimum-weight-magnitude.md)。 Mainnetでハブを使用するには、MWMは14を使用する必要があります。 | 9 |
| <a name="monitorInterval"></a> `--monitorInterval` | ハブが預け入れアドレスをチェックし、[`user_address`テーブルと`user_address_balance`テーブル](../references/database-tables.md)を更新する間隔（ミリ秒）。トークンを含むものは、次のスイープに含まれます。0=無効。 | 60000 |
| `--signingMode` | バンドルに署名するために使用するメソッド。この値は"local"または"remote"です（署名サーバーを実行している場合）。 | "local" |
| `--signingProviderAddress` | 署名サーバーのURLまたはIPアドレス。`--signingMode`に "remote"値がある場合は、このフラグも使用する必要があります。 | "0.0.0.0:50052" |
| `--signingServerChainCert` | SSL暗号化を使用するようにハブを設定し（`--authMode="ssl"`）、[署名サーバー](../how-to-guides/install-the-signing-server.md)がある場合は、SSL証明書チェーンへのパスをこのフラグに渡す必要があります。 | "/dev/null" |
| `--signingServerKeyCert` | SSL暗号化を使用するようにハブを設定し（`--authMode="ssl"`）、[署名サーバー](../how-to-guides/install-the-signing-server.md)がある場合は、SSL証明書キーへのパスをこのフラグに渡す必要があります。| "/dev/null" |
| `--signingServerSslCert` | SSL暗号化を使用するようにハブを設定し（`--authMode="ssl"`）、[署名サーバー](../how-to-guides/install-the-signing-server.md)がある場合は、SSL証明書へのパスをこのフラグに渡す必要があります。 | "/dev/null" |
| `--sweepInterval` | ハブがスウィープ間で待機する間隔（ミリ秒）。0=無効。 | 600000 |
| `--powMode` | [プルーフオブワーク](root://dev-essentials/0.1/concepts/minimum-weight-magnitude.md)のモード。`local`か`remote`か。 | local |
| `-fetchTransactionMessages` | ハブが預け入れトランザクションのメッセージをデータベースに格納するかどうか | false |

## hub/service/sweep_service.ccからのフラグ
<!-- ## Flags from hub/service/sweep_service.cc -->

| **フラグ** | **説明** | **デフォルト値** |
| :--------- | :------- | :--------------- |
| <a name="sweepLimits"></a> `--sweep_max_deposit` | スウィープに含めるユーザー預け入れの最大数 | 5 |
| `--sweep_max_withdraw` | スウィープに含める取り出しリクエストの最大数 | 7 |
