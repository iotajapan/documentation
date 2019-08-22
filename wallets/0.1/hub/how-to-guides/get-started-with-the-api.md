# gRPC APIを使い始める
<!-- # Get started with the gRPC API -->

**ハブは、任意のgRPCクライアントを使用して呼び出すことができるいくつかのgRPCメソッドを公開しています。これらの方法で、データベースおよびIOTAネットワークとやり取りすることでユーザーのIOTAトークンを管理できます。このガイドでは、新しい預け入れアドレスを持つ新しいユーザーを作成するためのgRPC APIの基本について学習します。**
<!-- **Hub exposes some gRPC methods that you can call using any gRPC client. These methods allow you to manage users' tokens by interfacing with the database and an IOTA network. In this guide, you'll learn the basics of the gRPC API to create a new user with some new deposit addresses.** -->

:::info:
このガイドは、[CLIクライアント](https://github.com/njpatel/grpcc)でgRPC APIをテストするのに役立ちます。実稼働環境では、利用可能な[gRPCライブラリ](https://grpc.io/about/)の1つからクライアントコードを生成することをお勧めします。
:::
<!-- :::info: -->
<!-- This guide helps you to test the gRPC API with [this CLI client](https://github.com/njpatel/grpcc). For production environments, we recommend generating client code from one of the available [gRPC libraries](https://grpc.io/about/). -->
<!-- ::: -->

## 前提条件
<!-- ## Prerequisites -->
[ハブをインストール](../how-to-guides/install-hub.md)し、そのガイドで使用しているサーバと同じサーバで実行する必要があります。
<!-- You must have [installed Hub](../how-to-guides/install-hub.md) and it must be running on the same server as the one you'll use in this guide. -->

---

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

3. `rpchub`ディレクトリに移動します。
  <!-- 3. Change into the `rpchub` directory -->

    ```bash
    cd rpchub
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

5. 新しいユーザーを作成します。
  <!-- 4. Create a new user -->

    ```bash
    client.createUser({userId: "Jake"}, pr)
    ```

    :::info:
    ステップ4のコンソールで、`createUser`メソッドが`CreateUserRequest`オブジェクトを受け取ることがわかります。そのオブジェクトは[APIリファレンス](../references/api-reference.md#hub.rpc.CreateUserRequest)で検索できます。

    `pr`引数は、結果をコンソールに出力するビルド済みのコールバック関数です。
    :::
    <!-- :::info: -->
    <!-- You can see in the console from step 3 that the `createUser` method takes a `CreateUserRequest` object. You can search for that object in the [API reference](../references/api-reference.md#hub.rpc.CreateUserRequest). -->
    <!--  -->
    <!-- The `pr` argument is a pre-built callback function that prints the result to the console. -->
    <!-- ::: -->

    これで、データベースに新しいユーザーができました。
    <!-- Now, you'll have a new user in the database. -->

    :::info:
    [user_account`テーブルをクエリする](../how-to-guides/query-the-database.md)ことでデータベースでこのユーザーを確認することができます。
    :::
    <!-- :::info: -->
    <!--  You can see this user in the database by [querying the `user_account` table](../how-to-guides/query-the-database.md). -->
    <!--  ::: -->

6. ユーザーの新しい預け入れアドレスを作成します。
  <!-- 5. Create a new deposit address for the user -->

    ```bash
    client.getDepositAddress({userId: "Jake"}, pr)
    ```

    コンソールに新しい預け入れアドレスが表示されます。
    <!-- You should see a new deposit address in the console. -->

7. チェックサムを持つ新しいアドレスを作成します。
  <!-- 7. Create a new address with the checksum -->

    ```bash
    client.getDepositAddress({userId: "Jake", includeChecksum: true}, pr)
    ```

    これで、ユーザーは2つの異なる`seeduuid`フィールドから作成された2つのアドレスを持つことになります。[`user_address`テーブルにクエリする](../how-to-guides/query-the-database.md)ことで、データベース内のこのデータを見ることができます。
    <!-- Now, that user will have two addresses that were created from two different `seeduuid` fields. You can see this data in the database by [querying the `user_address` table](../how-to-guides/query-the-database.md). -->

    :::info:
    データベースでは、アドレスは常にチェックサムなしで保存されます。

    すべてのアドレスは固有のシードから作成され、すべてのシードは`seeduuid`フィールドの値と[`salt`](../references/command-line-flags.md)パラメータ（もし与えられていれば）のハッシュ値から作られます。
    :::
    <!-- :::info: -->
    <!-- In the database, addresses are always saved without the checksum. -->
    <!--  -->
    <!-- All addresses are created from unique seeds, and all seeds are created from a hash of the values of the `seeduuid` field and the [`salt`](../references/command-line-flags.md) parameter (if provided). -->
    <!-- ::: -->

8. **Ctrl**+**C**を2回押して、gRPCクライアントを停止します。
  <!-- 8. Press **Ctrl**+**C** twice to stop the gRPC client -->

:::success:おめでとうございます！:tada:
2つの異なる預け入れアドレスを持つ新しいユーザーを作成するために2つのgRPCメソッドを使用しました。
これらの預け入れアドレスはそれぞれ、固有のシードから導出したものです。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've successfully used two gRPC methods to create a new user with two different deposit addresses. -->
<!-- Each of these deposit addresses was derived from a unique seed. -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

1. IOTAトークンをユーザーの預け入れアドレスの1つに預け入れる。
<!-- 1. Deposit IOTA tokens into one of the user deposit addresses -->
2. [`sweepSubscription`](../references/api-reference.md#hub.rpc.SweepSubscriptionRequest)メソッドを使用して、新しいスウィープイベントを購読する。こうすることで、スウィープが行われるとハブから通知されます。
<!-- 2. Use the [`sweepSubscription`](../references/api-reference.md#hub.rpc.SweepSubscriptionRequest) method to subscribe to new sweep events. This way, Hub will let you know when a sweep takes place. -->
4. [`userWithdraw`](../references/api-reference.md#hub.rpc.UserWithdrawRequest)メソッドを使用して、預け入れアドレスからのIOTAトークンの取り出しを依頼する。
<!-- 3. Use the [`userWithdraw()`](../references/api-reference.md#hub.rpc.UserWithdrawRequest) method to make a withdrawal request from the deposit address -->
