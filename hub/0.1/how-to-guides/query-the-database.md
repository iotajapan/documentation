# ユーザーのアドレスのシードUUIDを調べる
<!-- # Find out the seed UUID of a user's address -->

**ユーザーデータを追跡するために、ハブはユーザーデータをデータベーステーブルに格納します。このデータベーステーブルの一部のデータは、gRPC APIに公開されていません。たとえば、シードを再作成できるように、アドレスのシードUUIDを見つけたい場合があります。このガイドでは、ハブデータベーステーブルの照会を開始するための基本を説明します。**
<!-- **To keep track of user data, Hub stores it in database tables. Some of the data in these tables isn't exposed to the gRPC API. For example, you may want to find out the seed UUID for an address so that you can recreate the seed. This guide shows you the basics to get started with querying the Hub database tables.** -->

:::info:
このガイドはMariaDBのようなMYSQLデータベースと一緒にハブをインストールしていると仮定します。
:::
<!-- :::info: -->
<!-- This guide assumes that you installed Hub with a MYSQL database such as MariaDB, which is the one we use in our installation guide. -->
<!-- ::: -->

## 前提条件
<!-- ## Prerequisites -->

[預け入れアドレスを持つユーザーを作成します](../how-to-guides/get-started-with-the-api.md)。
<!-- [Create a user who has a deposit address](../how-to-guides/get-started-with-the-api.md). -->

---

1. MYSQLに接続します。
  <!-- 1. Connect to the MYSQL -->

    ```sql
    mysql -u root -p
    ```

2. `hub`データベースを選択します。
  <!-- 2. Select the `hub` database -->

    ```sql
    USE hub
    ```

3. データベース内のすべてのテーブルのリストを表示します。
  <!-- 3. Display a list of all the tables in the database -->

    ```sql
    SHOW TABLES;
    ```

4. ユーザーのアドレスとそれに関連付けられたシードUUIDに関する情報を表示します。
  <!-- 4. Display information about your users' addresses and their associated seed UUIDs -->

    ```sql
    SELECT * from user_address;
    ```

    :::info:
    別のテーブルのデータを表示するには、`user_address`を表示したいテーブルの名前に置き換えます。
    :::
    <!-- :::info: -->
    <!-- To display data from another table, replace `user_address` with the name of that table. -->
    <!-- ::: -->

    次のようなものが見えるはずです。
    <!-- You should see something like the following: -->

    ```sql
    | id | address | user_id | seed_uuid | created_at | balance |
    +----+---------+---------+-----------+-------------+--------+
    |  1 | RPMENBAM9QOGSCMPIMHLFB9SCCUEUIYM9ODNXYZBUGB9PVZJTSNEZRLJXIPAAJDQTZMJSTDLFUHR9JFSD | 1 | Fnb3MRpr8gacuh1n1CMQQja5Dr53rCqrs4i9kiw0wYysft2d7vzjaPHJRAHjIkPd | 2019-03-28 13:38:48 |       0 |
    |  2 | DAUBDIQEAOMNWYL9OLBQUABXIHVEDWEBFPPZCSVUS9HIKWFUSGHLAULFNFWPFQUHCIFKPRRZVFAGBBFJZ |       2 | TqHIfR0GCIKJPCZ8Q2z6UUQcJi4McGuUjJpkNloDR28cIL7hrMZDtQNAtTqyT2bJ | 2019-04-01 13:39:55 |       0 |
    |  3 | S9OEPEMNSYOZKHPUDWCSH9KJDLDLWHTCNIHBDRTCQHFERTEABJVMRHUX9MIBGCYJINTSAWHVDFFXIIAFA |       2 | N5WrisKpidnvrYSr0aSxtncRr0SH8kUHJfYPEKNZU7DjO2XjL24cxRg6RBTMLpY | 2019-04-01 13:49:43 |       0 |
    +----+--------+---------+-----------------+----------+-------+
    ```

5. MYSQLから切断します。
  <!-- 5. Disconnect from MYSQL -->

    ```sql
    QUIT
    ```

:::info:
MYSQLとさまざまなコマンドについて学ぶためには、[MYSQLのウェブサイト](https://dev.mysql.com/doc/refman/8.0/en/tutorial.html)をご覧ください。
:::
<!-- :::info: -->
<!-- To learn about MYSQL and the different commands, [see their website](https://dev.mysql.com/doc/refman/8.0/en/tutorial.html). -->
<!-- ::: -->
