# ハブ概要
<!-- # Hub overview -->

**ハブは、暗号資産交換用のヘッドレスマルチユーザーウォレットです。ハブは、RESTful APIとgRPC APIを介して、ユーザーのIOTAトークンの預け入れや取り出しを管理する安全な方法を提供します。**
<!-- **Hub is a headless multi-user wallet for cryptocurrency exchanges. Hub offers you a secure way to manage deposits and withdrawals of users' IOTA tokens through a RESTful API and a gRPC API.** -->

ハブを使用すると、以下のプロセスを通じてIOTAを独自のアプリケーションに統合できます。
<!-- Hub helps you to integrate IOTA into your own applications through the following processes: -->

* [トランザクション監視](#トランザクション監視)
<!-- * [Transaction monitoring](#transaction-monitoring) -->
* [シード作成](#シード作成)
<!-- * [Seed creation](#seed-creation) -->
* [トークン保護](#トークン保護)
<!-- * [Token protection](#token-protection) -->

ハブを使用して、ユーザーがIOTAトークンをハブに預け入れられるようにするアプリケーションを構築できます。ユーザーがIOTAトークンを預け入れると、ハブは残高をデータベースに記録します。次に、ハブはそれらのIOTAトークンをハブ所有者のIOTAアドレスに転送します。このようにして、ハブ所有者はIOTAトークンを管理し、IOTAトークンをユーザーに対して安全に保つことができます。後でユーザーが取り出しを要求した場合、ハブ所有者はユーザーの残高を確認し、一連のトランザクションをノードに送信することでその取り出しを発行できます。
<!-- You can use Hub to build applications that allow users to deposit IOTA tokens into it. When users deposit IOTA tokens, Hub keeps a record of their balances in a database. Then, Hub transfers those tokens to the Hub owner's IOTA address. This way the Hub owner has control over the tokens and can keep them safe for the users. If a user later requests a withdrawal, the Hub owner can issue that withdrawal by checking the user's balance and sending a bundle of transactions to a node. -->

預け入れや取り出しなどのすべてのハブ機能は、[gRPC APIエンドポイント](../how-to-guides/get-started-with-the-grpc-api.md)または[RESTful APIエンドポイント](../references/restful-api-reference.md)を呼び出すことによって行われます。
<!-- All Hub functions such as deposits and withdrawals are done by calling either [gRPC API endpoints](../how-to-guides/get-started-with-the-grpc-api.md) or [RESTful API endpoints](../references/restful-api-reference.md). -->

## トランザクション監視
<!-- ## Transaction monitoring -->

トランザクションがIOTAネットワークによって受け入れられる前に、トランザクションは確定されなければなりません。ユーザーが自分のアドレスの1つにIOTAトークンを預け入れたとき、またはハブ所有者が取り出しを発行したときに、トランザクションはペンティぐ状態のままになります。そのため、確認の遅れを避けるために、Hubは保留中のトランザクションのデータベースを自動的に[再接続して促進]できるように保持します。そのため、確定の遅れを避けるために、ハブは、ペンディング中のトランザクションを自動的に[再添付と促進]をできるように、ペンディング中のトランザクションのデータベースを保持します。
<!-- Before any transaction is accepted by an IOTA network, it must be confirmed. When a user deposits IOTA tokens into one of their addresses, or when the Hub owner issues a withdrawal, the transactions may become stuck in a pending state. So, to avoid delays in confirmation, Hub keeps a database of pending transactions so that it can automatically [reattach and promote](root://dev-essentials/0.1/concepts/reattach-rebroadcast-promote.md) them. -->

## シード作成
<!-- ## Seed creation -->

IOTAネットワークの各クライアントには、[シード](root://getting-started/0.1/introduction/what-is-a-seed.md)と呼ばれる秘密のパスワードがあります。これは、[アドレスの作成とトランザクションのバンドルへの署名](root://dev-essentials/0.1/concepts/addresses-and-signatures.md)に使用されます。アドレスはトランザクションの送受信元のアカウントであり、署名はアドレスの所有権を証明するものです。
<!-- Each client in an IOTA network has a secret password called a [seed](root://getting-started/0.1/introduction/what-is-a-seed.md), which is used to create [addresses and to sign bundles of transactions](root://dev-essentials/0.1/concepts/addresses-and-signatures.md). Addresses are the accounts from which transactions are sent and received, and signatures prove ownership of an address. -->

各ユーザーのデポジットアドレスは、[Argon2](https://www.argon2.com/)ハッシュ関数を使用して新しいシードから導出します。次の値はシードを作成するために使用されます。
<!-- Each user's deposit addresses is derived from a new seed, using the [Argon2](https://www.argon2.com/) hashing function. The following values are used to create a seed: -->

* シードUUID：[`seeduuid`フィールド](../references/database-tables.md#user_account)に格納されている、ランダムに生成された普遍的に一意な識別子
<!-- * Seed UUID: A randomly generated universally unique identifier that is stored in a [`seeduuid` field](../references/database-tables.md#user_account) -->
* ソルト：オプションの[`salt`フラグ](../references/command-line-flags.md)で定義される文字列
<!-- * Salt: Characters that you can define in an optional [`salt` flag](../references/command-line-flags.md) -->

:::info:
データベースには、ユーザーが所有しているIOTAトークン量の記録が含まれています。IOTAトークンはユーザーのアドレスには保存されません。代わりに、ユーザーのIOTAトークンは[スウィープ](../concepts/sweeps.md)中にハブ所有者のアドレスに転送されます。
:::
<!-- :::info: -->
<!-- The database contains a record of how many IOTA tokens a user has. The IOTA tokens are not kept on the user's addresses. Instead, they are transferred to the Hub owners address during a [sweep](../concepts/sweeps.md). -->
<!-- ::: -->

## トークン保護
<!-- ## Token protection -->

IOTAは、バンドルに署名するためにWinternitzワンタイム署名方式を使用します。その結果、各署名は秘密鍵の約半分を公開します。秘密鍵でバンドルに一度署名するのは安全です。したがって、ユーザーがアドレスからIOTAトークンを取り出すと、そのアドレスは「使用済み」と見なされ、そのアドレスからは二度とIOTAトークンを取り出すことはできません。
<!-- IOTA uses the Winternitz one-time signature scheme to sign bundles. As a result, each signature exposes around half of the private key. Signing a bundle once with a private key is safe. So, when a user withdraws from an address, that address is considered 'spent' and must never be withdrawn from again. -->

:::info:
[IOTAの署名について、そしてなぜ一度取り出したアドレスから再度取り出してはいけないのかを学びます](root://dev-essentials/0.1/concepts/addresses-and-signatures.md#address-reuse)。
:::
<!-- :::info: -->
<!-- [learn about signatures in IOTA and why you must never withdraw from an address more than once](root://dev-essentials/0.1/concepts/addresses-and-signatures.md#address-reuse). -->
<!-- ::: -->

ユーザーが使用済みアドレスからIOTAトークンを取り出せないようにするために、ハブには次の機能があります。
<!-- To help users not to withdraw from spent addresses, Hub has the following features: -->

**取り出し管理：** ユーザーのアドレスからIOTAトークンを取り出す前に、ハブは同じアドレスに対してペンディング中の預け入れトランザクションがないこと、および以前のすべての預け入れトランザクションが確定済みであることを確認します。どのアドレスから取り出されたかを追跡するために、ハブはアドレスをデータベースに格納します。アドレスからIOTAトークンが取り出されている場合、ハブはユーザーがそのアドレスから再びIOTAトークンを取り出すのを防ぎます。
<!-- **Withdrawal management:** Before withdrawing tokens from a user's address, Hub makes sure that no deposit transactions are pending for that same address, and that all previous deposit transactions have been confirmed. To keep track of which addresses have been withdrawn from, Hub stores the addresses in the database. When an address has been withdrawn from, Hub stops users from withdrawing from that address again. -->

**預け入れアドレス管理：** ハブは、すべての預け入れに対して新しいシードから新しいアドレスを導出します。そのために、ハブは取り出し管理を使用して、アドレスからすでにIOTAトークンが取り出されているかどうかを確認します。そして、アドレスからIOTAトークンが取り出されている場合、ハブは新しい預け入れアドレスの導出に使用する新しいシードUUIDを作成します。
<!-- **Deposit address management:** Hub derives a new address from a new seed for every deposit. To do so, Hub uses the withdrawal management to check whether an address was already withdrawn from. If an address has been withdrawn from, Hub creates a new seed UUID to use to derive a new deposit address. -->

**スウィープ：** ユーザーの取り出しリクエストを実行すると、ハブは[スウィープ](../concepts/sweeps.md)と呼ばれるバンドルを作成します。これは、ユーザーの預け入れアドレスからハブ所有者のアドレスの1つに資金を移動するためのものです。
<!-- **Sweeps:** When actioning a user's withdrawal request, Hub creates a bundle, called a [sweep](../concepts/sweeps.md), that also moves funds from users' deposit addresses to one of the Hub owner's addresses. -->

## 制限事項
<!-- ## Limitations -->

ユーザーが使用済みアドレスにIOTAトークンを預け入れた場合、APIの1つの`recoverFunds`メソッドを使用できます。
<!-- If a user deposits tokens into a spent address, you can use the `recoverFunds` method in one of the APIs. -->

ユーザーが使用済みアドレスにトークンを預け入れた場合は、gRPC APIを使用して[それらのトークンを取り出すバンドルを作成](https://github.com/iotaledger/rpchub/blob/master/docs/hip/001-sign_bundle.md)できます。
<!-- If a user deposits tokens into a spent address, you can use the gRPC API to [create a bundle that withdraws those tokens](https://github.com/iotaledger/rpchub/blob/master/docs/hip/001-sign_bundle.md). -->

## リポジトリ
<!-- ## Repository -->

[GithubのHubソースコード](https://github.com/iotaledger/rpchub)を見る。
<!-- Go to the Hub source code on [Github](https://github.com/iotaledger/rpchub) -->
