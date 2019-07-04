# データベーステーブル
<!-- # Database tables -->

**Hubは、預け入れアドレスや取り出しリクエストなどの情報をデータベーステーブルに記録することによって、ユーザーのIOTAトークンを管理します。この情報は、gRPC APIを使用するか、データベースサーバに問い合わせることによって見つけることができます。**
<!-- **Hub manages users' IOTA tokens by recording information such as deposit addresses and withdrawal requests in database tables. You can find this information by using the gRPC API or by querying the database server.** -->


| **テーブル** | **説明** |
| :----------- | :------- |
| `hub_address`          | ハブ所有者のアドレスについて。これらのアドレスは、スウィープ中に残余トークンが送信されるアドレスです。 |
| `hub_address_balance`  | ハブ所有者のアドレスの残高更新について。ハブは、アドレスの1つを含むアクションが完了するたびにこのテーブルを更新します。 |
| `signed_uuids`         | IOTAトークンを取り出した（使用済みまたは署名済みとも呼ばれる）アドレスのシードUUIDの記録 |
| `sweep`                | 確定済みおよびペンディング中のスウィープについて |
| `sweep_tails`          | スウィープの末尾トランザクションについて。ハブはこのテーブルを使用して、スウィープが確定されたかどうかを確認します。 |
| `user_account`         | ユーザーのハブアカウントについて |
| `user_account_balance` | ユーザーのハブアカウントの残高について |
| `user_address`         | ユーザーの預け入れアドレスについて |
| `user_address_balance` | ユーザーの預け入れアドレスの残高について。ハブは、アドレスの1つを含むアクションが完了するたびにこのテーブルを更新します。 |
| `withdrawal`           | ユーザーの取り出しリクエスト依頼の残高について |


## Hub_address

このテーブルには、ユーザーの預け入れアドレスのトークンがスウィープで転送されるハブ所有者のアドレスに関する情報が含まれています。
<!-- This table contains information about the Hub owner's addresses to which the tokens in users' deposit addresses are transferred in sweeps. -->

