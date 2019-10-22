# ハブを暗号通貨交換所に統合する
<!-- # Integrate Hub into a cryptocurrency exchange -->

**ユーザーの残高をどのように管理するかに応じて、ハブをさまざまな方法で交換所に統合できます。**
<!-- **You can integrate Hub into an exchange in many ways, depending on how you want to manage your users' balances.** -->

デフォルトでは、ハブはすべてのユーザーの残高の記録をデータベースに保持し、ユーザーがハブアカウントを介して相互にトレードできるようにします。
<!-- By default, Hub keeps a record of all user balances in a database, and allows users to trade with each other through their Hub accounts. -->

このガイドでは、2つの統合オプションについて説明します。
<!-- In this guide, we discuss two integration options: -->

- **オプション1. ハブでユーザーの残高を管理する（デフォルト）：** トレードとユーザー残高を管理する最も簡単な方法
<!-- - **Option 1: Manage user balances in Hub (default):** Easiest way to manage trades and user balances -->
- **オプション2. ハブ外でユーザーの残高を管理する：** すべてのユーザーのIOTAトークンをハブの外部に保存する最も簡単な方法
<!-- - **Option 2. Manage user balances outside of Hub:** Easiest way to store all users' IOTA tokens outside of Hub -->

## 統合オプション1. ハブでユーザーの残高を管理する
<!-- ## Integration option 1. Manage user balances in Hub -->

ハブは、それぞれが残高を追跡しているユーザーアカウントをサポートします。このようにして、ユーザーは自分が所有するIOTAトークンと同数だけIOTAトークンをトレードできます。
<!-- Hub supports user accounts that each have a tracked balance. This way, users can trade and withdraw only as many tokens as they own. -->

トレードを実行するには、`processTransfers`または `userWithdraw`API呼び出しを使用できます。
<!-- To action a trade, you can use the `processTransfers` or the `userWithdraw` API calls. -->

:::info:
`processTransfers`API呼び出しを使用する場合、タングル上ではIOTAトークンは転送されません。代わりに、ユーザーの残高はデータベース上で更新されます。これは、ユーザーが取り出しをリクエストできるIOTAトークンの量に影響します。
:::
<!-- :::info: -->
<!-- When you use the `processTransfers` API call, no tokens are transferred on the Tangle. Instead, the users' balances are updated in the database, which affects how many tokens users can request to withdraw. -->
<!-- ::: -->

:::info:
`userWithdraw`API呼び出しを使用すると、IOTAトークンはタングル上で転送され、データベース上のユーザーの残高も更新されます。
:::
<!-- :::info: -->
<!-- When you use the `userWithdraw` API call, the tokens are transferred on the Tangle and the users' balances are updated in the database. -->
<!-- ::: -->

`userWithdraw`API呼び出しを使用すると、ハブ所有者のアドレスではなく、ユーザーのアドレスからのみ取り出すことができるため、ハブの外側（コールドウォレット）にトークンを保存することは困難です。
<!-- Because the `userWithdraw` API call allows you to withdraw only from a user address and not the Hub owner's addresses, it's difficult to store tokens outside of Hub (in a cold wallet). -->

| **ユーザーのアクション** | **交換所のアクション** | **ハブのAPI呼び出し** |
| :----------------------- | :--------------------- | :--------------------- |
| 交換所にIOTAアカウントでサインアップします。 | ハブに新しいユーザーを作成します。 | `CreateUser` |
| IOTAトークンを預け入れる預け入れアドレスをリクエストします。 | ユーザーの新しい預け入れアドレスを作成します。 | `getDepositAddress` |
| IOTAトークンをアドレスに預け入れます。 | 預け入れがハブ所有者の新しいアドレスの1つにスウィープされた後、交換所のフロントエンドでユーザーに通知します。 | `balanceSubscription` |
| ハブ外のアドレスへの取り出しをリクエストします。 | 取り出しを実行します。 | `userWithdraw` |
| 別のユーザーからトークンを購入します。 | ハブデータベースの残高を更新することにより、2人のユーザー間のトレードを処理します。 | `processTransfers` |

<!-- |**User action**|**Exchange action**|**Hub API call**| -->
<!-- |:----------|:--------------|:-----------| -->
<!-- |:----------|:--------------|:-----------| -->
<!-- |Signs up for an IOTA account on the exchange|Creates a new user in Hub| `CreateUser`| -->
<!-- |Requests a deposit address in which to deposit IOTA tokens|Creates a new deposit address for the user|`getDepositAddress`| -->
<!-- |Deposits IOTA tokens into the address|Notifies the user on the exchange frontend after the deposit has been swept to one of the Hub owner's new addresses| `balanceSubscription`| -->
<!-- |Requests a withdrawal to an address outside of Hub|Actions the withdrawal|`userWithdraw`| -->
<!-- |Buys tokens from another user|Actions the trade between the two users by updating their balances in the Hub database|`processTransfers`| -->

