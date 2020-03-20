# ドキュメントスタイルガイド
<!-- # Documentation style guide -->

IOTAの知識を持つすべての人に，ドキュメントに貢献することをお勧めします．ドキュメントについて議論するには，[Discord](https://discord.iota.org)の`#documentation-discussion`チャンネルに参加してください．
<!-- We encourage everyone with knowledge of IOTA to contribute to our documentation. To discuss the documentation, join our `#documentation-discussion` channel on [Discord](https://discord.iota.org). -->

新しいコンテンツを提供するには：
<!-- To contribute new content: -->

1. [オープンイシュー](https://github.com/iotaledger/documentation/issues)をご覧ください．
<!-- 1. See our [open issues](https://github.com/iotaledger/documentation/issues) -->
2. 作業したいイシューを選択するか，新しいイシューを作成して新しいコンテンツを提案します．
<!-- 2. Choose something that you'd like to work on, or create a new issue to suggest new content -->
3. [GitHubをセットアップします](#set-up-github)．
<!-- 3. [Set up GitHub](#set-up-github) -->
4. イシューにコメントして，作業を希望することをお知らせください．
<!-- 4. Comment on the issue to let us know that you'd like to work on it -->
5. [スタイルガイド](#writing-guide)に従って，コンテンツの作成を開始します．
<!-- 5. Start writing your content, following our [style guide](#writing-guide) -->
6. [コンテンツをGitHubリポジトリにプッシュします](#push-content-to-our-github-repository)．
<!-- 6. [Push your content to our GitHub repository](#push-content-to-our-github-repository) -->

既存のコンテンツに対して提案を行うには：
<!-- To make suggestions to existing content: -->

1. [GitHubをセットアップします](#set-up-github)．
<!-- 1. [Set up GitHub](#set-up-github) -->
2. 変更する記事を見つけます．
<!-- 2. Find the article that you want to change -->
3. [スタイルガイド](#style-guide)に従って変更を開始します．
<!-- 3. Start making your changes, following our [style guide](#style-guide) -->
4. [GitHubリポジトリにコンテンツをプッシュします](#push-content-to-our-github-repository)．
<!-- 4. [Push your content to our GitHub repository](#push-content-to-our-github-repository) -->

Thanks! :heart:

<a name="set-up-github"></a>
## GitHubをセットアップする
<!-- ## Set up GitHub -->

ドキュメントは，バージョン管理ツールであるGitHubでホストされています．新しいコンテンツを作成したり，既存のコンテンツへの変更を提案するには，GitまたはGitHubを使用する必要があります．
<!-- Our documentation is hosted on GitHub, which is a version control tool. To create new content, or suggest changes to existing content, you must use Git or GitHub. -->

GitHubアカウントを既にお持ちで，コンピューターにGitがセットアップされている場合は，[新しいブランチを作成する](#create-a-new-branch)に進んでください．
<!-- If you already have a GitHub account and Git is set up on your computer, go straight to [Create a new branch](#create-a-new-branch). -->

1. GitHubアカウントをまだ持っていない場合は，[新しいGitHubアカウントを作成します](https://github.com/)．
<!-- 1. [Create a new GitHub account](https://github.com/) if you don't already have one -->
2. [Gitをセットアップします](https://help.github.com/articles/set-up-git/)．
<!-- 2. [Set up Git](https://help.github.com/articles/set-up-git/) -->
3. [ドキュメントリポジトリ](https://github.com/iotaledger/documentation.git)に移動し，ページ上部の[**Fork**]をクリックします．
<!-- 3. Go to our [documentation repository](https://github.com/iotaledger/documentation.git) and click **Fork** at the top of the page -->
4. コマンドラインインターフェイスで次を実行して，フォークをローカルマシンにコピーします．
  <!-- 4. Copy your fork to your local machine by doing the following in the command-line interface: -->

    ```bash
    git clone https://github.com/{your username}/documentation
    ```

5. 次を実行して，ルートリポジトリへの参照を作成します．
  <!-- 5. Create a reference to the root repository by doing the following: -->

    ```bash
    cd documentation
    git remote add upstream https://github.com/iotaledger/documentation.git
    git fetch upstream
    ```

これで，`documentation`ディレクトリにはすべてのドキュメントファイルが含まれます．
<!-- Now, your `documentation` directory will contain all the documentation files. -->

<a name="create-a-new-branch"></a>
### 新しいブランチを作成する
<!-- ### Create a new branch -->

ブランチは，コントリビューションを分離することでコンテンツを確認するのに役立ちます．
<!-- Branches help us to review content by separating contributions. -->

新しいブランチを作成するときは，変更を簡単に追加できるように，各ブランチが1つの記事のみに対応していることを確認してください．次の種類のコントリビューションが新しいブランチに適しています．
<!-- When you create a new branch make sure each branch addresses only one article to make it easy for us to add your changes. The following types of contribution are appropriate for a new branch: -->

- 新しい記事（単一のマークダウンファイル）
<!-- - A new article ( a single markdown file) -->
- 文法の編集とスペルの修正，および既存の記事に関するその他の提案
<!-- - Grammar edits and spelling corrections, and any other suggestions for an existing article -->

1. コマンドラインインターフェイスを開きます．
<!-- 1. Open a command-line interface -->

2. 以下を実行します．
  <!-- 2. Do the following: -->

    ```bash
    git pull upstream develop:<your branch name>
    git push origin <your branch name>
    ```

3. ブランチのローカルコピーで作業を開始するには，次の手順を実行します．
  <!-- 3. To start working on your local copy of the branch, do the following: -->

    ```bash
    git checkout <your branch name>
    ```

記事を書いたり編集するときは，[スタイルガイド](#style-guide)に従ってください．
<!-- Please follow our [style guide](#style-guide) when you write and edit articles. -->

<a name="push-content-to-our-github-repository"></a>
###コンテンツをGitHubリポジトリにプッシュする
<!-- ### Push content to our GitHub repository -->

ブランチのローカルコピーのコンテンツを作成または編集した後に，次のステップでそれらをIOTA財団のリポジトリにプッシュします．
<!-- After writing or editing content in your local copy of the branch, the next step is to push them to our repository. -->

1. 変更をaddします．
  <!-- 1. Add your changes -->

    ```bash
    git add .
    ```

    :::info:
    アカウントのデフォルトIDを設定するよう求められる場合があります．
    :::
    <!-- :::info: -->
    <!-- You may be asked to set your account's default identity. -->
    <!-- ::: -->

    ```bash
    Please tell me who you are
    Run
    git config --global user.email "you@example.com"
    git config --global user.name "Your Name"
    ```

2. 変更をコミットします．
  <!-- 2. Commit your changes -->

    ```bash
    git commit -m "<Describe the changes you made>"
    ```

    :::info:
    変更をコミットすると，スクリプトが[コンテンツの検証](#validation-script)を開始して，リンクが壊れていないことを確認し，スペルミスを探します．
    :::
    <!-- :::info: -->
    <!-- When you commit your changes, a script will start [validating your content](#validation-script) to make sure that no links are broken and to look for spelling mistakes. -->
    <!-- ::: -->

    :::info:
    作業中の後続のコミットで同じファイルに追加の変更を加えます． すべての変更が同じコミットにある必要はありません．
    :::
    <!-- :::info: -->
    <!-- Make any additional changes to the same files in subsequent commits as you work. Not all changes need to be in the same commit. -->
    <!-- ::: -->

3. 変更をプッシュします．
  <!-- 3. Push your changes -->

    ```bash
    git push origin <your branch name>
    ```

4. GitHubで，`iotaledger/documentation`からフォークしたリポジトリに移動し，ページの上部にある** Pull Request **をクリックします．
<!-- 4. In GitHub, go to the repository that you forked from `iotaledger/documentation`, and click **Pull Request** at the top of the page -->

5. ベースブランチが`iotaledger/documentation@develop`であり，ヘッドブランチが`<your username>/documentation@<your branch name>`であることを確認します．
<!-- 5. Make sure that the base branch is `iotaledger/documentation@develop` and the head branch is `<your username>/documentation@<your branch name>` -->

6. **Update Commit Range**または**Compare & pull request**をクリックします．
<!-- 6. Click **Update Commit Range** or **Compare & pull request** -->

7. プルリクエストにタイトルを付け，行っているすべての変更を説明します．
<!-- 7. Give your pull request a title, and describe all the changes you're making -->

8. **submit**をクリックします．
<!-- 8. Click **Submit** -->

:::success:
ありがとうございました！ プルリクエストを処理します．編集が必要な場合は，作成したプルリクエストのコメントで質問します．
:::
<!-- :::success: -->
<!-- Thank you! We will now process your pull request. If there are any edits to make, we will ask you in the comments of the pull request you created. -->
<!-- ::: -->

作業を続行し，以前と同じように新しい変更をコミット/プッシュできます．更新はペンディング中のプルリクエストに表示されます．
<!-- You can continue working and commit/push new changes like you did before. Any updates will appear in the pending pull request. -->

<a name="validation-script"></a>
### 検証スクリプト
<!-- ### Validation script -->

このリポジトリを変更する場合は，コンテンツをコミットする前に検証スクリプトを実行する必要があります．
<!-- If you make changes to this repository you should run the validation script before commiting any content. -->

コンテンツを検証するには，まず`npm install`で依存関係がインストールされていることを確認してから，次のスクリプトを実行します．
<!-- To validate the content, first make sure you have installed the dependencies with `npm install`, then run the following script: -->

```shell
node buildProjects
```

このスクリプトは次のことを行います．
<!-- This script will do the following: -->

- コンソールにエラーを出力します．
<!-- - Print errors to the console -->
- `projects-summary.log`ファイルを作成します．これはコンテンツの構造を示し，エラーも強調表示します．
<!-- - Create a `projects-summary.log` file, which shows the structure of the content and also highlights the errors -->
- スペルミスや提案の可能性があるものを含む`spelling-summary.md`ファイルを作成します．
<!-- - Create a spelling-summary.md file, which contains any possible spelling mistakes and suggestions -->

スペルチェッカーは，舞台裏で[Hunspell](https://en.wikipedia.org/wiki/Hunspell)を使用します．
<!-- The spell checker uses [Hunspell](https://en.wikipedia.org/wiki/Hunspell) behind the scenes. -->

スペルチェッカーを強化するために，正規表現をサポートする`dictionary.json`ファイルに単語を追加できます． 例えば：
<!-- To enhance the spell checker, you can add words to the dictionary.json file, which supports regular expressions. For example: -->

```json
{
    "global": [
        "(P|p)ermission(less|ed)"
    ],
    "smartcity": [
        "AstroPi"
    ]
}
```

単一のプロジェクトのみを検証する場合は，特定のフォルダ名のコマンドラインオプションを使用してスクリプトを実行できます． 例えば，`getting-started`ディレクトリ内のコンテンツのみを検証するには：
<!-- If you want to validate just a single project you can run the script with a command-line option of the specific folder name. For example, to validate only the content in the `getting-started` directory: -->

```shell
node buildProjects getting-started
```

## 構造
<!-- ## Structure -->

`documentation`ディレクトリ内のコンテンツは，Webページ上でレンダリングできるように構成されています．
<!-- The content in the `documentation` directory is structured in a way that allows us to render it on a webpage. -->

### トップレベルのナビゲーション
<!-- ### Top level navigation -->

`projects.md`ファイルには，次のトップレベルのナビゲーションラベルとリンクが含まれています．
<!-- The `projects.md` file contains the top level navigation labels and links for the following: -->

- ホームページのフローティングメニュー
<!-- - Home page floating menu -->
- フッター
<!-- - Footer -->
- バーガーメニューのナビゲーション
<!-- - Burger menu navigation -->

ファイル内のアイテムの順序によって，ナビゲーション内の順序が決まります．
<!-- The order of the items in the file determines the order in the navigation. -->

:::info:
そのプロジェクトに対応するサブディレクトリに`home.md`ファイルも含まれている場合にのみ，アイテムがナビゲーションに表示されます．リンクは，`projects.md`ファイルの場所に相対的でなければなりません．
:::
<!-- :::info: -->
<!-- An item will appear in the navigation only if the corresponding sub-directory for that project also contains a `home.md` file. The links must be relative to the location of the `projects.md` file. -->
<!-- ::: -->

ファイルの内容は，マークダウンリンクのリストです．
<!-- The content of the file is a list of markdown links. -->

### ホームページ
<!-- ### Home page -->

プロジェクトの`home.md`ファイルには，ホームページに表示するコンテンツが含まれています．ファイルには，そのセクションの説明を決定するレベル1の見出しが含まれています．さらに，コンテンツへの直接リンクを含むレベル2ヘッダーへ続くリンクがあります．
<!-- The `home.md` file of a project contains the content to display on the home page. The file contains a level 1 heading, which determines the description for that section. In addition there are links followed by level 2 header which contain the direct links into the content. -->

リンクは，`home.md`ファイルの場所に相対的でなければなりません．
<!-- The links must be relative to the location of the `home.md` file. -->

### バージョンディレクトリ
<!-- ### Version directories -->

各プロジェクトディレクトリの下には，コンテンツのバージョン番号にちなんだ名前の別のディレクトリがあります． 複数のバージョンディレクトリが存在する場合，バージョンセレクタが表示されます． このバージョンセレクタを使用すると，読者は表示するコンテンツのバージョンを選択できます．
<!-- Under each project directory is another directory that's named after the version number for the content. If more than one version directory exists, the version selector will be displayed. This version selector allows the reader to choose which version of the content they want to see. -->

各バージョンディレクトリには，そのプロジェクトバージョンに必要なすべてのコンテンツと，左ナビゲーションパネルに表示するアイテムを含む`doc-index.md`ファイルが含まれています．
<!-- Each version directory contains all of the content required for that project version as well as a `doc-index.md` file, which contains the items that you want to appear in the left navigation panel. -->

リンクは`doc-index.md`ファイルに関連している必要があります．
<!-- The links must be relative to the `doc-index.md` file. -->

別のプロジェクトにリンクする場合は，次の構造を使用します`root://another-project/0.1/some-content.md`．
<!-- If you want to link to another project, use the following structure `root://another-project/0.1/some-content.md`. -->
