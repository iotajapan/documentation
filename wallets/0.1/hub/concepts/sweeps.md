# スウィープ
<!-- # Sweeps -->

**スイープは、ユーザーの取り出しと預け入れの残高のバンドルです。ユーザーがIOTAトークンをハブアドレスに預け入れるか、ハブアドレスから取り出すと、ハブはそれらの転送を1つのスイープに結合します。取り出しの合計が預け入れの合計よりも少ない場合、ハブは残りの残高をハブ所有者に属する新しいアドレスに転送します。預け入れの合計が取り出しの合計よりも少ない場合、ハブはハブ所有者のアドレスのトークンを使用して、残りの取り出しの残高を満たします。**
<!-- **A sweep is a bundle that balances users' withdrawals and deposits. When users deposit IOTA tokens into a Hub address or when users withdraw them from one, Hub combines those transfers into a sweep. If the total amount of withdrawals is less than the total amount of deposits, then Hub transfers the remaining balance to a new address that belongs to the Hub owner. If the total amount of deposits is less than the total amount of withdrawals, Hub uses the tokens in the Hub owner's addresses to fulfill the remaining withdrawal balance.** -->

スイープは、ハブ所有者に属する安全なアドレスにユーザーの預け入れが転送されることを保証するオプションの安全機能です。
<!-- Sweeps are an optional safety feature that assure that deposited tokens are transferred to a secure address that belongs to the Hub owner. -->

## スイープの理由
<!-- ## The reason for sweeps -->

IOTAはWinternitzワンタイム署名スキームを使用して署名を作成します。その結果、各署名は秘密鍵の約半分を公開してしまいます。秘密鍵を使用してバンドルに1回署名することは安全です。同じ秘密鍵で異なるバンドルに署名すると、攻撃者が秘密鍵を総当たり攻撃し、アドレスからIOTAトークンを盗む可能性があります。そのため、ユーザーがアドレスから取り出すと、そのアドレスは`使用済み`とみなされ、2度と取り出してはいけません。
<!-- IOTA uses the Winternitz one-time signature scheme to create signatures. As a result, each signature exposes around half of the private key. Signing a bundle once with the a private key is safe. Signing a different bundle with the same private key may allow attackers to brute force the private key and steal IOTA tokens from the address. So, when a user withdraws from an address, that address is considered 'spent' and must never be withdrawn from again. -->

ハブは、定期的にユーザーの預け入れアドレスからハブ所有者のアドレスにIOTAトークンを転送することにより、ユーザーが同じアドレスから2回以上取り出すリスクを減らします。
<!-- Hub reduces the risk of a user withdrawing from the same address more than once by transferring IOTA tokens from users' deposit addresses to a Hub owner's address at regular intervals. -->

このように、取り出しはハブによって管理され、ユーザーが同じアドレスから2回以上取り出しを行えないようにします。
<!-- This way, any withdrawals are managed by Hub to make sure that the user does not -->

:::danger:
交換所のフロントエンドで、ユーザーの預け入れアドレスが1回限りのものであることを明確にする必要があります。
:::
<!-- :::danger: -->
<!-- You must make it clear on the exchange frontend that users' deposit addresses are for one-time use. -->
<!-- ::: -->

:::info:
[使用済みアドレスの詳細と、使用済みアドレスからは2度とIOTAトークンを取り出してはいけない理由](root://dev-essentials/0.1/concepts/addresses-and-signatures.md#address-reuse)を知る。
:::
<!-- :::info: -->
<!-- [Discover the details about spent addresses and why you must never withdraw from an address more than once](root://dev-essentials/0.1/concepts/addresses-and-signatures.md#address-reuse). -->
<!-- ::: -->

## スイープの仕組み
<!-- ## How sweeps work -->

スイープを行うために、ハブは定期的な間隔で[`--monitorInterval`および`--sweepInterval`](../references/command-line-flags.md#monitorInterval)フラグで定義された次のことを行います。
<!-- To do a sweep, Hub does the following at regular intervals that are defined by the [`--monitorInterval` and `--sweepInterval`](../references/command-line-flags.md#monitorInterval) flags: -->

1. 残高がゼロではないすべての預け入れアドレスを検索します。
  <!-- 1. Find all deposit address that have a non-zero balance -->

2. これらの預け入れアドレスがペンディング中のスイープにあるかどうかを確認します。ペンディング中のスイープにある預け入れアドレスは、新しいスイープには含まれません。
  <!-- 2. Check whether those deposit addresses are in any pending sweeps. Any deposit addresses that are in pending sweeps aren't included in a new sweep. -->

3. ペンディング中のユーザーの取り出しリクエストを確認します。
  <!-- 3. Check for pending user withdrawal requests -->

4. ハブ所有者の新しいアドレスを作成します。
  <!-- 4. Create a new address for the Hub owner -->

5. すべての預け入れIOTAトークンを選択した取り出しアドレスに転送するスイープを作成します。1回のスイープで発行できる預け入れ/取り出しの数は、[`--sweep_max_deposit`および`--sweep_max_withdrawal`](../references/command-line-flags.md#sweepLimits)フラグによって制限されます。
  <!-- 5. Create a sweep that transfers any deposited IOTA tokens to the chosen withdrawal addresses. The number of deposits and withdrawals that can be issued in a single sweep is limited by the [`--sweep_max_deposit` and `--sweep_max_withdrawal`](../references/command-line-flags.md#sweepLimits) flags. -->

6. スイープ内の末尾トランザクションの包含状態を確認して、確定されたかどうかを判断します。スイープ内のトランザクションが確定されるまで、ハブは末尾トランザクションを[再添付と促進](root://dev-essentials/0.1/concepts/reattach-rebroadcast-promote.md)します。
  <!-- 6. Check the inclusion state of the tail transaction in the sweep to determine if it's been confirmed. Hub [reattaches and promotes](root://dev-essentials/0.1/concepts/reattach-rebroadcast-promote.md) the tail transaction until the transactions in the sweep are confirmed. -->

7. スイープのトランザクションが確定されたら、データベーステーブルのユーザーの残高を更新します。
  <!-- 7. Update the users' balances in the database tables when the transactions in the sweep are confirmed -->

    :::info:包含状態について詳しく知る
    [トランザクションが確定されたかどうかを確認する](root://dev-essentials/0.1/how-to-guides/check-transaction-confirmation.md)方法を参照します。
    :::
    <!-- :::info:Want to learn more about inclusion states? -->
    <!-- Find out how to [check if a transaction is confirmed](root://dev-essentials/0.1/how-to-guides/check-transaction-confirmation.md). -->
    <!-- ::: -->
