# シードをバックアップする
<!-- # Back up your seed -->

**シードは秘密のパスワードであり、シードは固有のアドレスと電子署名を生成するために使われます。トリニティはモバイル端末またはコンピュータにシードを厳重に保管します。その結果、モバイル端末やコンピュータを失うことになった場合、シードを失うことになります。シード（およびIOTAトークン）が失われないようにするには、シードをバックアップしてバックアップを安全な場所に保管する必要があります。**
<!-- **Your seed is your secret password, which is used to generate unique addresses and digital signatures. Trinity secures and stores your seed on your mobile device or computer. As a result, if you were to ever lose your mobile device or computer, your seeds would be lost. To avoid losing your seed (and your IOTA tokens), you must back it up and keep the backup in a safe place.** -->

:::danger:重要
あなたのシードへのアクセス権を持っている人は誰でもあなたのIOTAトークンへのアクセス権を持っています。あなたのシードをスクリーンショットしないでください。
:::
<!-- :::danger:Important -->
<!-- Anyone who has access to your seed has access to your IOTA tokens. Do not screenshot your seed. -->
<!-- ::: -->

アカウントを作成すると（または[アカウント設定ページからシードにアクセスする](../how-to-guides/manage-your-account.md)と）、次のいずれかの方法でシードをバックアップできます。
<!-- When you create an account (or [access your seed from the account management page](../how-to-guides/manage-your-account.md)) you can back up your seed, using one of the following options: -->

- シード金庫へ書き出す（最も安全）
<!-- - SeedVault file (most secure) -->
- 手書きで紙に保存する
<!-- - Paper copy -->
- 印刷する（最低限の安全）
<!-- - Printed copy (least secure) -->

シードをバックアップした後、トリニティは正しいシードを確実にバックアップしたことを確認するためにシードを再入力するように求めてきます。
<!-- After backing up your seed, Trinity asks you to re-enter your seed to make sure that you backed up the correct one. -->

### シードをシード金庫ファイルとしてエクスポートする
<!-- ### Export your seed as a SeedVault file -->

シードをシード金庫ファイルにエクスポートするのが最も安全な方法です（[ハードウェアウォレット](../concepts/hardware-wallet.md)を使用するのとは別として）。
<!-- Exporting your seed to a SeedVault file is the most secure option (apart from using a [hardware wallet](../concepts/hardware-wallet.md)). -->

シード金庫は、[kdbxファイルフォーマット](https://keepass.info/help/kb/kdbx_4.html)を使用するパスワード暗号化ファイルです。このフォーマットは[KeePass](https://keepass.info/)パスワードマネージャによっても使用されています。
<!-- SeedVault is a password-encrypted file that uses the [kdbx file format](https://keepass.info/help/kb/kdbx_4.html). This format is also used by the [KeePass](https://keepass.info/) password manager. -->

シードをシード金庫またはKeePassパスワードマネージャにエクスポートして保存し、必要に応じてシード金庫を使ってトリニティにログインすることができます。
<!-- You can export and store your seed in SeedVault or in the KeePass password manager, and use it to log into Trinity when necessary. -->

### シードを手書きで紙に保存する
<!-- ### Write your seed on a piece of paper -->

シードを左から右、上から下に書いてください。**シードが正しく書かれていることを確認してください**。
<!-- Write your seed from left to right, top to bottom. **Check that your seed is written correctly.** -->

必ずあなたのシードの3文字のチェックサムを書き留め、チェックサムをシードとは別に保存してください。このチェックサムは、正しいシードを入力したかどうかを確認できる安全メカニズムです。間違った文字を1つでも入力した場合、トリニティに表示されるチェックサムは異なります。
<!-- Make sure to write your seed's 3-letter checksum and keep it separate from your seed. This checksum is a safety mechanism that allows you to check whether you entered the correct seed. If you enter one wrong character, the checksum that's displayed in Trinity will be different. -->

### シードを印刷する
<!-- ### Print your seed -->

シードを印刷するのは便利ですが、安全ではありません。セキュリティがよくわからない場合は、別の方法でシードをバックアップしてください。
<!-- Although printing your seed is convenient, it can be unsafe. If you're unsure about security, then use another option to back up your seed. -->

:::danger:重要
シードをPDFファイルで絶対に印刷しないでください。

公共のプリンターやWiFiに接続されているプリンターからシードを絶対に印刷しないでください。
:::
<!-- :::danger:Important -->
<!-- Never print your seed on PDF file. -->
<!--  -->
<!-- Never print your seed from a public printer or one that's connected to WiFi. -->
<!-- ::: -->
