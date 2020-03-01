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

## Lint
Text の Lint には
- [textlint](https://github.com/textlint/textlint)
- [textlint-rule-preset-ja-technical-writing](https://github.com/textlint-ja/textlint-rule-preset-ja-technical-writing)
- を使用しています．

## 翻訳チェック
翻訳チェックには
- [prh](https://github.com/prh/prh)
- [textlint-rule-prh](https://github.com/textlint-rule/textlint-rule-prh)
- を使用しています．
- 詳しい翻訳対応表は `doc_iotajapan_com.yml` を参照のこと．

## 重要単語の英日対訳表

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
# Documentation Markdown

Documentation can be viewed on our web site at [docs.iota.org](https://docs.iota.org)

This repository contains the content which is utilised by the documentation platform in the [documentation platform repository](https://github.com/iotaledger/documentation-platform).

## Contribute

If you'd like to help us write new content, or to edit existing content, please refer to the [style guide](./contribution/0.1/style-guide.md).
