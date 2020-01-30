# P2Pエネルギーグリッド概要
<!-- # Peer-to-peer energy grid -->

**エネルギーの供給はしばしば自動化されますが，支払いシステムの統合には費用がかかり，多くの場合，手動の介入が必要です．IOTA を使用してピアツーピア（P2P）エネルギーグリッドを作成すると，電力の転送とその電力の自動支払いを自動化できます．この自動化により，インフラストラクチャがより動的になり，更新が容易になります．**
<!-- **Although the supply of energy is often automated, the payment system is expensive to integrate and often requires manual intervention. By creating a peer-to-peer (P2P) energy grid with IOTA, you can automate the transfer of power and the automatic payment for that power. This automation makes your infrastructure more dynamic and easier to update.** -->

## ビジネスケース
<!-- ## Business case -->

ピアツーピアエネルギー（P2P）取引は，2つ以上のグリッドに接続された関係者間のエネルギーの売買です．たとえば，過剰な太陽エネルギーを持っている人は誰でもそのエネルギーを他の人に転送して売ることができ，消費者は自分のエネルギーをどこから調達するかについてより自由になります．
<!-- Peer-to-peer energy (P2P) trading is the buying and selling of energy between two or more grid-connected parties. For example, anyone with excess solar energy can transfer and sell that energy to others, giving consumers more freedom over where they source their energy. -->

現時点では，余分な太陽エネルギーは小さな固定価格でグリッドに送り返されます．過剰なエネルギーに対処するこの方法により，エネルギー企業はそのエネルギーの価値を制御できます．
<!-- At the moment, excess solar energy is sent back to the grid for a small fixed price. This way of dealing with excess energy gives the energy companies control over how much that energy is worth. -->

次の表は，このビジネスケース例で考慮されるさまざまな利害関係者と役割をまとめたものです．この表は，要返却資産の処理に関係するすべての関係を追跡することがどれほど複雑かを示しています．
<!-- The table below summarizes the different stakeholders and roles considered in this example business case. The table also shows how complex it is to keep track of all the relations involved in the handling of returnable assets. -->

| **エンティティ** | **役割** |
| :--------------- | :------- |
| **エネルギー源** | 生産者のために太陽や風などから電気を生成する |
| **生産者** | 複数のエネルギー源を管理し，グリッドから支払いを受け取る |
| **消費者** | 支払いと引き換えにグリッドから供給される電力を使用する |
| **グリッド** | 生産者から消費者への配電を調整し，支払いを分配する |

<!-- |**Entity** | **Role**| -->
<!-- |:---|:---| -->
<!-- |**Source**|Generate electricity such as solar or wind for a producer| -->
<!-- |**Producer**| Manage several sources and receive payment from the grid| -->
<!-- |**Consumer** |Use the power provided by the grid in return for payment| -->
<!-- |**Grid**| Coordinate the distribution of power from the producers to the consumers, and distribute payments| -->

![P2P Energy PoC - Use Case Picture](../images/p2p_use_case.png)

## 課題
<!-- ## Challenges -->

グリッドを経由せずにエネルギーを他の人に販売できるようにするには，エネルギー生産者が支払いを許可する共通のネットワークを必要とします．
<!-- To be able to sell energy to others without going through the grid, energy producers need a common network that allows payments. -->

## 解決策
<!-- ## Solution -->

IOTA テクノロジーを使用することにより，エンティティは，追跡可能でイミュータブルな方法でグリッドと通信できます．
<!-- By using IOTA technologies, entities can communicate with the grid in a traceable and immutable way. -->

## デモ
<!-- ## Demo -->

現在，このアプリケーションのデモンストレーションもデプロイ手順も利用できません．
<!-- Neither a demonstration of this application nor deployment instructions are  available at the moment. -->

## 追加のリソース
<!-- ## Additional resources -->

---------------
#### iota.js クライアントライブラリ ####
[Link](root://client-libraries/0.1/getting-started/js-quickstart.md)

iota.js クライアントライブラリを使ってトランザクションを作成，送信，受信する方法を学びます．
---
#### MAM の説明 ####
[Link](https://blog.iota.org/introducing-masked-authenticated-messaging-e55c1822d50e)

マスクされた認証済みメッセージングは，タングルを介して暗号化されたデータストリームへのアクセスを公開および制御する機能を追加するセカンドレイヤーのデータ通信プロトコルです．MAM の機能の詳細についてはこちらをご覧ください．
---
#### MAM の GitHub リポジトリ ####
[Link](https://github.com/iotaledger/mam.client.js)

JavaScript で MAM をテストするには，コードとクイックスタートの手順を読んでください．
---------------
