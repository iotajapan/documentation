# C でマイクロペイメントを送信する
<!-- # Send a micropayment in C -->

**このガイドでは，[転送バンドル](root://getting-started/0.1/transactions/bundles.md)を[ノード](root://getting-started/0.1/network/nodes.md)に送信して，1 IOTA のマイクロペイメントを送信します．**
<!-- **In this guide, you send a micropayment of 1 IOTA by sending a [transfer bundle](root://getting-started/0.1/transactions/bundles.md) to a [node](root://getting-started/0.1/network/nodes.md).** -->

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

## 手順1. テスト IOTA トークンを取得する
<!-- ## Step 1. Get test IOTA tokens -->

テスト IOTA トークンをデブネットで送信するには，ノードはシードに属するアドレスの1つについて0より大きい残高の履歴を持っている必要があります．デブネットで使用するテスト IOTA トークンを取得するには，デブネット蛇口を使用できます．
<!-- To send test IOTA tokens on the Devnet, the nodes must have a record of a greater than 0 balance for one of the addresses that belongs to your seed. To get test IOTA tokens to use on the Devnet, you can use the Devnet faucet. -->

1\. 新しいシードを作成し，バックアップします．
<!-- 1\. Create a new seed and back it up -->

--------------------
### Linux
```bash
cat /dev/urandom |tr -dc A-Z9|head -c${1:-81}
```
---
### macOS
```bash
cat /dev/urandom |LC_ALL=C tr -dc 'A-Z9' | fold -w 81 | head -n 1
```
---
### Windows PowerShell
```bash
$b=[byte[]] (1..81);(new-object Security.Cryptography.RNGCryptoServiceProvider).GetBytes($b);-join($b|%{[char[]] (65..90+57..57)[$_%27]})
```
--------------------

2\. [シードから新しいアドレスを生成します](../c/generate-an-address.md)．
<!-- 2\. [Generate a new address for your seed](../c/generate-an-address.md) -->

3\. アドレスをコピーして[デブネット蛇口](https://faucet.devnet.iota.org)にペーストし，トークンがアドレスに転送されるのを待ちます．
<!-- 3\. Copy and paste your address into the [Devnet faucet](https://faucet.devnet.iota.org), then wait for the tokens to be transferred to your address -->

## 手順2. 転送バンドルを作成し，送信する
<!-- ## Step 2. Create and send a transfer bundle -->

あるアドレスから別のアドレスにテスト IOTA トークンを転送するには，転送バンドルを作成してノードに送信する必要があります．この転送バンドルには，アドレスから IOTA トークンを取り出すための入力トランザクションと，それらを別のアドレスに預け入れるための出力トランザクションが必要です．
<!-- To transfer your test tokens from one address to another, you need to create and send a transfer bundle. This bundle needs an input transaction to withdraw the IOTA tokens from your address and an output transaction to deposit them into another address. -->

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

2. シードを定義します．このシードを，テスト IOTA トークンを持つアドレスを所有するシードに置き換えます．
  <!-- 2. Define your seed. Replace this seed with one that owns an address with test IOTA tokens -->

    ```cpp
    static tryte_t const *const MY_SEED =
    (tryte_t *)"HFYPTJZIVDJ9TQTQPIJXRXVJVOBOXAHZYZMVQBMFQYEAITJZKPHKUKERFQNAXNVTJRAHBGFJJIZTOTGJC";
    ```

    :::info:
    このバンドルは IOTA トークンを転送するため，シードを使用してバンドルに署名します．したがって，このシードのアドレスの1つには少なくとも1つの IOTA トークンが含まれている必要があります．
    :::
    <!-- :::info: -->
    <!-- Because this bundle transfers IOTA tokens, the seed is used to sign it. Therefore, this seed's addresses must contain at least 1 IOTA token. -->
    <!-- ::: -->

3. IOTA トークンの送信先のアドレスを定義します．
  <!-- 3. Define the address to which you want to send your IOTA token -->

    ```cpp
    static tryte_t const *const RECEIVER_ADDR =
    (tryte_t *)"ZLGVEQ9JUZZWCZXLWVNTHBDX9G9KZTJP9VEERIIFHY9SIQKYBVAHIMLHXPQVE9IXFDDXNHQINXJDRPFDX";
    ```

4. 転送する IOTA トークンの量と送信先のアドレスを指定する `transfers` 配列を作成します．
  <!-- 4. Create a `transfers` array that specifies the amount of IOTA tokens you want to transfer and the address to send the tokens to -->

    ```cpp
    // アドレスに 1i を送信するインプットトランザクションオブジェクトを定義する
    transfer_t tf = {};
    // シードをトライトからトリットに変換する
    flex_trit_t seed[NUM_FLEX_TRITS_ADDRESS];
    flex_trits_from_trytes(seed, NUM_TRITS_ADDRESS, MY_SEED, NUM_TRYTES_ADDRESS, NUM_TRYTES_ADDRESS);

    // 受信者のアドレスをトライトからトリットに変換する
    flex_trits_from_trytes(tf.address, NUM_TRITS_ADDRESS, RECEIVER_ADDR, NUM_TRYTES_ADDRESS, NUM_TRYTES_ADDRESS);

    // トランザクションタグをトライトからトリットに変換する
    flex_trits_from_trytes(tf.tag, NUM_TRITS_TAG, (const tryte_t *)"CCLIENT99999999999999999999", NUM_TRYTES_TAG,
                            NUM_TRYTES_TAG);

    // 受信者のアドレスに送信する IOTA トークンの量
    tf.value = 1;

    transfer_array_add(transfers, &tf);
    ```

5. `transfers` 配列からバンドルを作成するには，[チップ選択](root://node-software/0.1/iri/concepts/tip-selection.md)，[リモートプルーフオブワーク](root://getting-started/0.1/transactions/proof-of-work.md)を処理する [`iota_client_send_transfer()`](https://github.com/iotaledger/entangled/blob/develop/cclient/api/extended/send_transfer.h) メソッドに渡し，バンドルをノードに送信します．
  <!-- 5. To create a bundle from your `transfers` array, pass it to the [`iota_client_send_transfer()`](https://github.com/iotaledger/entangled/blob/develop/cclient/api/extended/send_transfer.h) method, which handles [tip selection](root://node-software/0.1/iri/concepts/tip-selection.md), [remote proof of work](root://getting-started/0.1/transactions/proof-of-work.md), and sending the bundle to the node -->

    ```cpp
    // 転送配列からバンドルを作成し，ノードに送信する
    ret_code = iota_client_send_transfer(s, seed, SECURITY_LEVEL, TIP_SELECTION_DEPTH, MINIMUM_WEIGHT_MAGNITUDE, false, transfers, NULL, NULL, NULL, bundle);

    printf("Sending 1 i to %s\n", RECEIVER_ADDR);
    printf("Send transfer function %s\n", error_2_string(ret_code));
    if (ret_code == RC_OK) {
        flex_trit_t const *bundle_hash = bundle_transactions_bundle_hash(bundle);
        printf("Bundle hash: ");
        flex_trit_print(bundle_hash, NUM_TRITS_HASH);
        printf("\n");
    }

    // オブジェクトのメモリを開放する
    bundle_transactions_free(&bundle);
    transfer_message_free(&tf);
    transfer_array_free(transfers);
    ```

    `iota_client_send_transfer()` メソッドは，シードのアドレスの残高をチェックするようにノードに要求します．転送を完了するのに十分な IOTA トークンがアドレスにある場合，`iota_client_send_transfer()` メソッドは入力トランザクションを作成して，転送を実行するために十分な全残高をアドレスから取り出します．次に，`iota_client_send_transfer()` メソッドはこれらのトランザクションを転送バンドルに追加し，IOTA トークンが取り出されるすべてのアドレスの秘密鍵でバンドルに署名します．
    <!-- This method asks the node to check the balance of your seed's addresses. If your addresses have enough IOTA tokens to complete the transfer, the method creates input transactions to withdraw the full balance from enough of your addresses to fulfill the transfer. Then, the method adds those transactions to the transfer bundle and signs the bundle with the private keys of any withdrawn addresses. -->

    :::info:
    シードがデバイスから離れることはありません．ライブラリはアドレスを生成し，アドレスをノードに送信します．
    :::
    <!-- :::info: -->
    <!-- Your seed never leaves your device. The library generates addresses and sends them to the node. -->
    <!-- ::: -->

    転送する金額が取り出すアドレスの残高より少ない場合，メソッドは別の出力トランザクションを作成して，残りの IOTA トークンをシードに属する未使用のアドレスに転送します．
    <!-- If the amount you want to transfer is less than the balance of your withdrawn addresses, the method creates another output transaction to transfer the remainder to an unspent address that belongs to your seed. -->

    コンソールに，送信したばかりのバンドルに関する情報が表示されます．
    <!-- In the console, you should see information about the bundle that you just sent: -->

    ```bash
    Sending 1 i to RJBYLCIOUKWJVCUKZQZCPIKNBUOGRGVXHRTTE9ZFSCGTFRKELMJBDDAKEYYCLHLJDNSHQ9RTIUIDLMUOB
    Send transfer function OK
    Bundle hash: OLEIHCDXBBQNU9TAAKQGRPTJFAH9RDXIGYIKKGXVUHNLGJLARH9RGISYEHNNIWGWYNKDRUZELOGRAIILD
    Sent IOTA tokens
    ```

:::success:おめでとうございます:tada:
最初の転送バンドルを送信しました．トランザクションはタングルにアタッチされ，ネットワークの残りの部分に転送されます．これで，トランザクションが確定し，残高が更新されるのを待つだけです．
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just sent your first transfer bundle. Your transactions are attached to the Tangle and will be forwarded to the rest of the network. Now, you just need to wait until the transaction is confirmed for your balance to be updated. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

これらのコードサンプルは [GitHub](https://github.com/JakeSCahill/c-iota-workshop) でホストされています．
<!-- These code samples are hosted on [GitHub](https://github.com/JakeSCahill/c-iota-workshop). -->

開始するには，デバイスに [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) がインストールされている必要があります．
<!-- To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device. -->

C 開発環境も必要です． C クライアントライブラリを初めて使用する場合は，[スタートガイド](../../getting-started/c-quickstart.md)を完了してください．
<!-- You also need a C development environment. If this is your first time using the C client library, complete our [getting started guide](../../getting-started/c-quickstart.md). -->

コマンドラインで，次を実行します．
<!-- In the command-line, do the following: -->

```bash
git clone https://github.com/iota-community/c-iota-workshop.git
cd c-iota-workshop
bazel run -c opt examples:send_tokens
```

コンソールに，送信したばかりのバンドルに関する情報が表示されます．
<!-- In the console, you should see information about the bundle that you just sent: -->

```bash
Sending 1 i to RJBYLCIOUKWJVCUKZQZCPIKNBUOGRGVXHRTTE9ZFSCGTFRKELMJBDDAKEYYCLHLJDNSHQ9RTIUIDLMUOB
Send transfer function OK
Bundle hash: OLEIHCDXBBQNU9TAAKQGRPTJFAH9RDXIGYIKKGXVUHNLGJLARH9RGISYEHNNIWGWYNKDRUZELOGRAIILD
Sent IOTA tokens
```

## 次のステップ
<!-- ## Next steps -->

[アドレスの残高を確認する](../c/check-balance.md)．
<!-- [Check the balance of your address](../c/check-balance.md). -->

今回のシナリオでは，バンドルの作成と送信にかかる時間の間にアドレスが使用されるかどうかを事前に知ることができません．
<!-- In this scenario, you wouldn't know in advance whether the address is spent during the time it takes to create and send your bundle. -->

たとえば，オンラインショッピングをしていて，チェックアウトには QR コードがあり，IOTA トークンで支払うオプションがあったとします．この QR コードには，トリニティに自動入力されるアドレスが含まれています．
<!-- For example, you are online shopping and the checkout has a QR code that gives you the option to pay in IOTA tokens. This QR code contains an address that is auto-populated in Trinity. -->

チェックアウトを完了して転送バンドルを送信するまでに，Web サイトの所有者が QR コードのアドレスから IOTA トークンを取り出したとします．これで QR コードのアドレスは使用済みアドレスとなり，支払者はこの使用済みアドレスへ IOTA トークンを送信してしまうことになります．
<!-- During the time it takes you to complete the checkout and send your transfer bundle, the website owner withdraws IOTA tokens from the address in the QR code. Now that address is spent, and you have just sent IOTA tokens to it. -->

これを防ぐには，[アカウントモジュール](../../account-module/introduction/overview.md)を使用して，アクティブか期限切れかを指定する条件付き預け入れアドレスを作成することをお勧めします．
<!-- To help stop this from happening, we recommend using the [account module](../../account-module/introduction/overview.md) to create conditional deposit addresses that specify whether they are active or expired. -->

現時点では，アカウントモジュールは以下の言語で利用できます．
<!-- At the moment, the account module is available in the following languages: -->

- Go
- Java
- JavaScript
