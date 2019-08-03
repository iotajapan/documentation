# スウィープ
<!-- # Sweeps -->

**スウィープは、ユーザーの取り出し操作を実行し、IOTAトークンをユーザーの預け入れアドレスからハブ所有者のアドレスの1つに転送するバンドルです。スウィープは、攻撃者が使用済みアドレスからトークンを盗む可能性を減らすためのオプションの安全機能です。**
<!-- **A sweep is a bundle that actions users' withdrawals and transfers IOTA tokens from users' deposit addresses to one of the Hub owner's addresses. Sweeps are an optional safety feature that reduces the likelihood of an attacker stealing tokens from a spent address.** -->

IOTAは、Winternitzワンタイム署名方式を使用して署名を作成します。その結果、各署名は秘密鍵の約半分を公開してしまいます。秘密鍵でバンドルに一度署名することは安全です。同じ秘密鍵を使用して別のバンドルに署名すると、攻撃者は秘密鍵に総当たり攻撃を行い、そのアドレスからIOTAトークンを盗むことができます。したがって、ユーザーがアドレスからIOTAトークンを取り出すと、そのアドレスは「使用済み」と見なされ、そのアドレスからは2度とIOTAトークンを取り出すことはできません。
<!-- IOTA uses the Winternitz one-time signature scheme to create signatures. As a result, each signature exposes around half of the private key. Signing a bundle once with the a private key is safe. Signing a different bundle with the same private key may allow attackers to brute force the private key and steal IOTA tokens from the address. So, when a user withdraws from an address, that address is considered 'used' and must never be withdrawn from again. -->

ユーザーがスウィープに関与する使用済みアドレスにIOTAトークンを入れると、そのアドレスのトークンが盗まれる危険があります。
<!-- If a user deposits IOTA tokens into a spent address such as one involved in a sweep, the tokens in that address are at risk of being stolen. -->

ハブは、IOTAトークンを定期的にユーザーの預け入れアドレスからハブの所有者のアドレスに転送することで、このリスクを軽減します。
<!-- Hub reduces this risk by transferring IOTA tokens from users' deposit addresses to a Hub owner's address at regular intervals. -->

:::danger:重要
使用済みアドレスを保護するためにスウィープを頼るべきではありません。ハブがスウィープを実行する前に、攻撃者が資金を盗むことができます。

したがって、使用済みアドレスにトークンを預け入れしないようにユーザーに通知することが重要です。
:::
<!-- :::danger:Important -->
<!-- You shouldn't rely on sweeps to protect spent addresses. An attacker could steal the funds before Hub can do a sweep. -->
<!--  -->
<!-- So, it's important that you inform users never to deposit tokens into a spent address. -->
<!-- ::: -->

:::info:
[使用済みアドレスの詳細と、使用済みアドレスからは2度とIOTAトークンを取り出してはいけない理由](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse)を知る。
:::
<!-- :::info: -->
<!-- [Discover the details about spent addresses and why you must never withdraw from an address more than once](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse). -->
<!-- ::: -->

スウィープを実行するために、Hubは[`--monitorInterval`フラグと`--sweepInterval`フラグ](../references/command-line-flags.md#monitorInterval)で定義されている定期的な間隔で以下を実行します。
<!-- To do a sweep, Hub does the following at regular intervals that are defined by the [`--monitorInterval` and `--sweepInterval`](../references/command-line-flags.md#monitorInterval) flags: -->

- ゼロではない残高があるすべての預け入れアドレスを探す。
<!-- - Find all deposit address that have a non-zero balance -->

- 上記の預け入れアドレスがペンディング中のバンドルに含まれているかどうかを確認する。ペンディング中のバンドルに含まれている預け入れアドレスはスウィープに含まれません。
<!-- - Check whether those deposit addresses are in any pending bundles. Any deposit addresses that are in pending bundles aren't included in the sweep. -->

- ペンディング中のユーザーの取り出しリクエストを確認する。
<!-- - Check for pending user withdrawal requests -->

- ハブ所有者の新しいアドレスを作成する。
<!-- - Create a new address for the Hub owner -->

- 取り出しリクエストを処理し、残りの残高をハブ所有者のアドレスに送信するバンドルを作成します。1つのバンドル内で実行できる取り出しと預け入れの数は[`--sweep_max_deposit`フラグと`--sweep_max_withdrawal`](../references/command-line-flags.md#sweepLimits)フラグによって制限されます。
<!-- - Create a bundle that actions withdrawal requests and sends the remaining balance to the Hub owner's addresses. The number of deposits and withdrawals that can be actioned in a single bundle is limited by the [`--sweep_max_deposit` and `--sweep_max_withdrawal`](../references/command-line-flags.md#sweepLimits) flags. -->

- スウィープの末尾トランザクションの包含状態を確認して、それが確定しているかどうかを確認する。スウィープのトランザクションが確定されるまで、ハブは末尾トランザクションの[再添付や促進](root://iota-basics/0.1/concepts/reattach-rebroadcast-promote.md)を行います。
<!-- - Check the inclusion state of the tail transaction in the sweep to determine if it's been confirmed. Hub will [reattach and promote](root://iota-basics/0.1/concepts/reattach-rebroadcast-promote.md) the tail transaction until the transactions in the sweep are confirmed. -->

- スウィープのトランザクションが確定されるとデータベーステーブル内のユーザの残高を更新する。
<!-- - Update the users' balances in the database tables when the transactions in the sweep are confirmed -->

:::info:包含状態についてもっと知りたいですか？
[トランザクションが確認されたかどうかを確認する方法](root://iota-basics/0.1/how-to-guides/check-transaction-confirmation.md)をご覧ください。
:::
<!-- :::info:Want to learn more about inclusion states? -->
<!-- Find out how to [check if a transaction is confirmed](root://iota-basics/0.1/how-to-guides/check-transaction-confirmation.md). -->
<!-- ::: -->
