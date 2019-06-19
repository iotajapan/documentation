# 無料のテストトークンを受け取る
<!-- # Receive free test tokens -->

**IOTAをテストするときは、あるアドレスから別のアドレスにIOTAトークンを送信したいと思います。Mainnet上では、IOTAトークンは取引所でトレードすることができる金銭的価値を持っています。危険を冒さずにテストトランザクションを送信するには、Devnetが使用できます。 [Devnet](../references/iota-networks.md)は、トークンが無料であること以外はMainnetとほぼ同じです。1Ki（1000i）の無料トークンをDevnet蛇口のウェブサイトから得ることができます。**
<!-- **When testing IOTA, you may want to try sending IOTA tokens from one address to another. On the Mainnet, these tokens have a monetary value that can be traded on exchanges. To send test transactions without risking anything, you can use the Devnet. The [Devnet](../references/iota-networks.md) is similar to the Mainnet, except the tokens are free. You can use the Devnet faucet website to receive 1Ki (1000) of free tokens.** -->

## 前提条件
<!-- ## Prerequisites -->

このチュートリアルを完了する前に、[新しいアドレス](root://iota-basics/0.1/how-to-guides/create-an-address.md)が必要です。
<!-- Before you complete this tutorial, you need to [a new address](root://iota-basics/0.1/how-to-guides/create-an-address.md). -->

:::danger:重要
メインネット上にIOTAトークンを所有している場合は、Devnet上で使用する新しいテストシードを作成することをお勧めします。
:::
<!-- :::danger:Important -->
<!-- If you own IOTA tokens on the Mainnet, we recommend creating a new test seed to use on the Devnet. -->
<!-- ::: -->

## アドレスに無料のテストトークンを送信する
<!-- ## Send free test tokens to your address -->

1. [Devnet蛇口](https://faucet.devnet.iota.org/)に行く。
<!-- 1. Go to [the Devnet faucet](https://faucet.devnet.iota.org/) -->

2. 入力フィールドに自分のアドレスをコピペする。
<!-- 2. Copy and paste your address into the input field -->

3. '私はロボットではありません'ボックスをクリックして、自分がロボットではないことを証明する。
<!-- 3. Prove that you're not a robot by clicking the 'I'm not a robot' box. -->

4. **Request**をクリックする。
  <!-- 4. Click **Request** -->

    :::info:
    リクエストボタンはアドレスが有効な場合のみ表示されます。有効なアドレスは、81または90[トライト](root://iota-basics/0.1/concepts/trinary.md)（有効なチェックサムを含む場合）で構成されています。
    :::
    <!-- :::info: -->
    <!-- The Request button appears only if your address is valid. A valid address consists of 81 or 90 [trytes](root://iota-basics/0.1/concepts/trinary.md)(including a valid checksum). -->
    <!-- ::: -->

5. **Check balance**をクリックする。
<!-- 5. Click **Check balance** -->

このリンクをクリックすると、Devnetタングルエクスプローラにアクセスします。このWebサイトはDevnet上のノードに接続し、無料トークンをアドレスに転送したばかりのバンドルに関する情報をノードから取得します。
<!-- This link takes you to a Devnet Tangle explorer. This website connects to nodes on the Devnet and requests information from them about the bundle that just transferred free tokens to your address. -->

バンドルが確定されると、1Ki（1000i）のテストトークンがアドレスの残高に追加されます。
<!-- When the bundle is confirmed, 1Ki (1000) tokens will be added to the balance of your address. -->

:::info:このトークンはDevnetでのみ有効です
他のIOTAネットワークは無料のテストトークンを先ほどのアドレスに転送したバンドルを受け取っても検証もしていません。それ故に、他のIOTAネットワークはアドレスの残高を更新してはいません。
:::
<!-- :::info:These tokens are valid only on the Devnet -->
<!-- Other IOTA networks have neither received nor validated the bundle that transferred the tokens to your address. As a result, they haven't updated the balance of your address. -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

[無料トークンを別のアドレスに転送](../tutorials/send-iota-tokens.md)してプロセスをテストする。
<!-- [Transfer your free tokens to another address](../tutorials/send-iota-tokens.md) to test the process. -->
