# ハードウェアウォレット
<!-- # Hardware wallet -->

**ハードウェアウォレットは、コールドストレージとも呼ばれ、資金を保護するための物理的な電子デバイスです。**
<!-- **A hardware wallet, sometimes referred to as cold storage, is a physical electronic device that secures your funds.** -->

ハードウェアウォレットは次のことを行います。
<!-- Hardware wallets do the following: -->
- シードを作成して保管します。
<!-- - Creates and stores seeds -->
- バリュートランザクションに署名します。
<!-- - Signs value transactions -->

ハードウェアウォレットは、セキュリティの追加層です。[ハードウェアウォレットでトリニティアカウントを作成した](https://trinity.iota.org/hardware)場合、ハードウェアウォレットがないとトランザクションを送信できません。
<!-- Hardware wallets are an extra layer of security. If you [create a Trinity account with a hardware wallet](https://trinity.iota.org/hardware), you can't send transactions without it. -->

## シード作成とシード保管
<!-- ## Seed creation and seed storage -->

シードは、ランダムに生成された一連の単語を使用して作成されます。シードが作成された後は、デバイスをコンピュータに接続してもシードはデバイスから離れません。
<!-- Seeds are created using a sequence of randomly generated words. After a seed has been created, it will never leave the device, even when you connect it to your computer. -->

ハードウェアウォレットを設定すると、ランダムに生成された単語を書き留めて失うことがないように指示されます。これらの単語はあなたのデバイスへの鍵です。
<!-- When you set up a hardware wallet, it will give you instructions to both write down and never lose your randomly generated words. These words are the key to your device. -->

ハードウェアウォレットを紛失した場合でも、ランダムに生成された単語を別のハードウェアウォレットに入力して、資金にアクセスすることができます。
<!-- If you ever lose a hardware wallet, you can still access your funds by entering your randomly generated words in another hardware wallet. -->

## トランザクション署名
<!-- ## Transaction signing -->

バリュートランザクションを送信するときは、正しい秘密鍵を使用して作成された有効な署名が含まれている必要があります。
<!-- When you send a value transaction, it must contain a valid signature that was created using the correct private key. -->

バンドルはデバイス上で署名されます。秘密鍵がハードウェアウォレットを出ることは決してありません。故に攻撃者は秘密鍵がないのであなたのIOTAトークンを盗むことは不可能です。
<!-- Bundles are signed on the device. Private keys never leave a hardware wallet, so it's impossible for an attacker to steal your IOTA tokens without it. -->

## サポートしているハードウェアウォレット
<!-- ## Supported hardware wallets -->

現時点では、トリニティは[Ledger Nano S](https://www.ledger.com/products/ledger-nano-s)と[Ledger Blue](https://www.ledger.com/products/ledger-blue)のみをサポートしています。
<!-- At the moment, Trinity supports only the [Ledger Nano S](https://www.ledger.com/products/ledger-nano-s) and the [Ledger Blue](https://www.ledger.com/products/ledger-blue). -->

## 制限事項
<!-- ## Limitations -->

トリニティでLedger Nano Sを使用すると、同じトランザクション内で2つ以上のアドレスから取り出すことはできません。
<!-- If you use a Ledger Nano S with Trinity, you can't withdraw from more than two addresses in the same transaction. -->

アドレスの残高を確認するには、**アカウント管理** > **アドレスの表示**をクリックします。
<!-- To check the balances of your addresses, go to **Account management** > **View addresses**. -->

## シナリオ例
<!-- ### Example scenario -->

- **アドレス0：** 200 Mi
<!-- - **Address 0:** 200 Mi -->
- **アドレス1：** 400 Mi
<!-- - **Address 1:** 400 Mi -->
- **アドレス3：** 50 Mi
<!-- - **Address 3:** 50 Mi -->

Ledger Nano Sを使用して、トリニティから650 Miを送信しようとします。このトランザクションでは、3つのアドレスから取り出すバンドルが必要になるため、有効ではないので送信されません。
<!-- You try to send 650 Mi from Trinity, using a Ledger Nano S. This transaction would require a bundle that withdraws from three addresses, so it won't be valid and won't send. -->
