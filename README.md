# IOTAの公式ドキュメントの日本語訳の中身

- オリジナル → [docs.iota.org](https://docs.iota.org)
    - オリジナルの外側 → [iotaledger/documentation-platform](https://github.com/iotaledger/documentation-platform)
    - オリジナルの中身 → [iotaledger/documentation](https://github.com/iotaledger/documentation)
- 日本語訳 → [docs.iotajapan.com](https://docs.iotajapan.com)
    - 日本語訳の外側 → [iotajapan/documentation-platform](https://github.com/iotajapan/documentation-platform)
    - 日本語訳の中身 → ココ

## コントリビュート
コントリビューションをしてくださる方は以下をご覧ください．
- [style guide](./contribution/0.1/style-guide.md)

## Lint と翻訳チェック
### Lint
Text の Lint には
- [textlint](https://github.com/textlint/textlint)
- [textlint-rule-preset-ja-technical-writing](https://github.com/textlint-ja/textlint-rule-preset-ja-technical-writing)
- を使用しています．

### 翻訳チェック
翻訳チェックには
- [prh](https://github.com/prh/prh)
- [textlint-rule-prh](https://github.com/textlint-rule/textlint-rule-prh)
- を使用しています．
- 詳しい翻訳対応表は `doc_iotajapan_com.yml` を参照のこと．

### Usage
- textlint と翻訳チェックを行うには以下を実行します．

```bash
npm run textlint
```
### 重要単語の英日対訳表

| English                       | 日本語                                               |
|:------------------------------|:-----------------------------------------------------|
| Attach                        | アタッチ，添付                                       |
| Body transaction              | ボディトランザクション                               |
| Build                         | ビルド，構築                                         |
| Conditional deposit addresses | 条件付きデポジットアドレス，条件付き預け入れアドレス |
| confirmed                     | 確定                                                 |
| Cumulative weight             | 累積荷重                                             |
| Decode                        | デコード，復号化                                     |
| Deposit                       | デポジット，預け入れ                                 |
| Depth                         | 深度，深さ                                           |
| Device                        | デバイス，端末                                       |
| Devnet                        | デブネット                                           |
| Fragment                      | フラグメント，断片                                   |
| Future set                    | フューチャーセット，未来集合                         |
| Head transaction              | ヘッドトランザクション，先頭トランザクション         |
| Immutable                     | イミュータブル，不変                                 |
| Inclusion states              | 包含状態                                             |
| Input transaction             | 入力トランザクション                                 |
| Ledger                        | 台帳                                                 |
| Ledger state                  | 台帳ステート                                         |
| List                          | リスト，一覧                                         |
| Mainnet                       | メインネット                                         |
| Micropayment                  | マイクロペイメント，少額決済                         |
| Neighbors                     | 隣接ノード，ネイバーノード                           |
| Nonce                         | ナンス                                               |
| Pending                       | ペンディング中，保留中                               |
| Permanodes                    | パーマノード                                         |
| Promote                       | プロモート，促進                                     |
| Proof-of-work                 | プルーフオブワーク，PoW                              |
| Prune                         | プルーニング，刈り取り                               |
| Reattach                      | 再アタッチ，再添付                                   |
| Rebroadcast                   | 再ブロードキャスト                                   |
| Output transaction            | 出力トランザクション                                 |
| Seed state                    | シードステート，シード状態                           |
| Solid                         | ソリッド，凝固済み                                   |
| Spent address                 | 使用済みアドレス                                     |
| Subscribe                     | サブスクライブする，購読する                         |
| Tail transaction              | テールトランザクション，末尾トランザクション         |
| Tip seleciton                 | チップ選択                                           |
| Tip transaction               | チップトランザクション                               |
| View                          | 概観，ビュー                                         |
| Withdraw                      | 取り出し                                             |
| Zero-value transaction        | ゼロトークントランザクション                         |

↓ Original
---
<h2 align="center">Developer Documentation for IOTA technology</h2>

<p align="center">
    <a href="https://docs.iota.org/docs/wallets/0.1/hub/introduction/overview" style="text-decoration:none;">
    <img src="https://img.shields.io/badge/Documentation%20portal-blue.svg?style=for-the-badge" alt="Developer documentation portal">
</p>
<p align="center">
  <a href="https://discord.iota.org/" style="text-decoration:none;"><img src="https://img.shields.io/badge/Discord-9cf.svg?logo=discord" alt="Discord"></a>
    <a href="https://iota.stackexchange.com/" style="text-decoration:none;"><img src="https://img.shields.io/badge/StackExchange-9cf.svg?logo=stackexchange" alt="StackExchange"></a>
    <a href="https://raw.githubusercontent.com/iotaledger/documentation/develop/LICENSE" style="text-decoration:none;"><img src="https://img.shields.io/github/license/iotaledger/documentation.svg" alt="Creative Commons 4.0 license"></a>
    <a href="https://buildkite.com/iota-foundation/documentation-platform-build-and-deploy" style="text-decoration:none;"><img src="https://badge.buildkite.com/f736fdb29f0102a9e640363d347bd7332ec2002aeb69916556.svg" alt="Build status"></a>
</p>
      
<p align="center">
  <a href="#about">About</a> ◈
  <a href="#prerequisites">Structure</a> ◈
  <a href="#supporting-the-project">Supporting the project</a> ◈
  <a href="#joining-the-discussion">Joining the discussion</a> 
</p>

---

## About

<<<<<<< HEAD
If you'd like to help us write new content, or to edit existing content, please refer to the [style guide](./contribution/0.1/style-guide.md).
=======
This repository contains all the documentation that's hosted on [docs.iota.org](https://docs.iota.org).

The documentation is written in markdown files and built by the platform in the [documentation platform repository](https://github.com/iotaledger/documentation-platform).

## Structure

The content is structured in a way that allows us to render it on a webpage.

### Top level navigation

The `projects.md` file contains the top level navigation labels and links for the following:

- Home page floating menu
- Footer
- Burger menu navigation

The order of the items in the file determines the order in the navigation.

:::info:
An item will appear in the navigation only if the corresponding sub-directory for that project also contains a `home.md` file. The links must be relative to the location of the `projects.md` file.
:::

The content of the file is a list of markdown links.

### Home page

The `home.md` file of a project contains the content to display on the home page. The file contains a level 1 heading, which determines the description for that section. In addition there are links followed by level 2 header which contain the direct links into the content.

The links must be relative to the location of the `home.md` file.

### Version directories

Under each project directory is another directory that's named after the version number for the content. If more than one version directory exists, a version selector will be displayed. This version selector allows the reader to choose which version of the content they want to see.

Each version directory contains all of the content required for that project version as well as a `doc-index.md` file, which contains the items that you want to appear in the left navigation panel.

The links must be relative to the `doc-index.md` file.

If you want to link to another project, use the following structure `root://another-project/0.1/some-content.md`.

## Supporting the project

If you'd like to help us write new content, or to edit existing content, please refer to the [contribution guidelines](.github/CONTRIBUTING.md).

## Joining the discussion

If you want to get involved in the community or just want to discuss IOTA, Distributed Registry Technology (DRT) and IoT with other people, feel free to join our [Discord](https://discord.iota.org/).
>>>>>>> upstream/develop
