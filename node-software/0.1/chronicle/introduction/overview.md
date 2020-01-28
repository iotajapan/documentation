# クロニクル概要
<!-- # Chronicle overview -->

**クロニクルは、[IRI ノード](root://node-software/0.1/iri/introduction/overview.md)に到達するすべてのトランザクションを安全で拡張性の高い分散データベースに保存できるパーマノードソリューションです。**
<!-- **Chronicle is a permanode solution that allows you to store all transactions that reach an [IRI node](root://node-software/0.1/iri/introduction/overview.md) in a distributed database that's secure and that scales well.** -->

:::info:
クロニクルは、IOTA テクノロジースタックの将来の戦略に合わせて Rust に移植されます。
:::
<!-- :::info: -->
<!-- Chronicle will be ported to Rust to align with the future strategy of the IOTA technology stack. -->
<!-- ::: -->

クロニクルは、`tx_trytes` [ZMQ イベント](root://node-software/0.1/iri/references/zmq-events.md)を介して IRI ノードからトランザクションを受信します。クロニクルはトランザクションを受信すると、[Elixir](https://elixir-lang.org/) アンブレラプロジェクトを介してトランザクションを処理し、[ScyllaDB](https://www.scylladb.com/) に保存します。
<!-- Chronicle receives transactions from IRI nodes through the `tx_trytes` [ZMQ event](root://node-software/0.1/iri/references/zmq-events.md). When Chronicle receives transactions, it processes them through an [Elixir](https://elixir-lang.org/) umbrella project, then it stores them in [ScyllaDB](https://www.scylladb.com/). -->

ScyllaDB は、パーティション分割、レプリケーション、メモリ内処理、一貫性などのビッグデータの問題を処理します。
<!-- ScyllaDB takes care of the big data concerns such as partitioning, replication, in-memory processing, and consistency. -->

Elixir は、Web 開発ツールと組み込みソフトウェア開発ツールに加えて、マイクロサービスを構築することで拡張できるネットワークを提供します。
<!-- Elixir provides web development tools and embedded software development tools plus a network that can be extended by building microservices. -->

クロニクルノードにデータリクエストを送信すると、ScyllaDB からデータがリクエストされ、レスポンスがフォーマットされて返されます。
<!-- When you send a data request to a Chronicle node, it requests the data from the ScyllaDB, then formats and returns you the response. -->

![Chronicle architecture](../images/architecture.png)

## ScyllaDB の仕組み
<!-- ## How ScyllaDB works -->

[ScyllaDB](https://docs.scylladb.com/using-scylla/) は、高スループットと低遅延を特徴とするリアルタイムのビッグデータデータベースです。
<!-- [ScyllaDB](https://docs.scylladb.com/using-scylla/) is a real-time, big data database featuring high throughput and low latency. -->

データは、[プライマリキー、パーティションキー、およびクラスタリングキー](http://sudotutorials.com/how-to-guides/cassandra/cassandra-primary-key-cluster-key-partition-key.html)を使用して、テーブル内の行と列に編成されます。
<!-- Data is organized into rows and columns in a table, using the [primary key, the partition key, and the clustering key](http://sudotutorials.com/how-to-guides/cassandra/cassandra-primary-key-cluster-key-partition-key.html). -->

プライマリキーは、テーブル内の各行の一意の識別子です。パーティションキーは、データの行を保存するノードを示します。クラスタリングキーは、データをパーティションにソートし、列の順序付けに影響します。
<!-- The primary key is a unique identifier for each row in a table. A partition key indicates which nodes store a row of data. Clustering keys sort data into a partition and affect how columns are ordered. -->

データがシャードと呼ばれる別個のコレクションに編成されると、データの保存と取得が高速になります。Scylla パーティションは、パーティションキーで識別される行を保持する論理ストレージユニットです。シャードは、同じパーティションキーを持つデータのグループです。
<!-- Storing and retrieving data is faster when the data is organized into distinct collections called shards. A Scylla partition is a logical storage unit that holds the rows identified by a partition key. A shard is a group of data with the same partition key. -->

信頼性と耐障害性を確保するために、Scylla はデータレプリカを複数のノードに保存します。これらのノードはレプリカノードと呼ばれます。パーティションはレプリカノードで繰り返されます。複製係数（RF）を設定することにより、レプリカの数を設定できます。
<!-- To ensure reliability and fault tolerance, Scylla stores data replicas on multiple nodes. These nodes are called replica nodes. Partitions are repeated on replica nodes. You can set the number of replicas by setting the replication factor (RF). -->

:::info:
[Scylla のフォールトトレランスの詳細](https://docs.scylladb.com/architecture/architecture-fault-tolerance/)をご覧ください。
:::
<!-- :::info: -->
<!-- [Learn more about fault tolerance in Scylla](https://docs.scylladb.com/architecture/architecture-fault-tolerance/). -->
<!-- ::: -->

![Data flow in Chronicle](../images/dataflow.png)

## 制限事項
<!-- ## Limitations -->

現時点では、トランザクションをフィルタリングし、それらのサブセットのみを保存することはできません。クロニクルを実行するとき、ノードが受信するすべてのトランザクションを保存する必要があります。
<!-- At the moment, it's not possible to filter transactions and store only a subset of them. When you run Chronicle, you must store all transactions that your node receives. -->

## ブログポスト
<!-- ## Blog posts -->

クロニクルに関する次のブログ投稿を読んでください。
<!-- Read the following blog posts about Chronicle: -->

- [Introducing Chronicle - A Permanode Solution](https://blog.iota.org/introducing-chronicle-a-permanode-solution-8e506a2e0813)

## リポジトリ
<!-- ## Repository -->

[GitHub](https://github.com/iotaledger/chronicle) 上のクロニクルのソースコードを参照してください。
<!-- Go to the Chronicle source code on [Github](https://github.com/iotaledger/chronicle). -->

## Discord チャンネル
<!-- ## Discord channels -->

[公式の Discord チャンネルに参加する](https://discord.iota.org)と、次のことができます。
<!-- [Join our Discord channel](https://discord.iota.org) where you can: -->

- IOTA 開発者およびコミュニティとの議論に参加する。
<!-- - Take part in discussions with IOTA developers and the community -->
- 助けを求める。
<!-- - Ask for help -->
- 他の人を助けるためにあなたの知識を共有する。
<!-- - Share your knowledge to help others -->

クロニクルに関するチャンネルは以下の2つです。
<!-- We have the following channels for Chronicle: -->

- **#chronicle-dev：**開発者がトピックについて議論し、GitHub の更新が表示される読み取り専用チャンネル
<!-- - **#chronicle-dev:** A read-only channel where developers discuss topics and where any GitHub updates are displayed -->

- **#chronicle-discussion：**誰でも自由にクロニクルについて議論できるオープンチャンネル
<!-- - **#chronicle-discussion:** An open channel where anyone is free to discuss Chronicle -->

## 次のステップ
<!-- ## Next steps -->

[クロニクルを実行](../how-to-guides/get-started.md)して、トランザクションの保存を開始します。
<!-- [Run Chronicle](../how-to-guides/get-started.md) to get started with storing transactions. -->
