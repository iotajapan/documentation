# アカウントへ/から支払いをする
<!-- # Make payments to/from your account -->

**支払いを行うには、送信者と受信者の両方が条件付き預け入れアドレス（CDA）を持っている必要があります。送信者にはIOTAトークンを含む期限切れのCDAが必要であり、受信者にはトークンを送信するためのアクティブなCDAが必要です。**
<!-- **To make payments, both the sender and the receiver need to have a conditional deposit address (CDA). The sender needs an expired CDA that contains IOTA tokens, and the receiver needs an active CDA to send tokens to.** -->

## CDAの条件
<!-- ## Conditions of a CDA -->

CDAを作成するときに、`timeoutAt`フィールドを指定して、アクティブか期限切れかを定義します。IOTAトークンをアクティブなアドレスに預け入れることができますが、アクティブなアドレスからIOTAトークンを取り出すことはできません。逆に期限切れのアドレスからはIOTAトークンを取り出すことができますが、期限切れのアドレスにIOTAトークンを預け入れることはできません。
<!-- When you create a CDA, you specify the `timeoutAt` field to define whether it's active or expired. You can deposit IOTA tokens into an active address. But, you can't withdraw tokens from an active address. You can withdraw tokens from an expired address. But, you can't deposit tokens into an expired address. -->

次の推奨フィールドのいずれかを指定することもできます。
<!-- You can also specify one of the following recommended fields: -->

* **multiUse（推奨）：**アドレスが複数の預け入れを受け取ることができるかどうかを指定するブール値。
<!-- * **multiUse (recommended):** A boolean that specifies if the address may receive more than one deposit. -->
* **expectedAmount（推奨）：**アドレスが受け取ると予想されるIOTAトークンの量。アドレスにこの金額が含まれている場合、期限切れと見なされます。この条件を指定することをお勧めします。
<!-- * **expectedAmount (recommended):** The amount of IOTA tokens that the address is expected to receive. When the address contains this amount, it's considered expired. We recommend specifying this condition. -->

:::info:
同じCDAで`expected_amount`と`multi_use`フィールドを指定することはできません。CDA条件に関する詳細なアドバイスについては、[FAQ](../references/cda-advice.md)を参照してください。
:::
<!-- :::info: -->
<!-- You can't specify the `expected_amount` and `multi_use` fields in the same CDA. Please refer to the [FAQ](../references/cda-advice.md) for more advice about CDA conditions. -->
<!-- ::: -->

| **フィールドの組合せ** | **取り出せる条件** |
| :----------------- | :------------- |
| `timeoutAt` | IOTAトークンが含まれている限り、CDAは取り出しに使用できます。 |
| `timeoutAt`および` multiUse`（推奨） | CDAは、預け入れの回数に関係なく、期限切れになるとすぐに取り出しに使用できます。`multiUse`フィールドが設定されたアドレスをいつ使用するかについては、[CDA FAQ](../references/cda-advice.md)を参照してください。 |
| `timeoutAt`および`expectedAmount`（推奨） | CDAは、予想金額が含まれるとすぐに取り出しに使用できます。`multi_use`フィールドが設定されているアドレスをいつ使用するかについては、[CDA FAQ](../references/cda-advice.md)を参照してください。 |

<!-- |  **Combination of fields** | **Withdrawal conditions** -->
<!-- | :----------| :----------| -->
<!-- |`timeoutAt` |The CDA can used in withdrawals as long as it contains IOTA tokens| -->
<!-- |`timeoutAt` and `multiUse` (recommended) |The CDA can be used in withdrawals as soon as it expires, regardless of how many deposits were made to it. See the [CDA FAQ](../references/cda-advice.md) on when to use addresses with the `multiUse` field set. | -->
<!-- |`timeoutAt` and `expectedAmount` (recommended) | The CDA can be used in withdrawals as soon as it contain the expected amount. See the [CDA FAQ](../references/cda-advice.md) on when to use addresses with the `multi_use` field set.| -->

:::warning:警告
CDAが`timeoutAt`フィールドのみで作成された場合、期限が切れていなくても残高がゼロではない状態になるとすぐに取り出しに使用できます。

使用済みアドレスからの取り出しを回避するために、可能な限り、`multiUse`フィールドまたは`expectedAmount`フィールドのいずれかを使用してCDAを作成することをお勧めします。
:::
<!-- :::warning:Warning -->
<!-- If a CDA was created with only the `timeoutAt` field, it can be used in withdrawals as soon as it has a non-zero balance even if it hasn't expired.  -->

