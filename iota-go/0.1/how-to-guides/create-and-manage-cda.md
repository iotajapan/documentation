# トランザクションを送受信する
<!-- # Send and receive transactions -->

**アカウントでトランザクションを送受信するには、条件付預け入れアドレス（CDA）を使用する必要があります。CDAは、アカウント内のIOTAトークンの取り出しおよび預け入れに使用される可能性がある条件を指定できるようにした特別なアドレスです。**
<!-- **To send and receive transactions with an account, you must use conditional deposit addresses (CDA). CDAs are special addresses that allow you to specify the conditions in which they may be used in account withdrawals and deposits.** -->

アカウントはCDAを使用して、[署名済みアドレスの再利用](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse)のリスクを軽減します。誰かにIOTAトークンを要求すると、一定期間アクティブなCDAを作成できます。このようにして、あなたは送信者にあなたがその時間の後にだけCDAからIOTAトークンを取り出すつもりであることを知らせます。その結果、送信者は、CDAの残り時間に応じて、IOTAトークンを預け入れるかどうかを決定できます。
<!-- Accounts use CDAs to reduce the [risks associated with spent addresses](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse). When you request IOTA tokens from someone, you can create a CDA that's active for a certain period of time. This way, you let the sender know that you intend to withdraw from that address only after that time. As a result, the sender can decide whether to make a deposit, depending on how much time is left on a CDA. -->

