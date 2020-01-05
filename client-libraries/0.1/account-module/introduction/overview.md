# アカウントモジュール概要
<!-- # Account module overview -->

**アカウントモジュールは、使用済みアドレスからの取り出しやペンディング中のトランザクションのプロモートと再アタッチを心配することなく、IOTA の支払いを簡素化するステートフルパッケージです。**
<!-- **The account module is a stateful package that simplifies IOTA payments without you having to worry about withdrawing from spent addresses or promoting and reattaching pending transactions.** -->

## アカウントモジュールの仕組み
<!-- ## How the account module works -->

アカウントモジュールを使用すると、ローカルデータベースのシードステートを追跡するアカウントオブジェクトを作成できます。
<!-- The account module allows you to build an account object that keeps track of your seed state in a local database. -->

シードステートを確認するには、エクスポートして、バックアップしたり、別のアカウントにインポートしたりできます。
<!-- To see your seed state, you can export it, allowing you to back it up and/or import it into another account. -->

### エクスポートされたシードステートの例
<!-- ### Example of an exported seed state -->

以下は、Go、Java、および JavaScript でエクスポートされたシード状態の例です。
<!-- These are examples of exported seed states in Go, Java, and JavaScript: -->

--------------------
### Go
```json
{
    // カウント ID
    // アカウントIDは、インデックス0およびセキュリティレベル2のアカウントのアドレスのハッシュです
    "id":"9KYMSUEUSOVQN9CPOHVHRNSYTZGBHTWYWR9LGJGYATUMQVNYFQXTEOLEMEACONMAR9AELKPVRCMGQ9MMD",
    // シードステートがエクスポートされた日付
    "date":"2019-12-09T11:32:25.1705032Z",
    // The last key index that was used to generate the addresses.
    // アドレスの生成に使用された最後のキーインデックス
    // これにより、アカウントは新しいアドレスを生成することを確認できます
    "key_index":2,
    // アクティブなアドレス、その条件、およびセキュリティレベル
    // これらにより、アカウントは、まだデポジットを受け取る可能性のあるアドレスからの取り出しを防ぐことができます
    "deposit_addresses":{
        "1":{
            "timeout_at":"2019-12-10T11:29:02.23687483Z",
            "multi_use":true,
            "security_level":2
        },
        "2":{
            "timeout_at":"2019-12-10T11:32:09.549810701Z",
            "multi_use":true,
            "security_level":3
        }
    },
    // すべてのペンディング中の転送バンドルのトライト
    // これらにより、アカウントがペンディング中のトランザクションをモニタリングし、必要に応じて再ブロードキャストまたは再添付できるようにします
    "pending_transfers":{}
}
```
---
### Java
```json
{
    // シードステートがエクスポートされた日付
    "exportedDate":1575890827622,
    // カウント ID
    // アカウントIDは、インデックス0およびセキュリティレベル2のアカウントのアドレスのハッシュです
    "id":"NJJHM9IKUAK9DF9B9WIHYSGQPLOPMJYEWXOYXN9BHCTCLLQKGYFDKFDWNYSQJLFFMJCABVDMG9S9DH9FY",
    "state":{
        // アドレスの生成に使用された最後のキーインデックス
        // これにより、アカウントは新しいアドレスを生成することを確認できます
        "keyIndex":2,
        // アクティブなアドレス、その条件、およびセキュリティレベル
        // これらにより、アカウントは、まだデポジットを受け取る可能性のあるアドレスからの取り出しを防ぐことができます
        "depositRequests":{
            "0":{
                "request":{
                    "timeOut":1575977014862,
                    "multiUse":true,
                    "expectedAmount":0
                    },
                "securityLevel":2
            },
            "1":{
                "request":{
                    "timeOut":1575977051855,
                    "multiUse":true,
                    "expectedAmount":0
                    },
                "securityLevel":3
            }
        },
    // すべてのペンディング中の転送バンドルのトライト
    // これらにより、アカウントがペンディング中のトランザクションをモニタリングし、必要に応じて再ブロードキャストまたは再添付できるようにします
    "pendingTransfers":{},
    // アカウントが新しい（生成されたアドレスはない）かどうか
    "new":false
    }
}
```
---
### JavaScript
```json
{
    // アドレスの生成に使用された最後のキーインデックス
    // これにより、アカウントは新しいアドレスを生成することを確認できます
    "lastKeyIndex": 2,
    // アクティブなアドレス、その条件、およびセキュリティレベル
    // これらにより、アカウントは、まだデポジットを受け取る可能性のあるアドレスからの取り出しを防ぐことができます
    "deposits":[
        {
            "address":"RDRPSRBQPEJFRXXJYOUF9AZKAWOSHKZFDOMXJQHLOFAOMVPTQEKDKDKTKQJQ9QKGQHSJGQQZCHTAVCLUW",
            "index":2,
            "security":3,
            "timeoutAt":1575974515544,
            "multiUse":false,
            "expectedAmount":0
        },
        {
            "address":"YDXSBOKPKVXLCSGAGBLY9XGPQQFHEWDNQIHTXIHIKQGEYVU9RBUPE9GRZMPXNLDRBUZTDOQFF9NIASMYC",
            "index":1,
            "security":2,
            "timeoutAt":1575974481246,
            "multiUse":false,
            "expectedAmount":0
        }],
    // すべてのペンディング中の転送バンドルのトライト
    // これらにより、アカウントがペンディング中のトランザクションをモニタリングし、必要に応じて再ブロードキャストまたは再添付できるようにします
    "withdrawals":[]
}
```
--------------------

