# アカウントへ/から支払いをする
<!-- # Make payments to/from your account -->

**支払いをするには、送信者と受信者の両方にCDAが必要です。送信者はIOTAトークンを含む期限切れのCDAを必要とし、受信者はアクティブなCDAを必要とします。**
<!-- **To make payments, both the sender and the receiver need to have a CDA. The sender needs an expired CDA that contains IOTA tokens, and the receiver needs an active CDA.** -->

:::info:
CDAはアカウント内でのみ使用でき、汎用クライアントライブラリメソッドでは使用できません。故に、CDAを使用するには、送信者と受信者の両方にアカウントが必要です。
:::
<!-- :::info: -->
<!-- CDAs can be used only in an account and not in the generic client library methods. As a result, both you and the sender must have an account to be able to use CDAs. -->
<!-- ::: -->

1. 必要なパッケージをインポートします。
  <!-- 1. Import the required packages -->

    ```go
    "time"

    "github.com/iotaledger/iota.go/account/deposit"
    "github.com/iotaledger/iota.go/account/oracle"
    oracle_time "github.com/iotaledger/iota.go/account/oracle/time"
    ```

2. 明日有効期限が切れる新しいCDAを作成します。
  <!-- 2. Create a new CDA that expires tomorrow -->

    ```go
    // Get the current time
    now, err := timesource.Time()
    handleErr(err)

    now = now.Add(time.Duration(24) * time.Hour)

    // Specify the conditions
    conditions := &deposit.Conditions{TimeoutAt: &now, MultiUse: true}

    cda, err := account.AllocateDepositAddress(conditions)
    handleErr(err)
    ```

