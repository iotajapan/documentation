# データマーケットプレイス
<!-- # Data Marketplace overview -->

**データサイロにより、異なるデータポイント間でデータを売買することが難しくなります。 この課題を克服するために、データマーケットプレイスはマスクされた認証済みメッセージング（Masked Authenticated Messaging, MAM）チャネルを使用してデータサイロを開き、ユーザーがデータと引き換えにIOTAトークンのマイクロペイメントをデータ所有者に行えるようにします。**
<!-- **Data silos make it difficult to buy and sell data among different data points. To overcome this challenge, the Data Marketplace uses Masked Authenticated Messaging (MAM) channels to open up the data silos and allow users to make micropayments of IOTA tokens to the data owners in exchange for data.** -->

データは機械経済とコネクテッドワールドの最も基本的な要素の1つです。データは[データ-情報-知識-知恵（DIKW）ピラミッド](https://en.wikipedia.org/wiki/DIKW_pyramid)の基礎です。
<!-- Data is one of the most fundamental ingredients in the machine economy and the connected world. It is the foundation of the Data — Information — Knowledge — Wisdom [(DIKW) pyramid](https://en.wikipedia.org/wiki/DIKW_pyramid). -->

![Data pyramid](../images/data-marketplace-otr-updates.png)

このピラミッドのさまざまな層はすべてデータに依存しています。たとえば、車のセンサーは、ドライバーが渋滞を回避することを可能にする知恵をもたらす可能性があります。
<!-- The different layers of this pyramid all rely on data. For example, sensors on cars can lead to the wisdom that allows drivers to bypass congestion: -->

* **データ：** センサーは道路を監視し、周囲の表面で反射する光子からの信号を受信します。
<!-- * **Data:** Sensors monitor the road and receive signals from the photons that reflect off the surface of their environment -->
* **情報：** センサデータは、動いている物体の速度とそれらの相対的な位置に関する意味のある情報を抽出するために使用されます。
<!-- * **Information:** Sensor data is used to extract meaningful information about the speed of the moving objects and their relative positions -->
* **知識：** 情報はコンテキスト化されて知識にまとめられ、無線で最新の情報を入手して道路上のドライバーに、事故により道路が渋滞している可能性があることを伝えます。
<!-- * **Knowledge:** Information is contextualized and distilled into knowledge, telling the drivers on the road via over-the-air updates that the road may be congested due to a crash -->
* **知恵：** 知識は運転手が代替ルートをとることを可能にします。
<!-- * **Wisdom:** Knowledge allows drivers to take an alternative route -->

## ビジネスケース
<!-- ## Business case -->

今後10年間で、750億台を超えるコネクテッドデバイスがさまざまな方法で相互作用するようになります。これらのデバイスは、ストレージから電気、そしてセンサーデータまで、あらゆるものを取引するという機械経済を生み出すでしょう。このパイロットプロジェクトのデータ交換は、機械経済の開発における潜在性を浮き彫りにし、探求するでしょう。データを生成する何千億ものデバイスにより、私たちは歴史の中で比類のないデータの増殖を見るでしょう。
<!-- Over the next decade, more than 75 billion connected devices will be interacting in different manners. These devices will give rise to a machine economy where they will trade everything from storage to electricity and sensor data. The data trade in this pilot project will highlight and explore the potential in these developments. With the prospect of tens of billions of devices generating data, we will see a proliferation of data that is unmatched in history. -->

![IoT devices](../images/data-marketplace-iot-stats.png)

## 課題
<!-- ## Challenges -->

ビッグデータが想定する壮大さの実現を妨げる最大の障害は、圧倒的多数のデータがいわゆるデータサイロに閉じ込められたままであるという事実です。
<!-- The largest obstacle preventing the fulfillment of the grandeur envisaged by Big Data is the fact that the overwhelming majority of data remains locked in what is called data silos. -->

![Data silos](../images/data-marketplace-data-silos.png)

データサイロが独自の閉鎖環境の外でデータを共有することはありません。この環境は大量の無駄なデータをもたらします。実際、多くの場合、データの99％以上が無効になっています（[出典：McKinsey 2015](https://www.mckinsey.com/mgi/overview/in-the-news/by-2025-internet-of-things-applications-could-have-11-trillion-impact)）。この失われたデータは、それがオープンで分散型のデータストリームの中にある場合、任意の当事者にとって貴重な情報を含む可能性があります。
<!-- Data silos do not, or very rarely, share data outside their own closed environment. This environment leads to enormous quantities of wasted data. In fact, often, over 99% of data is lost to the void ([source: McKinsey 2015](https://www.mckinsey.com/mgi/overview/in-the-news/by-2025-internet-of-things-applications-could-have-11-trillion-impact)). This lost data could contain valuable information to any compensating party if it were in an open, decentralized data stream. -->

![Open data](../images/data-marketplace-open-data.png)

面倒で無駄な現状にはいくつかの理由があります。一方では、データは、保存と転送のコストが時間の経過とともにますます少なくなるという意味で自由であることを望んでいます。一方、大量のデータは非常に貴重であり、自由に生成することはできません。これらの正反対の条件により、ビッグデータを本当に大きくするためにはグリッドロックを解除する必要があります。これの主な原因は、技術的な観点からデータ共有がより安くなっている一方で、仲介料のために、細かい粒度の高いデータをリアルタイムで販売するのは法外に高くつくことがあります。1回のデータ購入を完了するために必要なすべての煩雑な事務的手続きは言うまでもありません。これらの条件により、リアルタイムのデータ取引はほとんど不可能になります。2025年までに、全データの約95％がリアルタイムでIoTデバイスによって生成されると予測されているため（[出典：IDC 2017](https://www.seagate.com/files/www-content/our-story/trends/files/idc-seagate-dataage-whitepaper.pdf)）、これは差し迫った問題です。
<!-- There are several reasons for the cumbersome and wasteful status quo. On the one hand, data wants to be free in the sense that its storage and transmission costs less and less over time. On the other hand, large quantities of data are extremely valuable and are not free to generate. These diametrically opposed conditions cause a gridlock that needs to be broken in order for Big Data to become truly big. A major cause of this is the fact that, while data sharing is becoming cheaper from a technological perspective, it is prohibitively expensive to sell fine, granular data in real-time due to intermediary fees — not to mention all the red tape one has to cut through in order to complete a single data purchase. These conditions make real-time data trade all but impossible. By 2025 it is projected that around 95% of all data will be generated by IoT devices in real-time ([source: IDC 2017](https://www.seagate.com/files/www-content/our-story/trends/files/idc-seagate-dataage-whitepaper.pdf)), so this is a pressing issue. -->

3番目の障害は、確実な信頼性とデータの監査証跡の欠如です。分散型元帳技術が採用される前は、データ転送プロトコルとデータベースは、仲介者攻撃やデータ改ざんなど、さまざまな攻撃を受けやすいものでした。データはデータが有効であることに価値があります。つまり、データ入力がゴミであれば、出力もゴミになります。
<!-- A third obstacle is the lack of ensured authenticity and audit trails of data. Before the adoption of distributed ledger technologies, data transmission protocols and databases were susceptible to various attacks, including middleman attacks and data tampering. Data is only as valuable as it is valid. In short, if the data input is garbage, the output will also be garbage. -->

## 解決策
<!-- ## Solution -->

IOTA財団は、概念実証（PoC）として、またオープンイノベーションエコシステムとして[データマーケットプレイス](https://data.iota.org)を立ち上げました。
<!-- The IOTA Foundation launched the [Data Marketplace](https://data.iota.org) as a proof of concept (PoC) and as an open innovation ecosystem. -->

IOTA財団は非営利団体としてオープンソース技術を生み出し、他の組織が自社のIOTA対応データマーケットプレイスを関連する製品やサービスで形成する力を高めるためにこのプロジェクトを開発しました。
<!-- As a non-profit organisation, the IOTA Foundation has developed this project to produce an open source technology and to empower other organisations to shape their own IOTA-enabled data marketplaces, with associated products and services. -->

![Data Marketplace](../images/data-marketplace.png)

データマーケットプレイスの成長は、IoT（Internet of Things）革命の避けられない結果です。船舶、工場、自動車、農場、建物などの物理的資産がデジタル化するにつれて、それらのデジタルツインは徐々に安全なデータ交換として機能するようになります。
<!-- The growth of data marketplaces is an inevitable result of the IoT (Internet of Things) revolution. As physical assets such as ships, factories, vehicles, farms or buildings become digital, their digital twins will gradually act as secure data exchanges. -->

データストリームがサイロを超えて急増し、組織全体に価値をもたらすにつれて、従来のバリューチェーンは価値のあるウェブに移行します。このパラダイムは、管理がより複雑になり、ビジネスにこれらのエコシステムの一部としての競争力を再考することを余儀なくします。データマーケットプレイスは、データを交換し、データストリームを収益化し、新しいスマートビジネスモデルの基盤を提供する手段として出現するでしょう。IOTA財団は、モノのインターネットのための、この新しい価値創造の波をモノの経済と呼びます。
<!-- As data streams surge across silos and carry value across organisations, traditional value chains will transition into a web of value. This paradigm will be more complex to administer, forcing business to rethink their competitive play as part of these ecosystems. Data marketplaces will emerge as a means to exchange data, monetise data streams and provide the basis of new smart business models. We refer to this new wave of value creation, for the Internet of Everything, as the Economy of Things. -->

:::info:
このデータマーケットプレイスは、IOTA財団による将来の商用サービスの開始点ではありません。
:::
<!-- :::info: -->
<!-- This Data Marketplace is not a launchpad for a future commercial service by the IOTA Foundation. -->
<!-- ::: -->

### イニシアチブの目的
<!-- ### Objectives of the initiative -->

このイニシアチブの目的は次のとおりです。
<!-- The initiative’s objective was to do the following: -->

* オープンソースの概念実証を作成します。
<!-- * Produce an open source proof of concept -->
* ビジネスモデルの革新を探ります。
<!-- * Explore business model innovation -->
* パーミッションレスイノベーションを促進するための共創エコシステムを育てます。
<!-- * Grow a co-creation ecosystem to foster permissionless innovation -->

その技術と同様に、イノベーションを可能にするためのIOTA財団のアプローチはオープンでパーミッションレスです。IOTA財団の目標は、エコシステムが新しいソリューションとビジネスモデルを共同で開発することを可能にすることであり、特定のソリューションを推進し市場に投入することではありません。
<!-- Like its technology, the IOTA Foundation’s approach to enabling innovation is open and permissionless. Our ambition is to enable the ecosystem to co-create and develop new solutions and business models, but not to drive and push specific solutions to market. -->

データマーケットプレイスは、参加者だけでなくIOTA財団自体のためにも、アジャイルで実験主導の集団的なイノベーションアプローチを可能にするように設計されています。このイニシアチブは、実環境での展開の要件とデータマーケットプレイスの参加者の要求によってIOTAテクノロジに挑戦します。IOTA財団はこれらの要求を満たすためにIOTA財団の技術を適応させることを目指します。
<!-- The Data Marketplace is designed to enable an agile, experiment-driven and collective approach to innovation for its participants, but also for the IOTA Foundation itself. This initiative challenges the IOTA technology with the requirements of real life deployments and the demand of the Data Marketplace participants. We aim to adapt our technology to meet these demands. -->

![Smart models](../images/data-marketplace-smart-models.png)

## デモ
<!-- ## Demo -->

アプリケーションのデモについては、[このWebサイト](https://data.iota.org/)を参照してください。
<!-- See this website for a [demonstration of the application](https://data.iota.org/). -->

## 追加のリソース
<!-- ## Additional resources -->

---------------
#### データマーケットプレイスのAPIドキュメント ####
[Link](https://data.iota.org/static/docs)

REST APIを介してデータマーケットプレイスと対話します。
---
#### GitHubリポジトリ ####
[Link](https://github.com/iotaledger/data-marketplace)

この設計図をテストするには、コードとクイックスタートの手順を読んでください。
---
#### iota.jsクライアントライブラリ ####
[Link](root://iota-js/0.1/introduction/overview.md)

iota.jsクライアントライブラリを使ってトランザクションを作成、送信、受信する方法を学びます。
---
#### MAMの説明 ####
[Link](https://blog.iota.org/introducing-masked-authenticated-messaging-e55c1822d50e)

マスクされた認証済みメッセージングは、タングルを介して暗号化されたデータストリームへのアクセスを公開および制御する機能を追加するセカンドレイヤーのデータ通信プロトコルです。MAMの機能の詳細についてはこちらをご覧ください。
---
#### MAMのGitHubリポジトリ ####
[Link](https://github.com/iotaledger/mam.client.js)

JavaScriptでMAMをテストするには、コードとクイックスタートの手順を読んでください。
---------------