## 条件付きデポジットアドレス
<!-- ## Conditional deposit addresses -->

アカウントを使用する多くの利点の1つは、アドレスがアクティブまたは期限切れになる条件を定義できることです。これらの条件は、送信者がトークンをアドレスに送信しても安全かどうかを判断するのに役立ちます。このため、アカウントのアドレスは_条件付きデポジットアドレス_（CDA）と呼ばれます。
<!-- One of the many benefits of using accounts is that you can define conditions in which your addresses are active or expired. These conditions help senders to decide whether it's safe to send tokens to an address. For this reason, addresses in accounts are called _conditional deposit addresses_ (CDA). -->

アカウントは CDA を使用して、[使用済みアドレス](root://getting-started/0.1/clients/addresses.md#spent-addresses)からの取り出しのリスクを軽減します。誰かから IOTA トークンをリクエストすると、一定期間アクティブな CDA を作成できます。このようにして、その時間後にのみそのアドレスから取り出しするつもりであることを送信者に知らせます。その結果、送信者は、CDA に残っている時間に応じて、デポジットを行うかどうかを決定できます。
<!-- Accounts use CDAs to help reduce the risk of withdrawing from [spent addresses](root://getting-started/0.1/clients/addresses.md#spent-addresses). When you request IOTA tokens from someone, you can create a CDA that's active for a certain period of time. This way, you let the sender know that you intend to withdraw from that address only after that time. As a result, the sender can decide whether to make a deposit, depending on how much time is left on a CDA. -->

### CDA の条件
<!-- ### Conditions of a CDA -->

CDA を作成するとき、`timeout_at` フィールドを指定して、CDA がアクティブか期限切れかを定義する必要があります。
<!-- When you create a CDA, you must specify the `timeout_at` field to define whether it's active or expired. -->

次の推奨フィールドのいずれかを指定することもできます。
<!-- You can also specify one of the following recommended fields: -->

- **multi_use（推奨）：**アドレスが複数のデポジットを受け取るかどうかを指定するブール値。
<!-- - **multi_use (recommended):** A boolean that specifies if the address may receive more than one deposit. -->
- **expected_amount（推奨）：**アドレスが受け取ると予想される IOTA トークンの量。アドレスにこの金額が含まれている場合、期限切れと見なされます。この条件を指定することをお勧めします。
<!-- - **expected_amount (recommended):** The amount of IOTA tokens that the address is expected to receive. When the address contains this amount, it's considered expired. We recommend specifying this condition. -->

|  **フィールドの組合せ** | **取り出し条件**
| :----------| :----------|
|`timeout_at` |CDA は、IOTA トークンが含まれている限り、取り出しに使用できます。|
|`timeout_at` と `multi_use` (推奨) |CDA は、デポジットの回数に関係なく、有効期限が切れ次第取り出しに使用できます。|
|`timeout_at` と `expected_amount` (推奨) |CDA は、予想金額が含まれるとすぐに取り出しに使用できます。|

<!-- |  **Combination of fields** | **Withdrawal conditions** -->
<!-- | :----------| :----------| -->
<!-- |`timeout_at` |The CDA can be used in withdrawals as long as it contains IOTA tokens| -->
<!-- |`timeout_at` and `multi_use` (recommended) |The CDA can be used in withdrawals as soon as it expires, regardless of how many deposits were made to it| -->
<!-- |`timeout_at` and `expected_amount` (recommended) | The CDA can be used in withdrawals as soon as it contain the expected amount| -->

:::warning:警告！
CDA が `timeout_at` フィールドのみで作成された場合、有効期限が切れていなくても残高がゼロより大きくなるとすぐに期限切れになります。したがって、使用済みアドレスからの取り出しを避けるために、可能な場合はいつでも、`multi_use` フィールドまたは `expected_amount` フィールドのいずれかを使用して CDA を作成することをお勧めします。
:::
<!-- :::warning:Warning -->
<!-- If a CDA was created with only the `timeout_at` field, it can be used in withdrawals as soon as it has a non-zero balance even if it hasn't expired. Therefore, to avoid withdrawing from a spent address, we recommend creating CDAs with either the `multi_use` field or with the `expected_amount` field whenever possible. -->
<!-- ::: -->

### CDA 作成のアドバイス
<!-- ### Advice for creating CDAs -->

CDA を作成するときは、次の質問を考慮する必要があります。
<!-- When creating a CDA, you should consider the following questions. -->

#### CDA のタイムアウトで指定する時間は？
<!-- #### How long should you specify in a CDA's timeout? -->

`timeout_at` フィールドで指定する値は、送信者がデポジットを行う速度に依存します。送信者と直接連絡していて、両方が送金の決済を待っている場合は、より短いタイムアウトを指定できます。
<!-- The value that you specify in the `timeout_at` field depends on how fast you expect the depositor to make a deposit. If you are in direct contact with the depositor and you are both waiting to settle the transfer, you can specify a shorter timeout. -->

:::danger:重要！
CDA が `timeout_at` フィールドのみで作成された場合、期限が切れていなくても残高がゼロより大きくなるとすぐに期限切れになります。そのため、使用済みアドレスからの取り出しを避けるために、可能な場合はいつでも `multi_use` フィールドまたは `expected_amount` フィールドを使用して CDA を作成することをお勧めします。
:::
<!-- :::danger:Important -->
<!-- If a CDA was created with only the `timeout_at` field, it can be used in outgoing payments as soon as it has a non-zero balance even if it hasn't expired. So, to avoid withdrawing from a spent address, we recommend creating CDAs with either the `multi_use` field or with the `expected_amount` field whenever possible. -->
<!-- ::: -->

#### `multi_use` CDA をいつ作成する必要があるか？
<!-- #### When should you create a multi-use CDA? -->

次の条件に該当する場合は、`multi_use` CDA を使用することをお勧めします。
<!-- We recommend that you use multi-use CDAs when the following conditions are true: -->

- あなたが、複数の送信者から任意の量の複数の支払いを期待している。
<!-- - You expect multiple payments of arbitrary value from multiple depositors. -->
- Web サイトなどで、CDA の作成と共有を完全に制御している。
<!-- - You fully control the creation and sharing of the CDA, for example on your website. -->
- `expected_amount` フィールドの値が設定された CDA を各送信者と個別に共有できない場合。
<!-- - When you cannot share a CDA with the `expected_amount` field value set with each depositor individually. -->

または、`timeout_at` フィールドの値がなくなったときに新しい CDA を確実にリクエストする送信者と通信する場合。
<!-- Or when communicating with depositors who reliably request a new CDA when the `timeout_at` field value is running out. -->

`multi_use` CDA アドレスのシナリオの1つは、Web サイトまたは画面などの他のデジタルメディアで寄付アドレスを共有するときなどです。マルチユース CDA アドレスを使用したシナリオでは、任意の量の複数のデポジットを受け取り、CDA の共有を完全に制御できます。CDAが `timeout_at` の値がなくなる72時間前になるたびに、Web サイトまたは画面で CDA を更新できます。これにより、送信者は、CDA を更新する前に送金した可能性のある支払いを完了するのに十分な時間を確保できます。
<!-- One scenario for `multi_use` CDA addresses is sharing a donation address on a website or other digital medium, such as a screen. In this scenario, you can receive multiple deposits of arbitrary value and you fully control the sharing of the CDA. You can refresh the CDA on the website or screen each time the CDA is 72 hours before its `timeout_at` value runs out. This gives depositors enough time to finalize the payments they may have sent before you refreshed the CDA. -->

#### `expected_amount` CDA をいつ作成する必要があるか？
<!-- #### When should you create a CDA with an expected amount? -->

デポジットの値が送信者と受信者の両方の観点から明らかな場合、`expected_amount` フィールドの値を指定する必要があります。たとえば、取引所から取り出したい場合、取り出したい量の `expected_amount` フィールドを持つ CDA を取引所に与えることができます。
<!-- You should specify the value of the `expected_amount` field when the value of the deposit is clear from both the depositor's and the receiver's point of view. For example, when you want to withdraw from an exchange, you can give the exchange a CDA with the expected amount that you want to withdraw. -->
