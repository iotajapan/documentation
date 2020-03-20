# C でアドレスの残高を確認する
<!-- # Check the balance of an address in C -->

**このガイドでは，[ノード](root://getting-started/0.1/network/nodes.md)から[アドレス](root://getting-started/0.1/clients/addresses.md)の [IOTA トークン](root://getting-started/0.1/clients/token.md)の残高をリクエストします．**
<!-- **In this guide, you request the balance of [IOTA tokens](root://getting-started/0.1/clients/token.md) on [addresses](root://getting-started/0.1/clients/addresses.md) from a [node](root://getting-started/0.1/network/nodes.md).** -->

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

2. 残高を確認するアドレスを定義し，トリットに変換します．
  <!-- 2. Define the address whose balance you want to check and convert it to trits -->

    ```cpp
    static tryte_t const *const ADDRESS =
        (tryte_t *)"TOKLOARHKXQCVPPVVIPIJGLUTLTKFHYGMBBLOXJFYGSARLOTYFFSDZNYCOBOCNPGRMJWZCQBNOROUCE9G";

    flex_trit_t hash[FLEX_TRIT_SIZE_243];

    // トライトをトリットに変換する
    if (flex_trits_from_trytes(hash, NUM_TRITS_HASH, ADDRESS, NUM_TRYTES_HASH, NUM_TRYTES_HASH) == 0) {
        printf("Error: converting flex_trit failed\n");
        goto done;
    }
    ```

3. [`get_balances()`](https://github.com/iotaledger/entangled/blob/develop/cclient/api/core/get_balances.h) メソッドを使用して，アドレスの現在の残高をノードに要求します．
  <!-- 3. Use the [`get_balances()`](https://github.com/iotaledger/entangled/blob/develop/cclient/api/core/get_balances.h) method to ask the node for the current balance of the address -->

    ```cpp
    // 閾値を設定する（これは使用されませんが，設定する必要があります）
    balance_req->threshold = 100;

    if ((ret_code = iota_client_get_balances(service, balance_req, balance_res)) == RC_OK) {
        hash243_queue_entry_t *q_iter = NULL;
        size_t balance_cnt = get_balances_res_balances_num(balance_res);
        printf("balances: [");
        for (size_t i = 0; i < balance_cnt; i++) {
            printf(" %" PRIu64 " ", get_balances_res_balances_at(balance_res, i));
        }
        printf("]\n");

        CDL_FOREACH(balance_res->references, q_iter) {
            printf("Milestone tail transaction hash: ");
            flex_trit_print(q_iter->hash, NUM_TRITS_HASH);
            printf("\n");
        }
    }

    done:

    // オブジェクトのメモリ領域を解放する
    get_balances_req_free(&balance_req);
    get_balances_res_free(&balance_res);

    return ret_code;
    ```

    コンソールに，IOTA トークンの残高と，残高を確定した最新のマイルストーンのトランザクションハッシュが表示されます．
    <!-- In the console, you should see a balance of IOTA tokens as well as the transaction hash of the latest milestone that confirmed your balance: -->

    ```bash
    balances: [ 0 ]
    Milestone tail transaction hash: MY9BYZKEPBBKBKNIJZSHVTRDVFLCJWVPQPTYUJ9FZ9XG9YRYMSERPBOK9OGOEEYIWLOCHOJNLRKIXW999
    Check balances done
    ```

:::success:おめでとうございます:tada:
アドレスの残高を確認しました．
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just checked the balance of an address. -->
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
bazel run -c opt examples:check_balances
```

コンソールに，IOTA トークンの残高と，残高を確定した最新のマイルストーンのトランザクションハッシュが表示されます．
<!-- In the console, you should see a balance of IOTA tokens as well as the transaction hash of the latest milestone that confirmed your balance: -->

```bash
balances: [ 0 ]
Milestone tail transaction hash: MY9BYZKEPBBKBKNIJZSHVTRDVFLCJWVPQPTYUJ9FZ9XG9YRYMSERPBOK9OGOEEYIWLOCHOJNLRKIXW999
Check balances done
```

## 次のステップ
<!-- ## Next steps -->

[タングルエクスプローラー](https://utils.iota.org)などのユーティリティを使用して，アドレスの残高を確認することもできます．
<!-- You can also check the balance of an address, using a utility such as the [Tangle explorer](https://utils.iota.org). -->

インスピレーションについては，[アプリ設計図](root://blueprints/0.1/introduction/overview.md)をご覧ください．
<!-- Take a look at our [app blueprints](root://blueprints/0.1/introduction/overview.md) for inspiration. -->
