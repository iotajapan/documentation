# ハブ概要
<!-- # Hub overview -->

**ハブは暗号通貨交換所のためのヘッドレスマルチユーザーウォレットです。ハブは、ユーザーのIOTAトークンの預け入れと取り出しを管理するための安全な方法を提供します。ユーザーがIOTAトークンを自分のハブアドレスの1つに預け入れると、スイープと呼ばれるプロセスでユーザーのトークンがハブ所有者のアドレスに転送されます。このようにして、ハブの所有者は、ユーザーが取り出しをリクエストするまでユーザーのトークンが安全であることを確認できます。**
<!-- **Hub is a headless multi-user wallet for cryptocurrency exchanges. Hub offers you a secure way to manage deposits and withdrawals of users' IOTA tokens. When a user deposits IOTA tokens into one of their Hub addresses, Hub transfers those tokens to the Hub owners address in a process called a sweep. This way, the Hub owner can make sure that users' tokens are secure until they request a withdrawal.** -->

ハブは、[gRPC API](../how-to-guides/get-started-with-the-api.md)を通じて以下のプロセスを自動化することにより、IOTAをそれぞれのアプリケーションに統合するのを助けます。
<!-- Hub helps you to integrate IOTA into your own applications by automating the following processes through a [gRPC API](../how-to-guides/get-started-with-the-api.md): -->