| **フィールド** | **説明** | **デフォルト値** |
| :------------- | :------- | :--------------- |
| `id`              | ハブ所有者のアドレスの固有ID | NULL |
| `address`         | 81トライトのアドレス（チェックサムは無し） | NULL |
| `seed_uuid`       | 普遍的に一意な識別子。この文字列は、アドレスを導出するシードを作成するために[Argon2](https://www.argon2.com/)ハッシュ関数で使用されます。ハブが`salt`フラグと一緒に起動されると、`salt`フラグの値はシードを作成するためのハッシュ関数でも使用されます。 | NULL |
| `is_cold_storage` | このアドレスのスウィープを無効にします。| 0 |
| `balance`         | アドレス内のIOTAトークンの総数 | 0 |
| `created_at`      | アドレスが`YYYY-MM-DD HH：MM：SS`の形式で保存された日時。 | 現在の日時 |

## Hub_address_balance

このテーブルには、ハブ所有者のアドレスの残高更新に関する情報が含まれています。
<!-- This table contains information about updates to the balance of the Hub owner's addresses. -->

| **フィールド** | **説明** | **デフォルト値** |
| :------------- | :------- | :--------------- |
| `id`          | 残高更新イベントの固有ID | NULL |
| `hub_address` | `hub_address`テーブル内のアドレスのID | NULL |
| `amount`      | アドレス内のIOTAトークンの総数 | 0 |
| `reason`      | 残高更新イベントの[3つの理由](../references/api-reference.md#hub.rpc.HubAddressBalanceReason)のうちの1つ | NULL |
| `sweep`       | `sweep`テーブル内のスウィープのID | NULL |
| `occured_at`  | 残高が`YYYY-MM-DD HH:MM:SS`の形式で更新された日時。 | 現在の日時 |

## Signed_uuids

このテーブルには、IOTAトークンが取り出された（使用済みまたは署名済みとも呼ばれる）アドレスのシードUUIDが含まれています。
<!-- This table contains the seed UUIDs for addresses that have been withdrawn from (also known as used or spent). -->

| **フィールド** | **説明** | **デフォルト値** |
| :------------- | :------- | :--------------- |
| `uuid` | 使用済みアドレスのシードUUID | NULL |

## Sweep

このテーブルには、スウィープに関する情報が含まれています。
<!-- This table contains information about sweeps. -->

| **フィールド** | **説明** | **デフォルト値** |
| :------------- | :------- | :--------------- |
| `id`               | スウィープイベントの固有ID | NULL |
| `bundle_hash`      | スウィープの81トライトのバンドルハッシュ | NULL |
| `trytes`           | スウィープのトランザクショントライト | NULL |
| `into_hub_address` | スウィープの残余トークンが送信されたアドレスのID。このIDは`hub_address`テーブルにあります。 | NULL |
| `confirmed`        | スウィープが確定されたかどうか。0=ペンディング中。 | 0 |
| `created_at`       | スウィープが`YYYY-MM-DD HH:MM:SS`の形式で作成された日時。 | 現在の日時 |

## Sweep_tails

このテーブルには、スイープが確定されたかどうかを確認するためにハブが使用する情報が含まれています。
<!-- This table contains information that Hub uses to check if a sweep is confirmed. -->

| **フィールド** | **説明** | **デフォルト値** |
| :------------- | :------- | :--------------- |
| `hash`       | スイープの81トライトの末尾トランザクションハッシュ | NULL |
| `sweep`      | 末尾トランザクションがバンドルに含まれているスイープのID。このIDは`sweep`テーブルにあります。 | NULL |
| `confirmed`  | スウィープが確定されたかどうか。0=ペンディング中。 | 0 |
| `created_at` | テーブルが`YYYY-MM-DD HH:MM:SS`の形式で作成された日時。 | 現在の日時 |

## User_account

このテーブルには、ユーザーのハブアカウントに関する情報が含まれています。
<!-- This table contains information about the users' Hub accounts. -->

| **フィールド** | **説明** | **デフォルト値** |
| :------------- | :------- | :--------------- |
| `id`         | ユーザーのアカウントの固有ID。各ユーザーは1つのアカウントを持ちます。 | NULL |
| `balance`    | ユーザーがハブで所有しているIOTAトークンの合計量 | 0 |
| `identifier` | ユーザーのハブ所有者定義ID | NULL |


## User_account_balance

このテーブルには、ユーザーのハブアカウントの残高に関する情報が含まれています。
<!-- This table contains information about the balances of users' Hub accounts. -->

| **フィールド** | **説明** | **デフォルト値** |
| :------------- | :------- | :--------------- |
| `user_id`    | `user_account`テーブル内のユーザーのアカウントのID | NULL |
| `amount`     | ユーザーがハブで所有しているIOTAトークンの合計量 | NULL |
| `reason`     | 残高更新イベントの[3つの理由](../references/api-reference.md#hub.rpc.HubAddressBalanceReason)のうちの1つ | NULL |
| `sweep`      | 残高を更新したスイープのID。このIDは`sweep`テーブルにあります。 | NULL |
| `withdrawal` | `withdrawal`テーブル内の取り出しのID | NULL |
| `occured_at` | ユーザーの残高が`YYYY-MM-DD HH:MM:SS`の形式で更新された日時。 | 現在の日時 |

## User_address

このテーブルには、ユーザーの預け入れアドレスに関する情報が含まれています。
<!-- This table contains information about the users' deposit addresses. -->

| **フィールド** | **説明** | **デフォルト値** |
| :------------- | :------- | :--------------- |
| `id`         | ユーザーのアドレスの固有ID。各アドレスは固有のシードから導出されます。 | NULL |
| `address`    | 81トライトのアドレス（チェックサムは無し） | NULL |
| `user_id`    | `user_account`テーブル内のユーザーのアカウントのID | NULL |
| `seed_uuid`  | 普遍的に一意な識別子。この文字列は、アドレスを導出するシードを作成するために[Argon2](https://www.argon2.com/)ハッシュ関数で使用されます。ハブが`salt`フラグと一緒に起動されると、`salt`フラグの値はシードを作成するためのハッシュ関数でも使用されます。 | NULL |
| `created_at` | アドレスが`YYYY-MM-DD HH:MM:SS`の形式で保存された日時。 | 現在の日時 |
| `balance`    | アドレス上のIOTAトークンの合計量 | 0 |


## User_address_balance

このテーブルには、ユーザーの預け入れアドレスの残高に関する情報が含まれています。
<!-- This table contains information about the balance of users' deposit addresses. -->

| **フィールド** | **説明** | **デフォルト値** |
| :------------- | :------- | :--------------- |
| `id`           | ユーザーの残高更新イベントの固有ID | NULL |
| `user_address` | `user_address`テーブル内のユーザーのアドレスのID | NULL |
| `amount`       | アドレス上のIOTAトークンの合計量 | NULL |
| `reason`       | 残高更新イベントの[3つの理由](../references/api-reference.md#hub.rpc.HubAddressBalanceReason)のうちの1つ | NULL |
| `tail_hash`    | スウィープの81トライトの末尾トランザクションハッシュ | NULL |
| `sweep`        | 残高を更新したスイープのID。このIDは`sweep`テーブルにあります。 | NULL |
| `message`      | 預け入れバンドル内の出力トランザクションの`signatureMessageFragment`フィールドの内容（単位：トライト） | NULL |
| `occured_at`   | 残高が`YYYY-MM-DD HH:MM:SS`の形式で更新された日時。 | 現在の日時 |

## Withdrawal

このテーブルには、ユーザーの取り出しリクエストに関する情報が含まれています。
<!-- This table contains information about the users' withdrawal requests. -->

| **フィールド** | **説明** | **デフォルト値** |
| :------------- | :------- | :--------------- |
| `id`             | ユーザーの取り出しイベントの固有ID | NULL |
| `uuid`           | 普遍的に一意な識別子。この文字列により、ユーザーは取り出しリクエストを識別してキャンセルすることができます。 | NULL |
| `user_id`        | `user_account`テーブル内のユーザーのアカウントのID | NULL |
| `amount`         | 取り出しの合計量 | NULL |
| `payout_address` | トークンの送信先の出力アドレス | NULL |
| `tag`            | 出力トランザクションの[`tag`フィールド](root://iota-basics/0.1/references/structure-of-a-transaction.md)の値 | NULL |
| `sweep`          | 取り出しを実行したスイープのID。このIDは`sweep`テーブルにあります。 | NULL |
| `requested_at`   | ユーザーが`YYYY-MM-DD HH:MM:SS`の形式で取り出しをリクエストした日時。 | 現在の日時 |
| `cancelled_at`   | `YYYY-MM-DD HH:MM:SS`の形式で取り出しがキャンセルされた日時。 | NULL |
