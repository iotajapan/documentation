# C でタングルからトランザクションを読み取る
<!-- # Read transactions from the Tangle in C -->

**このガイドでは，[ノード](root://getting-started/0.1/network/nodes.md)に接続し，アドレスでフィルタリングするように要求することにより，[トランザクション](root://getting-started/0.1/transactions/transactions.md)をタングルから取得します．次に，トランザクション内のメッセージをデコードし，コンソールに出力します．**
<!-- **In this guide, you get [transactions](root://getting-started/0.1/transactions/transactions.md) from the Tangle by connecting to a [node](root://getting-started/0.1/network/nodes.md) and asking it to filter them by their addresses. Then, you decode the message in the transaction and print it to the console.** -->

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは，[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します．
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

ネットワーク設定は `config.h` ファイルで定義されます．これは[スタートガイド](../../getting-started/c-quickstart.md)で作成します．
<!-- The network settings are defined in a `config.h` file, which we create in the [getting started guide](../../getting-started/c-quickstart.md). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. パッケージをインポートします．
  <!-- 1. Import the packages -->

    ```cpp
    #include "cclient/api/core/core_api.h"
    #include "cclient/api/extended/extended_api.h"

    #include "common/trinary/tryte_ascii.h"
    #include "utils/time.h"
    #include <inttypes.h>

    #include "iota_client_service/config.h"
    #include "iota_client_service/client_service.h"
    ```

2. トランザクションをフィルタリングしてトリットに変換するために使用するアドレスを定義します．
  <!-- 2. Define the address that you want to use to filter transactions and convert it to trits -->

    ```cpp
    static tryte_t const *const ADDRESS =
        (tryte_t *)"ZLGVEQ9JUZZWCZXLWVNTHBDX9G9KZTJP9VEERIIFHY9SIQKYBVAHIMLHXPQVE9IXFDDXNHQINXJDRPFDXNYVAPLZAW";

    flex_trit_t hash[FLEX_TRIT_SIZE_243];

    // アドレスをトライトからトリットへ変換する
    if (flex_trits_from_trytes(hash, NUM_TRITS_HASH, address, NUM_TRYTES_HASH, NUM_TRYTES_HASH) == 0) {
        printf("Failed to convert trytes to trits\n");
        goto done;
    }
    ```

    :::info:
    C ライブラリは，すべてのトランザクションフィールドがトリットであると想定しています．
    :::
    <!-- :::info: -->
    <!-- The C library expects all transaction fields to be in trits. -->
    <!-- ::: -->

3. [`find_transaction_objects()`](https://github.com/iotaledger/entangled/blob/develop/cclient/api/extended/find_transaction_objects.h) メソッドを使用して，`address` フィールドの値でトランザクションを取得します．次に，トランザクションの `signatureMessageFragment` フィールドにあるメッセージをデコードし，コンソールに出力します．
  <!-- 3. Use the [`find_transaction_objects()`](https://github.com/iotaledger/entangled/blob/develop/cclient/api/extended/find_transaction_objects.h) method to get transactions by the value of their `address` field. Then, decode the message in the `signatureMessageFragment` fields of the transactions and print it to the console -->

    ```cpp
    // アドレスのトリットを find_transactions_req_t に追加する
    if ((ret_code = hash243_queue_push(&find_tran->addresses, hash)) != RC_OK) {
        printf("Error: push queue %s\n", error_2_string(ret_code));
        goto done;
    }

    // アドレスに送信された全てのトランザクションを見つける
    if ((ret_code = iota_client_find_transaction_objects(s, find_tran, out_tx_objs)) == RC_OK) {
        // ノードが返したトランザクションの総数を出力する
        printf("find transaction count: %lu\n", transaction_array_len(out_tx_objs));
        iota_transaction_t *tx1 = transaction_array_at(out_tx_objs, TX_INDEX);
        // 見つかった最初のトランザクションに関する情報を出力する
        if (tx1) {
            printf("dump first transaction:\n");
            printf("value = %" PRId64 ", current_index in the bundle = %" PRIu64 ", last_index of the bundle = %" PRIu64 "\n", transaction_value(tx1),
                    transaction_current_index(tx1), transaction_last_index(tx1));
            printf("address: ");
            flex_trit_print(transaction_address(tx1), NUM_TRITS_ADDRESS);
            printf("\n");

            // トランザクションの signatureMessageFragment フィールドの値を出力する
            printf("data: ");
            tryte_t * trytes = transaction_message(tx1);
            size_t tryte_size = strlen((char*)trytes);
            char buffer[tryte_size / 2 + 1];
            trytes_to_ascii(trytes, tryte_size, buffer);

            printf("%s\n", buffer);
        }
    } else {
        printf("Error: %s \n", error_2_string(ret_code));
    }

    done:
    // オブジェクトのメモリを開放する
    find_transactions_req_free(&find_tran);
    transaction_array_free(out_tx_objs);

    return ret_code;
    ```

    コンソールに，トランザクションとそのメッセージに関する情報が表示されます．
    <!-- In the console, you should see information about the transaction and its message: -->

    ```bash
    find transaction count: 84
    dump first transaction:
    value = 0, curr_index = 0, last_index = 0
    addr: ZLGVEQ9JUZZWCZXLWVNTHBDX9G9KZTJP9VEERIIFHY9SIQKYBVAHIMLHXPQVE9IXFDDXNHQINXJDRPFDX
    data: Hello world
    Transaction read
    ```

:::success:おめでとうございます:tada:
タングルからトランザクションを見つけて読み取りました．
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just found and read a transaction from the Tangle. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

これらのコードサンプルは [GitHub](https://github.com/iota-community/c-iota-workshop) でホストされています．
<!-- These code samples are hosted on [GitHub](https://github.com/iota-community/c-iota-workshop). -->

開始するには，デバイスに [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) がインストールされている必要があります．
<!-- To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device. -->

C 開発環境も必要です． C クライアントライブラリを初めて使用する場合は，[スタートガイド](../../getting-started/c-quickstart.md)を完了してください．
<!-- You also need a C development environment. If this is your first time using the C client library, complete our [getting started guide](../../getting-started/c-quickstart.md). -->

コマンドラインで，次を実行します．
<!-- In the command-line, do the following: -->

```bash
git clone https://github.com/iota-community/c-iota-workshop.git
cd c-iota-workshop
bazel run -c opt examples:receive_hello
```

コンソールに，トランザクションとそのメッセージに関する情報が表示されます．
<!-- In the console, you should see information about the transaction and its message: -->

```bash
find transaction count: 84
dump first transaction:
value = 0, curr_index = 0, last_index = 0
addr: ZLGVEQ9JUZZWCZXLWVNTHBDX9G9KZTJP9VEERIIFHY9SIQKYBVAHIMLHXPQVE9IXFDDXNHQINXJDRPFDX
data: Hello world
Transaction read
```

## 次のステップ
<!-- ## Next steps -->

[新しいアドレスを生成する](../c/generate-an-address.md)．
<!-- [Generate a new address](../c/generate-an-address.md). -->