### IOTAトークンをハブ外に保存する
<!-- ### Store IOTA tokens outside of Hub -->

IOTAトークンをハブ外に保存するには、ハブ所有者のアドレスの1つから新しいコールドウォレットアドレスにIOTAトークンを転送する必要があります。
<!-- To store IOTA tokens outside of Hub, you need to transfer them from one of the Hub owner's addresses to the new cold wallet address. -->

:::warning:警告！
ハブ外にIOTAトークンを転送すると、取り出しリクエストを処理できなくなるリスクがあります。
:::
<!-- :::warning:Warning -->
<!-- When you transfer tokens outside of Hub, you're at risk of not being able to process withdrawal requests. -->
<!-- ::: -->

1. ハブを止めます。
  <!-- 1. Stop Hub -->

2. コールドストレージとして使用する`hub_address`テーブル行のハブアドレスに対して、[`is_cold_storage`](../references/database-tables.md#hub_address)フィールドを1に設定します。
  <!-- 2. Set the [`is_cold_storage`](../references/database-tables.md#hub_address) field to 1 for any Hub addresses in the `hub_address` table row that you want to use as cold storage. -->

3. 選択したハブアドレスのシードUUIDを保存し、そのシードUUIDをハブから削除します。
  <!-- 3. Save the seed UUIDs of your chosen Hub addresses, then delete it from Hub -->

4. シードUUID（および設定されている場合はソルト）を使用して、ハブ外の選択したハブアドレスのシードを再作成します。
  <!-- 4. Use the seed UUID (and the salt if you set one) to recreate the seed for your chosen Hub addresses outside of Hub -->

5. シードを使用して、IOTAトークンをハブアドレスからハブ外の新しいアドレスに転送します。
  <!-- 5. Use the seed to transfer the IOTA tokens from the Hub addresses to the new address outside of Hub -->

6. ハブを再起動します。
  <!-- 6. Restart Hub -->

:::info:
ハブ外にトークンを保存することに十分な関心がある場合、このタスクを簡単にする特別なAPI呼び出しを作成できます。[Discord](https://discord.iota.org)でご連絡ください。
:::
<!-- :::info: -->
<!-- If enough interest exists for storing tokens outside of Hub, we can create a specialized API call that makes this task easier. Please reach out to us on [Discord](https://discord.iota.org). -->
<!-- ::: -->

### IOTAトークンをハブに送り返す
<!-- ### Send IOTA tokens back to Hub -->

取り出しリクエストを処理するには、IOTAトークンをハブに戻し、スウィープで使用できるようにする必要があります。
<!-- To process withdrawal requests, you may need to transfer IOTA tokens back into Hub so that it can use them in a sweep. -->

1. ハブを止めます。
  <!-- 1. Stop Hub -->

2. トークンを送信したい`hub_address`テーブル行のハブアドレスの`is_cold_storage`フィールドを0に設定します。
  <!-- 2. Set the `is_cold_storage` field to 0 for any Hub addresses in the `hub_address` table row that you want to send tokens to -->

3. ハブデータベース内のアドレスのシードUUIDを復元します。
  <!-- 3. Restore the seed UUID for the addresses in the Hub database -->

4. ハブを再起動します。
  <!-- 4. Restart Hub -->

## 統合オプション2. ハブ外のユーザー残高を管理する
<!-- ## Integration option 2. Manage user balances outside of Hub -->

ハブの外部でユーザーの残高を管理することにより、ハブ外でトークンを簡単に保存できます。ただし、独自のシステムでユーザーの残高を追跡する必要があります。
<!-- By managing user balances outside of Hub, it's easier to store tokens outside of Hub. But, you need to keep track of users' balances in your own system. -->

### 前提条件
<!-- ### Prerequisites -->

1. 新しいシードを使用してコールドウォレットを作成します。
<!-- 1. Create a cold wallet, using a new seed -->
2. ホットウォレットとして使用する新しいユーザーを作成します。
<!-- 2. Create a new user to use as a hot wallet -->

| **ユーザーのアクション** | **交換所のアクション** | **ハブのAPI呼び出し** |
| :----------------------- | :--------------------- | :--------------------- |
| 交換所にIOTAアカウントでサインアップします。 | ハブに新しいユーザーを作成します。 | `CreateUser` |
| IOTAトークンを預け入れる預け入れアドレスをリクエストします。 | ユーザーの新しい預け入れアドレスを作成します。 | `GetDepositAddress` |
| IOTAトークンをアドレスに預け入れます。 | 預け入れがハブ所有者の新しいアドレスの1つにスウィープされた後、交換所のフロントエンドでユーザーに通知します。次に、交換所のバックエンドでユーザーの残高を更新します。| `BalanceSubscription` |
| | ホットウォレットがすべてのIOTAトークンを所有するように、ハブデータベースを更新します。 | `ProcessTransfers` |
| ハブ外のアドレスへの取り出しをリクエストします。 | ホットウォレットからユーザーが選択したアドレスへの取り出しを発行します。 | `UserWithdraw` |
| 別のユーザーからトークンを購入します。 | ユーザーが交換所で暗号資産を売買する場合、ホットウォレットがすべてのトークンを所有しているため、ハブには何も記録されません。その結果、交換所はハブ外のすべてのアカウンティングを処理する必要があります。 | `ProcessTransfers` |

<!-- |**User action**|**Exchange action**|**Hub API call**| -->
<!-- |:----------|:--------------|:-----------| -->
<!-- |:----------|:--------------|:-----------| -->
<!-- |Signs up for an IOTA account on the exchange|Creates a new user in Hub| `CreateUser`| -->
<!-- |Requests a deposit address in which to deposit IOTA tokens|Creates a new deposit address for the user|`GetDepositAddress`| -->
<!-- |Deposits IOTA tokens into the address|Notifies the user on the exchange frontend after the deposit has been swept to one of the Hub owner's new addresses. Then, updates the user's balance on the exchange backend.|`BalanceSubscription`| -->
<!-- | |Updates the Hub database so that the hot wallet owns all the IOTA tokens|`ProcessTransfers`| -->
<!-- |Requests a withdrawal to an address outside of Hub|Actions the withdrawal from the hot wallet to the user's chosen address|`UserWithdraw`| -->
<!-- |Buys tokens from another user|When users buy and sell cryptocurrencies on the exchange, nothing is recorded in Hub because as far as Hub is aware, the hot wallet owns all the tokens. As a result, the exchange must handle all the accounting outside of Hub.|None| -->

### IOTAトークンをハブ外に保存する
<!-- ### Store IOTA tokens outside of Hub -->

IOTAトークンをハブ外に保存するには、IOTAトークンをホットウォレットアドレスからコールドウォレットアドレスに転送する必要があります。
<!-- To store IOTA tokens outside of Hub, you need to transfer them from the hot wallet addresses to a cold wallet address. -->

:::warning:警告！
ハブアドレスの外部にIOTAトークンを転送すると、取り出しリクエストを処理できなくなるリスクがあります。
:::
<!-- :::warning:Warning -->
<!-- When you transfer tokens outside of Hub addresses, you're at risk of not being able to process withdrawal requests. -->
<!-- ::: -->

1. `userWithdraw`API呼び出しを使用して、ホットウォレットから新しいアドレスへの取り出しを実行します。
  <!-- 1. Action a withdrawal from the hot wallet to a new address, using the `userWithdraw` API call -->

  :::warning:警告
  このシナリオでは、ハブは新しいアドレスが既に使用済みであるかどうかを確認できません。
  :::
  <!-- :::warning:Warning -->
  <!-- In this scenario, Hub cannot check whether the new address was already withdrawn from. -->
  <!-- ::: -->

### IOTAトークンをハブに送り返す
<!-- ### Send IOTA tokens back to Hub -->

取り出しリクエストを処理するには、IOTAトークンをハブに戻し、スウィープで使用できるようにする必要があります。
<!-- To process withdrawal requests, you may need to transfer IOTA tokens back into Hub so that it can use them in a sweep. -->

1. `getDepositAddress`API呼び出しを使用して、ホットウォレットの新しい預け入れアドレスを作成します。
  <!-- 1. Create a new deposit address for the hot wallet, using the `getDepositAddress` API call -->

  :::warning:警告
  同じアドレスから2回以上取り出しを行なってはいけません。
  :::
   <!-- :::warning:Warning -->
   <!-- Addresses must never be withdrawn from more than once. -->
   <!-- ::: -->

2. 外部アドレスからこの預け入れアドレスにIOTAトークンを送信します。
  <!-- 2. Send tokens from the external address to this deposit address -->

ハブは預け入れを受け取り、スウィープの一環として預け入れをハブ所有者のアドレスに転送します。預け入れの合計残高が十分でない場合、ハブ所有者のアドレスにあるIOTAトークンを使用して取り出しリクエストを処理できます。
<!-- Hub receives the deposit and transfers them to a Hub owner's address as part of a sweep. The tokens in the Hub owner's addresses can be used to fulfill withdrawal requests if the total balance of deposits is not enough. -->

:::info:
[スウィープの仕組み](../concepts/sweeps.md)に関する詳細を参照します。
:::
