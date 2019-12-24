# APIリファレンス
<!-- # API reference -->

**ハブには、タングルおよびハブデータベースへの接続を簡素化するgRPC APIがあります。このAPIリファレンスを使用して、メソッドを見つけ、メソッドが行うことを学びます。**
<!-- **Hub has a gRPC API that simplifies connections to the Tangle and the Hub database. Use this API reference to find methods and learn what they do.** -->

:::info:
gRPCとprotobufに慣れていない場合は、[gRPCクイックスタートガイド](https://grpc.io/docs/quickstart/)に従うことをお勧めします。または、[RESTful APIを使用](../references/restful-api-reference.md)することができます。
:::
<!-- :::info: -->
<!-- If you’re not familiar with gRPC and protobuf, we recommend following the [gRPC quickstart guide](https://grpc.io/docs/quickstart/). Or, you can [use the RESTful API](../references/restful-api-reference.md). -->
<!-- ::: -->

<a name="hub.proto"></a>

## hub.proto

<a name="hub.rpc.Hub"></a>

### Hub

| **メソッド名** | **リクエストタイプ** | **レスポンスタイプ** | **説明** |
| -------------- | -------------------- | -------------------- | -------- |
| CreateUser | [CreateUserRequest](#hub.rpc.CreateUserRequest) | [CreateUserReply](#hub.rpc.CreateUserRequest) | ハブに新しいユーザーを作成します。 |
| GetAddressInfo | [GetAddressInfoRequest](#hub.rpc.GetAddressInfoRequest) | [GetAddressInfoReply](#hub.rpc.GetAddressInfoRequest) | 預け入れアドレスを所有しているユーザーのIDを取得します。 |
| GetBalance | [GetBalanceRequest](#hub.rpc.GetBalanceRequest) | [GetBalanceReply](#hub.rpc.GetBalanceRequest) | ユーザーの利用可能残高を取得します。 |
| GetDepositAddress | [GetDepositAddressRequest](#hub.rpc.GetDepositAddressRequest) | [GetDepositAddressReply](#hub.rpc.GetDepositAddressRequest) | ユーザーの新しい預け入れアドレスを作成します。 |
| GetSeedForAddress | [GetSeedForAddressRequest](#hub.rpc.GetSeedForAddressRequest) | [GetSeedForAddressReply](#hub.rpc.GetSeedForAddressReply)  | 特定の預け入れアドレスの生成に使用されたシードを取得します。 |
| GetStats | [StatsRequest](#hub.rpc.StatsRequest) | [StatsReply](#hub.rpc.StatsRequest) | ハブが現在管理しているすべてのユーザーの合計残高を取得します。 |
| GetUserHistory | [GetUserHistoryRequest](#hub.rpc.GetUserHistoryRequest) | [GetUserHistoryReply](#hub.rpc.GetUserHistoryRequest) | ユーザーの残高履歴を取得します。 |
| ProcessTransferBatch | [ProcessTransferBatchRequest](#hub.rpc.ProcessTransferBatchRequest) | [ProcessTransferBatchReply](#hub.rpc.ProcessTransferBatchRequest) | 取引所からの買い/売りのバッチを処理します。このバッチの合計金額は0でなければならないことに注意してください。 |
| BalanceSubscription | [BalanceSubscriptionRequest](#hub.rpc.BalanceSubscriptionRequest) | [BalanceEvent](#hub.rpc.BalanceSubscriptionRequest) | 指定されたタイムスタンプ以降のすべての残高変更のストリームを監視します。 |
| RecoverFunds | [RecoverFundsRequest](#hub.rpc.RecoverFundsRequest) | [RecoverFundsReply](#hub.rpc.RecoverFundsReply) | IOTAトークンを使用済みアドレスからリカバーします。 |
| SignBundle | [SignBundleRequest](#hub.rpc.SignBundleRequest) | [SignBundleReply](#hub.rpc.SignBundleRequest) | バンドルハッシュの署名を取得します。 |
| SweepDetail | [SweepDetailRequest](#hub.rpc.SweepDetailRequest) | [SweepDetailReply](#hub.rpc.SweepDetailRequest) | スウィープに関する詳細情報を取得します。 |
| SweepInfo | [SweepInfoRequest](#hub.rpc.SweepInfoRequest) | [SweepEvent](#SweepEvent) | 特定の取り出しまたはバンドルハッシュのスウィープに関する情報を取得します。 |
| SweepSubscription | [SweepSubscriptionRequest](#hub.rpc.SweepSubscriptionRequest) | [SweepEvent](#SweepEvent) | 指定されたタイムスタンプ以降のすべてのスウィープのストリームを監視します。 |
| UserWithdraw | [UserWithdrawRequest](#hub.rpc.UserWithdrawRequest) | [UserWithdrawReply](#hub.rpc.UserWithdrawRequest) | ユーザーに取り出しリクエストを送信します。 |
| UserWithdrawCancel | [UserWithdrawCancelRequest](#hub.rpc.UserWithdrawCancelRequest) | [UserWithdrawCancelReply](#hub.rpc.UserWithdrawCancelRequest) | ユーザーの取り出しリクエストの取り消しを送信します。スウィープに含まれていない場合にのみ可能です。 |
| WasAddressSpentFrom | [WasAddressSpentFromRequest](#hub.rpc.WasAddressSpentFromRequest) | [WasAddressSpentFromReply](#hub.rpc.WasAddressSpentFromReply) | 預け入れアドレスからすでに取り出されているかどうかを調べます。 |
| WasWithdrawalCancelled | [WasWithdrawalCancelledRequest](#hub.rpc.WasWithdrawalCancelledRequest) | [WasWithdrawalCancelledReply](#hub.rpc.WasWithdrawalCancelledReply) | 取り出しがキャンセルされたかどうかを調べます。 |

<a name="messages.proto"></a>

## messages.proto

<a name="hub.rpc.BalanceSubscriptionRequest"></a>

### BalanceSubscriptionRequest

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| newerThan | [uint64](#uint64) | singular | Unixエポックからミリ秒単位でのイベントを開始した時からの、残高の変更に対して`BalanceEvent`オブジェクトを返します。 |

<a name="hub.rpc.BalanceEvent"></a>

### BalanceEvent

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| userAccountEvent | [UserAccountBalanceEvent](#hub.rpc.UserAccountBalanceEvent) | singular | ユーザーのアカウントの残高の変更に関する詳細を含むイベントオブジェクト |
| userAddressEvent | [UserAddressBalanceEvent](#hub.rpc.UserAddressBalanceEvent) | singular | ユーザーの預け入れアドレスの残高の変更に関する詳細を含むイベントオブジェクト |
| hubAddressEvent  | [HubAddressBalanceEvent](#hub.rpc.HubAddressBalanceEvent)   | singular | ハブ所有者のアドレスのいずれかの残高の変更に関する詳細を含むイベントオブジェクト |

<a name="hub.rpc.CreateUserRequest"></a>

### CreateUserRequest

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| userId | [string](#string) | singular | 新しいユーザーの一意のID |

<a name="hub.rpc.CreateUserReply"></a>

### CreateUserReply

現在使用されていません。
<!-- Currently not used. -->

<a name="hub.rpc.GetAddressInfoRequest"></a>

### GetAddressInfoRequest

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| address | [string](#string) | singular | 所有者を検索する81トライトの預け入れアドレス（チェックサムなし） |

<a name="hub.rpc.GetAddressInfoReply"></a>

### GetAddressInfoReply

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| userId | [string](#string) | singular | 預け入れアドレスを所有するユーザーのID |

<a name="hub.rpc.GetBalanceRequest"></a>

### GetBalanceRequest

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| userId | [string](#string) | singular | 残高を表示するユーザーのID |

<a name="hub.rpc.GetBalanceReply"></a>

### GetBalanceReply

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| available | [int64](#int64) | singular | 現在の取り出しとトレードに利用できるユーザーの合計残高 |

<a name="hub.rpc.GetDepositAddressRequest"></a>

### GetDepositAddressRequest

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| userId | [string](#string) | singular | 新しい預け入れアドレスを作成するユーザーのID |
| includeChecksum | [bool](#bool) | singular | アドレスに最後に9トライテットチェックサムを含めるかどうか |

<a name="hub.rpc.GetDepositAddressReply"></a>

### GetDepositAddressReply

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| address | [string](#string) | singular | 新しい預け入れアドレス |

<a name="hub.rpc.GetSeedForAddressRequest"></a>

### GetSeedForAddressRequest

To use this method, you must run Hub with the [`--GetSeedForAddress_enabled` flag](../references/command-line-options.md#signBundle).

|**Field**|**Type**|**Rules**|**Description**|
| --------------- | ----------------- | ----- | ----------- |
| userId          | [string](#string) |   singular    |  The ID of the user that owns the deposit address|
| address | [string](#string)   |   singular    | The deposit address whose seed you want to generate          |

<a name="hub.rpc.GetSeedForAddressReply"></a>

### GetSeedForAddressReply

|**Field**|**Type**|**Rules** |**Description**|
| ------- | ----------------- | ----- | ----------------------------- |
| seed | [string](#string) |  singular     | A new deposit address |

<a name="hub.rpc.GetUserHistoryRequest"></a>

### GetUserHistoryRequest

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| userId | [string](#string) | singular | ユーザーID |
| newerThan | [uint64](#uint64) | singular | UNIXエポック後にミリ秒単位で発生した`UserAccountBalanceEvent`オブジェクトの一覧を取得します。 |

<a name="hub.rpc.GetUserHistoryReply"></a>

### GetUserHistoryReply

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| events | [UserAccountBalanceEvent](#hub.rpc.UserAccountBalanceEvent) objects | repeated | 特定のユーザーのすべての残高イベントオブジェクトの一覧 |

<a name="hub.rpc.HubAddressBalanceEvent"></a>

### HubAddressBalanceEvent

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| hubAddress | [string](#string) | singular | ハブ所有者のアドレス |
| amount | [int64](#int64) | singular | 残高に変更した量 |
| reason | [HubAddressBalanceReason](#hub.rpc.HubAddressBalanceReason) | singular | バランスが変わった理由 |
| sweepBundleHash | [string](#string) | singular | 更新された残高になったスウィープのバンドルハッシュ |
| timestamp | [uint64](#uint64) | singular | イベントが発生したミリ秒単位のUnixエポック |

<a name="hub.rpc.ProcessTransferBatchReply"></a>

### HubAddressBalanceReason

| **名前** | **番号** | **説明** |
| :------- | :------- | :------- |
| HUB_UNKNOWN | 0 |              |
| INBOUND     | 1 | スウィープインバウンド（残余アドレスとして使用） |
| OUTBOUND    | 2 | スウィープアウトバウンド（入力として使用） |

### ProcessTransferBatchReply

<a name="hub.rpc.ProcessTransferBatchRequest"></a>

### ProcessTransferBatchRequest

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| transfers | [ProcessTransferBatchRequest.Transfer](#hub.rpc.ProcessTransferBatchRequest.Transfer) objects | repeated | 転送中に使用するユーザーのアカウントを指定する転送オブジェクト |

<a name="hub.rpc.ProcessTransferBatchRequest.Transfer"></a>

### ProcessTransferBatchRequest.Transfer

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| userId | [string](#string) | singular | 転送中に残高を使用するユーザーのID |
| amount | [int64](#int64)   | singular | 転送用のトークンの量 |

### RecoverFundsRequest

このメソッドを使用するには、[`--RecoverFunds_enabled`フラグ](../references/command-line-options.md#recoverFunds)を指定してハブを実行する必要があります。
<!-- To use this method, you must run Hub with the [`--RecoverFunds_enabled` flag](../references/command-line-options.md#recoverFunds). -->

<a name="hub.rpc.RecoverFundsRequest"></a>

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| userId | [string](#string) | singular | 資金を回収したい使用済みアドレスのユーザーのID |
| address | [string](#string) |  singular | ユーザーの使用済みアドレス（チェックサムなし） |
| validateChecksum | [string](#string) | singular | アドレスを検証するかどうか。`payoutAddress`フィールドが90トライトアドレス（チェックサム付き）の場合、このフィールドを`true`に設定します。 |
| payoutAddress | [string](#string) | singular | `アドレス`フィールドのアドレスの合計残高の転送先アドレス（チェックサムを含む場合があります） |

<a name="hub.rpc.RecoverFundsReply"></a>

### RecoverFundsReply

現在使用されていません。
<!-- Currently not used. -->

### SignBundleRequest

このメソッドを使用するには、[`--SignBundle_enabled`フラグ](../references/command-line-options.md#signBundle)を指定してハブを実行する必要があります。
<!-- To use this method, you must run Hub with the [`--SignBundle_enabled` flag](../references/command-line-options.md#signBundle). -->

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| address          | [string](#string) | singular | 取り出したいユーザーの預け入れアドレス（チェックサムを含む場合があります） |
| bundleHash       | [string](#string) | singular | 署名が必要なバンドルハッシュ |
| authentication   | [string](#string) | singular | 認証トークン（使用する場合） |
| validateChecksum | [bool](#bool)     | singular | アドレスを検証するかどうか。`address`フィールドが90トライトアドレス（チェックサム付き）の場合、このフィールドを`true`に設定します。 |

<a name="hub.rpc.SignBundleReply"></a>

### SignBundleReply

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| signature | [string](#string) | singular | バンドルの署名 |

<a name="hub.rpc.StatsRequest"></a>

### StatsRequest

<a name="hub.rpc.SweepDetailRequest"></a>

<a name="hub.rpc.StatsReply"></a>

### StatsReply

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| totalBalance | [uint64](#uint64) | singular | ハブが現在管理しているすべてのユーザーアカウントの合計残高 |

### SweepDetailRequest

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| bundleHash | [string](#string) | singular | 詳細が必要なスウィープのバンドルハッシュ |

<a name="hub.rpc.SweepDetailReply"></a>

### SweepDetailReply

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| confirmed | [bool](#bool) | singular | スウィープの確定ステータス |
| trytes | [string](#string) | repeated | スウィープのトランザクショントライト |
| tailHash | [string](#string) | repeated | スウィープの末尾トランザクションハッシュ（再添付されたスウィープごとに、新しい末尾トランザクションハッシュが生成されます。） |

<a name="hub.rpc.SweepEvent"></a>

### SweepEvent

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| bundleHash | [string](#string) | singular | スウィープのバンドルハッシュ |
| timestamp | [uint64](#uint64) | singular | スイープが作成されたときのUNIXタイムスタンプ |
| withdrawalUUID | [string](#string) | repeated | スウィープに含まれているすべてのユーザーの取り出しリクエストのUUID |

<a name="hub.rpc.SweepInfoRequest"></a>

### SweepInfoRequest

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| withdrawalUUID | [string](#string) | singular | スウィープへの包含を確認する取り出しUUID |
| bundleHash | [string](#string) | singular | 確認するスウィープのバンドルハッシュ |

<a name="hub.rpc.SweepSubscriptionRequest"></a>

### SweepSubscriptionRequest

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| newerThan | [uint64](#uint64) | singular | このUnixエポックよりも新しいスウィープをミリ秒単位で監視します。 |

<a name="hub.rpc.UserAccountBalanceEvent"></a>

### UserAccountBalanceEvent

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| userId | [string](#string) | singular | アカウントの残高が変更されたユーザーのID |
| timestamp | [uint64](#uint64) | singular | MSでエポックから残高変更が発生したまでの時間 |
| type | [UserAccountBalanceEventType](#hub.rpc.UserAccountBalanceEventType) | singular | アカウント残高の変更を引き起こしたイベントのタイプ |
| amount | [int64](#int64) | singular | 残高に変更した量 |
| sweepBundleHash or withdrawalUUID | [string](#string) | singular | `DEPOSIT`イベントのバンドルハッシュか`WITHDRAWAL`イベントか`WITHDRAWAL_CANCELED`イベントの取り出しUUIDのいずれかが含まれます。 |

<a name="hub.rpc.UserAccountBalanceEventType"></a>

### UserAccountBalanceEventType

| **名前** | **番号** | **説明** |
| :------- | :------- | :------- |
| UAB_UNKNOWN         | 0 | 未使用 |
| DEPOSIT             | 1 | ユーザーアカウントへの預け入れ（正の量） |
| BUY                 | 2 | ユーザーが転送バッチの一部として購入したトークン（正の量） |
| WITHDRAWAL          | 3 | ユーザーの取り出しリクエスト（負の量） |
| WITHDRAWAL_CANCELED | 4 | キャンセルされたユーザーの取り出しリクエスト（正の量） |
| SELL                | 5 | ユーザーが転送バッチの一部として売却したトークン（負の量） |

<a name="hub.rpc.UserAddressBalanceEvent"></a>

### UserAddressBalanceEvent

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| userId | [string](#string) | singular | アカウントの残高が変更されたユーザーのID |
| userAddress | [string](#string) | singular | 残高が変更されたアドレス |
| amount | [int64](#int64) | singular | 残高に変更した量 |
| reason | [UserAddressBalanceReason](#hub.rpc.UserAddressBalanceReason) | singular | アドレスの残高が変更された理由 |
| tail transaction hash or bundle hash | [string](#string) | singular | `DEPOSIT`理由の末尾トランザクションハッシュまたは`SWEEP`理由のバンドルハッシュのいずれかが含まれます。 |
| timestamp | [uint64](#uint64) | singular | MSでエポックからバランス変更が発生したまでの時間 |

<a name="hub.rpc.UserAddressBalanceReason"></a>

### UserAddressBalanceReason

| **名前** | **番号** | **説明** |
| :------- | :------- | :------- |
| UADD_UNKNOWN | 0 | 不明 |
| UA_DEPOSIT   | 1 | 新規ユーザーの預け入れ |
| UA_SWEEP     | 2 | 新しいスウィープ |

<a name="hub.rpc.UserWithdrawCancelRequest"></a>

### UserWithdrawCancelRequest

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| withdrawalUUID | [string](#string) | singular | キャンセルする取り出しUUID |

<a name="hub.rpc.UserWithdrawCancelReply"></a>

### UserWithdrawCancelReply

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| success | [bool](#bool) | singular | 取り出しがキャンセルされたかどうか |

<a name="hub.rpc.UserWithdrawRequest"></a>

### UserWithdrawRequest

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| userId | [string](#string) | singular | 取り出しをリクエストしたユーザーのID |
| payoutAddress | [string](#string) | singular | 取り出しが預け入れられるアドレス（チェックサムなし） |
| amount | [uint64](#uint64) | singular | リクエストされた取り出し量 |
| tag | [string](#string) | singular | 取り出しトランザクションに追加するタグ |
| validateChecksum | [bool](#bool) | singular | アドレスを検証するかどうか |

<a name="hub.rpc.UserWithdrawReply"></a>

### UserWithdrawReply

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| withdrawalUUID | [string](#string) | singular | 取り出しのUUID |

<a name="hub.rpc.WasWithdrawalCancelledRequest"></a>

### WasWithdrawalCancelledRequest

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| withdrawalUUID | [string](#string) | singular | 取り消しを確認するための取り出しUUID |

<a name="hub.rpc.WasWithdrawalCancelledReply"></a>

<a name="hub.rpc.WasAddressSpentFromRequest"></a>

### WasAddressSpentFromRequest

預け入れアドレスからすでに取り出されているかどうかを調べます。
<!-- Find out if a deposit address has already been withdrawn from. -->

このエンドポイントが`true`を返す場合、これ以上IOTAトークンを預け入れないでください。
<!-- If this endpoint returns true, you should not deposit any more IOTA tokens into it. -->

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| address          | [string](#string) | singular | 使用済みステータスを確認するユーザーの預け入れアドレス（チェックサムを含めることができます） |
| validateChecksum | [bool](#bool)     | singular | アドレスを検証するかどうか。`address`フィールドが90トライアドレス（チェックサム付き）の場合、このフィールドを`true`に設定します。 |

<a name="hub.rpc.WasAddressSpentFromReply"></a>

### WasAddressSpentFromReply

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| wasAddressSpentFrom | [bool](#bool) | singular | アドレスが使用されているかどうか |

<a name="hub.rpc.StatsRequest"></a>

### WasWithdrawalCancelledReply

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| wasCancelled | [bool](#bool) | singular | 取り出しがキャンセルされたかどうか |

<a name="hub.rpc.Error"></a>

<a name="hub.rpc.HubAddressBalanceReason"></a>

### Error

エラーはシリアル化され、ステータスの詳細フィールドに保存されます。
<!-- Errors are serialized and stored in the Status' detail field. -->

| **フィールド** | **タイプ** | **ルール** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| code | [ErrorCode](#hub.rpc.ErrorCode) | singular | メソッドが失敗した理由を説明するエラーコード |

<a name="hub.rpc.ErrorCode"></a>

### ErrorCode

これらはすべて、ハブから返される可能性のあるエラーコードです。
<!-- These are all the error codes that can be returned by the Hub. -->

| **名前** | **番号** | **説明** |
| :------- | :------- | :------- |
| EC_UNKNOWN                      | 0 | 未使用 |
| USER_EXISTS                     | 1 | ユーザーIDは既に存在します。 |
| USER_DOES_NOT_EXIST             | 2 | ユーザーIDは存在しません。 |
| INSUFFICIENT_BALANCE            | 3 | この操作に対してユーザーの残高が不足しています。 |
| BATCH_INVALID                   | 4 | バッチが無効です（合計が0になっていないか、ユーザーIDが一意ではありません）。 |
| BATCH_INCONSISTENT              | 5 | バッチが矛盾しています（十分な残高がないままユーザーのアカウントから資金を削除しようとしています）。 |
| BATCH_AMOUNT_ZERO               | 6 | 取引に関連する金額が無効です（0より大きくまたは小さくなければなりません）。 |
| UNKNOWN_ADDRESS                 | 7 | アドレスがハブに認識されていません。 |
| WITHDRAWAL_CAN_NOT_BE_CANCELLED | 8 | 取り出しは既にスウィープまたはキャンセルされています。 |
| INELIGIBLE_ADDRESS              | 9 | アドレスはリクエストされた操作の資格がありません。 |
| INVALID_AUTHENTICATION          | 10 | 指定された認証トークンは無効です。 |
| CHECKSUM_INVALID                | 11 | 指定されたアドレスに無効なチェックサムが含まれています。 |
| SIGNING_FAILED                  | 12 | rpc signing_serverの呼び出しに失敗しました（GetSignatureForUUID）。 |
| GET_ADDRESS_FAILED              | 13 | rpc signing_serverの呼び出しに失敗しました（GetAddressForUUID）。 |
| GET_SECURITY_LEVEL_FAILED       | 14 | rpc signing_serverの呼び出しに失敗しました（GetSecurityLevel）。 |
| IRI_CLIENT_UNAVAILABLE          | 15 | ノードのAPIの呼び出しに失敗しました。 |
| ADDRESS_WAS_ALREADY_SPENT       | 16 | 指定されたアドレスは既に使用されています。 |
| INVALID_UUID                    | 17 | 指定されたUUIDは無効です。 |
| WRONG_USER_ADDRESS              | 18 | 指定されたアドレスはユーザーに属していません。 |
| ADDRESS_BALANCE_ZERO            | 19 | 指定されたアドレスにはIOTAトークンが含まれていません。 |

## スカラー値タイプ
<!-- ## Scalar Value Types -->

| **.protoタイプ**               | **メモ** | **C++での型** | **Javaでの型** | **Pythonでの型** |
| :----------------------------- | :------- | :------------ | :------------- | :--------------- |
| <a name="double" /> double     |          | double | double | float |
| <a name="float" /> float       |          | float | float | float |
| <a name="int32" /> int32       | 可変長符号化を使用します。負の数をエンコードするのは非効率的です。- あなたのフィールドが負の値を持つ可能性が高い場合は、代わりにsint32を使用してください。 | int32 | int | int |
| <a name="int64" /> int64       | 可変長符号化を使用します。負の数をエンコードするのは非効率的です。- あなたのフィールドが負の値を持つ可能性が高い場合は、代わりにsint64を使用してください。 | int64 | long | int/long |
| <a name="uint32" /> uint32     | 可変長符号化を使用します。 | uint32 | int | int/long |
| <a name="uint64" /> uint64     | 可変長符号化を使用します。 | uint64 | long | int/long |
| <a name="sint32" /> sint32     | 可変長符号化を使用します。符号付きint値。これらは通常のint32よりも効率的に負の数をエンコードします。 | int32 | int | int |
| <a name="sint64" /> sint64     | 可変長符号化を使用します。符号付きint値。これらは通常のint64よりも効率的に負の数をエンコードします。 | int64 | long | int/long |
| <a name="fixed32" /> fixed32   | 常に4バイト。値がよく2<sup>28</sup>より大きくなる場合は、uint32より効率的です。 | uint32 | int | int |
| <a name="fixed64" /> fixed64   | 常に8バイト。値がよく2<sup>56</sup>より大きくなる場合は、uint64より効率的です。 | uint64 | long | int/long |
| <a name="sfixed32" /> sfixed32 | 常に4バイト。 | int32 | int | int |
| <a name="sfixed64" /> sfixed64 | 常に8バイト。 | int64 | long | int/long |
| <a name="bool" /> bool         |               | bool | boolean | boolean |
| <a name="string" /> string     | 文字列には常にUTF-8エンコードまたは7ビットASCIIテキストを含める必要があります。 | string | String | str/Unicode |
| <a name="bytes" /> bytes       | 任意のバイトシーケンスを含めることができます。 | string | ByteString | str |
