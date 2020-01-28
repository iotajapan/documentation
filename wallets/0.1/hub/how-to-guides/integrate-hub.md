# 統合オプション
<!-- # Integration options -->

**ユーザーの残高をどのように管理するかに応じて，ハブをさまざまな方法で交換所に統合できます．**
<!-- **You can integrate Hub into an exchange in many ways, depending on how you want to manage your users' balances.** -->

このワークフローガイドでは，2つの統合オプションについて説明します．
<!-- In this workflow guide, we discuss two integration options: -->

- **オプション1. ハブでユーザーの残高を管理する（デフォルト）：**トレードとユーザー残高を管理する最も簡単な方法
<!-- - **Option 1: Manage user balances in Hub (default):** Easiest way to manage trades and user balances -->
- **オプション2. ハブ外でユーザーの残高を管理する：**すべてのユーザーの IOTA トークンをハブの外部に保存する最も簡単な方法
<!-- - **Option 2. Manage user balances outside of Hub:** Easiest way to store all users' IOTA tokens outside of Hub -->

## 統合オプション1. ハブでユーザーの残高を管理する
<!-- ## Integration option 1. Manage user balances in Hub -->

デフォルトでは，ハブはすべてのユーザーの残高の記録をデータベースに保持し，ユーザーがハブアカウントを介して相互に取引できるようにします．このようにして，ユーザーは所有している IOTA トークンと同数の IOTA トークンのみをトレードおよび取り出しができます．
<!-- By default, Hub keeps a record of all users' balances in a database, and allows users to trade with each other through their Hub accounts. This way, users can trade and withdraw only as many tokens as they own. -->

| **ユーザーのアクション** | **交換所のアクション** | **ハブのAPI呼び出し** |
| :----------------------- | :--------------------- | :--------------------- |
| 交換所に IOTA アカウントでサインアップします． | ハブに新しいユーザーを作成します． | `CreateUser` |
| IOTA トークンをデポジットするデポジットアドレスをリクエストします． | ユーザーの新しいデポジットアドレスを作成します． | `getDepositAddress` |
| IOTAトークンをアドレスにデポジットします． | [スウィープ](../concepts/sweeps.md)にデポジットが含まれた後，取引所フロントエンドでユーザーに通知します． | `balanceSubscription` |
| ハブ外のアドレスへの取り出しをリクエストします． | 取り出しを実行します． | `userWithdraw` |
| 別のユーザーから IOTA トークンを購入します． | ハブデータベースの残高を更新することにより，2人のユーザー間のトレードを処理します． | `processTransfers` |

<!-- |**User action**|**Exchange action**|**Hub API call**| -->
<!-- |:----------|:--------------|:-----------| -->
<!-- |Signs up for an IOTA account on the exchange|Creates a new user in Hub| `CreateUser`| -->
<!-- |Requests a deposit address in which to deposit IOTA tokens|Creates a new deposit address for the user|`getDepositAddress`| -->
<!-- |Deposits IOTA tokens into the address|Notifies the user on the exchange frontend after the deposit has been included in a [sweep](../concepts/sweeps.md)| `balanceSubscription`| -->
<!-- |Requests a withdrawal to an address outside of Hub|Actions the withdrawal|`userWithdraw`| -->
<!-- |Buys tokens from another user|Actions the trade between the two users by updating their balances in the Hub database|`processTransfers`| -->

### ハブの外部でIOTAトークンを保存する
<!-- ### Store IOTA tokens outside of Hub -->

ハブの所有者のアドレスがすべてのユーザーの取り出しを処理するのに十分な IOTA トークンを常に持っていることを確認するために，`userWithdraw` API 呼び出しにより，ハブ所有者のアドレスではなく，ユーザーのアドレスからのみ取り出すことができます．
<!-- To make sure that the Hub owner's addresses always have enough IOTA tokens to process all user withdrawals, the `userWithdraw` API call allows you to withdraw only from a user's address and not the Hub owner's addresses. -->

IOTA トークンをハブの外部に転送するには，ハブ所有者のアドレスをコールドストレージとしてマークして，ハブがスウィープで使用できないようにする必要があります．
<!-- To transfer IOTA tokens outside of Hub, you need to mark the Hub owner's address as cold storage so that Hub cannot use it in a sweep. -->

