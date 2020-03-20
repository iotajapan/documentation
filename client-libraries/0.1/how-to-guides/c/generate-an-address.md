# C でアドレスを生成する
<!-- # Generate an address in C -->

**このガイドでは，[セキュリティレベル](root://getting-started/0.1/clients/security-levels.md)を指定して[シード](root://getting-started/0.1/clients/seeds.md)の新しいアドレスを生成する方法を学習します．**
<!-- **In this guide, you learn how to generate a new address for a [seed](root://getting-started/0.1/clients/seeds.md) with a given [security level](root://getting-started/0.1/clients/security-levels.md).** -->

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは，[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続し，セキュリティレベル2のアドレスを生成します．
<!-- In this guide, we connect to a [node](root://getting-started/0.1/network/nodes.md) on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet), and we generate an address with security level 2. -->

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

    #include "iota_client_service/config.h"
    ```

2. アドレスを生成するのシードを定義します．
  <!-- 2. Define a seed for which to generate an address -->

    ```cpp
    static tryte_t const *const SEED =
        (tryte_t *)"G9JEMIRJKUXDKUPPAIMEQSGVADYLSJRSBTEIRDWSCTLCVQOJWBM9XESTWTSONOTDDQUXMYCNVAKZWPPYW";
    ```

3. [`get_new_address()`](https://github.com/iotaledger/entangled/blob/develop/cclient/api/extended/get_new_address.h) メソッドを使用して，未使用のアドレスを生成します．
  <!-- 3. Use the [`get_new_address()`](https://github.com/iotaledger/entangled/blob/develop/cclient/api/extended/get_new_address.h) method to generate an unspent address -->

    ```cpp
    flex_trit_t seed[FLEX_TRIT_SIZE_243];

    // シードをトライトからトリットへ変換する
    // トリットとトライトの詳細については，IOTA ドキュメントポータルを参照してください：https://docs.iota.org/docs/getting-started/0.1/introduction/ternary
    if (flex_trits_from_trytes(seed, NUM_TRITS_ADDRESS, SEED, NUM_TRYTES_ADDRESS, NUM_TRYTES_ADDRESS) == 0) {
        printf("Failed to convert trytes to trits\n");
        return ret;
    }

    // 1つの新しいアドレスを取得するためのオプションを設定する
    address_opt_t opt = {.security = SECURITY_LEVEL, .start = 0, .total = 0};

    // 新しいアドレスを取得し，コンソールに出力する
    if ((ret = iota_client_get_new_address(s, seed, opt, &addresses)) == RC_OK) {
        printf("Your new address is: ");
        flex_trit_print(addresses->prev->hash, NUM_TRITS_ADDRESS);
        printf("\n");
    } else {
        printf("Failed to get a new address: Error code %s\n", error_2_string(ret));
    }
    // オブジェクトのメモリを開放する
    hash243_queue_free(&addresses);

    return ret;
    ```

指定された `start` インデックスから開始して，接続されたノードは次の操作を実行してアドレスが使用済みかどうかを確認します．
<!-- Starting from the given `start` index, the connected node checks if the address is spent by doing the following: -->

- アドレスから IOTA トークンを取り出す入力トランザクションがタングルの概観内にあるかどうかを検索する．
<!-- - Search its view of the Tangle for input transactions that withdraw from the address -->
- 使用済みアドレスのリスト内にアドレスがあるかどうかを検索する．
<!-- - Search for the address in the list of spent addresses -->

指定されたインデックスのアドレスが使用されていると，ノードは使用されていないアドレスを見つけるまでインデックスをインクリメントします．
<!-- If an address with the given index is spent, the index is incremented until the node finds one that isn't spent. -->

:::warning:
ノードでアドレス応答を生成するこの方法は，アドレスに関する有効なデータを返します．アドレスをより細かく制御するには，使用済みのアドレスをローカルデータベースに記録しておくことをお勧めします．
:::
<!-- :::warning: -->
<!-- This way of generating addresses replies on the node to return valid data about your addresses. To have more control over your addresses, we recommend keeping track of spent addresses in your own local database. -->
<!-- ::: -->

コンソールにアドレスが表示されます．
<!-- In the console, you should see an address. -->

```bash
Your address is: WKJDF9LVQCVKEIVHFAOMHISHXJSGXWBJFYEQPOQKSVGZZFLTUUPBACNQZTAKXR9TFVKBGYSNSPHRNKKHA
```

:::success:おめでとうございます:tada:
新しい未使用のアドレスが生成されました．このアドレスは，トランザクションを送信したい人と共有できます．
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just generated a new unspent address. You can share this address with anyone who wants to send you a transaction. -->
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
bazel run -c opt examples:generate_address
```

コンソールにアドレスが表示されます．
<!-- In the console, you should see an address. -->

```bash
Your address is: WKJDF9LVQCVKEIVHFAOMHISHXJSGXWBJFYEQPOQKSVGZZFLTUUPBACNQZTAKXR9TFVKBGYSNSPHRNKKHA
```

## 次のステップ
<!-- ## Next steps -->

[新しいアドレスにテスト IOTA トークンを送信する](../c/transfer-iota-tokens.md)．
<!-- [Send test IOTA tokens to your new address](../c/transfer-iota-tokens.md). -->
