# アカウントモジュール概要
<!-- # Account module overview -->

**アカウントは、トランザクションの送受信を簡単にするためのオブジェクトです。アカウントは、アドレスやペンディング中のバンドルハッシュなどのデータをローカルデータベースに保存します。このデータにより、使用済みアドレスからの取り出しやペンディング中のトランザクションの促進や再添付を心配せずにIOTAネットワークと対話できます。**
<!-- **An account is an object that makes it easier to send and receive transactions. Accounts store data such as addresses and pending bundle hashes in a local database. This data allows you to interact with an IOTA network without worrying about withdrawing from spent addresses or promoting and reattaching pending transactions.** -->

アカウントはIOTAプロトコルの複雑さを抽象化し、支払いの実行に集中できるようにします。アカウントでは、2種類の支払いがあります。
<!-- Accounts abstract the complexity of the IOTA protocol and allow you to focus on making payments. In accounts, we have two types of payment: -->

* **入金：** アカウントにIOTAトークンを預け入れるバンドル
<!-- * **Incoming payment:** A bundle that deposits IOTA tokens into an account -->
* **出金：** アカウントからIOTAトークンを取り出すバンドル
<!-- * **Outgoing payment:** A bundle that withdraws IOTA tokens from an account -->

## 条件付き預け入れアドレス
<!-- ## Conditional deposit addresses -->

