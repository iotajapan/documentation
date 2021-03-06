# トリニティ上で機能を開発する
<!-- # Develop features on Trinity -->

**アプリケーションだけでなく，トリニティは新しい機能を開発できるオープンソースプラットフォームです．このガイドでは，トリニティ用の自分自身の機能を開発する方法を学びます．**
<!-- **As well as an application, Trinity is an open-source platform on which you can develop new features. In this guide, you learn how to develop your own features for Trinity.** -->

:::danger:
2020年2月11日，IOTA 財団は，一部のユーザーのシードと Trinity パスワードが侵害された Trinity ウォレットに対する攻撃に気付きました．[Trinity アカウントの保護](../how-to-guides/protect-trinity-account.md)に関するアドバイスを確認してください．
:::
<!-- :::danger: -->
<!-- On 11 February 2020, the IOTA Foundation became aware of an attack on the Trinity wallet, during which some users’ seeds and Trinity passwords were compromised. Please check our advice for [protecting your Trinity account](../how-to-guides/protect-trinity-account.md). -->
<!-- ::: -->

トリニティ開発を始めるには，次のようにします．
<!-- To get started with Trinity development, do the following: -->

1. 共有依存関係をインストールします．
<!-- 1. Install the shared dependencies -->
2. デスクトップ開発環境またはモバイル開発環境をインストールします．
<!-- 2. Install the desktop development environment or the mobile development environment -->
3. コードに貢献します．
<!-- 3. Contribute to the code -->
4. プルリクエストを送信します．
<!-- 4. Submit a pull request -->

トリニティデスクトップは React で書かれて Electron 上にビルドされていますが，トリニティモバイルは React Native で書かれています．
<!-- Trinity desktop is written in React and built on Electron, whereas Trinity mobile is written in React Native. -->

