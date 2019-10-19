# gRPC API入門
<!-- # Get started with the gRPC API -->

**ハブは、任意のgRPCクライアントを使用して呼び出すことができるいくつかのgRPCメソッドを公開しています。これらの方法で、ハブデータベースおよびIOTAノードとやり取りすることでユーザーのIOTAトークンを管理できます。このガイドでは、新しい預け入れアドレスを持つ新しいユーザーを作成するためのgRPC APIの基本について学習します。**
<!-- **Hub exposes some gRPC methods that you can call using any gRPC client. These methods allow you to manage users' tokens by interfacing with the Hub database and an IOTA node. In this guide, you learn the basics of the gRPC API to create a new user with some new deposit addresses.** -->

## 前提条件
<!-- ## Prerequisites -->

[ハブのインストール](../how-to-guides/install-hub.md)が必要であり、このガイドで使用するサーバーと同じサーバーでハブを実行している必要があります。
<!-- You must have [installed Hub](../how-to-guides/install-hub.md) and it must be running on the same server as the one you use in this guide. -->

## 手順1. gRPCクライアントをセットアップする
<!-- ## Step 1. Set up the gRPC client -->

API呼び出しを送信する前に、API呼び出しを作成できるgRPCクライアントが必要です。
<!-- Before you can send API calls, you need a gRPC client that can create them. -->