* [トランザクション監視](#トランザクション監視)
<!-- * [Transaction monitoring](#transaction-monitoring) -->
* [シード作成](#シード作成)
<!-- * [Seed creation](#seed-creation) -->
* [トークン保護](#トークン保護)
<!-- * [Token protection](#token-protection) -->

## トランザクション監視
<!-- ## Transaction monitoring -->

トランザクションがIOTAネットワークによって受け入れられる前に、トランザクションは確定されなければなりません。そのため、確定の遅れを避けるために、ハブは確定されるまでトランザクションの[再添付と促進](root://iota-basics/0.1/concepts/reattach-rebroadcast-promote.md)を行います。
<!-- Before any transaction is accepted by an IOTA network, it must be confirmed. So, to avoid delays in confirmation, Hub [reattaches and promotes](root://iota-basics/0.1/concepts/reattach-rebroadcast-promote.md) transactions until they are confirmed. -->

:::info:コンセンサスについてより詳しく知る
[タングル](root://iota-basics/0.1/concepts/the-tangle.md)を参照します。
:::
<!-- :::info:Want to learn more about consensus? -->
<!-- Read about [the Tangle](root://iota-basics/0.1/concepts/the-tangle.md). -->
<!-- ::: -->

## シード作成
<!-- ## Seed creation -->

IOTAネットワークの各クライアントには、[シード](root://getting-started/0.1/introduction/what-is-a-seed.md)と呼ばれる秘密のパスワードがあります。これは、[アドレスの作成とバンドルの署名](root://iota-basics/0.1/concepts/addresses-and-signatures.md)に使用されます。アドレスはトランザクションの送受信元のアカウントであり、署名はアドレスの所有権を証明するものです。
<!-- Each client in an IOTA network has a secret password called a [seed](root://getting-started/0.1/introduction/what-is-a-seed.md), which is used to create [addresses and to sign bundles](root://iota-basics/0.1/concepts/addresses-and-signatures.md). Addresses are the accounts from which transactions are sent and received, and signatures prove ownership of an address. -->

ハブは、[Argon2](https://www.argon2.com/)ハッシュ関数を使用して、各ユーザーの預け入れアドレスの新しいシードを作成します。次の値はシードを作成するために使用されます。
<!-- Hub creates a new seed for each user's deposit address, using the [Argon2](https://www.argon2.com/) hashing function. The following values are used to create a seed: -->

* シードUUID：[`seeduuid`フィールド](../references/database-tables.md#user_account)に格納されている、ランダムに生成された普遍的に一意な識別子
<!-- * Seed UUID: A randomly generated universally unique identifier that is stored in a [`seeduuid` field](../references/database-tables.md#user_account) -->
* ソルト：オプションの[`salt`フラグ](../references/command-line-flags.md)で定義される文字列
<!-- * Salt: Characters that you can define in an optional [`salt` flag](../references/command-line-flags.md) -->

:::info:
データベースには、ユーザーが所有しているIOTAトークン量の記録が含まれています。IOTAトークンはユーザーのアドレスには保存されません。代わりに、ユーザーのIOTAトークンは[スイープ](../concepts/sweeps.md)中にハブ所有者のアドレスに転送されます。ユーザーが後で[`userWithdrawRequest`コマンド](../references/api-reference.md#hub.rpc.UserWithdrawRequest)をトリガーした時に、ハブはユーザーのIOTAトークンを選択されたアドレスに送信するための新しいスイープを作成します。
:::
<!-- :::info: -->
<!-- The database contains a record of how many IOTA tokens a user has. The IOTA tokens are not kept in the user's addresses. Instead, they are transferred to the Hub owners address during a [sweep](../concepts/sweeps.md). If a user later triggers a [`userWithdrawRequest` command](../references/api-reference.md#hub.rpc.UserWithdrawRequest), Hub creates a new sweep to send the user's tokens to the chosen addresses. -->
<!-- ::: -->

## トークン保護
<!-- ## Token protection -->

IOTAは、Winternitzワンタイム署名方式を使用して署名を作成します。その結果、各署名は秘密鍵の約半分を公開してしまいます。秘密鍵でバンドルに一度署名することは安全です。したがって、ユーザーがアドレスからIOTAトークンを取り出すと、そのアドレスは「使用済み」と見なされ、そのアドレスからは二度とIOTAトークンを取り出すことはできません。
<!-- IOTA uses the Winternitz one-time signature scheme to create signatures. As a result, each signature exposes around half of the private key. Signing a bundle once with the a private key is safe. So, when a user withdraws from an address, that address is considered 'spent' and must never be withdrawn from again. -->

:::info:
[使用済みアドレスの詳細と、使用済みアドレスからは2度とIOTAトークンを取り出してはいけない理由](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse)を知る。
:::
<!-- :::info: -->
<!-- [Discover the details about spent addresses and why you must never withdraw from an address more than once](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse). -->
<!-- ::: -->

ユーザーが使用済みアドレスからIOTAトークンを取り出せないようにするために、ハブには次の機能があります。
<!-- To help users not to withdraw from spent addresses, Hub has the following features: -->

**取り出し管理：** ユーザーのアドレスからIOTAトークンを取り出す前に、ハブは同じアドレスに対してペンディング中の預け入れトランザクションがないこと、および以前のすべての預け入れトランザクションが確定済みであることを確認します。どのアドレスから取り出されたかを追跡するために、ハブはアドレスをデータベースに格納します。アドレスからIOTAトークンが取り出されている場合、ハブはユーザーがそのアドレスから再びIOTAトークンを取り出すのを防ぎます。
<!-- **Withdrawal management:** Before withdrawing tokens from a user's address, Hub makes sure that no deposit transactions are pending for that same address, and that all previous deposit transactions have been confirmed. To keep track of which addresses have been withdrawn from, Hub stores the addresses in the database. When an address has been withdrawn from, Hub stops users from withdrawing from that address again. -->

**預け入れアドレス管理：** ハブは、すべての預け入れに対して新しいシードから新しいアドレスを導出します。そのために、ハブは取り出し管理を使用して、アドレスからすでにIOTAトークンが取り出されているかどうかを確認します。そして、アドレスからIOTAトークンが取り出されている場合、ハブは新しい預け入れアドレスの導出に使用する新しいシードUUIDを作成します。
<!-- **Deposit address management:** Hub derives a new address from a new seed for every deposit. To do so, Hub uses the withdrawal management to check whether an address was already withdrawn from. If an address has been withdrawn from, Hub creates a new seed UUID to use to derive a new deposit address. -->

**スウィープ：** ユーザーの取り出しリクエストを実行すると、ハブは[スイープ](../concepts/sweeps.md)と呼ばれるバンドルを作成します。これは、ユーザーの預け入れアドレスからハブ所有者のアドレスの1つに資金を移動するためのものです。
<!-- **Sweeps:** When actioning a user's withdrawal request, Hub creates a bundle, called a [sweep](../concepts/sweeps.md), that also moves funds from users' deposit addresses to one of the Hub owner's addresses. -->

## 制限事項
<!-- ## Limitations -->

ハブは、使用済みアドレスからユーザーがIOTAトークンを取り出すのを防ぐのに役立ちますが、ユーザーが使用済みアドレスに預け入れるのを妨げるものではありません。
<!-- Hub helps to stop users from withdrawing from spent addresses, but it doesn't stop users from depositing into them. -->

ユーザーが使用済みアドレスにトークンを預け入れた場合は、gRPC APIを使用して[それらのトークンを取り出すバンドルを作成](https://github.com/iotaledger/rpchub/blob/master/docs/hip/001-sign_bundle.md)できます。
<!-- If a user deposits tokens into a spent address, you can use the gRPC API to [create a bundle that withdraws those tokens](https://github.com/iotaledger/rpchub/blob/master/docs/hip/001-sign_bundle.md). -->

## リポジトリ
<!-- ## Repository -->

[GithubのHubソースコード](https://github.com/iotaledger/rpchub)を見る。
<!-- Go to the Hub source code on [Github](https://github.com/iotaledger/rpchub) -->
