# 用語集
<!-- # Glossary -->

**この用語集には、IOTA 固有の定義が含まれています。**
<!-- **This glossary contains definitions specific to IOTA.** -->

用語はカテゴリごとにアルファベット順にリストされています。
<!-- Terms are listed in alphabetical order by category. -->

### 頭字語
<!-- ### Acronyms -->

<dl><dt>CTPS</dt><dd>1秒あたりの確定済みトランザクション数</dd></dl>
<!-- <dl><dt>CTPS</dt><dd>Number of Confirmed Transactions Per Second</dd></dl> -->

<dl><dt>TPS</dt><dd>[IRI ノード](#iota-terms)を介した1秒あたりのトランザクション処理数</dd></dl>
<!-- <dl><dt>TPS</dt><dd>Number of Transactions Per Second processing through an [IRI node](#iota-terms)</dd></dl> -->

### 暗号法
<!-- ### Cryptography -->

<dl><dt>Curl</dt><dd>IOTA で使用されるメインハッシュ関数。Curl はスポンジ構造に基づいており、これは [Keccak creators](https://en.wikipedia.org/wiki/SHA-3)（SHA-3）によって発明されました。Curl は、モノのインターネット（IoT）用に設計されています。</dd></dl>
<!-- <dl><dt>Curl</dt><dd>Main hash function that's used in IOTA. Curl is based on a sponge construction, which was invented by the [Keccak creators](https://en.wikipedia.org/wiki/SHA-3) (SHA-3). Curl is designed for the Internet of things (IoT). </dd></dl> -->

<dl><dt>Kerl</dt><dd>[2の補数](https://en.wikipedia.org/wiki/Two%27s_complement)を使用して、243トリットから48バイトへの入出力の追加変換を含む Keccek-384 ハッシュ関数。</dd></dl>
<!-- <dl><dt>Kerl</dt><dd>Keccek-384 hash function that includes the additional conversion of its input and output from/to 243 trits to 48 bytes, using [two's complement](https://en.wikipedia.org/wiki/Two%27s_complement).</dd></dl> -->

<a name="iota-terms"></a>
## IOTA 用語
<!-- ## IOTA terms -->

<dl><dt>たんぽぽ</dt><dd>多数のチップトランザクションに囲まれた1つの中央トランザクションで構成されるタングル形成。</dd></dl>
<!-- <dl><dt>blowball</dt><dd>Tangle formation that consists of one central transaction, which is surrounded by a large number of tip transactions.</dd></dl> -->

<dl><dt>クライアント</dt><dd>IRI ノードに接続して IOTA ネットワークと対話し、トランザクションを送信するソフトウェアを実行するデバイス。</dd></dl>
<!-- <dl><dt>client</dt><dd>Device running software that connects to an IRI node to interact with an IOTA network and send transactions.</dd></dl> -->

<a name="subgraph"></a><dl><dt>サブグラフ</dt><dd>マイルストーントランザクションとチップトランザクションの間のすべてのトランザクションを含む台帳のセクション</dd></dl>
<!-- <a name="subgraph"></a><dl><dt>subgraph</dt><dd>Section of the ledger that contains all transactions between a milestone transaction and tip transactions</dd></dl> -->

<a name="transactions"></a>
### トランザクション
<!-- ### Transactions -->

<dl><dt>承認者</dt><dd>トランクトランザクションまたはブランチトランザクションとして別のトランザクションを直接参照するトランザクション</dd></dl>
<!-- <dl><dt>approver</dt><dd>Transaction that directly references another transaction as its trunk transaction or branch transaction</dd></dl> -->

<dl><dt>未来集合</dt><dd>同じトランザクションを承認するトランザクションのグループ</dd></dl>
<!-- <dl><dt>future set</dt><dd>Group of transactions that approve the same transaction</dd></dl> -->

<dl><dt>不整合・矛盾</dt><dd>利用できない資金の取り出しにつながるトランザクションの状態</dd></dl>
<!-- <dl><dt>inconsistent</dt><dd>State of a transaction when it leads to withdrawing non-available funds</dd></dl> -->

<dl><dt>包含状態</dt><dd>[IRI ノード](#iota-terms)がトランザクションの受け入れまたはトランザクションの確定を確認するために実行するプロセス。トランザクションとチップトランザクションのリストが与えられた場合、チップトランザクションがそのトランザクションを参照している場合に包含状態は `true` です。</dd></dl>
<!-- <dl><dt>inclusion state</dt><dd>Process that an [IRI node](#iota-terms) performs to check either the acceptance of a transaction or the confirmation of a transaction. Given a transaction and a list of tip transactions, the inclusion state is true if the tip transactions reference that transaction.</dd></dl> -->

<dl><dt>無効</dt><dd>トランザクションが非凝固、不整合、または古すぎるトランザクションを参照している場合のトランザクションの状態。</dd></dl>
<!-- <dl><dt>invalid</dt><dd>State of a transaction when it's either non-solid, inconsistent, or it references a transaction that's too old.</dd></dl> -->

<dl><dt>ソリッド・凝固済み</dt><dd>履歴全体（すべての直接的および間接的に参照されるトランザクション）がIRI ノードの台帳にある場合のトランザクションの状態</dd></dl>
<!-- <dl><dt>solid</dt><dd>State of a transaction when an IRI node has its entire history (all directly and indirectly referenced transactions) in its ledger</dd></dl> -->

<dl><dt>末尾トランザクション</dt><dd>バンドル内のトランザクション0。[IRIノード](#iota-terms) は各末尾トランザクションのトランクトランザクションを走査することにより、バンドルを再構築および検証します。</dd></dl>
<!-- <dl><dt>tail transaction</dt><dd>Transaction 0 in a bundle. [IRI nodes](#iota-terms) reconstruct and validate bundles by traversing the trunk transaction of each tail transaction.</dd></dl> -->

<dl><dt>重み付きランダムウォーク</dt><dd>[ノード](#iota-terms)がチップ選択中に使用して、台帳のチップトランザクションへのパスを見つけるアルゴリズム。</dd></dl>
<!-- <dl><dt>weighted random walk</dt><dd>Algorithm that a [node](#iota-terms) uses during tip selection to find a path to a tip transaction in the ledger.</dd></dl> -->