:::warning:警告！
ハブの外部に IOTA トークンを転送すると，取り出しリクエストを処理できなくなるリスクがあります．
:::warning:Warning
:::
<!-- :::warning:Warning -->
<!-- When you transfer tokens outside of Hub, you're at risk of not being able to process withdrawal requests. -->
<!-- ::: -->

1. ハブを止めます．
  <!-- 1. Stop Hub -->

2. コールドストレージとして使用する `hub_address` テーブル行のハブアドレスに対して，[`is_cold_storage`](../references/database-tables.md#hub_address) フィールドを1に設定します．
  <!-- 2. Set the [`is_cold_storage`](../references/database-tables.md#hub_address) field to 1 for any Hub addresses in the `hub_address` table row that you want to use as cold storage. -->

3. 選択したハブアドレスのシード UUID を保存し，そのシード UUID をハブから削除します．
  <!-- 3. Save the seed UUIDs of your chosen Hub addresses, then delete them from Hub -->

4. シード UUID（および設定されている場合はソルト）を使用して，ハブ外の選択したハブアドレスのシードを再作成します．
  <!-- 4. Use the seed UUID (and the salt if you set one) to recreate the seed for your chosen Hub addresses outside of Hub -->

5. シードを使用して，IOTA トークンをハブアドレスからハブ外の新しいアドレスに転送します．
  <!-- 5. Use the seed to transfer the IOTA tokens from the Hub addresses to the new address outside of Hub -->

6. ハブを再起動します．
  <!-- 6. Restart Hub -->

:::info:
ハブ外に IOTA トークンを保存することに十分な関心がある場合，このタスクを簡単にする特別な API 呼び出しを作成できます．[Discord](https://discord.iota.org)でご連絡ください．
:::
<!-- :::info: -->
<!-- If enough interest exists for storing tokens outside of Hub, we can create a specialized API call that makes this task easier. Please reach out to us on [Discord](https://discord.iota.org). -->
<!-- ::: -->

### IOTA トークンをハブに送り返す
<!-- ### Send IOTA tokens back to Hub -->

IOTA トークンをハブの外部に保存する場合，ユーザーの取り出しを処理できるように，ハブの外部に保存した IOTA トークンをハブに戻す必要があります．
<!-- If you store IOTA tokens outside of Hub, you may need to transfer them back into Hub to be able to process a user's withdrawal. -->

1. ハブを止めます．
  <!-- 1. Stop Hub -->

2. IOTA トークンを送信する `hub_address` テーブル行のハブアドレスの `is_cold_storage` フィールドを`0`に設定します．
  <!-- 2. Set the `is_cold_storage` field to 0 for any Hub addresses in the `hub_address` table row to which you want to send IOTA tokens -->

3. ハブデータベースにハブアドレスのシード UUID を保存します．
  <!-- 3. Save the seed UUIDs for the Hub addresses in the Hub database -->

4. ハブを再起動します．
  <!-- 4. Restart Hub -->

## 統合オプション2. ハブ外のユーザー残高を管理する
<!-- ## Integration option 2. Manage user balances outside of Hub -->

ハブの外部でユーザーの残高を管理することにより，ハブの外部で IOTA トークンを簡単に保存できます．しかし，そうすることで，あなた自身のシステムでユーザーの残高を追跡する必要があります．
<!-- By managing user balances outside of Hub, it's easier to store tokens outside of Hub. But, by doing so, you need to keep track of users' balances in your own system. -->

### 前提条件
<!-- ### Prerequisites -->

1. ハブの外部にコールドウォレットアドレスを作成します．
<!-- 1. Create a cold wallet address outside of Hub -->
2. ホットウォレットとして使用する新しいユーザーを作成します．
<!-- 2. Create a new user to use as a hot wallet -->

| **ユーザーのアクション** | **交換所のアクション** | **ハブの API 呼び出し** |
| :----------------------- | :--------------------- | :--------------------- |
| 交換所に IOTA アカウントでサインアップします． | ハブに新しいユーザーを作成します． | `CreateUser` |
| IOTA トークンをデポジットするデポジットアドレスをリクエストします． | ユーザーの新しいデポジットアドレスを作成します． | `GetDepositAddress` |
| IOTA トークンをアドレスにデポジットします． | デポジットがハブ所有者の新しいアドレスの1つにスウィープされた後，交換所のフロントエンドでユーザーに通知します．次に，交換所のバックエンドでユーザーの残高を更新します．| `BalanceSubscription` |
| | ホットウォレットがすべての IOTA トークンを所有するように，ハブデータベースを更新します． | `ProcessTransfers` |
| ハブ外のアドレスへの取り出しをリクエストします． | ホットウォレットからユーザーが選択したアドレスへの取り出しを発行します． | `UserWithdraw` |
| 別のユーザーから IOTA トークンを購入します． | ユーザーが交換所で暗号資産を売買する場合，ホットウォレットがすべての IOTA トークンを所有しているため，ハブには何も記録されません．その結果，交換所はハブ外のすべてのアカウンティングを処理する必要があります． | `ProcessTransfers` |

<!-- |**User action**|**Exchange action**|**Hub API call**| -->
<!-- |:----------|:--------------|:-----------| -->
<!-- |Signs up for an IOTA account on the exchange|Creates a new user in Hub| `CreateUser`| -->
<!-- |Requests a deposit address in which to deposit IOTA tokens|Creates a new deposit address for the user|`GetDepositAddress`| -->
<!-- |Deposits IOTA tokens into the address|Notifies the user on the exchange frontend after the deposit has been swept to one of the Hub owner's new addresses. Then, updates the user's balance on the exchange backend.|`BalanceSubscription`| -->
<!-- | |Updates the Hub database so that the hot wallet owns all the IOTA tokens|`ProcessTransfers`| -->
<!-- |Requests a withdrawal to an address outside of Hub|Actions the withdrawal from the hot wallet to the user's chosen address|`UserWithdraw`| -->
<!-- |Buys tokens from another user|When users buy and sell cryptocurrencies on the exchange, nothing is recorded in Hub because as far as Hub is aware, the hot wallet owns all the tokens. As a result, the exchange must handle all the accounting outside of Hub.|None| -->

### IOTA トークンをハブ外に保存する
<!-- ### Store IOTA tokens outside of Hub -->

IOTA トークンをハブ外に保存するには，IOTA トークンをホットウォレットアドレスからコールドウォレットアドレスに転送する必要があります．
<!-- To store IOTA tokens outside of Hub, you need to transfer them from the hot wallet addresses to a cold wallet address. -->

:::warning:警告！
ハブアドレスの外部に IOTA トークンを転送すると，取り出しリクエストを処理できなくなるリスクがあります．
:::
<!-- :::warning:Warning -->
<!-- When you transfer tokens outside of Hub addresses, you're at risk of not being able to process withdrawal requests. -->
<!-- ::: -->

1. `userWithdraw` API 呼び出しを使用して，ホットウォレットから新しいアドレスへの取り出しを実行します．
  <!-- 1. Action a withdrawal from the hot wallet to a new address, using the `userWithdraw` API call -->

  :::warning:警告！
  このシナリオでは，ハブは新しいアドレスが既に使用済みであるかどうかを確認できません．
  :::
  <!-- :::warning:Warning -->
  <!-- In this scenario, Hub cannot check whether the new address was already withdrawn from. -->
  <!-- ::: -->

### IOTA トークンをハブに送り返す
<!-- ### Send IOTA tokens back to Hub -->

IOTA トークンをハブの外部に保存する場合，ユーザーの取り出しを処理できるように，ハブの外部に保存した IOTA トークンをハブに戻す必要があります．
<!-- If you store IOTA tokens outside of Hub, you may need to transfer them back into Hub to be able to process a user's withdrawal. -->

1. `getDepositAddress` API 呼び出しを使用して，ホットウォレットの新しいデポジットアドレスを作成します．
  <!-- 1. Create a new deposit address for the hot wallet, using the `getDepositAddress` API call -->

2. 外部アドレスからこのデポジットアドレスに IOTA トークンを送信します．
<!-- 2. Send tokens from the external address to this deposit address -->

ハブはデポジットを受け取り，[スウィープ](../concepts/sweeps.md)の一部としてハブ所有者のアドレスに転送します．デポジットの合計残高が十分でない場合，ハブ所有者のアドレスにある IOTA トークンを使用して取り出しリクエストを処理できます．
<!-- Hub receives the deposit and transfers them to a Hub owner's address as part of a [sweep](../concepts/sweeps.md). The tokens in the Hub owner's addresses can be used to fulfill withdrawal requests if the total balance of deposits is not enough. -->
