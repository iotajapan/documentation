 ゼロメッセージキュー
<!-- # Zero message queue -->

** ゼロメッセージキュー（Zero message queue, ZMQ）は、クライアントがIRIで発生したイベントを購読することを可能にするオープンソースのリアルタイムメッセージングライブラリです。**
<!-- **Zero message queue (ZMQ) is an open-source, real-time messaging library that allows clients to subscribe to events that happen in the IRI.** -->

ZMQでは、IRIノードは特定のイベントが発生したときにキューにメッセージを追加することによって発行者として機能し、一方クライアントはキューからメッセージを取り出して処理することによって購読者として機能します。
<!-- In the ZMQ, IRI nodes act as the publisher by adding messages to the queue when certain events happen, whereas clients act as the subscriber by taking those messages from the the queue and processing them. -->

クライアントアプリケーションがZMQイベントを購読すると、ZMQイベントが発生したときにIRIノードがクライアントアプリケーションに通知するので、IOTAネットワークでリアルタイムデータを利用するアプリケーションを作成できます。
<!-- When client applications subscribe to a ZMQ event, the IRI node notifies them when it happens, allowing you to create applications that harness real-time data in an IOTA network. -->

:::info:
クライアントが接続しているIRIノードの[`ZMQ-ENABLED`設定パラメータ](../references/iri-configuration-options.md#zmq-enabled)が`true`に設定されている場合にのみ、クライアントはZMQを購読することができます。
:::
<!-- :::info: -->
<!-- Clients can subscribe to the ZMQ only if the IRI node that they're connected to has the [`ZMQ-ENABLED` configuration parameter](../references/iri-configuration-options.md#zmq-enabled) set to `true`. -->
<!-- ::: -->

## ユースケース例
<!-- ## Example use cases -->

* 1つのアドレスに対して、トランザクションが送信され、確定したことを監視する。
<!-- * Monitor one of your address for when a transaction is sent to it and confirmed -->
* [thetangle.org](https://thetangle.org/)などのタングルビジュアライゼーションWebサイトを作成する。
<!-- * Create a Tangle visualisation website, such as [thetangle.org](https://thetangle.org/) -->
* [例を試してみる](../how-to-guides/subscribe-to-events-in-an-iri-node.md)。
<!-- * [Try our example](../how-to-guides/subscribe-to-events-in-an-iri-node.md) -->
