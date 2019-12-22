# デスクトップ版のダウンロードを検証する
<!-- # Verify your Trinity desktop download -->

**トリニティのデスクトップ版をダウンロードするとき、IOTA 財団の GitHub リポジトリから正しいものをダウンロードしたことを確認するために、トリニティのデスクトップ版の信頼性を検証することができます。このガイドでは、トリニティのデスクトップ版のダウンロードを検証する方法を学びます。**
<!-- **When you download Trinity Desktop, you may want to verify its authenticity to make sure that you downloaded the correct one from the IOTA Foundation GitHub repository. In this guide, you learn how to verify Trinity desktop downloads.** -->

トリニティデスクトップの信頼性を検証するために、SHA256ハッシュおよびコード署名を使用できます。これら2つの手順の説明は、オペレーティングシステムによって異なります。
<!-- To verify the authenticity of Trinity Desktop, you can use the SHA256 hash and code signature. Instructions for both of these steps differ, depending on your operating system. -->

## Windows
<!-- ## Windows operating system -->

### SHA256ハッシュを検証する
<!-- ### Verify the SHA256 hash -->

1. コマンドラインインターフェイスを開きます。
  <!-- 1. Open a command-line interface -->

2. トリニティデスクトップの`.exe`ファイルのSHA256ハッシュを計算します。パスをトリニティの`.exe`ファイルへのパスに置き換えます。
  <!-- 2. Create a SHA256 hash of the Trinity Desktop `.exe` file. Replace the path with the path to your Trinity `.exe` file. -->

    ```
    certUtil -hashfile path\to\trinity-desktop-version.exe SHA256
    ```

    たとえば、ファイルが`C:\Users\yourname\Downloads`ディレクトリにある場合は、次の手順を実行します。
    <!-- For example, if the file is in the `C:\Users\yourname\Downloads` directory, do the following: -->

    ```
    certUtil -hashfile C:\Users\yourname\Downloads\trinity-desktop-0.3.2.exe SHA256
    ```

