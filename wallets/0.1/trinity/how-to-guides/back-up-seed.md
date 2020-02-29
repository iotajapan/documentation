# シードをバックアップする
<!-- # Back up your seed -->

**トリニティは，デバイス上の[シード](root://getting-started/0.1/clients/seeds.md)を保護および保存します．その結果，モバイルデバイスまたはコンピューターを紛失した場合，シードは失われます．シード（および IOTA トークン）が失われないようにするには，シードをバックアップし，バックアップを安全な場所に保管する必要があります．このガイドでは，シードをバックアップする方法を学びます．**
<!-- **Trinity secures and stores your [seed](root://getting-started/0.1/clients/seeds.md) on your device. As a result, if you were to ever lose your mobile device or computer, your seed would be lost. To avoid losing your seed (and your IOTA tokens), you must back it up and keep the backup in a safe place. In this guide, you learn how to back up your seed.** -->

:::danger:
2020年2月11日、IOTA 財団は、一部のユーザーのシードと Trinity パスワードが侵害された Trinity ウォレットに対する攻撃に気付きました。[Trinity アカウントの保護](../how-to-guides/protect-trinity-account.md)に関するアドバイスを確認してください。
:::
<!-- :::danger: -->
<!-- On 11 February 2020, the IOTA Foundation became aware of an attack on the Trinity wallet, during which some users’ seeds and Trinity passwords were compromised. Please check our advice for [protecting your Trinity account](../how-to-guides/protect-trinity-account.md). -->
<!-- ::: -->

アカウントを作成すると（または[アカウント設定ページからシードにアクセスする](../how-to-guides/manage-your-account.md)と），次のいずれかの方法でシードをバックアップできます．
<!-- When you create an account (or [access your seed from the account management page](../how-to-guides/manage-your-account.md)) you can back up your seed, using one of the following options: -->

- シード金庫へ書き出す（最も安全）
<!-- - SeedVault file (most secure) -->
- 手書きで紙に保存する
<!-- - Paper copy -->
- 印刷する（最低限の安全）
<!-- - Printed copy (least secure) -->

シードをバックアップした後，トリニティは正しいシードを確実にバックアップしたことを確認するためにシードを再入力するように求めてきます．
<!-- After backing up your seed, Trinity asks you to re-enter your seed to make sure that you backed up the correct one. -->

## シードをシード金庫ファイルとしてエクスポートする
<!-- ## Export your seed as a SeedVault file -->

シードをシード金庫ファイルにエクスポートするのが最も安全な方法です（[ハードウェアウォレット](../concepts/hardware-wallet.md)を使用するのとは別として）．
<!-- Exporting your seed to a SeedVault file is the most secure option (apart from using a [hardware wallet](../concepts/hardware-wallet.md)). -->

シード金庫は，[kdbx ファイルフォーマット](https://keepass.info/help/kb/kdbx_4.html)を使用するパスワード暗号化ファイルです．このフォーマットは [KeePass](https://keepass.info/) パスワードマネージャによっても使用されています．
<!-- SeedVault is a password-encrypted file that uses the [kdbx file format](https://keepass.info/help/kb/kdbx_4.html). This format is also used by the [KeePass](https://keepass.info/) password manager. -->

シードをシード金庫または KeePass パスワードマネージャーにエクスポートして保存できます．
<!-- You can export and store your seed in SeedVault or in the KeePass password manager. -->

## シードを手書きで紙に保存する
<!-- ## Write your seed on a piece of paper -->

シードを左から右，上から下に書いてください．**シードが正しく書かれていることを確認してください**．
<!-- Write your seed from left to right, top to bottom. **Check that your seed is written correctly.** -->

必ずあなたのシードの3文字の[チェックサム](root://getting-started/0.1/clients/checksums.md)を書き留め，チェックサムをシードとは別に保存してください．このチェックサムは，正しいシードを入力したかどうかを確認できる安全メカニズムです．間違った文字を1つでも入力した場合，トリニティに表示されるチェックサムは異なります．
<!-- Make sure to write your seed's 3-letter [checksum](root://getting-started/0.1/clients/checksums.md) and keep it separate from your seed. This checksum is a safety mechanism that allows you to check whether you entered the correct seed. If you enter one wrong character, the checksum that's displayed in Trinity will be different. -->

## シードを印刷する
<!-- ## Print your seed -->

シードを印刷するのは便利ですが，安全ではありません．セキュリティがよくわからない場合は，別の方法でシードをバックアップしてください．
<!-- Although printing your seed is convenient, it can be unsafe. If you're unsure about security, then use another option to back up your seed. -->

## 割けるべきこと
<!-- ## What to avoid -->

シードを安全に保つために，次のことは絶対にしないでください．
<!-- To keep your seed safe you should never do any of the following: -->

- シードをスクリーンショットで撮影すること．
<!-- - Screenshot your seed. -->
- PDF ファイルでシードを保存すること．
<!-- - Print your seed on PDF file -->
- 公共のプリンターや公共の WiFi に接続されたプリンターでシードを印刷すること．
<!-- - Print your seed from a public printer or one that's connected to WiFi -->

:::success:
これで，シードのバックアップができました．トリニティのパスワードを忘れた場合は，シードバックアップを使用してトリニティにログインできます．
:::
<!-- :::success: -->
<!-- Now you have a backup of your seed. If you ever forget your Trinity password, you can use your seed backup to log into Trinity. -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

[トランザクションを送信する](../how-to-guides/send-a-transaction.md)．
<!-- [Send a transaction](../how-to-guides/send-a-transaction.md) -->