開発者は [GitHub issues](https://github.com/iotaledger/trinity-wallet/issues) に貢献することをお勧めします．
<!-- Developers are encouraged to contribute to the [GitHub issues](https://github.com/iotaledger/trinity-wallet/issues). -->

## 前提条件
<!-- ## Prerequisites -->

トリニティで開発するには，コンピュータに以下のものが必要です．
<!-- To develop on Trinity, your computer must have the following: -->
- [Node.js 10 or 11](https://nodejs.org/dist/)
- [Yarn](https://yarnpkg.com/lang/en/docs/install/)
- [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

:::info:
これらのプログラムをインストールした後にデバイスを再起動してください．
:::
<!-- :::info: -->
<!-- Restart your device after installing these programs. -->
<!-- ::: -->

## 共有依存関係をインストールする
<!-- ## Install the shared dependencies -->

1. `trinity-wallet` のリポジトリをクローンします．
  <!-- 1. Clone the `trinity-wallet` repository -->

    ```bash
    git clone https://github.com/iotaledger/trinity-wallet.git
    ```

2. `trinity-wallet` ディレクトリに移動します．
  <!-- 2. Change into the `trinity-wallet` directory -->

    ```bash
    cd trinity-wallet
    ```

3. 共有依存関係をインストールします．
  <!-- 3. Install the shared dependencies -->

    ```bash
    yarn deps:shared
    ```

次に，新しい機能を構築するプラットフォームに応じて，デスクトップまたはモバイル開発環境のいずれかをインストールします．
<!-- Now, depending on which platform you want to build your new feature, install either the desktop or mobile development environment. -->

## デスクトップ開発環境をインストールする
<!-- ## Install the desktop development environment -->

1\. Windows または Linux オペレーティングシステムを使用している場合は，追加のツールとパッケージをインストールする必要があります．
<!-- 1\. If you're using a Windows or Linux operating system, you need to install some extra tools and packages. -->

--------------------
### Linux

```bash
sudo apt install build-essential libudev-dev libusb-1.0-0 libusb-1.0-0-dev
sudo apt install gcc-4.8 g++-4.8 && export CXX=g++-4.8
```
---
### Fedora

```bash
yum install build-essential libudev-dev libusb-1.0-0 libusb-1.0-0-dev libusbx-devel gcc-4.8 g++-4.8 && export CXX=g++-4.8
```
---
### Windows Vista と Windows 7

1. [.NET Framework 4.5.1](https://www.microsoft.com/en-us/download/details.aspx?id=4077) をインストールします．

2. Visual C++ ビルドツールと Python 2.7 をインストールします．

    ```bash
    npm install --global windows-build-tools
    ```

3. OpenSSL VC++ 静的64ビットライブラリをインストールします．

    ```bash
    git clone https://github.com/Microsoft/vcpkg C:\src\vcpkg
    cd C:\src\vcpkg
    .\bootstrap-vcpkg.bat
    .\vcpkg install openssl:x64-windows-static
    ```
---
### その他の Windows バージョン

1. Visual C++ ビルドツールと Python 2.7 をインストールします．

    ```bash
    npm install --global windows-build-tools
    ```

2. OpenSSL VC++ 静的64ビットライブラリをインストールします．

    ```bash
    git clone https://github.com/Microsoft/vcpkg C:\src\vcpkg
    cd C:\src\vcpkg
    .\bootstrap-vcpkg.bat
    .\vcpkg install openssl:x64-windows-static
    ```
--------------------

2\. デスクトップの依存関係をインストールします．
  <!-- 2\. Install the desktop dependencies -->

```bash
npm run deps:desktop
```

3\. `desktop` ディレクトリに移動します．
  <!-- 3\. Change into the `desktop` directory -->

```bash
cd src/desktop
```

4\.. トリニティデスクトップをビルドします．
  <!-- 4\. Build Trinity desktop -->

```bash
npm run build
```

5\. これで，実行可能ファイルをコンパイルすることも，開発モードでトリニティを実行することもできます．
  <!-- 5\. Now, you can either compile an executable file, or run Trinity in development mode -->

- 実行可能ファイルが必要な場合は，トリニティをコンパイルします．
  <!-- - If you want an executable file, compile Trinity -->

    ```bash
    npm run compile:mac
    ```

    :::info:
    `mac` の部分をお使いのオペレーティングシステムに変更してください：`mac`，`win`，または`linux`．
    :::
    <!-- :::info: -->
    <!-- Change `mac` to your operating system: `mac`, `win`, or `linux`. -->
    <!-- ::: -->

    トリニティがコンパイルされると，実行可能ファイルとインストールファイルは `src/desktop/out/` ディレクトリに配置されます．
    <!-- When Trinity is compiled, the executable file and the installation files will be located in the `src/desktop/out/` directory. -->

- 開発モードでトリニティを実行したい場合は，アプリを起動します．
  <!-- - If you want to run Trinity in development mode, start the app -->

    ```bash
    npm start
    ```

    ビルドの準備が整うと，トリニティが開きます．
    <!-- Trinity will open when the build is ready. -->

## モバイル開発環境をインストールする
<!-- ## Install the mobile development environment -->

1. [React Native の依存関係をインストールします](https://facebook.github.io/react-native/docs/getting-started.html#installing-dependencies-2)．
  <!-- 1. [Install the React Native dependencies](https://facebook.github.io/react-native/docs/getting-started.html#installing-dependencies-2) -->

2. iOS オペレーティングシステムをターゲットにしている場合は，[CocoaPod をインストールします](https://cocoapods.org/#install)．
  <!-- 2. If you are targeting the iOS operating system, [install CocoaPods](https://cocoapods.org/#install) -->

3. iOS オペレーティングシステムをターゲットとし，Xcode 10を使用している場合，[レガシービルドシステムを有効にします](https://stackoverflow.com/questions/51205221/how-can-i-use-the-legacy-build-system-with-xcode-10s-xcodebuild)．
  <!-- 3. If you are targeting the iOS operating system and you are using Xcode 10+, [enable the legacy build system](https://stackoverflow.com/questions/51205221/how-can-i-use-the-legacy-build-system-with-xcode-10s-xcodebuild) -->

4. モバイルの依存関係をインストールします．
  <!-- 4. Install the mobile dependencies -->
    ```bash
    yarn deps:mobile
    ```

5. `mobile` ディレクトリに移動します．
  <!-- 5. Change into the `mobile` directory -->
    ```bash
    cd src/mobile
    ```

これで，トリニティとロガーを実行できます．
<!-- Now, you can run Trinity and the logger. -->

--------------------
### iOS
1. CocoaPods で追加の依存関係をインストールします．

    ```bash
    cd ios && pod install && cd ..
    ```

2. トリニティを実行します．

    ```bash
    yarn ios:dev
    ```

3. ロガーを実行します．

    ```bash
    yarn log:ios
    ```
---
### Android
1. アプリケーションを実行します．

    ```bash
    yarn android:dev
    ```

2. ロガーを実行します．

    ```bash
    yarn log:android
    ```
--------------------

## コードに貢献する
<!-- ## Contribute to the code -->

1. `develop` ブランチから新しい Git ブランチを作ります．
  <!-- 1. Create a new Git branch from the `develop` branch -->

    ```bash
    git checkout -b feat/my-awesome-new-feature
    ```

:::info:
あなたのブランチに名前をつけるために接頭辞を使ってください（例えば，`feat/my-awesome-new-feature` または `bug/something-not-working`）．
:::
<!-- :::info: -->
<!-- Use a prefix to name your branches (for example, `feat/my-awesome-new-feature` or `bug/something-not-working`). -->
<!-- ::: -->

このブランチにすべての変更を加えます．
<!-- Make all your changes to this branch. -->

### 新しいテーマを作成する
<!-- ### Create a new theme -->

テーマは，モバイルおよびデスクトップのウォレットのUIで使用される配色で構成されています．すべてのテーマは，`/src/shared/themes/` ディレクトリ内の別々の JavaScript ファイルにあります．
<!-- A theme consists of a color scheme used by the UI of the mobile and desktop wallets. All themes are located in separate JavaScript files in the `/src/shared/themes/` directory. -->

1. `themes` ディレクトリに新しい JavaScript ファイルを作成し，そのファイルに `Classic.js` ファイルの内容をコピーします．
<!-- 1. In the `themes` directory, create a new JavaScript file and copy the contents of the Classic.js file into it -->

2. 作成するテーマに合わせて色の値を変更してファイルを保存します．
<!-- 2. Change the color values to suit your theme and save the file -->

3. `themes.js` ファイル内で，作成したテーマをインポートして `themes` 定数に追加します．
<!-- 3. In the `themes.js` file, import your theme and add it to the `themes` constant -->

4. `src/shared/locales/en/translation.json` に移動し，作成したテーマの名前を `themes` オブジェクトに追加します．このステップを見逃した場合，作成したテーマの名前はトリニティでは正しくありません．
<!-- 4. Go to `src/shared/locales/en/translation.json`, and add the name of your theme to the `themes` object. If you miss this step, the name of your theme will not be correct in Trinity. -->

### 新しい文字列をローカライズする
<!-- ### Localize new strings -->

テキストの追加または変更を含む貢献をする場合は，以下のローカライズ指示に従ってください．
<!-- If you are making a contribution that includes adding or changing text, follow these localization instructions. -->

1. `translate` 高階コンポーネント（HOC）をインポートします．
  <!-- 1. Import the `translate` higher order component (HOC) -->

    ```javascript
    import { withNamespaces } from ‘react-i18next’;
    ```

2. `t` 定数を作成し，それを `props` オブジェクトに設定します
  <!-- 2. Create a `t` constant, and set it to the `props` object -->

    ```javascript
    const { t } = this.props;
    ```

3. キーの翻訳を取得するように i18next に伝えます（キーに適切な名前を付けます）．
  <!-- 3. Tell i18next to get the translations for your keys (give the key an appropriate name). -->

    ```javascript
    <Text>{t(‘helloWorld’)}</Text>
    ```

4. コンポーネントをラップします．
  <!-- 4. Wrap the component -->

    ```javascript
    export withNamespaces(‘myContainer’)(MyContainer);
    ```

5. キーの翻訳を `src/shared/locales/en/` ディレクトリの `translation.json` ファイルに追加します．
  <!-- 5. Add the translations for your keys to the translation.json file in the `src/shared/locales/en/` directory -->

    ```json
    "myContainer":{
        "helloWorld": "Hello world!"
    }
    ```

プルリクエストが `develop` ブランチにマージされると，[Crowdin](https://crowdin.com/project/iota-trinity-wallet) にあなたの文字列が表示されます．コミュニティは Crowdin であなたの文字列を翻訳することによってトリニティに貢献することができます．
<!-- Your strings will be shown on [Crowdin](https://crowdin.com/project/iota-trinity-wallet) when we merge your pull request into the `develop` branch. The community can contribute to Trinity by translating your strings on Crowdin. -->

### プルリクエストを送信する
<!-- ### Submit a pull request -->

変更を加えたら，GitHub で新しいプルリクエストを送信します．
<!-- After you've made your changes, submit a new pull request on GitHub. -->

あなたのブランチをソースブランチとして使用し，`develop` ブランチをターゲットブランチとして使用します．
<!-- Use your branch as the source branch and use the `develop` branch as the target branch. -->

## トラブルシューティング
<!-- ## Troubleshooting -->

開発モードでトリニティを実行している間以下の問題のいくつかを見つけるかもしれません．
<!-- You may find some of these issues while running Trinity in development mode. -->

### バージョンアップ後にトリニティが起動しない
<!-- ### Trinity does not start after a version update -->

1. 場合によっては，IOTA 財団は依存関係を更新して構成を変更します．`trinity-wallet` ディレクトリで `npm install` を実行して，依存関係を再インストールしてください．次に，`npm run build` を実行して，ウォレットをもう一度ビルドします．
  <!-- 1. Sometimes, we update the dependencies and change the configurations. Try to reinstall the dependencies by doing `npm install` in the `trinity-wallet` directory. Then, build the wallet again by doing `npm run build` -->

2. トリニティの異なるバージョン間で切り替えると，開発環境はユーザー構成ファイルをクリアしません．以前に別のバージョンのトリニティを実行したことがある場合は，構成ファイルを削除してから，トリニティを再試行してください．
  <!-- 2. When you switch between different versions of Trinity, the development environment does not clear the user configuration files. If you have ever ran a different version of Trinity before, remove the configuration files, then try and run Trinity again -->

--------------------
### macOS
```bash
rm -rf ~/Library/Application Support/Electron
```
---
### Windows
```bash
Remove-Item –path %APPDATA%/Electron –recurse
```
---
### Linux
```bash
rm -rf ~/.config/Electron
```
--------------------

### 空白の画面でトリニティが開く
<!-- ### Trinity opens with a blank screen -->

1. 開発者ツールウィンドウが開いてフォーカスされている間に **Ctrl** + **R**（macOSでは **cmd** + **R**）を押して，トリニティをリロードします．
  <!-- 1. Reload Trinity by pressing **Ctrl**+ **R** (**cmd**+**R** on macOS) while the Developer tools window is open and in focus. -->

2. 開発者ツールのコンソールでエラーを確認します．エラーを修正するか，GitHub で [issue](https://github.com/iotaledger/trinity-wallet/issues) として報告してください．