3. CDAがまだアクティブであることを確認した後で、CDAに預け入れを送信します。
  <!-- 3. After making sure that the CDA is still active, send a deposit to it -->

    ```go
    bundle, err := account.Send(cda.AsTransfer())
    handleErr(err)

    fmt.Printf("Made deposit into %s in the bundle with the following tail transaction hash %s\n", cda.Address, bundle[0].Hash)
    ```

    標準出力に次のようなものが表示されるはずです。
    <!-- You should see something like the following in the output: -->

    ```bash
    Made deposit into DL9CSYICJVKQRUTWBFUCZJQZ9WNBSRJOA9MGOISQZGGHOCZTXVSKDIZN9HBORNGDWRBBAFTKXGEJIAHKDTMAUX9ILA in the bundle with the following tail transaction hash WZEATTRJYENRALJTWPVGDQZHETIDJXPUROUM9BBPS9RJEELDMU9YNZFBSDGPQHZHMXBVCKITSMDEEQ999
    ```

    :::info:
    このサンプルコードを無料のテストトークンでテストしたい場合は、[Devnet蛇口](root://getting-started/0.1/tutorials/receive-test-tokens.md)からテストトークンを取得します。
    :::
    <!-- :::info -->
    <!-- If you want to test this sample code with free test tokens, [request some from the Devnet faucet](root://getting-started/0.1/tutorials/receive-test-tokens.md). -->
    <!-- ::: -->

    :::info:
    CDAの最後の9文字はチェックサムで、アドレスとそのアドレスのすべての条件のハッシュ値です。トリニティやDevent蛇口はまだCDAをサポートしていないため、このチェックサムは、トリニティやDeventの蛇口とは互換性がありません。

    トリニティやDevent蛇口の入力フィールドにアドレスを貼り付ける前に、チェックサムを削除します。
    :::
    <!-- :::info: -->
    <!-- The last 9 characters of a CDA are the checksum, which is a hash of the address and all of its conditions. This checksum is not compatible with Trinity or the Devent faucet because they don't yet support CDAs. -->
    <!--  -->
    <!-- Remove the checksum before pasting your address into the input field of either of these applications. -->
    <!-- ::: -->

## 意思決定プロセスを自動化する
<!-- ## Automate the decision-making process -->

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

## アカウント全体の残高を1つのCDAに転送する
<!-- ## Transfer your entire account balance to one CDA -->

できるだけ少ない数のCDAで残高の大部分を維持することをお勧めします。こうすることで、支払いをより速く、そしてより少ないトランザクションですみます。そのために、利用可能残高を新しいCDAに転送することができます。
<!-- You may want to keep the majority of your balance on as few CDAs as possible. This way, making payments is faster and requires fewer transactions. To do so, you can transfer you available balance to a new CDA. -->

:::info:
利用可能残高は、すべての期限切れCDAの総残高です。この残高は取り出しても安全です。

アカウントの合計残高には、まだアクティブで期限切れではないCDAが含まれています。この残高を取り出すのは危険です。
:::
<!-- :::info: -->
<!-- Available balance is the total balance of all expired CDAs. This balance is safe to withdraw. -->
<!--  -->
<!-- Your account's total balance includes CDAs that are still active as well as expired. This balance is unsafe to withdraw. -->
<!-- ::: -->

1. アカウントの利用可能残高の合計を予想量とするCDAを作成します。
  <!-- 1. Create a CDA that has your account's total available balance as its expected amount -->

    ```go
    // Get the current time
    now, err := timesource.Time()
    handleErr(err)

    now = now.Add(time.Duration(24) * time.Hour)

    // Specify the conditions
    conditions := &deposit.Conditions{TimeoutAt: &now, MultiUse: false, ExpectedAmount: account.AvailableBalance() }

    cda, err := account.AllocateDepositAddress(conditions)
    handleErr(err)
    ```

2. 使用可能残高の合計をCDAに転送します。
  <!-- 2. Transfer your total usable balance to the CDA -->

    ```go
    bundle, err := account.Send(cda.AsTransfer())
    handleErr(err)

    fmt.Printf("Made deposit into %s in the bundle with the following tail transaction hash %s\n", cda.Address, bundle[0].Hash)
    ```

## CDAを送信者に送信する
<!-- ## Send someone your CDA -->

送信者にあなたのアカウントにIOTAトークンを送って欲しい場合、送信者にあなたのCDAを送信する必要があります。
<!-- If you want a depositer to send IOTA tokens to your account, you need to send them your CDA. -->

CDAは記述的なオブジェクトなので、送信する前にCDAを任意の形式にシリアル化できます。`generateCDA()`メソッドは以下のフィールドを持つCDAオブジェクトを返します。CDAを送信者に配布する前に、任意の形式にシリアル化することができます。
<!-- Because CDAs are descriptive objects, you can serialize them into any format before sending them. The `generateCDA()` method returns a CDA object with the following fields. You can serialize a CDA into any format before distributing it to senders. -->

```js
{
   address, // The last 9 trytes are the checksum
   timeout_at,
   multi_use,
   expected_amount
}
```

例えば、これらのフィールドをシリアル化してマグネットリンクを作成できます。
<!-- For example, you can serialize these fields to create a magnet link. -->

### CDAをマグネットリンクにシリアル化する
<!-- ### Serialize a CDA into a magnet link -->

シリアル化するための組み込みメソッドを使って、CDAからマグネットリンクを作成できます。
<!-- The built-in method for serializing a CDA is to create a magent link. -->

1. CDAをマグネットリンクにシリアル化するには、CDAオブジェクトの`AsMagnetLink()`メソッドを使用します。
  <!-- 1. To serialize the CDA into a magent link, use the `AsMagnetLink()` method of the CDA object -->

    ```go
    fmt.Println(cda.AsMagnetLink())
    // iota://MBREWACWIPRFJRDYYHAAME…AMOIDZCYKW/?timeout_at=1548337187&multi_use=1&expected_amount=0
    ```

2. マグネットリンクにシリアル化されたCDAにトランザクションを送信するには、マグネットリンクをCDAに逆シリアル化します。
  <!-- 2. To send a transaction to a CDA that's been serialized into a magnet link, deserialize the magent link into a CDA -->

    ```go
    cda, err := deposit.ParseMagnetLink(cda.AsMagnetLink())
    handleErr(err)
    ```

## 次のステップ
<!-- ## Next steps -->

[アカウントを別の端末にインポートできるように、アカウントをエクスポートします](../how-to-guides/import-seed-state.md)。
<!-- [Try exporting your account so you can import it onto another device](../how-to-guides/import-seed-state.md). -->
