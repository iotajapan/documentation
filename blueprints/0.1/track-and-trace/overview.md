# トラック&トレース概要
<!-- # Track and trace overview -->

**デューデリジェンスとベンダーに依存しないデータソースの両方が欠如しているため、流通ネットワーク内の回収可能アセットは多くの場合、元の所有者に返却されません。この設計図では、IOTAタングルおよび他のIOTAテクノロジを使用して、IBCSグループが回収可能アセットの回収のためのトラック&トレースシステムをどのように実施したかを説明しています。**
<!-- **Due to a lack of both due diligence and a vendor-neutral data source, returnable assets that are used within distribution networks are often not returned to their original owners. This blueprint describes how the IBCS Group implemented a track and trace system for recovery of returnable assets, using the IOTA Tangle and other IOTA technologies.** -->

![Track and Trace](../images/track-and-trace-thumbnail.png)

この設計図は[IBCSグループ](https://www.ibcsgroup.com/)のビジネスプロセスに統合することを提示しています。この設計図は、他の組織が自分のシナリオやビジネスプロセスを複製して適応するためのガイドです。
<!-- We present the integration of this blueprint in [IBCS Group](https://www.ibcsgroup.com/) business processes. This blueprint is a guide for other organizations to replicate and adapt for their own scenarios and business processes. -->

## ビジネスケース
<!-- ## Business case -->

回収可能アセットは、配送および物流チェーンの複数の関係者によって使用されるアセットです。
<!-- A returnable asset is an asset that's used by multiple parties in the distribution and logistics chain. -->

たとえば、ガラス製造業界では、ガラスラックは回収可能アセットです。このアセットは、ガラス製造業者（アセットの所有者）から販売業者にガラスを出荷するために使用されます。ガラスラックをガラス製造業者に返却する代わりに、ガラス販売業者はガラスラックを窓製造業者に出荷することもできます。それから、窓製造業者はガラスラックを顧客に窓を届けるために使うかもしれません。
<!-- For example, in the glass manufacturing industry, a glass rack is a returnable asset. This asset is used to ship glasses from a glass producer (owner of the asset) to a distributor. Instead of returning the glass rack to the glass producers, the glass distributors might use them to ship other glasses to a window manufacturer. Then, the window manufacturers might use it to deliver windows to their customers. -->

複数の当事者が回収可能アセットを使用すると、所有者は回収可能アセットを見失いがちです。
<!-- When multiple parties use a returnable asset, it's easy for the owner to lose track of it. -->

このようなアセットの置き忘れは、アセット所有者にとって経済的損失となります。アセットが不足してくると、回収可能アセットはアセット所有者の配達を遂行する能力に影響を与え、回収可能アセットを回収するために時間とリソースを浪費します。
<!-- Misplacement of such assets represents an economic loss for asset owners. When assets are missing, it affects the owners ability to fulfill deliveries and wastes their time and resources on recovering them. -->

以下の表は、このビジネス例で想定されているさまざまな利害関係者と役割をまとめたものです。この表には、回収可能アセットの処理に関わるすべての関係を追跡するのがどれほど複雑かも示されています。
<!-- The table below summarizes the different stakeholders and roles considered in our example business case. The table also shows how complex it is to keep track of all the relations involved in the handling of returnable assets. -->

| **利害関係者** | **役割** |
| :------------- | :------- |
| **ガラス製造業者** | ガラスを卸売業者に（直接）または窓製造業者に（物流プロバイダを通して）配達します。回収可能アセットを所有しています。 |
| **窓製造業者** | 直接または物流プロバイダを通じて、窓を窓再販業者に届けます。自身の回収可能アセットを所有し、配達に使用することがあります。あるいは、ガラス製造業者から受け取った回収可能アセットを再利用することもあります。 |
| **卸売業者** | 直接または物流プロバイダを通じて配送された回収可能アセットと一緒に配送されたガラスまたは窓を受け取ります。回収可能アセットを再利用して、ガラスや窓ガラスをさらに流通チェーンに届けることができます。 |
| **窓の再販業者** | 卸売業者から窓を受け取り、受け取った回収可能アセットを使って最終顧客への配送を手配します。 |
| **顧客** | 窓の再販業者から直接窓を受け取るか、ガラス製造業者と窓の製造業者の回収可能アセットを扱っている物流プロバイダを介して受け取ります。多くの場合、回収可能アセットを処分するか、誰にどのように返品するか分かりません。 |
| **物流プロバイダ** | 流通チェーンに沿ってアセットと回収可能アセットを移動させます。 |

下図は、簡略化された利害関係者マップと、回収可能アセットと接触したときに各利害関係者が実行する必要があるさまざまなアクションを示しています。以下のエンティティが表示されています。
<!-- This image presents a simplified stakeholders map and the different actions each stakeholder should perform when in contact with a returnable asset. The following entities are represented: -->

**点線の矢印：** 回収可能アセットの経路
<!-- **Dotted arrows:** Path of a returnable asset -->
**緑色の丸：** 保管者などの利害関係者
<!-- **Green circles:** Stakeholders such as custodians -->
**紫色の丸：** 回収可能アセットの保有者
<!-- **Purple circle:** Owner of the returnable asset -->

![Returnable assets stakeholder map](../images/track-and-trace-returnable-assets-stakeholders.png)

### 課題
<!-- ### Challenges -->

これまでのところ、回収可能アセットのトラック&トレースは、以下の理由で失敗しています。
<!-- So far, tracking and tracing returnable assets has been unsuccessful for the following reasons: -->

- 保管者は、回収可能アセットに経済的価値を見出していません。代わりに、回収可能アセットは使い捨てと見なされる可能性が高くなります
<!-- - Custodians don't see economic value in a returnable asset. Instead, returnable assets are more likely to be seen as disposable -->
- 保管者は、回収可能アセットの追跡を支援するインセンティブも、そうしないことに対する説明責任も負っていません。
<!-- - Custodians are neither incentivized to help track returnable assets nor held accountable for not doing so -->
- 回収可能アセットを追跡するには、さまざまな保管者の専売システムに保存されているデータにアクセスする必要があります。これらの異なる専売システムは、データを予測、マッピング、および統合するための複雑さを増し、そして回収可能アセットを追跡する上で認識されている利点を超えています。
<!-- - Tracking returnable assets requires access to data that's stored in a number of proprietary systems, all of which belong to different custodians. These different systems increase the complexity to predict, map, and integrate the data, and exceed the perceived benefits in tracking returnable assets. -->

集中管理されたデータベースを使用してアセット管理を報告することは不可能です。これは、集中管理されたデータベースによって、さまざまな利害関係者、顧客、流通チェーンに関する専売の知識が第三者に明らかにされるためです。そのうえ、前もって知られていない多くの利害関係者のために想定されるすべての可能な相互作用を追跡することができるシステムを作成するのは困難です。
<!-- It's not possible to report the custody of assets using a centralized database because this database will also reveal to third parties, proprietary knowledge about different stakeholders, customers, and distribution chains. Moreover, it will be difficult to create a system that's able to track all the possible interactions envisioned for a number of stakeholders that aren't known up front. -->

### 解決策
<!-- ### Solution -->

IOTAは、専売のシステムを統合することなく、回収可能アセットに関する情報をシームレスに収集および共有するための解決策を提供します。情報を収集している間、IOTAはセカンドレイヤー技術のMAMプロトコルを使用して収集される情報のアクセス制御も保証できます。
<!-- IOTA provides a solution to seamlessly collect and share information about returnable assets without the need to integrate any proprietary system. While doing that, IOTA can still guarantee access control of the collected information through the use of the second layer MAM protocol. -->

IOTAネットワーク全体に広がるパーミッションレスな性質のおかげで、インフラストラクチャを運営する人々の間で信頼は必要とされません。
<!-- Thanks to the permissionless nature of IOTA, no trust is required among those who run the infrastructure because it's spread across the whole IOTA network. -->

さらに、このシステムは、台帳に情報を書き込むすべての関係者の知識を必要としません。これによりシステムが簡素化され、必要に応じてすべての利害関係者および業界部門が最小限の統合およびオンボーディング時間で使用できるようになります。
<!-- In addition, the system does not need knowledge of all parties who write information to the ledger. This simplifies the system so that it can be used by all stakeholders and industry sectors as needed and with minimum integration and onboarding time. -->

この解決策は、以下の利点をもたらします。
<!-- This solution leads to the following benefits: -->

- アセット所有者は、自分の回収可能アセットを追跡して返品を要求できます。
<!-- - Asset owners can track and request the return of their returnable assets -->

- アセット所有者は、足りないアセットの検索に関連する時間とコストを節約できます。
<!-- - Asset owners can save the time and the costs associated with searching for missing assets -->

- アセット所有者は、自分が利用できる回収可能アセットの正確な数と正確な位置を知ることによって、自社製品の出荷をより正確に予測し、計画することができます。
<!-- - Asset owners can better predict and plan shipments of their production by knowing the exact number and the exact location of any returnable assets that are available to them -->

- 保管者は、自分が取り扱っている回収可能アセットを追跡し、返品したことで報酬を得て、アセット所有者の評判を高めることができます。
<!-- - Custodians can track the returnable assets they handle, be rewarded for returning them, and increase their reputation with asset owners -->

- 保管者は、欠落していると公言された回収可能アセットの要求のサポートに関連するコストを節約できます。
<!-- - Custodians can save costs associated with supporting requests for returnable assets that are declared missing -->

:::info:
将来のシナリオでは、IOTAトークン（およびQubicスマートコントラクト）を使用することで、アセットが処理される国や通貨にかかわらず、回収可能アセットのトラック&トレースに参加する保管者に報酬を与えるインセンティブを作成できます。
:::
<!-- :::info: -->
<!-- Future scenarios the use of the IOTA token (and Qubic smart contracts) could allow you to create incentives to reward custodians who participate in tracking and tracing returnable assets, despite the country and the currency in which the assets are handled. -->
<!-- ::: -->

## デモ
<!-- ## Demo -->

アプリケーションのデモについては、[このWebサイト](http://tradedemo.iota.org)を参照してください。
<!-- See this website for a [demonstration of the application](http://tradedemo.iota.org). -->

## 追加リソース
<!-- ## Additional resources -->

---------------
#### iota.jsクライアントライブラリ ####
[Link](root://client-libraries/0.1/getting-started/js-quickstart.md)

iota.jsクライアントライブラリを使ってトランザクションを作成、送信、受信する方法を学びます。
---
#### MAMの説明 ####
[Link](https://blog.iota.org/introducing-masked-authenticated-messaging-e55c1822d50e)

マスク認証メッセージングは、タングルを介して暗号化されたデータストリームへのアクセスを公開および制御する機能を追加するセカンドレイヤーのデータ通信プロトコルです。MAMの機能の詳細についてはこちらをご覧ください。
---
#### MAMのGitHubリポジトリ ####
[Link](https://github.com/iotaledger/mam.client.js)

JavaScriptでMAMをテストするには、コードとクイックスタートの手順を読んでください。
---------------