:::info:
CDAはアカウント内でのみ使用でき、汎用[クライアントライブラリメソッド](root://client-libraries/0.1/introduction/overview.md)では使用できません。その結果、CDAを使用できるようにするには、あなたと送信者の両方にアカウントが必要です。
:::
<!-- :::info: -->
<!-- CDAs can be used only in an account and not in the generic [client library methods](root://client-libraries/0.1/introduction/overview.md). As a result, both you and the sender must have an account to be able to use CDAs. -->
<!-- ::: -->

## CDAを作成する
<!-- ## Create a CDA -->

CDAを作成するには、CDAがアクティブか期限切れかを定義する次の条件を指定します。
<!-- To create a CDA, specify the following condition, which defines whether it's active or expired: -->

* **timeoutAt（必須）：** アドレスが期限切れになる時刻。
<!-- * **timeout_at (required):** The time at which the address expires -->

と、以下の推奨フィールドのうちの1つを指定できます。
<!-- And one of the following, recommended fields: -->

* **multiUse（推奨）：** アドレスに複数の預け入れを許可するかどうかを指定するブール値。
<!-- * **multi_use (recommended):** A boolean that specifies if the address may be sent more than one deposit. -->
* **expectedAmount（推奨）：** アドレスに含まれると予想されるIOTAトークンの量。アドレスにこの量のIOTAトークンが含まれている場合、期限切れと見なされます。この条件を使用することを強くお勧めします。
<!-- * **expected_amount (recommended):** The amount of IOTA tokens that the address is expected to contain. When the address contains this amount, it's considered expired. We highly recommend using this condition. -->

:::info:
CDAに`expected_amount`フィールドと`multi_use`フィールドを同時に指定することはできません。詳細については[FAQ](../references/cda-faq.md)を参照してください。
:::
<!-- :::info: -->
<!-- You can't specify the `expected_amount` and `multi_use` fields in the same CDA. Please refer to the [FAQ](../references/cda-faq.md) for more information. -->
<!-- ::: -->

| **フィールドの組合せ** | **IOTAトークンを取り出せる条件** |
| :--------------------- | :----------------------------- |
| `timeoutAt` | CDAは、IOTAトークンが含まれている限り、取り出しに使用できます。 |
| `timeoutAt`と`multiUse`（推奨） | CDAは、預け入れられた量に関係なく、有効期限が切れるとすぐに取り出しに使用できます。 `multiUse`フィールドが設定されたアドレスをいつ使用するかについては、[CDA FAQ](../references/cda-faq.md)を参照してください。 |
| `timeoutAt`と`expectedAmount`（推奨） | CDAは、予想された量が含まれるとすぐに取り出しに使用できます。 `expectedAmount`フィールドが設定されたアドレスをいつ使用するかについては、[CDA FAQ](../references/cda-faq.md)を参照してください。 |

:::warning:注意
`timeoutAt`フィールドのみを使用してCDAを作成した場合は、期限切れになっていなくてもゼロではない残高になるとすぐに取り出しに使用できます。

署名済みアドレスの再利用を避けるために、可能であれば、`multiUse`フィールドまたは`expectedAmount`フィールドのいずれかを`timeoutAt`と併用してCDAを作成することをお勧めします。
:::
<!-- :::warning:Warning -->
<!-- If a CDA was created with only the `timeout_at` field, it can be used in withdrawals as soon as it has a non-zero balance even if it hasn't expired. -->
<!--  -->
<!-- To avoid withdrawing from a spent address, we recommend creating CDAs with either the `multi_use` field or with the `expected_amount` field whenever possible. -->
<!-- ::: -->

## 前提条件
<!-- ## Prerequisites -->

[新しいアカウントを作成](../how-to-guides/create-account.md)してください。
<!-- [Create a new account](../how-to-guides/create-account.md). -->

このガイドは[概要の「はじめに」](../README.md)で紹介されている、プロジェクトの依存関係を管理するための[Goモジュール](https://github.com/golang/go/wiki/Modules)を使っていると仮定します。
<!-- This guide assumes that you've followed our [Getting started guide](../README.md) and are using the [Go modules](https://github.com/golang/go/wiki/Modules) to manage dependencies in your project. -->

## 新しいCDAを作成する
<!-- ## Create a new CDA -->

1. 必要なパッケージをインポートします。
  <!-- 1. Import the required packages -->

    ```go
    "time"

    "github.com/iotaledger/iota.go/account/deposit"
    "github.com/iotaledger/iota.go/account/oracle"
    oracle_time "github.com/iotaledger/iota.go/account/oracle/time"
    ```

2. アカウントのタイムソースオブジェクトから現在の時刻を保存します。
  <!-- 2. Store the current time from your account's timesource object -->

    ```go
    // get current time
    now, err := timesource.Time()
    handleErr(err)
    ```

3. CDAの有効期限を定義します。
  <!-- 3. Define an expiration time for the CDA -->

    ```go
    // define the time after which the CDA expires
    // (in this case after 72 hours)
    now = now.Add(time.Duration(72) * time.Hour)
    ```

4. 有効期限付きの新しいmulti-use CDAを作成します。
  <!-- 4. Create a new multi-use CDA with an expiration time -->

    ```go
    // allocate a new deposit address with timeout conditions.
    conditions := &deposit.Conditions{TimeoutAt: &now, MultiUse: true}
    cda, err := account.AllocateDepositAddress(conditions)
    handleErr(err)
    ```

### オラクルを作成する
<!-- ### Create an oracle -->

CDAは、バンドルの作成、送信、および確定にかかる時間内に期限切れになる可能性があります。そのため、CDAの条件に応じて、CDAに預け入れるかどうかを決定する必要があります。この意思決定プロセスを自動化するために、CDAに預け入れるかどうかについての決定（trueまたはfalse）を返す[オラクル](https://github.com/iotaledger/iota.go/tree/master/account/oracle)を作成できます。
<!-- A CDA may expire during the time it takes for a bundle to be created, sent, and confirmed. So, you need to make a decision about whether to deposit into a CDA, depending on its conditions. To automate this decision-making process, you can create an [oracle](https://github.com/iotaledger/iota.go/tree/master/account/oracle) that returns a decision (true or false) about whether to deposit into it. -->

オラクルはオラクルソースを引数として取り、オラクルソースがCDAに預け入れることを決定した場合は`true`を返します。
<!-- Oracles take an oracle source as an argument and return `true` if the oracle source decides that you may deposit into the CDA. -->

1. [`TimeDecider`オラクルソース](https://github.com/iotaledger/iota.go/tree/master/account/oracle/time)を使用して、CDAの有効期限が少なくとも30分離れているかどうかを確認します。この30分で、バンドルを送信して確定する時間が与えられます。
  <!-- 1. Use the [`TimeDecider` oracle source](https://github.com/iotaledger/iota.go/tree/master/account/oracle/time) to check if the CDA's expiration time is at least 30 minutes away. These 30 minutes give the bundle time to be sent and confirmed. -->

    ```go
    threshold := time.Duration(30)*time.Minute
    // timeDecider is an OracleSource
    timeDecider := oracle_time.NewTimeDecider(timesource, threshold)
    // Create a new SendOracle with the given OracleSources
    sendOracle := oracle.New(timeDecider)
    ```

    :::info:
    `time`パッケージとの競合を避けるために、 `"github.com/iotaledger/iota.go/account/oracle"`インポートにプレフィックスを追加する必要があります。この例では、`oracle_time`プレフィックスを使用しています。
    :::
    <!-- :::info: -->
    <!-- To avoid conflicts with the `time` package, you must add a prefix to the `"github.com/iotaledger/iota.go/account/oracle"` import. In this example, we use the `oracle_time` prefix. -->
    <!-- ::: -->

2. オラクルを呼び出すには、CDAを`sendOracle`オブジェクトの`OkToSend()`メソッドに渡します。
  <!-- 2. To call the oracle, pass the CDA to the `OkToSend()` method of the `sendOracle` object -->

    ```go
    // Ask the SendOracle whether we should make a deposit
    ok, rejectionInfo, err := sendOracle.OkToSend(cda)
    handleErr(err)
    if !ok {
        fmt.Println("Won't send transaction: ", rejectionInfo)
        return
    }
    ```

## CDAへIOTAトークンを預け入れる
<!-- ## Deposit IOTA tokens into a CDA -->

1. CDAに予想量が含まれている場合は、CDAオブジェクトを`account.Send()`メソッドに渡すことで、予想量のIOTAトークンをCDAに預け入れることができます。
1. When a CDA contains an expected amount, you can deposit that amount into it by passing the object to the `account.Send()` method.

    ```go
    bundle, err := account.Send(cda.AsTransfer())
    handleErr(err)

    fmt.Printf("Made deposit into %s in the bundle with the following tail transaction hash %s\n", cda.Address, bundle[0].Hash)
    ```

:::info:
Devnetで自分のアカウントをテストしていて、十分な残高がない場合は、[Devnet蛇口](https://faucet.devnet.iota.org/)からDevnetトークンを取得できます。
:::
<!-- :::info: -->
<!-- If you're testing your account on the Devnet and you don't have enough balance, use the [Devnet faucet](https://faucet.devnet.iota.org/) to request Devnet tokens. -->
<!-- ::: -->

## CDAを配布する
<!-- ## Distribute a CDA -->

CDAは記述的なオブジェクトなので、それらを任意の形式にシリアル化して配布することができます。たとえば、`timeout_at`、`multi_use`、および`expected_amount`の各パラメータを使用して、CDAのマグネットリンクを作成できます。
<!-- Because CDAs are descriptive objects, you can serialize them into any format and distribute them. For example, you can create a magnet-link for a CDA, with the `timeout_at`, `multi_use`, and `expected_amount` parameters. -->

1. CDAをマグネットリンクにシリアル化するには、CDAオブジェクトの`AsMagnetLink()`メソッドを使用します。
<!-- 1. To serialize the CDA into a magent link, use the `AsMagnetLink()` method of the CDA object -->

    ```go
    fmt.Println(cda.AsMagnetLink())
    // iota://MBREWACWIPRFJRDYYHAAME…AMOIDZCYKW/?timeout_at=1548337187&multi_use=1&expected_amount=0
    ```

2. マグネットリンクをCDAにパースするには、`deposit`オブジェクトの`ParseMagnetLink()`メソッドを使用します。
2. To parse the magnet link into a CDA, use the `ParseMagnetLink()` method of the `deposit` object

    ```go
    cda, err := deposit.ParseMagnetLink(cda.AsMagnetLink())
    handleErr(err)
    ```

## 次のステップ
<!-- ## Next steps -->

[アカウントのイベントをリッスンする](../how-to-guides/listen-to-events.md)。
<!-- [Listen to events in your account](../how-to-guides/listen-to-events.md). -->
