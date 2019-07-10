# トラック&トレース概要
<!-- # Track and trace overview -->

**デューデリジェンスとベンダーに依存しないデータソースの両方が欠如しているため、流通ネットワーク内の回収可能資産は多くの場合、元の所有者に返却されません。この設計図では、IOTAタングルおよび他のIOTAテクノロジを使用して、IBCSグループが回収可能資産の回収のためのトラック&トレースシステムをどのように実施したかを説明しています。**
<!-- **Due to a lack of both due diligence and a vendor-neutral data source, returnable assets that are used within distribution networks are often not returned to their original owners. This blueprint describes how the IBCS Group implemented a track and trace system for recovery of returnable assets, using the IOTA Tangle and other IOTA technologies.** -->

![Track and Trace](../images/track-and-trace-thumbnail.png)

この設計図は[IBCSグループ](https://www.ibcsgroup.com/)のビジネスプロセスに統合することを提示します。この設計図は、他の組織が自分のシナリオやビジネスプロセスを複製して適応するためのガイドです。
<!-- We present the integration of this blueprint in [IBCS Group](https://www.ibcsgroup.com/) business processes. This blueprint is a guide for other organizations to replicate and adapt for their own scenarios and business processes. -->

## ビジネスケース
<!-- ## Business case -->

回収可能資産は、配送および物流チェーンの複数の関係者によって使用される資産です。
<!-- A returnable asset is an asset that's used by multiple parties in the distribution and logistics chain. -->

たとえば、ガラス製造業界では、ガラスラックは回収可能資産です。この資産は、ガラス製造業者（資産の所有者）から販売業者にガラスを出荷するために使用されます。ガラスラックをガラス製造業者に返却する代わりに、ガラス販売業者はガラスラックを窓製造業者に出荷することもできます。それから、窓製造業者はガラスラックを顧客に窓を届けるために使うかもしれません。
<!-- For example, in the glass manufacturing industry, a glass rack is a returnable asset. This asset is used to ship glasses from a glass producer (owner of the asset) to a distributor. Instead of returning the glass rack to the glass producers, the glass distributors might use them to ship other glasses to a window manufacturer. Then, the window manufacturers might use it to deliver windows to their customers. -->

複数の当事者が返却可能資産を使用すると、所有者は返却可能資産を見失いがちです。
<!-- When multiple parties use a returnable asset, it's easy for the owner to lose track of it. -->

そのような資産の置き忘れは、資産所有者にとって経済的損失となります。資産が不足していると、返却可能資産は所有者の配達を遂行する能力に影響を与え、返却可能資産を回収するために時間とリソースを浪費します。
<!-- Misplacement of such assets represents an economic loss for asset owners. When assets are missing, it affects the owners ability to fulfil deliveries and wastes their time and resources on recovering them. -->

以下の表は、このビジネス例で考慮されているさまざまな利害関係者と役割をまとめたものです。この表には、返却可能資産の処理に関わるすべての関係を追跡するのがどれほど複雑かも示されています。
<!-- The table below summarizes the different stakeholders and roles considered in our example business case. The table also shows how complex it is to keep track of all the relations involved in the handling of returnable assets. -->

| **利害関係者** | **役割** |
| :------------- | :------- |
| **ガラス製造業者** | ガラスを配給業者に（直接）または窓製造業者に（物流プロバイダを通して）配達します。返却可能資産を所有しています。 |
| **窓製造業者** | 直接または物流プロバイダを通じて、窓を窓再販業者に届けます。自身の返却可能資産を所有し、配達に使用することがあります。あるいは、ガラス製造業者から受け取った返却可能資産を再利用することもあります。 |
| **Distributors** | 直接または物流プロバイダーを通じて配送された返品可能な資産と一緒に配送された眼鏡または窓を受け取ります。返品可能な資産を再利用して、眼鏡や窓ガラスをさらに流通チェーンに届けることができます。Receive glasses or windows delivered directly or through logistic providers together with returnable assets used for the delivery. Re-use the returnable assets to deliver glasses or windows further down to the distribution chain. |
| **Window Resellers** | Receive windows from distributors and use received returnable assets to arrange their delivery to end-customers. |
| **Customers** | Receive windows directly from windows resellers or through logistic providers which use glass producers and windows manufacturers returnable assets. Often dispose returnable assets or do not know to whom and how to return. |
| **Logistic Providers** | Move assets and returnable assets along the distribution chain. |

This image presents a simplified stakeholders map and the different actions each stakeholder should perform when in contact with a returnable asset. The following entities are represented:

**Dotted arrows:** Path of a returnable asset
**Green circles:** Stakeholders such as custodians
**Purple circle:** Owner of the returnable asset

![Returnable assets stakeholder map](../images/track-and-trace-returnable-assets-stakeholders.png)

### Challenges

So far, tracking and tracing returnable assets has been unsuccessful for the following reasons:

- Custodians don't see economic value in a returnable asset. Instead, returnable assets are more likely to be seen as disposable
- Custodians are neither incentivized to help track returnable assets nor held accountable for not doing so
- Tracking returnable assets requires access to data that's stored in a number of proprietary systems, all of which belong to different custodians. These different systems increase the complexity to predict, map, and integrate the data, and exceed the perceived benefits in tracking returnable assets.

It's not possible to report the custody of assets using a centralized database because this database will also reveal to third parties, proprietary knowledge about different stakeholders, customers, and distribution chains. Moreover, it will be difficult to create a system that's able to track all the possible interactions envisioned for a number of stakeholders that aren't known up front.

### Solution

IOTA provides a solution to seamlessly collect and share information about returnable assets without the need to integrate any proprietary system. While doing that, IOTA can still guarantee access control of the collected information through the use of the second layer MAM protocol.

Thanks to the permissionless nature of IOTA, no trust is required among those who run the infrastructure because it's spread across the whole IOTA network.

In addition, the system does not need knowledge of all parties who write information to the ledger. This simplifies the system so that it can be used by all stakeholders and industry sectors as needed and with minimum integration and onboarding time.

This solution leads to the following benefits:

- Asset owners can track and request the return of their returnable assets

- Asset owners can save the time and the costs associated with searching for missing assets

- Asset owners can better predict and plan shipments of their production by knowing the exact number and the exact location of any returnable assets that are available to them

- Custodians can track the returnable assets they handle, be rewarded for returning them, and increase their reputation with asset owners

- Custodians can save costs associated with supporting requests for returnable assets that are declared missing

:::info:
Future scenarios the use of the IOTA token (and Qubic smart contracts) could allow you to create incentives to reward custodians who participate in tracking and tracing returnable assets, despite the country and the currency in which the assets are handled.
:::

## Demo

See this website for a [demonstration of the application](http://tradedemo.iota.org).
 	 	 
## Additional resources

---------------
#### iota.js client library ####
[Link](root://iota-js/0.1/README.md)

Learn how to use the iota.js client library to create, send, and receive transactions.
---
#### MAM eloquently explained ####
[Link](https://blog.iota.org/introducing-masked-authenticated-messaging-e55c1822d50e)

Masked Authenticated Messaging is a second layer data communication protocol that adds functionality to publish and control access to an encrypted data stream, over the Tangle. Learn more about how MAM works.
---
#### MAM GitHub repository ####
[Link](https://github.com/iotaledger/mam.client.js)

Read the code and some quickstart instructions to test MAM with JavaScript.
---------------
