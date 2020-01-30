# シードを作成する
<!-- # Create a seed -->

**[アドレス](root://getting-started/0.1/clients/addresses.md)のメッセージや [IOTA トークン](root://getting-started/0.1/clients/token.md)の所有権を証明するには，[シード](root://getting-started/0.1/clients/seeds.md)が必要です．このガイドでは，自分自身のシードを作成します．**
<!-- **You need a [seed](root://getting-started/0.1/clients/seeds.md) to prove your ownership of either messages and/or any [IOTA tokens](root://getting-started/0.1/clients/token.md) on your [addresses](root://getting-started/0.1/clients/addresses.md). In this guide, you create your own unique seed.** -->

シードを作成するには，[トリニティでアカウントを作成](root://wallets/0.1/trinity/how-to-guides/create-an-account.md)するか，以下のオプションのいずれかを使用できます．
<!-- To create a seed, you can either [create an account in Trinity](root://wallets/0.1/trinity/how-to-guides/create-an-account.md), or you can use one of the following options: -->

- コマンドラインで作成する
<!-- - Use the command line -->
- KeePass で作成する
<!-- - Use KeePass -->

## コマンドラインで作成する
<!-- ## Use the command line -->

[Windows の PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/getting-started/getting-started-with-windows-powershell?view=powershell-6)，[Linux ターミナル](https://www.howtogeek.com/140679/beginner-geek-how-to-start-using-the-linux-terminal/)，[macOS のターミナル](https://macpaw.com/how-to/use-terminal-on-mac)などのコマンドラインインターフェイスは，シードのランダムな文字列を生成するツールを提供します．
<!-- Command-line interfaces, such as [PowerShell in Windows](https://docs.microsoft.com/en-us/powershell/scripting/getting-started/getting-started-with-windows-powershell?view=powershell-6), the [Linux Terminal](https://www.howtogeek.com/140679/beginner-geek-how-to-start-using-the-linux-terminal/) or [Terminal for macOS](https://macpaw.com/how-to/use-terminal-on-mac), offer tools for generating random characters for seeds. -->

--------------------
### Linux
1\. コマンドラインインターフェイスで次の操作を行います．

```bash
cat /dev/urandom |tr -dc A-Z9|head -c${1:-81}
```

2\. シードをコピーしてファイルにペーストし，安全な場所に保存します．
---
### macOS
1\. コマンドラインインターフェイスで次の操作を行います．

```bash
cat /dev/urandom |LC_ALL=C tr -dc 'A-Z9' | fold -w 81 | head -n 1
```

2\. シードをコピーしてファイルにペーストし，安全な場所に保存します．
---
### Windows
1\. Powershell で次の操作を行います．

```bash
$b=[byte[]] (1..81);(new-object Security.Cryptography.RNGCryptoServiceProvider).GetBytes($b);-join($b|%{[char[]] (65..90+57..57)[$_%27]})
```

2\. シードをコピーしてファイルにペーストし，安全な場所に保存します．
--------------------

## KeePass で作成する
<!-- ## Use KeePass -->

KeePass は，暗号化されたデータベースにパスワードを保存するパスワードマネージャーで，1つのマスターパスワードまたはキーファイルでロックを解除できます．
<!-- KeePass is a password manager that stores passwords in encrypted databases, which can be unlocked with one master password or key file. -->

以下の手順は Windows 用ですが，Linux または macOS で KeePass を使用することもできます．
<!-- These instructions are for Windows, but you can also use KeePass on Linux or macOS. -->

1. [KeePass インストーラーをダウンロードします](https://keepass.info/)．
<!-- 1. [Download the KeePass installer](https://keepass.info/) -->

2. インストーラーを開き，画面の指示に従います．
<!-- 2. Open the installer and follow the on-screen instructions -->

3. KeePass を開き，**New** をクリックします．
  <!-- 3. Open KeePass and click **New** -->

    ![Creating a new KeePass database](../images/keypass-new.png)

4. 指示に従って KeePass ファイルをコンピューターに保存したら，空のスペースを右クリックして **Add entry** をクリックします．
  <!-- 4. After you've followed the instructions and saved the KeePass file on your computer, right click the empty space and click **Add entry** -->

    ![Adding a new KeePass entry](../images/keepass-add-entry.png)

5. **Generate a password** をクリックします．
  <!-- 5. Click **Generate a password** -->

    ![Selecting the KeePass password generator](../images/keypass-password-generator.png)

6. 次のオプションのみを選択し，**OK** をクリックします．
  <!-- 6. Select only the following options and click **OK**: -->

- Length of generated password: 81
- Upper-case (A, B, C, ...)
- Also include the following characters: 9

7. **OK** をクリックし，シードを保存します．
<!-- 7\. Click **OK** to save your seed -->

## シードを保存するためのアドバイス
<!-- ## Advice for storing seeds -->

紙やファイルなど，さまざまな方法でシードを保存できます．ただし，100％安全なストレージオプションはないため，リスクを考慮し，可能な限り軽減する必要があります．
<!-- You can store your seed in many ways, such as on paper or in a file. But, no storage option is 100% safe, so you should consider the risks and mitigate them as much as possible. -->

### 安全に保存するためにすべきこと
<!-- ### What should you do? -->

以下のリストは，シードを安全に保存するためのアドバイスを提供します．
<!-- This list offers you some advice for storing your seed: -->

- シードを保管する物理的な場所が，火災，洪水，盗難，その他の物理的なリスクから保護されていることを確認する．
<!-- - Make sure that the physical location in which you store your seed is protected from fires, floods, theft, and other physical risks. -->

- シードのコピーを銀行の金庫または金庫に保管する．
<!-- - Keep a copy of your seed in a bank vault or safe deposit box -->

- パスワードマネージャー（KeePassなど）またはパスフレーズやキーファイルで保護された仮想金庫を使用する．
<!-- - Use a password manager (such as KeePass) or virtual vault that is protected by a passphrase and/or a key file -->

- 暗号化されたディスクまたは NAS を使用して，シードまたはパスワードデータベースを保存する．
<!-- - Use an encrypted disk or NAS to store your seed or password database -->

### 安全に保存するためにしてはいけないこと
<!-- ### What should you not do? -->

以下のリストはあなたのシードを失うことを避けるいくつかの方法を提供します．
<!-- This list offers you some ways to avoid losing your seed: -->

- シードを公共のプリンターで印刷しない．
<!-- - Do not print your seed with a public printer -->

- USB ドライブや外部ハードディスクなどの保護されていないデバイスにシードを放置しない．
<!-- - Do not leave your seed on unprotected devices such as USB drives or external hard disks -->

- シードをパブリッククラウドサービスに保存しない．
<!-- - Do not store your seed on a public cloud service -->

- あなたのシードを誰とも共有しない．
<!-- - Do not share your seed with anyone -->

## 次のステップ
<!-- ## Next steps -->

JavaScript クライアントライブラリを使用して，[シードの未使用アドレスを生成します](root://client-libraries/0.1/how-to-guides/js/generate-an-address.md)．
<!-- Use the JavaScript client library to [generate an unspent address for your seed](root://client-libraries/0.1/how-to-guides/js/generate-an-address.md). -->
