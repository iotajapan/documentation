# ハブを暗号資産取引所に統合する
<!-- # Integrate Hub into a crytocurrency exchange -->

**ユーザーの残高の管理方法に応じて、ハブをさまざまな方法で取引所に統合できます。**
<!-- **Hub can be integrated into an exchange in many ways, depending on how you want to manage your users' balances.** -->

このガイドでは、2つのシナリオについて説明します。
<!-- In this guide, we discuss two possible scenarios: -->

- **シナリオA：** IOTAトークンが外部のオンラインウォレット（別名：ホットウォレット）に手動で転送される複数のユーザーアカウント。
<!-- - **Scenario A:** Multiple user accounts where IOTA tokens are manually transferred to an external online wallet (also known as a hot wallet). -->
- **シナリオB：** 個々の残高を持つ複数のユーザーアカウント。
<!-- - **Scenario B:** Multiple user accounts with individual balances. -->

## シナリオA
<!-- ## Scenario A -->

あなたはハブの内部会計設定に頼りたくないかもしれません。その理由の1つは、単にネット取引を転送するのが面倒すぎることです。したがって、預け入れが成功すると、すべてのトークンが中央集権のホットウォレットに転送される可能性があります。これにより、このホットウォレットからオフラインウォレット（別名：コールドウォレット）アドレスに取り出し、必要に応じてアカウントに再度預け入れることで、トークンのコールドストレージを簡単に処理できます。その後、このホットウォレットからユーザーの取り出しも処理されます。
<!-- You may not want to rely on Hub's internal accounting setup. One such reason might simply be that forwarding netted trades is too cumbersome. -->
<!-- Therefore, after a successful deposit, all tokens might be transferred to a central hot wallet. This will also allow you to deal with cold storage of tokens easily by withdrawing from this hot wallet to an offline wallet (also known as a cold wallet) address and depositing back into the account as necessary. User withdrawals are then also processed from this hot wallet. -->

:::warning:警告
このシナリオでは、ハブはデータベース内のユーザーの残高を追跡しません。
:::
<!-- :::warning:Warning -->
<!-- In this scenario, Hub doesn't keep track of users' balances in the database. -->
<!-- ::: -->

### 初期設定
<!-- ### Initial setup -->

取引所がホットウォレットを作成します。
<!-- Exchange creates a hot wallet -->

#### ユーザー登録
<!-- #### User signup -->

取引所は新しいハブユーザーを作成し、ユーザーIDを渡します。
<!-- Exchange creates a new Hub user, passing in a userid. -->

### ユーザー預け入れ
<!-- ### User deposit -->

1. ユーザーが預け入れアドレスを要求します（`GetDepositAddress`）。
<!-- 1. User requests deposit address (`GetDepositAddress`) -->
2. ユーザーがトークンを預け入れます。
<!-- 2. User deposits tokens -->
3. 取引所は、`BalanceSubscription`エンドポイントを使用して、新しい更新をポーリングします。
<!-- 3. Exchange polls for new updates, using the `BalanceSubscription` endpoint -->
4. 預け入れ（＆スウィープ）が成功すると、取引所は`ProcessTransfers`エンドポイントを呼び出し、すべての新規ユーザーの預け入れをホットウォレットに転送します。
<!-- 4. Upon successful deposit (& sweep), exchange calls the `ProcessTransfers` endpoint, transferring all new user deposits to the hot wallet -->

### ユーザー取り出し
<!-- ### User withdrawal -->

1. ユーザーが取引所のフロントエンドで取り出しを要求します。
<!-- 1. User requests withdrawal on the exchange's frontend -->
2. 取引所はホットアドレスからユーザーアドレスへの取り出しを発行します（`UserWithdraw`）。
<!-- 2. Exchange issues withdrawal from hot to user address (`UserWithdraw`) -->
3. ハブはこの取り出しをスウィープの一部として処理します。
<!-- 3. Hub processes this withdrawal as part of a sweep -->

### コールドウォレットにつぎ足す
<!-- ### Cold wallet topup -->

取引所がホットからコールドウォレットアドレスへの取り出しを発行します（`UserWithdraw`）。
<!-- Exchange issues withdrawal from hot to cold wallet address that wasn't withdrawn from (`UserWithdraw`) -->

  :::warning:警告
  このシナリオでは、ハブはアドレスからすでに取り出されているかどうかを確認しません。
  :::
  <!-- :::warning:Warning -->
  <!-- In this scenario, Hub doesn't check whether an address was already withdrawn from. -->
  <!-- ::: -->

### ホットウォレットにつぎ足す
<!-- ### Hot wallet topup -->