:::info:
このガイドは、[GRPCCコマンドラインクライアント](https://github.com/njpatel/grpcc)でgRPC APIをテストするのに役立ちます。

実稼働環境では、利用可能な[gRPCライブラリ](https://grpc.io/about/)のいずれかからクライアントコードを生成することをお勧めします。
:::
<!-- :::info: -->
<!-- This guide helps you to test the gRPC API with [a GRPCC command-line client](https://github.com/njpatel/grpcc). -->

<!-- For production environments, we recommend generating client code from one of the available [gRPC libraries](https://grpc.io/about/). -->
<!-- ::: -->

1. npmをインストールします。
  <!-- 1. Install npm -->

    ```bash
    sudo apt install -y npm
    ```

2. `grpcc`パッケージをインストールします。
  <!-- 2. Install the `grpcc` package -->

    ```bash
    sudo npm install -g grpcc
    ```

3. `hub`ディレクトリに移動します。
  <!-- 3. Change into the `hub` directory -->

    ```bash
    cd hub
    ```
4. gRPCクライアントを起動します。
  <!-- 3. Start the gRPC client -->

    ```bash
    grpcc -i -a localhost:50051 -p proto/hub.proto
    ```

    :::info:
    バイナリがインストールされていないというエラーメッセージが表示される場合は、`node_modules`ディレクトリを削除してから、`sudo npm install -g --unsafe-perm grpcc`コマンドで`grpcc`パッケージを再インストールします。
    :::
    <!-- :::info: -->
    <!-- If you see an error message about a binary not being installed, try removing your `node_modules` directory, then reinstall the `grpcc` package with the following command: `sudo npm install -g --unsafe-perm grpcc`. -->
    <!-- ::: -->

    次のようなものが見えるはずです。
    <!-- You should see something like the following: -->

    ```bash
    Connecting to hub.rpc.Hub on localhost:50051. Available globals:

    client - the client connection to Hub
        createUser (CreateUserRequest, callback) returns CreateUserReply
        getBalance (GetBalanceRequest, callback) returns GetBalanceReply
        getDepositAddress (GetDepositAddressRequest, callback) returns GetDepositAddressReply
        userWithdraw (UserWithdrawRequest, callback) returns UserWithdrawReply
        userWithdrawCancel (UserWithdrawCancelRequest, callback) returns UserWithdrawCancelReply
        getUserHistory (GetUserHistoryRequest, callback) returns GetUserHistoryReply
        processTransferBatch (ProcessTransferBatchRequest, callback) returns ProcessTransferBatchReply
        balanceSubscription (BalanceSubscriptionRequest, callback) returns BalanceEvent
        getStats (GetStatsRequest, callback) returns GetStatsReply
        sweepSubscription (SweepSubscriptionRequest, callback) returns SweepEvent
        getAddressInfo (GetAddressInfoRequest, callback) returns GetAddressInfoReply
        sweepInfo (SweepInfoRequest, callback) returns SweepEvent
        signBundle (SignBundleRequest, callback) returns SignBundleReply
        sweepDetail (SweepDetailRequest, callback) returns SweepDetailReply

    printReply - function to easily print a unary call reply (alias: pr)
    streamReply - function to easily print stream call replies (alias: sr)
    createMetadata - convert JS objects into grpc metadata instances (alias: cm)
    printMetadata - function to easily print a unary call's metadata (alias: pm)

    Hub@localhost:50051> (node:6023) DeprecationWarning: grpc.load: Use the @grpc/proto-loader module with grpc.loadPackageDefinition instead
    ```

    :::info:
    These methods are all documented in the [gRPC API reference](../references/grpc-api-reference.md).
    :::

## 手順2. ハブにIOTAトークンを預け入れる
<!-- ## Step 2. Deposit IOTA tokens into Hub -->

gRPCクライアントがある場合は、gRPCクライアントを使用してAPI呼び出しをハブに送信し、ユーザーとその預け入れを管理できます。
<!-- When you have a gRPC client, you can use it to send API calls to Hub to manage users and their deposits. -->

1. 新しいユーザーを作成する
  <!-- 1. Create a new user -->

    ```bash
    client.createUser({userId: "Jake"}, pr)
    ```

    :::info:
    `createUser()`メソッドは`CreateUserRequest`オブジェクトを受け取ります。[APIリファレンス](../references/grpc-api-reference.md#hub.rpc.CreateUserRequest)で`CreateUserRequest`オブジェクトを検索できます。

    `pr`引数は、結果をコンソールに出力するビルド済みのコールバック関数です。
    :::
    <!-- :::info: -->
    <!-- You can see in the console from step 3 that the `createUser()` method takes a `CreateUserRequest` object. You can search for that object in the [API reference](../references/grpc-api-reference.md#hub.rpc.CreateUserRequest). -->
    <!-- The `createUser()` method takes a `CreateUserRequest` object. You can search for that object in the [API reference](../references/grpc-api-reference.md#hub.rpc.CreateUserRequest). -->

    <!-- The `pr` argument is a pre-built callback function that prints the result to the console. -->
    <!-- ::: -->

    これで、データベースに新しいユーザーができました。
    <!-- Now, you have a new user in the database. -->

    :::info:
    このユーザーは、[`user_account`テーブルのクエリ](../how-to-guides/query-the-database.md)によってハブデータベースで確認できます。
    :::
    <!-- :::info: -->
    <!-- You can see this user in the Hub database by [querying the `user_account` table](../how-to-guides/query-the-database.md). -->
    <!-- ::: -->

2. ユーザーの新しい預け入れアドレスを作成します。
  <!-- 2. Create a new deposit address for the user -->

    ```bash
    client.getDepositAddress({userId: "Jake"}, pr)
    ```

    コンソールに新しい預け入れアドレスが表示されます。
    <!-- You should see a new deposit address in the console. -->

3. チェックサムを持つ新しい預け入れアドレスを作成します。
  <!-- 3. Create a new deposit address with the checksum -->

    ```bash
    client.getDepositAddress({userId: "Jake", includeChecksum: true}, pr)
    ```

    これで、ユーザーには2つの異なる`seeduuid`フィールドから作成された2つのアドレスがあります。[`user_address`テーブルのクエリ](../how-to-guides/query-the-database.md)によってデータベース内のこのデータを見ることができます。
    <!-- Now, the user has two addresses that were created from two different `seeduuid` fields. You can see this data in the database by [querying the `user_address` table](../how-to-guides/query-the-database.md). -->

    :::info:
    データベースでは、アドレスは常にチェックサムなしで保存されます。
    :::
    <!-- :::info: -->
    <!-- In the database, addresses are always saved without the checksum. -->
    <!-- ::: -->

4. いくつかのIOTAトークンをユーザーの預け入れアドレスのいずれかに送信します。
  <!-- 4. Send some IOTA tokens to one of the user's deposit addresses -->

    :::info:
    [トリニティ](root://wallets/0.1/trinity/introduction/overview.md)は公式のIOTAウォレットであり、IOTAトークンを簡単に送信できます。
    :::
    <!-- :::info: -->
    <!-- [Trinity](root://wallets/0.1/trinity/introduction/overview.md) is the official IOTA wallet, which makes it easy to send IOTA tokens. -->
    <!-- ::: -->

5. ユーザーの残高と履歴を取得します。
  <!-- 5. Get the balance and history for the user -->

	```bash
	client.getBalance({userId: "Jake"}, pr)
	```

手順4でIOTAトークンを預け入れアドレスに送信した場合、出力には次のように表示されます。
<!-- If you sent IOTA tokens to the deposit address in step 4, the output should display something like the following: -->

```shell
10 i available for 'Jake'
History:
events {
	timestamp: 1540856214000
	type: DEPOSIT
	amount: 10
}
```

[thetangle.org](https://thetangle.org/)などのタングルエクスプローラーで預け入れアドレスの履歴を見ると、ハブがデポジットアドレスから別のアドレス（ハブの所有者のユーザーが取り出しを要求するまで資金が集められるアドレス）に資金を移動したことがわかります。このプロセスは[スウィープ](../concepts/sweeps.md)と呼ばれます。
<!-- If you look at the deposit address history in a Tangle explorer such as [thetangle.org](https://thetangle.org/), you will see that Hub moved the funds away from the deposit address and into another address (Hub owner's address where funds are aggregated until a user requests a withdrawal). This process is called a [sweep](../concepts/sweeps.md). -->

6. **Ctrl** + **C**を2回押して、gRPCクライアントを停止します。
<!-- 6. Press **Ctrl**+**C** twice to stop the gRPC client -->

:::success:おめでとうございます:tada:
新しいユーザーを正常に作成し、ハブがIOTAトークンの預け入れを処理する方法をテストしました。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've successfully created a new user and tested how Hub handles deposits of IOTA tokens. -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

[デモ取引所のセットアップ](../how-to-guides/create-a-demo-exchange.md)を行い、ハブの統合をテストする。
<!-- [Set up a demo exchange](../how-to-guides/create-a-demo-exchange.md) to test an integration of Hub. -->

[取引所にハブを統合する](../how-to-guides/integrate-hub.md)。
<!-- [Integrate Hub into your exchange](../how-to-guides/integrate-hub.md). -->
