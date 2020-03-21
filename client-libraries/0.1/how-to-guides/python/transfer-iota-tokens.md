# Python でマイクロペイメントを送信する
<!-- # Send a micropayment in Python -->

**このガイドでは，[転送バンドル](root://getting-started/0.1/transactions/bundles.md)を[ノード](root://getting-started/0.1/network/nodes.md)に送信して，1 IOTA のマイクロペイメントを送信します．**
<!-- **In this guide, you send a micropayment of 1 IOTA by sending a [transfer bundle](root://getting-started/0.1/transactions/bundles.md) to a [node](root://getting-started/0.1/network/nodes.md).** -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには，次のパッケージをインストールする必要があります．
<!-- To complete this guide, you need to install the following package: -->

```bash
pip install pyota
```

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは，以下のネットワーク設定で[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します．
<!-- In this guide, we connect to a [node](root://getting-started/0.1/network/nodes.md) on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet) with the following network settings: -->

- **[最小重量値 (MWM)](root://getting-started/0.1/network/minimum-weight-magnitude.md)**：9
<!-- - **[Minimum weight magnitude](root://getting-started/0.1/network/minimum-weight-magnitude.md)**: 9 -->

- **[深度 (depth)](root://getting-started/0.1/transactions/depth.md)**：3
<!-- - **[Depth](root://getting-started/0.1/transactions/depth.md)**: 3 -->

## 手順1. テスト IOTA トークンを取得する
<!-- ## Step 1. Get test IOTA tokens -->

テスト IOTA トークンをデブネットで送信するには，ノードはシードに属するアドレスの1つにおいて0より大きい残高のレコードを持っている必要があります．デブネットで使用するテスト IOTA トークンを取得するには，デブネット蛇口を使用できます．
<!-- To send test IOTA tokens on the Devnet, the nodes must have a record of a greater than 0 balance for one of the addresses that belongs to your seed. To get test IOTA tokens to use on the Devnet, you can use the Devnet faucet. -->

1\. 新しいシードを作成し，シードをパックアップします．
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
---
### Python
```python
from iota.crypto.types import Seed
seed = Seed.random()
print(seed)
```
--------------------

2\. [シードの新しいアドレスを生成します](../python/generate-an-address.md)．
<!-- 2\. [Generate a new address for your seed](../python/generate-an-address.md) -->

3\. アドレスをコピーして[デブネット蛇口]にペーストし，テスト IOTA トークンがアドレスに転送されるのを待ちます．
<!-- 3\. Copy and paste your address into the [Devnet faucet](https://faucet.devnet.iota.org), then wait for the tokens to be transferred to your address -->

## 手順2. 転送バンドルを作成し，送信する
<!-- ## Step 2. Create and send a transfer bundle -->

あるアドレスから別のアドレスにテスト IOTA トークンを転送するには，転送バンドルを作成してノードに送信する必要があります．この転送バンドルには，アドレスから IOTA トークンを取り出すための入力トランザクションと，それらを別のアドレスに預け入れるための出力トランザクションが必要です．
<!-- To transfer your test tokens from one address to another, you need to create and send a transfer bundle. This bundle needs an input transaction to withdraw the IOTA tokens from your address and an output transaction to deposit them into another address. -->

1. パッケージをインポートします．
  <!-- 1. Import the packages -->

    ```python
    from iota import Iota
    from iota import ProposedTransaction
    from iota import Address

    ```

2. シードを定義します．このシードを，テスト IOTA トークンを持つアドレスを所有するシードに置き換えます．
  <!-- 2. Define your seed. Replace this seed with one that owns an address with test IOTA tokens -->

    ```python
    seed = 'JBN9ZRCOH9YRUGSWIQNZWAIFEZUBDUGTFPVRKXWPAUCEQQFS9NHPQLXCKZKRHVCCUZNF9CZZWKXRZVCWQ'
    ```

    :::info:
    このバンドルは IOTA トークンを転送するため，シードを使用してバンドルに署名します．したがって，このシードのアドレスの1つには少なくとも1つの IOTA トークンが含まれている必要があります．
    :::
    <!-- :::info: -->
    <!-- Because this bundle transfers IOTA tokens, the seed is used to sign it. Therefore, this seed's addresses must contain at least 1 IOTA token. -->
    <!-- ::: -->

3. ノードに接続します．
  <!-- 3. Connect to a node -->

    ```python
    api = Iota('https://nodes.devnet.iota.org:443', seed, testnet = True)
    ```

    :::info:
    `testnet` 引数は[最小重量値](root://getting-started/0.1/network/minimum-weight-magnitude.md)を9に設定します．
    :::
    <!-- :::info: -->
    <!-- The `testnet` argument sets the [minimum weight magnitude](root://getting-started/0.1/network/minimum-weight-magnitude.md) to 9. -->
    <!-- ::: -->

4. IOTA トークンを送信するアドレスを定義します．
  <!-- 4. Define the address to which you want to send your IOTA token -->

    ```python
    address = 'ZLGVEQ9JUZZWCZXLWVNTHBDX9G9KZTJP9VEERIIFHY9SIQKYBVAHIMLHXPQVE9IXFDDXNHQINXJDRPFDXNYVAPLZAW'
    ```

5. 転送する IOTA トークンの量とトークンの送信先アドレスを指定する `ProposedTransaction` オブジェクトを作成します．
  <!-- 5. Create a `ProposedTransaction` object that specifies the amount of IOTA tokens you want to transfer and the address to send the tokens to -->

    ```python
    tx = ProposedTransaction(
      address=Address(address),
      value = 1
    )
    ```

6. `ProposedTransaction` オブジェクトから転送バンドルを作成するには，[チップ選択](root://node-software/0.1/iri/concepts/tip-selection.md)，[リモートプルーフオブワーク](root://getting-started/0.1/transactions/proof-of-work.md)を処理する [`send_transfer()`](https://pyota.readthedocs.io/en/latest/api.html#send-transfer) メソッドに渡し，バンドルを[ノード](root://getting-started/0.1/network/nodes.md)に送信します ．
  <!-- 6. To create a transfer bundle from your `ProposedTransaction` object, pass it to the [`send_transfer()`](https://pyota.readthedocs.io/en/latest/api.html#send-transfer) method, which handles [tip selection](root://node-software/0.1/iri/concepts/tip-selection.md), [remote proof of work](root://getting-started/0.1/transactions/proof-of-work.md), and sending the bundle to the [node](root://getting-started/0.1/network/nodes.md) -->

    ```python
    result = api.send_transfer(transfers=[tx] )
    print('Bundle: ')
    print(result['bundle']
    ```

    このメソッドは，シードのアドレスの残高をチェックするようにノードに要求します．アドレスに転送を完了するのに十分な IOTA トークンがある場合，このメソッドは入力トランザクションを作成して，転送を実行するために十分な全残高をアドレスから取り出します．次に，ライブラリはそれらのトランザクションを転送バンドルに追加し，IOTA トークンが取り出されるアドレスの秘密鍵でバンドルに署名します．
    <!-- This method asks the node to check the balance of your seed's addresses. If your addresses have enough IOTA tokens to complete the transfer, the method creates input transactions to withdraw the full balance from enough of your addresses to fulfill the transfer. Then, the library adds those transactions to the transfer bundle and signs the bundle with the private keys of any withdrawn addresses. -->

    :::info:
    シードがデバイスから離れることはありません．

    ライブラリはアドレスを生成し，アドレスのみをノードに送信します．
    :::
    <!-- :::info: -->
    <!-- Your seed never leaves your device. -->

    <!-- The library generates addresses on your local device and sends them to the node. -->
    <!-- ::: -->

    転送する金額が IOTA トークンを取り出すアドレスの残高より少ない場合，メソッドは別の出力トランザクションを作成して，残りをシードに属する未使用アドレスに転送します．
    <!-- If the amount you want to transfer is less than the balance of your withdrawn addresses, the method creates another output transaction to transfer the remainder to an unspent address that belongs to your seed. -->

    コンソールに，送信したばかりのトランザクションのバンドルハッシュが表示されます．
    <!-- In the console, you should see the bundle hash of the transaction you just sent. -->

:::success:おめでとうございます:tada:
最初の転送バンドルを送信しました．トランザクションはタングルにアタッチされ，ネットワークの残りの部分に転送されます．これで，トランザクションが確定し，残高が更新されるのを待つだけです．
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just sent your first transfer bundle. Your transactions are attached to the Tangle and will be forwarded to the rest of the network. Now, you just need to wait until the transaction is confirmed for your balance to be updated. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

[REPL.it ツール](https://repl.it)を使用して，ブラウザーでサンプルコードを実行できます．
<!-- We use the [REPL.it tool](https://repl.it) to allow you to run sample code in the browser. -->

緑色のボタンをクリックして，このガイドのサンプルコードを実行し，ウィンドウで結果を確認できます．
<!-- Click the green button to run the sample code in this guide and see the results in the window. -->

このサンプルコードを実行する前に，シードを自分自身のテストシードに置き換えます．
<!-- Before you run this sample code, replace the seed with your own test seed. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Send-IOTA-tokens-on-the-Devnet-Python?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## 次のステップ
<!-- ## Next steps -->

[アドレスの残高を確認する](../python/check-balance.md)．
<!-- [Check the balance of your address](../python/check-balance.md). -->

[タングルエクスプローラー](https://utils.iota.org)などのユーティリティを使用して，トランザクションを読み取ることができます．検索バーにバンドルハッシュを入力するだけです．
<!-- You can also read your transaction, using a utility such as the [Tangle explorer](https://utils.iota.org). Just enter your bundle hash in the search bar. -->