1. 取引所がホットユーザーの取り出しアドレスを要求します（`GetDepositAddress`）。
<!-- 1. Exchange requests deposit address for hot user (`GetDepositAddress`) -->

  :::warning:警告
  アドレスから二度以上取り出してはいけません。
  :::
  <!-- :::warning:Warning -->
  <!-- Addresses must never be withdrawn from more than once. -->
  <!-- ::: -->

2. 取引所はコールドウォレットからこの取り出しアドレスにトークンを送信します。
<!-- 2. Exchange sends tokens from cold wallet to this deposit address -->
3. ハブは預け入れを受け取り、スウィープの一部として内部アドレスに移動させます。
<!-- 3. Hub receives deposit and moves to internal address as part of a sweep -->

### ユーザーBは取引所でユーザーAからIOTAトークンを購入する
<!-- ### User B buys IOTA tokens from user A on the exchange -->

ハブでは何も行われず、すべての会計は取引所側で内部的に行われます。
<!-- No action happens on Hub, all accounting is done internally on the exchange side. -->

### 長所と短所の議論
<!-- ### Discussion of the pros and cons -->

- (+) コールド/ホットトークンの管理が簡単です。
<!-- - (+) Easy management of cold / hot tokens -->
- (+) 取引所側で統合する方が簡単かもしれません。
<!-- - (+) Likely to be easier to integrate on exchange side. -->
- (-) 残高はハブ内でユーザーごとのレベルで追跡されないため、セキュリティ保証が低下します。
<!-- - (-) Reduced security guarantees because balances are not tracked on a per-user level inside Hub. -->
- (-) 取引所は、ハブとは無関係にIOTAトークンの合計量を追跡する必要があります。
<!-- - (-) Exchange needs to keep track of total amount of IOTA tokens independently of Hub. -->

## Scenario B

As Hub supports independent user accounts with individual balances, it arguably makes sense to rely on this as an added security measure. Balances are tracked per user, and therefore a user can only use as many tokens as the user is tracking for them. However, this approach currently complicates the cold/hot wallet flow. 

### Cold/hot wallets

As opposed to Scenario A, it is not so easy to move tokens from multiple users to a cold wallet. However, it is possible to have Hub ignore some of the Hub owner's addresses. For this, the [`is_cold_storage`](../references/database-tables.md#hub_address) field in the `hub_address` table row needs to be set to 1. This will cause the `SweepService` to ignore this address for any sweeps.

For increased security, the `seed_uuid` of this hub address should also be deleted from the database and stored externally.

At the moment, the only way that this can be achieved is through manual database updates. It is recommended to stop Hub while marking such hub addresses as cold storage. There is no negative effect on operations if Hub is stopped.

Using the `salt` that's passed at startup and the `seed_uuid` it is always possible to recompute Hub address's seed outside of Hub.

Should sufficient interest exist for this integration scenario, it is possible to provide specialized endpoints for this.

### Initial setup

None. Start Hub.

### User sign up

Exchange creates new Hub user, passing in a per-user userid.

### User deposit

1. User requests deposit address (`GetDepositAddress`)
2. User deposits tokens
3. Hub moves the new deposit to an internal address
3. Exchange polls for new updates via `BalanceSubscription` and notifies user on their frontend once the deposit has been registered or once it has been swept successfully

### User withdrawal

1. User requests withdrawal on exchange frontend.
2. Exchange issues withdrawal from user's Hub account to payout address (`UserWithdraw`)
3. Hub processes this withdrawal as part of a sweep.

### Cold wallet topup

1. Exchange stops Hub
2. Exchange decides which Hub addresses it wants to mark as cold storage
3. Exchange sets [`is_cold_storage`](../references/database-tables.md#hub_address) to `1` on these `hub_address` rows and stores the `seed_uuid` externally.
   There are multiple scenarios for achieving this:
   - Use a vault service
   - Use paper backups
   - Some RDBMS support partitioning the table into multiple storage locations.
4. Exchange restarts Hub
   
### Hot wallet topup

1. Exchange decides which cold storage addresses it wants to reactivate
2. Exchange stops Hub
3. Exchange sets [`is_cold_storage`](../references/database-tables.md#hub_address) to `0` on these `hub_address` rows and restores the `seed_uuid` values
4. Exchange restarts hub

### User B buys tokens from user A on the exchange

1. If user B doesn't already exist, User B is created on Hub (`CreateUser`)
2. As part of next batch, exchange issues a transfer between the two users (`ProcessTransfers`)

### Discussion of the pros and cons

- (+) Balances are tracked on a per-user level and thus Hub can do a sanity check on the requests the exchange sends.
- (+) Exchange can easily do a sanity check that its backend is tracking the same `(user, balance)` values as Hub.
- (-) More complicated cold wallet setup
