# APIリファレンス
<!-- # API reference -->

**ハブには、タングルおよびハブデータベースへの接続を簡素化するgRPC APIがあります。このAPIリファレンスを使用して、メソッドを見つけ、メソッドが行うことを学びます。**
<!-- **Hub has a gRPC API that simplifies connections to the Tangle and the Hub database. Use this API reference to find methods and learn what they do.** -->

<a name="hub.proto"></a>

## hub.proto

<a name="hub.rpc.Hub"></a>

### Hub

| **メソッド名** | **リクエストタイプ** | **レスポンスタイプ** | **説明** |
| ---------- | ---------------- | ---------------- | ---- |
| CreateUser | [CreateUserRequest](#hub.rpc.CreateUserRequest) | [CreateUserReply](#hub.rpc.CreateUserRequest) | ハブに新しいユーザーを作成します。 |
| GetBalance | [GetBalanceRequest](#hub.rpc.GetBalanceRequest) | [GetBalanceReply](#hub.rpc.GetBalanceRequest) | ユーザーの利用可能残高を返します。 |
| GetDepositAddress | [GetDepositAddressRequest](#hub.rpc.GetDepositAddressRequest) | [GetDepositAddressReply](#hub.rpc.GetDepositAddressRequest) | ユーザーの新しい預け入れアドレスを作成します。 |
| UserWithdraw | [UserWithdrawRequest](#hub.rpc.UserWithdrawRequest) | [UserWithdrawReply](#hub.rpc.UserWithdrawRequest) | ユーザーに取り出しリクエストを送信します。 |
| UserWithdrawCancel | [UserWithdrawCancelRequest](#hub.rpc.UserWithdrawCancelRequest) | [UserWithdrawCancelReply](#hub.rpc.UserWithdrawCancelRequest) | ユーザーの取り出しリクエストの取り消しを試みます。スウィープがまだ始まっていない限り可能です。 |
| GetUserHistory | [GetUserHistoryRequest](#hub.rpc.GetUserHistoryRequest) | [GetUserHistoryReply](#hub.rpc.GetUserHistoryRequest) | ハブに記録されているユーザーの全残高の変更履歴を返します。 |
| ProcessTransferBatch | [ProcessTransferBatchRequest](#hub.rpc.ProcessTransferBatchRequest) | [ProcessTransferBatchReply](#hub.rpc.ProcessTransferBatchRequest) | 取引所からの買い/売りのバッチを処理します。このバッチの金額の合計は0でなければならないことに注意してください！ |
| BalanceSubscription | [BalanceSubscriptionRequest](#hub.rpc.BalanceSubscriptionRequest) | [BalanceEvent](#hub.rpc.BalanceSubscriptionRequest) | 提供されたタイムスタンプ以降に表示されるすべての残高変更のストリームを生成します。 |
| GetStats | [StatsRequest](#hub.rpc.StatsRequest) | [StatsReply](#hub.rpc.StatsRequest) | ハブに関する統計を提供します。 |
| SweepSubscription | [SweepSubscriptionRequest](#hub.rpc.SweepSubscriptionRequest) | [SweepEvent](#hub.rpc.SweepSubscriptionRequest) | 指定されたタイムスタンプ以降のすべてのスウィープのストリームを生成します。 |
| GetAddressInfo | [GetAddressInfoRequest](#hub.rpc.GetAddressInfoRequest) | [GetAddressInfoReply](#hub.rpc.GetAddressInfoRequest) | ハブによって所有されている場合、そのアドレスに関する情報を提供します。 |
| SweepInfo | [SweepInfoRequest](#hub.rpc.SweepInfoRequest) | [SweepEvent](#hub.rpc.SweepInfoRequest) | 特定の取り出しまたはバンドルハッシュのスウィープに関する情報を提供します。 |
| SignBundle | [SignBundleRequest](#hub.rpc.SignBundleRequest) | [SignBundleReply](#hub.rpc.SignBundleRequest) | バンドルハッシュの署名を提供します。 |
| SweepDetail | [SweepDetailRequest](#hub.rpc.SweepDetailRequest) | [SweepDetailReply](#hub.rpc.SweepDetailRequest) | バンドルハッシュのスウィープに関する詳細情報を提供します。 |
| WasWithdrawalCancelled | [WasWithdrawalCancelledRequest](#hub.rpc.WasWithdrawalCancelledRequest) | [WasWithdrawalCancelledReply](#hub.rpc.WasWithdrawalCancelledReply) | 取り出しがキャンセルされた場合、trueを返します。 |

<a name="messages.proto"></a>

## messages.proto

<a name="hub.rpc.BalanceEvent"></a>

### BalanceEvent

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| userAccountEvent | [UserAccountBalanceEvent](#hub.rpc.UserAccountBalanceEvent) |       |      |
| userAddressEvent | [UserAddressBalanceEvent](#hub.rpc.UserAddressBalanceEvent) |       |      |
| hubAddressEvent  | [HubAddressBalanceEvent](#hub.rpc.HubAddressBalanceEvent)   |       |      |

<a name="hub.rpc.BalanceSubscriptionRequest"></a>

### BalanceSubscriptionRequest

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| newerThan | [uint64](#uint64) |       | 今回より新しい残高（エポック以降のMS）が取得されます。 |

<a name="hub.rpc.CreateUserRequest"></a>

### CreateUserRequest

新しいユーザーを作成するリクエスト。
<!-- Request to create a new user. -->

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| userId | [string](#string) |       |             |

<a name="hub.rpc.CreateUserReply"></a>

### CreateUserReply

新しいユーザーを作成するための返信（現在は未使用）。
<!-- Reply for creating a new user (currently unused). -->

<a name="hub.rpc.GetAddressInfoReply"></a>

### GetAddressInfoReply

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| userId | [string](#string) |       | アドレスを所有するユーザーID |

<a name="hub.rpc.GetAddressInfoRequest"></a>

### GetAddressInfoRequest

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| address | [string](#string) |       | 調べるアドレス |

<a name="hub.rpc.GetBalanceRequest"></a>

### GetBalanceRequest

ユーザーの現在利用可能な残高を返すリクエスト。
<!-- Request to return a user's currently available balance. -->

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| userId | [string](#string) |       |             |

<a name="hub.rpc.GetBalanceReply"></a>

### GetBalanceReply

ユーザーの現在利用可能な残高を返します。
<!-- Returns a user's currently available balance -->

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| available | [int64](#int64) |       | 現在、取り出し/トレードに利用可能なユーザーの残高 |

<a name="hub.rpc.GetDepositAddressRequest"></a>

### GetDepositAddressRequest

ユーザーの新しい預け入れアドレスを作成するためのリクエスト。
<!-- Request for creating a new deposit address for a user. -->

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| userId          | [string](#string) |       |             |
| includeChecksum | [bool](#bool)     |       |             |

<a name="hub.rpc.GetDepositAddressReply"></a>

### GetDepositAddressReply

新しい預け入れアドレスを含む返信。
Reply containing the new deposit address.

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| address | [string](#string) |       | 新しく作成された預け入れアドレス |

<a name="hub.rpc.GetUserHistoryRequest"></a>

### GetUserHistoryRequest

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| userId    | [string](#string) |       | ユーザーID |
| newerThan | [uint64](#uint64) |       | 今回より新しいレコード（エポック以降のMS）が取得されます。 |

<a name="hub.rpc.GetUserHistoryReply"></a>

### GetUserHistoryReply

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| events | [UserAccountBalanceEvent](#hub.rpc.UserAccountBalanceEvent) | repeated | 与えられたユーザーに関するすべてのユーザー残高イベントの一覧 |

<a name="hub.rpc.HubAddressBalanceEvent"></a>

### HubAddressBalanceEvent

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| hubAddress      | [string](#string)                                           |       |             |
| amount          | [int64](#int64)                                             |       |             |
| reason          | [HubAddressBalanceReason](#hub.rpc.HubAddressBalanceReason) |       |             |
| sweepBundleHash | [string](#string)                                           |       |             |
| timestamp       | [uint64](#uint64)                                           |       |             |

<a name="hub.rpc.ProcessTransferBatchReply"></a>

### ProcessTransferBatchReply

<a name="hub.rpc.ProcessTransferBatchRequest"></a>

### ProcessTransferBatchRequest

このリクエストには、ネット転送バッチが含まれています。ユーザーIDは一意である必要があります。
<!-- This request contains a netted transfer batch. -->
<!-- User IDs must be unique. -->

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| transfers | [ProcessTransferBatchRequest.Transfer](#hub.rpc.ProcessTransferBatchRequest.Transfer) | repeated |             |

<a name="hub.rpc.ProcessTransferBatchRequest.Transfer"></a>

### ProcessTransferBatchRequest.Transfer

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| userId | [string](#string) |       |             |
| amount | [int64](#int64)   |       |             |

<a name="hub.rpc.SignBundleRequest"></a>

### SignBundleRequest

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| address          | [string](#string) |       | 署名する必要があるハブ所有者のIOTAアドレス（チェックサムなし） |
| bundleHash       | [string](#string) |       | 署名する必要があるバンドルハッシュ |
| authentication   | [string](#string) |       | 認証トークン（使用する場合） |
| validateChecksum | [bool](#bool)     |       | 署名者がアドレスを検証する必要があるかどうか |

<a name="hub.rpc.SignBundleReply"></a>

### SignBundleReply

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| signature | [string](#string) |       | 署名の結果 |

<a name="hub.rpc.StatsRequest"></a>

### StatsRequest

<a name="hub.rpc.SweepDetailRequest"></a>

<a name="hub.rpc.StatsReply"></a>

### StatsReply

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| totalBalance | [uint64](#uint64) |       | Total balance currently managed by the hub. |

### SweepDetailRequest

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| bundleHash | [string](#string) |       | スイープのバンドルハッシュ |

<a name="hub.rpc.SweepDetailReply"></a>

### SweepDetailReply

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| confirmed | [bool](#bool)     |          | スウィープの確定ステータス |
| trytes    | [string](#string) | repeated | スウィープのトランザクショントライト |
| tailHash  | [string](#string) | repeated | スウィープの末尾ハッシュ（バンドルごとに複数の再添付） |

<a name="hub.rpc.SweepEvent"></a>

### SweepEvent

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| bundleHash     | [string](#string) |          |             |
| timestamp      | [uint64](#uint64) |          |             |
| withdrawalUUID | [string](#string) | repeated |             |

<a name="hub.rpc.SweepInfoRequest"></a>

### SweepInfoRequest

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| withdrawalUUID | [string](#string) |       | 調べる取り出しUUID |
| bundleHash     | [string](#string) |       | スウィープのバンドルハッシュ |

<a name="hub.rpc.SweepSubscriptionRequest"></a>

### SweepSubscriptionRequest

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| newerThan | [uint64](#uint64) |       | 今回より新しいスウィープ（エポック以降のMS）が取得されます。 |

<a name="hub.rpc.UserAccountBalanceEvent"></a>

### UserAccountBalanceEvent

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| userId          | [string](#string) |       |                                          |
| timestamp       | [uint64](#uint64) |       | MSで残高変更が発生した時点からの経過時間 |
| **type**        | [UserAccountBalanceEventType](#hub.rpc.UserAccountBalanceEventType) |       |                                                |
| amount          | [int64](#int64)   |       |                                          |
| sweepBundleHash | [string](#string) |       | 預け入れ時にはスウィープバンドルハッシュが含まれます。取り出しまたは取り出しキャンセル時には取り出しuuidが含まれます。 |
| withdrawalUUID  | [string](#string) |       |                                          |

<a name="hub.rpc.UserAddressBalanceEvent"></a>

### UserAddressBalanceEvent

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| userId      | [string](#string) |       |           |
| userAddress | [string](#string) |       |           |
| amount      | [int64](#int64)   |       |           |
| reason      | [UserAddressBalanceReason](#hub.rpc.UserAddressBalanceReason) |       |   |
| hash        | [string](#string) |       | 預け入れているバンドルの末尾（reason == DEPOSITの場合）スイープのバンドルハッシュ（reason == SWEEPの場合） |
| timestamp   | [uint64](#uint64) |       |           |

<a name="hub.rpc.UserWithdrawCancelRequest"></a>

### UserWithdrawCancelRequest

すでに提出された取り出しをキャンセルするリクエスト。
<!-- Request to cancel an already submitted withdrawal. -->

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| withdrawalUUID  | [string](#string) |       | キャンセルされる取り出しUUID |

<a name="hub.rpc.UserWithdrawCancelReply"></a>

### UserWithdrawCancelReply

取り出しキャンセルの返信。
<!-- Reply for withdrawal cancellation. -->

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| success | [bool](#bool) |       | 取り出しを取り消すことができれば`true` |

<a name="hub.rpc.UserWithdrawRequest"></a>

### UserWithdrawRequest

ユーザーが送信した取り出しをリクエストします。
<!-- Requests a user-submitted withdrawal. -->

ユーザーが利用可能な十分なバランスを持っていない場合、これは失敗します。
<!-- This will fail if the user does not have sufficient balance available. -->

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| wasCancelled     | [bool](#bool) |       |          |
| payoutAddress    | [string](#string) |       | ユーザーが支払いをリクエストするアドレス。チェックサムなしである必要があります。 |
| amount           | [uint64](#uint64) |       | リクエストされた取り出し額 |
| tag              | [string](#string) |       | 取り出しのタグ |
| validateChecksum | [bool](#bool)     |       | コマンドはアドレスを検証する必要があります。 |

<a name="hub.rpc.UserWithdrawReply"></a>

### UserWithdrawReply

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| withdrawalUUID  | [string](#string) |       | 取り出しのUUID |

<a name="hub.rpc.WasWithdrawalCancelledRequest"></a>

### WasWithdrawalCancelledRequest

取り出しがキャンセルされたかどうかを確認するリクエスト。
<!-- Requests to check if a withdrawal was canceled. -->

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| withdrawalUUID  | [string](#string) |       | 取り消しを確認するための取り出しUUID |

<a name="hub.rpc.WasWithdrawalCancelledReply"></a>

### WasWithdrawalCancelledReply

取り出しがキャンセルされたかどうかを返します。
<!-- Returns whether a withdrawal was canceled. -->

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| wasCencelled  | [bool](#bool) |       | 取り出しがキャンセルされた場合、trueを返します。 |

<a name="hub.rpc.Error"></a>

### Error

エラーはシリアル化され、ステータスの詳細フィールドに保存されます。
<!-- Errors are serialized and stored in the Status' detail field. -->

| **フィールド** | **タイプ** | **ラベル** | **説明** |
| -------------- | ---------- | ---------- | -------- |
| code  | [ErrorCode](#hub.rpc.ErrorCode) |       |             |

<a name="hub.rpc.ErrorCode"></a>

### ErrorCode

ハブから返される可能性があるエラーコード。
<!-- Error codes that can be returned by the hub. -->

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
| WITHDRAWAL_CAN_NOT_BE_CANCELLED | 8 | 取り出しはすでにスウィープされたか取り消されました。 |
| INELIGIBLE_ADDRESS              | 9 | アドレスがリクエストされた操作に適格ではありません。 |
| INVALID_AUTHENTICATION          | 10 | 提供された認証トークンが無効です。 |
| CHECKSUM_INVALID                | 11 | 提供されたアドレスに無効なチェックサムが含まれています。 |
| SIGNING_FAILED                  | 12 | rpc signing_serverの呼び出しに失敗しました（GetSignatureForUUID）。 |
| GET_ADDRESS_FAILED              | 13 | rpc signing_serverの呼び出しに失敗しました（GetAddressForUUID）。 |
| GET_SECURITY_LEVEL_FAILED       | 14 | rpc signing_serverの呼び出しに失敗しました（GetSecurityLevel）。 |

<a name="hub.rpc.HubAddressBalanceReason"></a>

### HubAddressBalanceReason

| **名前** | **番号** | **説明** |
| :------- | :------- | :------- |
| HUB_UNKNOWN | 0 |              |
| INBOUND     | 1 | スウィープインバウンド（残余アドレスとして使用） |
| OUTBOUND    | 2 | スウィープアウトバウンド（入力として使用） |

<a name="hub.rpc.UserAccountBalanceEventType"></a>

### UserAccountBalanceEventType

| **名前** | **番号** | **説明** |
| :------- | :------- | :------- |
| UAB_UNKNOWN         | 0 | 未使用 |
| DEPOSIT             | 1 | ユーザーアカウントへの預け入れ（正の量） |
| BUY                 | 2 | ユーザーが取引バッチの一部として購入したトークン（正の量） |
| WITHDRAWAL          | 3 | ユーザーの取り出しリクエスト（負の量） |
| WITHDRAWAL_CANCELED | 4 | キャンセルしたユーザーの取り出しリクエスト（負の量） |
| SELL                | 5 | ユーザーが取引バッチの一部として売却したトークン（負の量） |

<a name="hub.rpc.UserAddressBalanceReason"></a>

### UserAddressBalanceReason

| **名前** | **番号** | **説明** |
| :------- | :------- | :------- |
| UADD_UNKNOWN | 0 |             |
| UA_DEPOSIT   | 1 | 追跡された新規ユーザーの預け入れ |
| UA_SWEEP     | 2 | ハブスウィープ |

## Scalar Value Types

| .protoタイプ                   | メモ                                                         | C++での型 | Javaでの型 | Pythonでの型 |
| ------------------------------ | ------------------------------------------------------------ | ------ -- | ---------- | ------------ |
| <a name="double" /> double     |                                                              | double | double | float |
| <a name="float" /> float       |                                                              | float  | float  | float |
| <a name="int32" /> int32       | 可変長符号化を使用します。負の数をエンコードするのは非効率的です - あなたのフィールドが負の値を持つ可能性が高い場合は、代わりにsint32を使用してください。 | int32    | int        | int         |
| <a name="int64" /> int64       | 可変長符号化を使用します。負の数をエンコードするのは非効率的です - あなたのフィールドが負の値を持つ可能性が高い場合は、代わりにsint64を使用してください。 | int64    | long       | int/long    |
| <a name="uint32" /> uint32     | 可変長符号化を使用します。 | uint32   | int        | int/long    |
| <a name="uint64" /> uint64     | 可変長符号化を使用します。 | uint64   | long       | int/long    |
| <a name="sint32" /> sint32     | 可変長符号化を使用します。符号付きint値。これらは通常のint32よりも効率的に負の数をエンコードします。 | int32    | int        | int         |
| <a name="sint64" /> sint64     | 可変長符号化を使用します。符号付きint値。これらは通常のint64よりも効率的に負の数をエンコードします。 | int64    | long       | int/long    |
| <a name="fixed32" /> fixed32   | 常に4バイト。値がよく2<sup>28</sup>より大きくなる場合は、uint32より効率的です。 | uint32   | int        | int         |
| <a name="fixed64" /> fixed64   | 常に8バイト。値がよく2<sup>56</sup>より大きくなる場合は、uint64より効率的です。 | uint64   | long       | int/long    |
| <a name="sfixed32" /> sfixed32 | 常に4バイト。 | int32    | int        | int         |
| <a name="sfixed64" /> sfixed64 | 常に8バイト。 | int64    | long       | int/long    |
| <a name="bool" /> bool         |                                                              | bool     | boolean    | boolean     |
| <a name="string" /> string     | 文字列には常にUTF-8エンコードまたは7ビットASCIIテキストを含める必要があります。 | string   | String     | str/Unicode |
| <a name="bytes" /> bytes       | 任意のバイトシーケンスを含めることができます。 | string   | ByteString | str         |
