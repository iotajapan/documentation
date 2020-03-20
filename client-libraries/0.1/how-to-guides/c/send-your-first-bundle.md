# C で "hello world" トランザクションを送信する
<!-- # Send a "hello world" transaction in C -->

**このガイドでは，ゼロトークントランザクションで "hello world" メッセージを送信します．ゼロトークントランザクションは，[IOTA トークン](root://getting-started/0.1/clients/token.md)を送信することなく[タングル](root://getting-started/0.1/network/the-tangle.md)にメッセージを保存するのに役立ちます．**
<!-- **In this guide, you send a "hello world" message in a zero-value transaction. These transactions are useful for storing messages on the [Tangle](root://getting-started/0.1/network/the-tangle.md) without having to send any [IOTA tokens](root://getting-started/0.1/clients/token.md).** -->

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは，以下のネットワーク設定で[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します．
<!-- In this guide, we connect to a [node](root://getting-started/0.1/network/nodes.md) on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet) with the following network settings: -->

- **[最小重量値](root://getting-started/0.1/network/minimum-weight-magnitude.md)**：9
<!-- - **[Minimum weight magnitude](root://getting-started/0.1/network/minimum-weight-magnitude.md)**: 9 -->

- **[深度](root://getting-started/0.1/transactions/depth.md)**：3
<!-- - **[Depth](root://getting-started/0.1/transactions/depth.md)**: 3 -->

これらの設定は `config.h` ファイルで定義されます．このファイルは，[入門ガイド](../../getting-started/c-quickstart.md)で作成します．
<!-- These settings are defined in a `config.h` file, which we create in the [getting started guide](../../getting-started/c-quickstart.md). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. パッケージをインポートします．
  <!-- 1. Import the packages -->

    ```cpp
    #include "cclient/api/core/core_api.h"
    #include "cclient/api/extended/extended_api.h"

    #include "common/trinary/tryte_ascii.h"
    #include <inttypes.h>
    #include "iota_client_service/config.h"
    #include "iota_client_service/client_service.h"
    ```

2. メッセージの送信先[アドレス](root://getting-started/0.1/clients/addresses.md)を定義します．
  <!-- 2. Define an [address](root://getting-started/0.1/clients/addresses.md) to which you want to send a message -->

    ```cpp
    static tryte_t const *const ADDRESS =
    (tryte_t *)"ZLGVEQ9JUZZWCZXLWVNTHBDX9G9KZTJP9VEERIIFHY9SIQKYBVAHIMLHXPQVE9IXFDDXNHQINXJDRPFDXNYVAPLZAW";
    ```

    :::info:
    このアドレスは誰のものである必要はありません．アドレスが有効であるには，81[トライト](root://getting-started/0.1/introduction/ternary.md)で構成される必要があります．
    :::
    <!-- :::info: -->
    <!-- This address does not have to belong to anyone. To be valid, the address just needs to consist of 81 [trytes](root://getting-started/0.1/introduction/ternary.md). -->
    <!-- ::: -->

3. アドレスに送信するメッセージを作成します．
  <!-- 3. Create a message that you want to send to the address -->

    ```cpp
    char message[] = "Hello world";
    ```

    :::info:
    ライブラリは[基本 ASCII 文字](https://en.wikipedia.org/wiki/ASCII#Printable_characters)のみをサポートします．その結果，アクセントやウムラウトなどの発音区別符号や日本語はサポートされず，エラーが発生します．
    :::
    <!-- :::info: -->
    <!-- The library supports only [basic ASCII characters](https://en.wikipedia.org/wiki/ASCII#Printable_characters). As a result, diacritical marks such as accents and umlauts aren't supported and result in an error. -->
    <!-- ::: -->

4. `transfers` 配列で，アドレスにメッセージを送信するゼロトークントランザクションを定義します．
  <!-- 4. In a `transfers` array, define a zero-value transaction that sends the message to the address -->

    ```cpp
    bundle_transactions_t *bundle = NULL;
    bundle_transactions_new(&bundle);
    // トランザクションデータを追加できる転送配列を作成する
    transfer_array_t *transfers = transfer_array_new();

    transfer_t tf = {};

    // アドレスをトライトからトリットへ変換する
    flex_trits_from_trytes(tf.address, NUM_TRITS_ADDRESS, ADDRESS, NUM_TRYTES_ADDRESS, NUM_TRYTES_ADDRESS);

    // ASCII メッセージをトリットへ変換し，転送オブジェクトに追加する
    transfer_message_set_string(&tf, message);

    // 転送配列に転送オブジェクトを追加する
    transfer_array_add(transfers, &tf);
    ```

    ライブラリは，トランザクションフィールドの値をトリットで扱うため，アドレスやメッセージを `transfer` オブジェクトに追加する前に，トライトをトリットに変換します．
    <!-- The library expects the values of the transaction fields in trits, so we convert the trytes to trits before adding them to the `transfer` object. -->

5. `transfers` 配列からバンドルを作成するには，[チップ選択](root://node-software/0.1/iri/concepts/tip-selection.md)，[リモートプルーフオブワーク](root://getting-started/0.1/transactions/proof-of-work.md)を処理する [`iota_client_send_transfer()`](https://github.com/iotaledger/entangled/blob/develop/cclient/api/extended/send_transfer.h) メソッドに渡し，バンドルをノードに送信します．
  <!-- 5. To create a bundle from your `transfers` array, pass it to the [`iota_client_send_transfer()`](https://github.com/iotaledger/entangled/blob/develop/cclient/api/extended/send_transfer.h) method, which handles [tip selection](root://node-software/0.1/iri/concepts/tip-selection.md), [remote proof of work](root://getting-started/0.1/transactions/proof-of-work.md), and sending the bundle to the node -->

    ```cpp
    // 転送配列からバンドルを作成し，バンドルをノードに送信する
    ret_code = iota_client_send_transfer(
            service, NULL, SECURITY_LEVEL, DEPTH,
            MINIMUM_WEIGHT_MAGNITUDE, false, transfers, NULL, NULL, NULL, bundle);

    printf("Send \'hello world\' %s\n", error_2_string(ret_code));
    if (ret_code == RC_OK) {
        flex_trit_t const *bundle_hash = bundle_transactions_bundle_hash(bundle);
        printf("Bundle hash: ");
        // コンソールにバンドルハッシュを表示する
        flex_trit_print(bundle_hash, NUM_TRITS_HASH);
        printf("\n");
    }

    // オブジェクトのメモリ領域を開放する
    bundle_transactions_free(&bundle);
    transfer_message_free(&tf);
    transfer_array_free(transfers);
    ```

    コンソールに，送信したばかりのトランザクションのバンドルハッシュが表示されます．
    <!-- In the console, you should see the bundle hash of the transaction you just sent. -->

    ```bash
    Send 'hello world' OK
    Bundle hash: BIBGYLIABG9KZRJVBPZOTIHKDFXABHUSDWXLYQGFJNZDGNVHWFXABHRNXQBDHNRWNFNSKEKKIUJJH9PWC
    Transaction was sent.
    ```

:::success:おめでとうございます:tada:
最初のゼロトークントランザクションを送信しました．トランザクションはタングルにアタッチされ，ネットワークの残りの部分に転送されます．このトランザクションはイミュータブルです．
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just sent your first zero-value transaction. Your transaction is attached to the Tangle, and will be forwarded to the rest of the network. This transaction is now immutable. -->
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
bazel run -c opt examples:send_hello
```

コンソールに，送信したばかりのトランザクションのバンドルハッシュが表示されます．
<!-- In the console, you should see the bundle hash of the transaction you just sent. -->

## 次のステップ
<!-- ## Next steps -->

[タングルからトランザクションを読み取る](../c/read-transactions.md)ことができるように，トランザクションの送信先アドレスをメモする．
<!-- Make a note of the address to which you sent the transaction so you can [read the transaction from the Tangle](../c/read-transactions.md). -->

[タングルエクスプローラー](https://utils.iota.org)などのユーティリティを使用して，トランザクションを読み取ることもできます．
<!-- You can also read your transaction, using a utility such as the [Tangle explorer](https://utils.iota.org). -->
