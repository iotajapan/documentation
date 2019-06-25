# トランザクションを送受信する
<!-- # Send and receive transactions -->

**アカウントでトランザクションを送受信するには、条件付預け入れアドレス（CDA）を使用する必要があります。CDAは、アカウント内のIOTAトークンの取り出しおよび預け入れに使用される可能性がある条件を指定できるようにした特別なアドレスです。**
<!-- **To send and receive transactions in an account, you must use conditional deposit addresses (CDA). CDAs are special addresses that allow you to specify the conditions in which they may be used in account withdrawals and deposits.** -->

アカウントはCDAを使用して、[署名済みアドレスの再利用](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse)のリスクを軽減します。誰かにIOTAトークンを要求すると、一定期間アクティブなCDAを作成できます。このようにして、あなたは送信者にあなたがその時間の後にだけCDAからIOTAトークンを取り出すつもりであることを知らせます。その結果、送信者は、CDAの残り時間に応じて、IOTAトークンを預け入れるかどうかを決定できます。
<!-- Accounts use CDAs to help reduce the [risks of withdrawing from spent addresses](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse). When you request IOTA tokens from a someone, you can create a CDA that's active for a certain period of time. This way, you let the sender know that you intend to withdraw from that address only after that time. As a result, the sender can decide whether to make a deposit, depending on how much time is left on a CDA. -->

:::info:
CDAはアカウント内でのみ使用でき、汎用[クライアントライブラリメソッド](root://client-libraries/0.1/introduction/overview.md)では使用できません。その結果、CDAを使用できるようにするには、あなたと送信者の両方にアカウントが必要です。
:::
<!-- :::info: -->
<!-- CDAs can be used only in an account and not in the generic [client library methods](root://client-libraries/0.1/introduction/overview.md). As a result, both you and the sender must have an account to be able to use CDAs. -->
<!-- ::: -->

## CDAのステート
<!-- ## State of a CDA -->

CDAは、アクティブ状態または期限切れ状態のどちらかです。 CDAの状態によって、CDAからIOTAトークンを取り出すことができるのか、CDAにIOTAトークンを預け入れることができるのかが決まります。
<!-- CDAs can be in either an active or expired state. The state of a CDA determines whether you can withdraw from it or deposit into it: -->

**アクティブCDA：** IOTAトークンをアクティブアドレスに預け入れることができます。アクティブなアドレスからトークンを取り出すことはできません。
<!-- **Active CDA:** You can deposit IOTA tokens into an active address. You can't withdraw tokens from an active address. -->

**期限切れCDA：** 期限切れのアドレスからトークンを取り出すことができます。期限切れのアドレスにトークンを預け入れることはできません。
<!-- **Expired CDA:** You can withdraw tokens from an expired address. You can't deposit tokens into an expired address. -->

## CDAのコンディション
<!-- ## Conditions of a CDA -->

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

## CDAを作成する
<!-- ## Create a CDA -->

1. CDAの有効期限を定義します。
  <!-- 1. Define an expiration time for the CDA -->

    ```java
    // In this case, after 72 hours
    Date hour = new Date(System.currentTimeMillis() + 72000 * 60 * 60);
    ```

2. 有効期限と予想量で新しいCDAを作成します。
  <!-- 2. Create a new CDA with an expiration time and expected amount -->

    ```java
    Future<ConditionalDepositAddress> cda = account.newDepositAddress(hour, false, account.usableBalance()).get();
    ```

## IOTAトークンをCDAに預け入れる
<!-- ## Deposit IOTA tokens into a CDA -->

1. CDAに予想量が含まれている場合は、CDAオブジェクトを`account.send()`メソッドに渡すことで、予想量を取り出すことができます。
  <!-- 1. When a CDA contains an expected amount, you can deposit that amount into it by passing the object to the `account.send()` method. -->

    ```java
    String magnet = "iota://YWEQLREFJQORXXKKEBBBDKOPAXHXJRGVPBUTBJFSRPPYVWWYUWSBDJTIUBJVFREXEAUZWRICKH9VBSQE9KPNLTCLNC/?timeout_at=1554472983208&multi_use=false&expected_amount=1000";
    ConditionalDepositAddress cda = DepositFactory.get().parse(magnet, MagnetMethod.class);
    Future<Bundle> bundle = account.send(
            cda.getDepositAddress().getHashCheckSum(),
            cda.getRequest().getExpectedAmount(),
            Optional.of("Thanks for that pizza!"), Optional.of("OMNOMNOM"));
    bundle.get();
    ```

:::info:
Devnetで自分のアカウントをテストしていて、十分な残高がない場合は、[Devnet蛇口](https://faucet.devnet.iota.org/)からDevnetトークンを取得してください。
:::
<!-- :::info: -->
<!-- If you're testing your account on the Devnet and you don't have enough balance, use the [Devnet faucet](https://faucet.devnet.iota.org/) to request Devnet tokens. -->
<!-- ::: -->

## アカウント全体の残高を1つのCDAに預け入れる
<!-- ## Deposit your entire account balance into one CDA -->

1. アカウントの使用可能残高の合計が予想量となるようにCDAを作成します。
  <!-- 1. Create a CDA that has your account's total usable balance as its expected amount -->

    ```java
    Date n = new Date(System.currentTimeMillis() + 10000 * 60 * 60);
    ConditionalDepositAddress address = account.newDepositAddress(n, false, account.usableBalance()).get();
    ```

    :::info:
    使用可能残高は、すべての期限切れCDAの総残高です。
    :::
    <!-- :::info: -->
    <!-- Usable balance is the total balance of all expired CDAs. -->
    <!-- ::: -->

2. オブジェクトを`account.send()`メソッドに渡して、使用可能な残高の合計をCDAに預け入れます。
  <!-- 2. Deposit your total usable balance into the CDA by passing the object to the `account.send()` method -->

    ```java
    Future<Bundle> bundle = account.send(
            address.getDepositAddress().getHashCheckSum(),
            address.getRequest().getExpectedAmount(),
            Optional.of("Sweep of all addresses"),
            Optional.of("IOTA9SWEEP"));
    bundle.get();
    ```

## CDAを配布する
<!-- ## Distribute a CDA -->

CDAは記述的なオブジェクトなので、それらを任意の形式にシリアル化して配布することができます。たとえば、`timeout_at`、`multi_use`、および`expected_amount`の各パラメータを使用して、CDAのマグネットリンクを作成できます。
<!-- Because CDAs are descriptive objects, you can serialize them into any format and distribute them. For example, you can create a magnet-link for a CDA, with the `timeout_at`, `multi_use`, and `expected_amount` parameters. -->

```
iota://MBREWACWIPRFJRDYYHAAME…AMOIDZCYKW/?timeout_at=1548337187&multi_use=false&expected_amount=1000
```

1. CDAをマグネットリンクにシリアル化するには、次の手順に従います。
  <!-- 1. To serialize a CDA to a magnet link, do the following: -->

    ```java
    String magnet = DepositFactory.get().build(cda.get(), MagnetMethod.class);

    System.out.println(magnet);
    // iota://YWEQLREFJQORXXKKEBBBDKOPAXHXJRGVPBUTBJFSRPPYVWWYUWSBDJTIUBJVFREXEAUZWRICKH9VBSQE9KPNLTCLNC/?timeout_at=1554472983208&multi_use=false&expected_amount=1000
    ```