<!-- To avoid withdrawing from a spent address, we recommend creating CDAs with either the `multiUse` field or with the `expectedAmount` field whenever possible. -->
<!-- ::: -->

:::info:
CDAはアカウント内でのみ使用でき、汎用クライアントライブラリメソッドでは使用できません。故に、CDAを使用するには、送信者と受信者の両方にアカウントが必要です。
:::
<!-- :::info: -->
<!-- CDAs can be used only in an account and not in the generic client library methods. As a result, both you and the sender must have an account to be able to use CDAs. -->
<!-- ::: -->

---

1. CDAを作成し、明日期限が切れるように設定します。
  <!-- 1. Create a CDA and set it to expire tomorrow -->

    ```js
    const cda = account
      .generateCDA({
        timeoutAt: Date.now() + 24 * 60 * 60 * 1000,
        expectedAmount: 1000
      });
    ```

    :::info:
    `timeSource()`メソッドでアカウントを作成した場合は、`timeoutAt`フィールドで`timeSource()`メソッドを呼び出すことができます。
    :::
    <!-- :::info: -->
    <!-- If you created an account with a `timeSource()` method, you can call that method in the `timeoutAt` field. -->
    <!-- ::: -->

2. CDAがまだアクティブであることを確認した後、預け入れを送信します。
  <!-- 2. After making sure that the CDA is still active, send a deposit to it -->

    ```js
    cda
      .tap(cda => console.log('Sending to:', cda.address))
      .then(cda =>
        account.sendToCDA({
          ...cda,
          value: 1000
        })
        .then((trytes) => {
          console.log('Successfully prepared transaction trytes:', trytes)
      })
      )
      .catch(err => {
          // エラーを処理します。
      });
    ```

    :::info:
    このサンプルコードを無料のテストトークンでテストしたい場合は、[Devnet蛇口](root://getting-started/0.1/tutorials/receive-test-tokens.md)からテストトークンを取得します。
    :::
    <!-- :::info: -->
    <!-- If you want to test this sample code with free test tokens, [request some from the Devnet faucet](root://getting-started/0.1/tutorials/receive-test-tokens.md). -->
    <!-- ::: -->

    :::info:
    CDAの最後の9文字はチェックサムで、アドレスとそのアドレスのすべての条件のハッシュ値です。トリニティやDevent蛇口はまだCDAをサポートしていないため、このチェックサムは、トリニティやDeventの蛇口とは互換性がありません。

    トリニティやDevent蛇口の入力フィールドにアドレスを貼り付ける前に、チェックサムを削除します。
    :::
    <!-- :::info: -->
    <!-- The last 9 characters of a CDA are the checksum, which is a hash of the address and all of its conditions. This checksum is not compatible with Trinity or the Devnet faucet because they don't yet support CDAs. -->

    <!-- Remove the checksum before pasting your address into the input field of either of these applications. -->
    <!-- ::: -->

出力には、トランザクションがペンディング中の場合はアドレスと末尾トランザクションハッシュが表示され、トランザクションが確定されると同じアドレスと末尾トランザクションハッシュが表示されます。
<!-- In the output, you should see an address and a tail transaction hash when the transaction is pending, and the same address and tail transaction hash when the transaction is confirmed. -->

アカウントの添付ルーチンは、未確定のトランザクションが確定されるまで添付を続けます。
<!-- The account's attachment routine will keep attaching unconfirmed transactions until they are confirmed. -->

`startAttaching()`と`stopAttaching()`メソッドを呼び出すことで添付ルーチンを開始または停止することができます。
<!-- You can start or stop the attachment routine by calling the `startAttaching()` and `stopAttaching()` methods. -->

```js
account.startAttaching({
  depth: 3,
  minWeightMagnitude: 9,
  delay: 30 * 1000

  // タングル内でチップ選択を開始するために戻る深さ
  depth: 3,

  // Devnetの最小重量値9
  minWeightMagnitude: 9,

  // 次の添付ラウンドまでの待機時間
  delay: 1000 * 30,

  // トランザクションが促進できなくなる深さ
  // maxDepthまでのトランザクションは自動的に再添付されます。
  maxDepth: 6
});

account.stopAttaching();
```

## 利用可能残高の合計を1つのCDAに転送する
<!-- ## Transfer your entire available balance to one CDA -->

できるだけ少ない数のCDAで残高の大部分を維持することをお勧めします。こうすることで、支払いをより速く、そしてより少ないトランザクションですみます。そのために、利用可能残高を新しいCDAに転送することができます。
<!-- You may want to keep the majority of your balance on as few CDAs as possible. This way, making payments is faster and requires fewer transactions. To do so, you can transfer you available balance to a new CDA. -->

:::info:
利用可能残高は、すべての期限切れCDAの総残高です。この残高は取り出しても安全です。

アカウントの合計残高には、まだアクティブで期限切れではないCDAが含まれています。この残高を取り出すのは危険です。
:::
<!-- :::info: -->
<!-- Available balance is the total balance of all expired CDAs. This balance is safe to withdraw. -->

<!-- Your account's total balance includes CDAs that are still active as well as expired. This balance is unsafe to withdraw. -->
<!-- ::: -->

1. アカウントの利用可能残高の合計を予想量とするCDAを作成し、利用可能残高の合計をそのCDAに転送します。
  <!-- 1. Create a CDA that has your account's total available balance as its expected amount, then transfer your total available balance to that CDA -->

    ```js
    account.getAvailableBalance()
    .then(balance => {
      const cda = account.generateCDA({
        timeoutAt: Date.now() + 24 * 60 * 60 * 1000,
        expectedAmount: balance
      })
      .then(cda => {
        account.sendToCDA({
          ...cda,
          value: balance
        })
        .then((trytes) => {
          console.log('Successfully prepared transaction trytes:', trytes)
        })
      })
    })
    .catch(err => {
      // エラーを処理します。
    });
    ```

## CDAを送信者に送信する
<!-- ## Send someone your CDA -->

送信者にあなたのアカウントにIOTAトークンを転送して欲しい場合、送信者にあなたのCDAを送信する必要があります。
<!-- If you want a depositer to transfer IOTA tokens to your account, you need to send them your CDA. -->

CDAは記述的なオブジェクトなので、送信する前にCDAを任意の形式にシリアル化できます。`generateCDA()`メソッドは以下のフィールドを持つCDAオブジェクトを返します。CDAを送信者に配布する前に、任意の形式にシリアル化することができます。
<!-- Because CDAs are descriptive objects, you can serialize them into any format before sending them. The `generateCDA()` method returns a CDA object with the following fields. You can serialize a CDA into any format before distributing it to senders. -->

```js
{
  address, // 最後の9トライとはチェックサムです。
  timeoutAt,
  multiUse,
  expectedAmount
}
```

例えば、これらのフィールドをシリアル化してマグネットリンクを作成できます。
<!-- For example, you can serialize these fields to create a magnet link. -->

### CDAをマグネットリンクにシリアル化する
<!-- ### Serialize a CDA into a magnet link -->

CDAをシリアル化するための組み込みの方法は、マグネットリンクを作成することです。
<!-- The built-in method for serializing a CDA is to create a magnet link. -->

1. CDAをマグネットリンクにシリアル化するには、[`@iota/cda`モジュール](https://github.com/iotaledger/iota.js/tree/next/packages/cda)の`serializeCDAMagnet()`メソッドを使います。
  <!-- 1. To serialize a CDA into a magnet link, use the `serializeCDAMagnet()` method in the [`@iota/cda` module](https://github.com/iotaledger/iota.js/tree/next/packages/cda) -->

    ```js
    const CDA = require('@iota/cda');

    const magnetLink = CDA.serializeCDAMagnet(cda);
    // iota://MBREWACWIPRFJRDYYHAAME…AMOIDZCYKW/?timeout_at=1548337187&multi_use=1
    ```

2. マグネットリンクにシリアル化されたCDAにトランザクションを送信するには、マグネットリンクを`parseCDAMagnet()`メソッドに渡し、その結果を`sendToCDA()`メソッドに渡します。
  <!-- 2. To send a transaction to a CDA that's been serialized into a magnet link, pass the magnet link to the `parseCDAMagnet()` method, then and pass the result to the`sendToCDA()` method -->

    ```js
    const magnetLink = 'iota://MBREWACWIPRFJRDYYHAAME…AMOIDZCYKW/?timeout_at=1548337187&multi_use=1&expected_amount=0';
    const { address, timeoutAt, multiUse, expectedAmount } = parseCDAMagnet(
      magnetLink
    );

    account.sendToCDA({
      address,
      timeoutAt,
      multiUse,
      expectedAmount,
      value: 1000
    })
      .then((trytes) => {
        console.log('Successfully prepared the transaction:', trytes)
      })
      .catch(err => {
        // エラーを処理します。
      });

    account.startAttaching({
      depth: 3,
      minWeightMagnitude : 9,
      delay: 30 * 1000 // 30秒の遅延
    });
    ```
