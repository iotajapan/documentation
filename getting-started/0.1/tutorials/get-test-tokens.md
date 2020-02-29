# テスト IOTA トークンを取得する
<!-- # Get test IOTA tokens -->

**IOTA をテストする場合，[IOTA トークン](../clients/token.md)をある[アドレス](root://getting-started/0.1/clients/addresses.md)から別の[アドレス](root://getting-started/0.1/clients/addresses.md)に送信できます．メインネットでは，IOTA トークンは取引所で取引できる金銭的価値を持っています．テスト[トランザクション](../transactions/transactions.md)を送信するには，無料のテストトークンを使用する[デブネット](../network/iota-networks.md)を使用できます．**
<!-- **When testing IOTA, you may want to send [IOTA tokens](../clients/token.md) from one [address](root://getting-started/0.1/clients/addresses.md) to another. On the Mainnet, these tokens have a monetary value that can be traded on exchanges. To send test [transactions](../transactions/transactions.md), you can use the [Devnet](../network/iota-networks.md), which uses free test tokens.** -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完了するには，未使用のアドレスが必要です．
<!-- To complete this guide, you need an unspent address. -->

<<<<<<< HEAD
:::danger:重要！
メインネットで IOTA トークンを所有している場合は，デブネットで使用する[新しいシードの作成](../tutorials/create-a-seed.md)をお勧めします．
=======
You can generate an unspent address, using any of the following options:

- [C client library](root://client-libraries/0.1/how-to-guides/c/generate-an-address.md)
- [Go client library](root://client-libraries/0.1/how-to-guides/go/generate-an-address.md)
- [Java client library](root://client-libraries/0.1/how-to-guides/java/generate-an-address.md)
- [JavaScript client library](root://client-libraries/0.1/how-to-guides/js/generate-an-address.md)
- [Python client library](root://client-libraries/0.1/how-to-guides/python/generate-an-address.md)
- [Trinity wallet](root://wallets/0.1/trinity/how-to-guides/receive-a-transaction.md)

:::danger:Important
If you own IOTA tokens on the Mainnet, we recommend [creating a new seed](../tutorials/create-a-seed.md) to use on the Devnet.
>>>>>>> upstream/develop
:::
<!-- :::danger:Important -->
<!-- If you own IOTA tokens on the Mainnet, we recommend [creating a new seed](../tutorials/create-a-seed.md) to use on the Devnet. -->
<!-- ::: -->

---

<<<<<<< HEAD
いくつかのテストトークンを取得するには，1 Ki のひと塊でトークンを配布するデブネット蛇口 Web サイトからいくつかを要求する必要があります．
<!-- To get some test tokens, you need to request some from the Devnet faucet website, which distributes tokens in batches of 1 Ki. -->
=======
To get some test tokens, you need to request some from a Devnet faucet website, which distributes tokens in batches of 1 Ki.
>>>>>>> upstream/develop

1. [デブネット蛇口](https://faucet.devnet.iota.org/)へアクセスします．
<!-- 1. Go to [the Devnet faucet](https://faucet.devnet.iota.org/) -->

2. アドレスをコピーし，入力フィールドにペーストします．
<!-- 2. Copy and paste your address into the input field -->

3. [reCAPTCHA](https://en.wikipedia.org/wiki/ReCAPTCHA) を完了します．
<!-- 3. Complete the [reCAPTCHA](https://en.wikipedia.org/wiki/ReCAPTCHA) -->

4. **Request** をクリックします．
  <!-- 4. Click **Request** -->

    :::info:
<<<<<<< HEAD
    リクエストボタンは，アドレスが有効な場合にのみ表示されます．
=======
    The Request button appears only if your address is valid (81 or 90 trytes long).
>>>>>>> upstream/develop
    :::
    <!-- :::info: -->
    <!-- The Request button appears only if your address is valid. -->
    <!-- ::: -->

5. **Check balance** をクリックします．
<!-- 5. Click **Check balance** -->

このリンクをクリックすると，デブネットタングルエクスプローラーが表示され，デブネット上のノードに接続し，無料トークンをアドレスに転送したばかりのバンドルに関する情報を要求します． バンドルが確定されると，アドレスの残高に1 Ki が追加されます．
<!-- This link takes you to a Devnet Tangle explorer, which connects to nodes on the Devnet and requests information from them about the bundle that just transferred the free tokens to your address. When the bundle is confirmed, 1 Ki will be added to the balance of your address. -->

:::info:テストトークンはデブネットでのみ有効です．
デブネット蛇口から取得したテストトークンは，デブネットノードに接続している場合にのみ使用できます．他の IOTA ネットワークでは，トークンをあなたのアドレスに転送したバンドルを受信も検証もしていません．その結果，他のネットワークに属するノードはあなたのアドレスの残高を更新していません．
:::
<!-- :::info:These tokens are valid only on the Devnet -->
<!-- You can use these tokens only when you're connected to a Devnet node. Other IOTA networks have neither received nor validated the bundle that transferred the tokens to your address. As a result, they haven't updated the balance of your address. -->
<!-- ::: -->

<<<<<<< HEAD
## 次のステップ
<!-- ## Next steps -->
=======
## Troubleshooting

If the Devnet faucet is not working, feel free to try the [community-built faucet](https://faucet.einfachiota.de/).

## Next steps
>>>>>>> upstream/develop

トリニティまたはクライアントライブラリのいずれかを使用して，今回取得したテストトークンをデブネットで転送できます．
<!-- You can transfer these tokens on the Devnet, using Trinity or one of the client libraries: -->

- [トリニティでテスト IOTA トークンを転送する](root://wallets/0.1/trinity/how-to-guides/send-a-transaction.md)．
<!-- - [Transfer your test tokens with Trinity](root://wallets/0.1/trinity/how-to-guides/send-a-transaction.md) -->

- [JavaScript クライアントライブラリを使って，テスト IOTA トークンを転送する](root://client-libraries/0.1/how-to-guides/js/transfer-iota-tokens.md)．
<!-- - [Transfer your test tokens with the JavaScript client library](root://client-libraries/0.1/how-to-guides/js/transfer-iota-tokens.md) -->