IOTAプロトコルでは、IOTAトークンを[シード](root://getting-started/0.1/introduction/what-is-a-seed.md)から導出された[アドレス](root://iota-basics/0.1/concepts/addresses-and-signatures.md)に送信する必要があります。これらのアドレスからは一度だけIOTAトークンを取り出すことができます。故に、すべての人が一度もIOTAトークンを取り出していないアドレスへ預け入れることが重要です。ただし、IOTAトークンを取り出す前に、誰がいつIOTAトークンをアドレスに預け入れようとしているのかを知ることは困難です。
<!-- In the IOTA protocol, IOTA tokens must be sent to [addresses](root://iota-basics/0.1/concepts/addresses-and-signatures.md), which are derived from your [seed](root://getting-started/0.1/introduction/what-is-a-seed.md). These addresses may be withdrawn from only once. As a result, it's important that no one deposits IOTA tokens into a withdrawn address. But, it's difficult to know when or if someone is going to deposit IOTA tokens into your address before you withdraw from it. -->

アカウント内のアドレスには、支払いに使用される可能性がある条件を指定できるようにする追加機能が付属しています。これらのアドレスは、条件付き預け入れアドレス（CDA）と呼ばれます。
<!-- In accounts, addresses come with extra features that allow you to specify the conditions in which they may be used in payments. These addresses are called conditional deposit addresses (CDA). -->

アカウントはCDAを使用して、[使用済みアドレスからの取り出しリスク](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse)を軽減します。誰かにIOTAトークンを要求すると、一定期間アクティブなCDAを作成できます。このようにして、送信者にその期間の後にだけそのアドレスから取り出すつもりであることを知らせます。その結果、送信者は、CDAの残り時間に応じて、預け入れるかどうかを決定できます。
<!-- Accounts use CDAs to help reduce the [risk of withdrawing from spent addresses](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse). When you request IOTA tokens from someone, you can create a CDA that's active for a certain period of time. This way, you let the sender know that you intend to withdraw from that address only after that time. As a result, the sender can decide whether to make a deposit, depending on how much time is left on a CDA. -->

### CDAの条件
<!-- ### Conditions of a CDA -->

CDAを作成するときには、CDAがアクティブか期限切れかを定義する次の条件を指定します。アクティブアドレスにはIOTAトークンを預け入れることができますが、取り出すことはできません。逆に期限切れアドレスからはIOTAトークンを取り出すことができますが、預け入れることはできません。
<!-- When you create a CDA, you specify the following condition, which defines whether it's active or expired. You can deposit IOTA tokens into an active address. But, you can't withdraw tokens from an active address. You can withdraw tokens from an expired address. But, you can't deposit tokens into an expired address. -->

* **timeoutAt（必須）：** アドレスが期限切れになる時刻。
<!-- * **timeoutAt (required):** The time at which the address expires -->

以下の推奨フィールドのいずれかを指定することもできます。
<!-- You can also specify one of the following recommended fields: -->

* **multiUse（推奨）：** アドレスに複数の預け入れを許可するかどうかを指定するブール値。
<!-- * **multiUse (recommended):** A boolean that specifies if the address may receive more than one deposit. -->
* **expectedAmount（推奨）：** アドレスが受信すると予想されるIOTAトークンの量。アドレスにこの量のIOTAトークンが含まれた場合、期限切れと見なされます。この条件を指定することをお勧めします。
<!-- * **expectedAmount (recommended):** The amount of IOTA tokens that the address is expected to receive. When the address contains this amount, it's considered expired. We recommend specifying this condition. -->

:::info:
CDAに`expected_amount`フィールドと`multi_use`フィールドを同時に指定することはできません。詳細については[FAQ](../references/cda-advice.md)を参照してください。
:::
<!-- :::info: -->
<!-- You can't specify the `expected_amount` and `multi_use` fields in the same CDA. Please refer to the [FAQ](../references/cda-advice.md) for more advice about CDA conditions. -->
<!-- ::: -->

| **フィールドの組合せ** | **IOTAトークンを取り出せる条件** |
| :--------------------- | :------------------------------- |
| `timeoutAt` | CDAは、IOTAトークンが含まれている限り、取り出しに使用できます。 |
| `timeoutAt`と`multiUse`（推奨） | CDAは、預け入れられた量に関係なく、有効期限が切れるとすぐに取り出しに使用できます。 `multiUse`フィールドが設定されたアドレスをいつ使用するかについては、[CDA FAQ](../references/cda-advice.md)を参照してください。 |
| `timeoutAt`と`expectedAmount`（推奨） | CDAは、予想された量が含まれるとすぐに取り出しに使用できます。 `expectedAmount`フィールドが設定されたアドレスをいつ使用するかについては、[CDA FAQ](../references/cda-advice.md)を参照してください。 |

<!-- |  **Combination of fields** | **Withdrawal conditions** -->
<!-- | :----------| :----------| -->
<!-- |`timeoutAt` |The CDA can used in withdrawals as long as it contains IOTA tokens| -->
<!-- |`timeoutAt` and `multiUse` (recommended) |The CDA can be used in withdrawals as soon as it expires, regardless of how many deposits were made to it. See the [CDA FAQ](../references/cda-advice.md) on when to use addresses with the `multiUse` field set. | -->
<!-- |`timeoutAt` and `expectedAmount` (recommended) | The CDA can be used in withdrawals as soon as it contain the expected amount. See the [CDA FAQ](../references/cda-advice.md) on when to use addresses with the `multi_use` field set.| -->

:::warning:警告
`timeoutAt`フィールドのみを使用してCDAを作成した場合、期限切れになっていなくてもゼロではない残高になるとすぐに取り出しに使用できます。

使用済みアドレスからの取り出しを避けるために、可能であれば、`multiUse`フィールドまたは`expectedAmount`フィールドのいずれかを`timeoutAt`と併用してCDAを作成することをお勧めします。
:::
<!-- :::warning:Warning -->
<!-- If a CDA was created with only the `timeoutAt` field, it can be used in withdrawals as soon as it has a non-zero balance even if it hasn't expired. -->
<!--  -->
<!-- To avoid withdrawing from a spent address, we recommend creating CDAs with either the `multiUse` field or with the `expectedAmount` field whenever possible. -->
<!-- ::: -->

## シードステート
<!-- ## Seed state -->

支払いを追跡するために、アカウントはすべてのCDAのステートをローカルデータベースに保存します。このステートはシードステートと呼ばれます。
<!-- To keep track of payments, accounts store the state of all CDAs in a local database. This state is called the seed state. -->

アカウントはこのデータを使用してアクティビティの履歴を保持し、ノードへの不要なAPI呼び出しを回避します。
<!-- Accounts use this data to keep a history of activity and to avoid making unnecessary API calls to nodes. -->

| **データ** | **目的** |
| :--------- | :------- |
| CDAの作成に使用された最後のキーインデックス | これまで使用されたことのない新しいCDAを作成する |
| すべてのアクティブなCDA | IOTAトークンの預け入れを受け取る可能性のあるCDAからのIOTAトークンの取り出しを停止する |
| ペンディング中の転送バンドル | ペンディング中のトランザクションを監視して、必要に応じて再ブロードキャストするか再添付を行う |

<!-- |**Data**| **Purpose**| -->
<!-- |:-----------------|:----------| -->
<!-- |The last key index that was used to create a CDA| Create a new CDA that has never been used before| -->
<!-- |All active CDAs|Stop withdrawals from CDAs that may receive deposits| -->
<!-- |Pending transfers| Monitor pending transactions and rebroadcast or reattach them if necessary| -->