3. 先ほど計算したSHA256ハッシュと[リリースノート](https://github.com/iotaledger/trinity-wallet/releases)のSHA256ハッシュを比較して、それらが一致することを確認します。
  <!-- 3. Compare your SHA256 hash with the one in the [release notes](https://github.com/iotaledger/trinity-wallet/releases) and make sure that they match -->

### コード署名を検証する
<!-- ### Verify the code signature -->

1. `trinity-desktop-version.exe`を右クリックします。
  <!-- 1. Right-click on `trinity-desktop-version.exe` -->

2. **デジタル署名** > **詳細** > **証明書の表示** に移動します。
  <!-- 2. Go to **Digital Signatures** > **Details** > **View Certificate** -->

3. `Certification Path`タブで、次の情報がパスと一致することを確認します。
  <!-- 3. In the Certification Path tab, make sure that the the following information matches the path: -->

    ```bash
    DigiCert
    DigiCert SHA2 Assured Code Signing CA
    IOTA Stiftung
    ```

4. 証明書のステータスが「この証明書は問題ありません」となっていることを確認します。
  <!-- 4. Make sure that the Certificate status reads "This certificate is OK." -->

## MacOS
<!-- ## MacOS operating system -->

### SHA256ハッシュを検証する
<!-- ### Verify the SHA256 hash -->

1. ターミナルを開きます。（`/Applications/Utilities/Terminal`にあります）
  <!-- 1. Open Terminal (in `/Applications/Utilities/Terminal`) -->

2. トリニティデスクトップの`.dmg`ファイルのSHA256ハッシュを計算します。パスをトリニティの`.dmg`ァイルへのパスに置き換えます。
  <!-- 2. Create a SHA256 hash of the Trinity Desktop `.dmg` file. Replace the path with the path to your Trinity `.dmg` file. -->

  ```bash
  shasum -a 256 /path/to/trinity-desktop-version.dmg
  ```

  たとえば、ファイルが`~/Downloads`にある場合は、次のようにします。
  <!-- For example, if the file is in `~/Downloads`, do the following: -->

  ```bash
  shasum -a 256 ~/Downloads/trinity-desktop-0.3.2.dmg
  ```

3. 先ほど計算したSHA256ハッシュと[リリースノート](https://github.com/iotaledger/trinity-wallet/releases)のSHA256ハッシュを比較して、それらが一致することを確認します。
  <!-- 3. Compare your SHA256 hash with the one in the [release notes](https://github.com/iotaledger/trinity-wallet/releases) and make sure that they match -->

### コード署名を検証する
<!-- ### Verify the code signature -->

#### 前提条件
<!-- #### Prerequisites -->

以下の指示を実行するには、[Xcodeコマンドラインツール](https://www.ics.uci.edu/~pattis/common/handouts/macmingweclipse/allexperimental/macxcodecommandlinetools.html)が必要です。
<!-- To follow these instructions you need [Xcode Command Line Tools](https://www.ics.uci.edu/~pattis/common/handouts/macmingweclipse/allexperimental/macxcodecommandlinetools.html). -->

1. ターミナルを開きます。（`/Applications/Utilities/Terminal`にあります）
  <!-- 1. Open Terminal (in `/Applications/Utilities/Terminal`) -->

2. `Trinity.app`ファイルの署名を確認します。パスを`Trinity.app`ファイルへのパスに置き換えます。このコマンドは、コードバイナリが実際に署名されているか、署名が有効であるか、すべてのシールされているコンポーネントが変更されていないか、そして署名がいくつかの基本的な整合性チェックに合格しているかどうかを確認します。
  <!-- 2. Verify the `Trinity.app` file's signature. Replace the path with the path to your `Trinity.app` file. This command confirms whether the code binaries are actually signed, the signature is valid, all the sealed components are unaltered, and the signature passes some basic consistency checks. -->

    ```bash
    codesign -d -vv /path/to/Trinity.app
    ```

    たとえば、ファイルが`/Applications`ディレクトリにある場合は、次の手順を実行します。
    <!-- For example, if the file is in the `/Applications` directory, do the following: -->

    ```bash
    codesign -d -vv /Applications/Trinity.app
    ```

3. 次の情報がコマンドの出力と一致することを確認してください。
  <!-- 3. Make sure that the following information matches the output of the command: -->

    ```bash
    Identifier=org.iota.trinity
    Authority=Developer ID Application: IOTA Stiftung (UG77RJKZHH)
    Authority=Developer ID Certification Authority
    Authority=Apple Root CA
    ```

4. システムポリシーに対して署名をテストします。パスを`Trinity.app`ファイルへのパスに置き換えます。
  <!-- 4. Test the signature against system policies. Replace the path with the path to your `Trinity.app` file. -->

    ```bash
    spctl -a -vv path/to/Trinity.app
    ```

    たとえば、ファイルが`/Applications`ディレクトリにある場合は、次の手順を実行します。
    <!-- For example, if the file is in the `/Applications` directory, do the following: -->

    ```bash
    spctl -a -vv /Applications/Trinity.app
    ```

5. 以下の情報がコマンドの出力と一致することを確認してください（トリニティが`/Applications`ディレクトリーにあると仮定しています）。
  <!-- 5. Make sure that the following information matches the output of the command (assuming Trinity is in the `/Applications` directory): -->

    ```bash
    /Applications/Trinity.app: accepted
    source=Developer ID
    origin=Developer ID Application: IOTA Stiftung (UG77RJKZHH)
    ```


## Linux
<!-- ## Linux operating system -->

### SHA256ハッシュを検証する
<!-- ### Verify the SHA256 hash -->

#### 前提条件
<!-- #### Prerequisites -->

以下の手順を実行には、ほとんどのLinuxディストリビューションに含まれている`sha256sum`パッケージが必要です。
<!-- To follow these instructions you need the `sha256sum` package, which is included with most Linux distributions. -->

1. ターミナルを開きます。
  <!-- 1. Open Terminal -->

2. トリニティデスクトップの実行可能ファイルのSHA256ハッシュを計算します。パスをトリニティ実行可能ファイルへのパスに置き換えます。
  <!-- 2. Create a SHA256 hash of the Trinity Desktop executable file. Replace the path with the path to your Trinity executable file. -->

  ```bash
  sha256sum path/to/trinity-desktop-version.AppImage
  ```

  たとえば、ファイルが`~/Downloads`にある場合は、次のようにします。
  <!-- For example, if the file is in `~/Downloads`, do the following: -->

  ```bash
  sha256sum ~/Downloads/trinity-desktop-0.3.2.AppImage
  ```

3. 先ほど計算したSHA256ハッシュと[リリースノート](https://github.com/iotaledger/trinity-wallet/releases)のSHA256ハッシュを比較して、それらが一致することを確認します。
  <!-- 3. Compare your SHA256 hash with the one in the [release notes](https://github.com/iotaledger/trinity-wallet/releases) and make sure that they match -->

### コード署名を検証する
<!-- ### Verify the code signature -->

1. [リリースノート](https://github.com/iotaledger/trinity-wallet/releases)の`Assets`セクションにある`.asc`ファイルと`.gpg`ファイルをダウンロードします。
  <!-- 1. Download the .asc and .gpg files in the Assets section of the [release notes](https://github.com/iotaledger/trinity-wallet/releases) -->

2. トリニティGPGキーをインポートします。パスをトリニティの`.gpg`ファイルへのパスに置き換えます。
  <!-- 2. Import the Trinity GPG key. Replace the path with the path to your Trinity `.gpg` file. -->

    ```bash
    gpg --import path/to/gpgfile
    ```

    たとえば、ファイルが`~/Downloads`ディレクトリにある場合は、次の手順を実行します。
    <!-- For example, if the file is in the `~/Downloads` directory, do the following: -->

    ```bash
    gpg --import ~/Downloads/iota.foundation.subkey.trinity.public.gpg
    ```

3. 次の情報がコマンドの出力と一致することを確認します。
  <!-- 3. Make sure that the following information matches the output of the command: -->

    ```bash
    gpg: key 46A440CCE5664A64: public key "IOTA Foundation (IOTA Foundation Identity) <contact@iota.org>"
    ```

4.  署名を検証します。
  <!-- 4. Verify the signature -->

    ```bash
    gpg --verify path/to/trinity-desktop-version.AppImage.asc path/to/trinity-desktop-version.AppImage
    ```

    たとえば、`.asc`ファイルと`.AppImage`ファイルが両方とも`~/Downloads`にある場合は、次の手順を実行します。
    <!-- For example, if the .asc and .AppImage files are both in `~/Downloads`, do the following: -->

    ```bash
    gpg --verify ~/Downloads/trinity-desktop-0.3.2.AppImage.asc ~/Downloads/trinity-desktop-0.3.2.AppImage
    ```

5. 次の情報がコマンドの出力と一致することを確認します。
  <!-- 5. Make sure that the following information matches the output of the command: -->

    ```bash
    gpg: Good signature from "IOTA Foundation (IOTA Foundation Identity) <contact@iota.org>"
    ```

## 次のステップ
<!-- ## Next steps -->

[トリニティアカウントを作成する](../how-to-guides/create-an-account.md)。
<!-- [Create a Trinity account](../how-to-guides/create-an-account.md). -->
