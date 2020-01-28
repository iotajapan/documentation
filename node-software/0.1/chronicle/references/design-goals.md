# 設計目標
<!-- # Design goals -->

**クロニクルは、ID データなどのユースケースを満たすための一連の設計目標に従って開発されています。**
<!-- **Chronicle is being developed according to a set of design goals that allow it to fulfill use cases such as identity data.** -->

## データ整合性の目標
<!-- ## Data integrity goals -->

IOTA のデータ整合性は、コンセンサスを通じて検証されます。クロニクルからのデータは、信頼できるノードのクォーラムを照会し、その結果を比較することで検証できます。
<!-- Data integrity in IOTA is verified through consensus. Data from Chronicle can be verified by querying a quorum of trusted nodes then comparing their results. -->

## セキュリティの目標
<!-- ## Security goals -->

セキュリティ保護はリスクに基づいて追加されます。承認、認証、物理的セキュリティ、暗号化などの保護が対処されていますが、サイバーセキュリティは継続的なプロセスです。たとえば、クロニクルノードはランダムにクエリできるため、攻撃者はどのノードがクエリされているかを見つけるのが困難になります。これにより、特定のノードに対する標的型攻撃を防ぐことができます。
<!-- Security protections are added based on risk. Protections such as authorization, authentication, physical security, and encryption have been addressed, but cybersecurity is an ongoing process. For example, Chronicle nodes can be queried randomly so attackers would have a difficult time discovering which nodes are being queried. This helps to prevent targeted attacks on specific nodes. -->

Elixir と ScyllaDB には保護機能が組み込まれていますが、管理者アカウントを保護するには、ネットワークセキュリティ、サーバーセキュリティ、ソーシャルエンジニアリングトレーニング、内部統制などのセキュリティ層が必要です。
<!-- Elixir and ScyllaDB have built-in protection, but securing the administrator accounts requires layers of security, including network security, server security, social engineering training, and internal controls. -->

## パフォーマンスの目標
<!-- ## Performance goals -->

クロニクルは分散化されているため、単一障害点がありません。これにより、ノードに到達できない場合のデータ損失と使用不能が回避されます。
<!-- Chronicle is distributed so it has no single point of failure. This avoids data loss and unavailability in case a node is not reachable. -->

クロニクルは、短時間で大量のトランザクションを保存するよう努めています。たとえば、300,000台の車両が5分ごとに1つのトランザクションを送信できるとします。過去のトランザクションは、最先端のクラウドソリューションに匹敵する時間内に取得する必要があります。クロニクル API を使用することは、トランザクションを検索してデータをすばやく取得するための最良の方法です。
<!-- Chronicle strives to save a large number of transactions in a short time frame.  For example, suppose 300,000 vehicles can send one transaction every 5 minutes. -->
<!-- Past transactions should be retrieved within a time comparable to state of the art cloud solutions. Using the Chronicle API is the best way to search for transactions and retrieve data quickly. -->

## ビジネルの目標
<!-- ## Business goals -->

組織は、Pay-Per-Request サービスを提供する場合があります。初期価格を固定してから、公正市場価値に基づいた手数料に置き換えることができます。
<!-- Organizations may offer a pay-per-request service. The initial price can be fixed then replaced by a fee based on fair market value. -->
