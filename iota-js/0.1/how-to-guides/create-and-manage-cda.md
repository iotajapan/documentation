# トランザクションを送受信する
<!-- # Send and receive transactions -->

**アカウントでトランザクションを送受信するには、条件付預け入れアドレス（CDA）を使用する必要があります。CDAは、アカウント内のIOTAトークンの取り出しおよび預け入れに使用される可能性がある条件を指定できるようにした特別なアドレスです。**
<!-- **To send and receive transactions in an account, you must use conditional deposit addresses (CDA). CDAs are special addresses that allow you to specify the conditions in which they may be used in account withdrawals and deposits.** -->

アカウントはCDAを使用して、[署名済みアドレスの再利用](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse)のリスクを軽減します。誰かにIOTAトークンを要求すると、一定期間アクティブなCDAを作成できます。このようにして、あなたは送信者にあなたがその時間の後にだけCDAからIOTAトークンを取り出すつもりであることを知らせます。その結果、送信者は、CDAの残り時間に応じて、IOTAトークンを預け入れるかどうかを決定できます。
<!-- Accounts use CDAs to help reduce the [risk of withdrawing from spent addresses](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse). When you request IOTA tokens from a someone, you can create a CDA that's active for a certain period of time. This way, you let the sender know that you intend to withdraw from that address only after that time. As a result, the sender can decide whether to make a deposit, depending on how much time is left on a CDA. -->

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
<!-- * **timeoutAt (required):** The time at which the address expires -->

次に、以下の推奨フィールドのいずれかを指定します。
<!-- Then specify one of the following recommended fields: -->

* **multiUse（推奨）:** アドレスが複数の預け入れを受け取る可能性があるかどうかを指定するブール値。
<!-- * **multiUse (recommended):** A boolean that specifies if the address may receive more than one deposit. -->
* **expectedAmount（推奨）：** アドレスに含まれると予想されるIOTAトークンの量。アドレスにこの量のIOTAトークンが含まれている場合、期限切れと見なされます。この条件を指定することをお勧めします。
<!-- * **expectedAmount (recommended):** The amount of IOTA tokens that the address is expected to contain. When the address contains this amount, it's considered expired. We recommend specifying this condition. -->

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
<!-- If a CDA was created with only the `timeoutAt` field, it can be used in withdrawals as soon as it has a non-zero balance even if it hasn't expired. -->
<!--  -->
<!-- To avoid withdrawing from a spent address, we recommend creating CDAs with either the `multiUse` field or with the `expectedAmount` field whenever possible. -->
<!-- ::: -->

## CDAを作成する
<!-- ## Create a CDA -->

1. CDAフィールドを`generateCDA()`メソッドに渡します。
  <!-- 1. Pass the CDA fields to the `generateCDA()` method -->

    ```js
    account.generateCDA({
        timeoutAt: Math.floor(new Date('7-16-2186').getTime() / 1000), // Date in seconds
        multiUse: true,
        security: 2
    })
        .then(cda => {
            // Do something with the CDA
            console.log(JSON.stringify(cda, null, 1));
        })
        .catch(error => {
            // Handle errors here
            console.log(error);
        })
    ```

:::info:
`timeSource()`メソッドでアカウントを作成した場合は、`timeSource()`メソッドを`timeoutAt`フィールドで呼び出すことができます。
:::
<!-- :::info: -->
<!-- If you created an account with a `timeSource()` method, you can call that method in the `timeoutAt` field. -->
<!-- ::: -->

## IOTAトークンをCDAに預け入れる
<!-- ## Deposit IOTA tokens into a CDA -->

1. CDAがまだアクティブであることを確認した後、`sendToCDA()`メソッドを使用してIOAトークンをCDAに預け入れます。
  <!-- 1. After making sure that the CDA is still active, use the `account.sendToCDA()` method to deposit IOTA tokens into it -->

    ```js
    account.sendToCDA({
        address: 'AT9GOVPQDDKAJ...ADFA9IRSV', // must include the checksum
        multiUse: false,
        timeoutAt: 6833278800,
        expectedAmount: 10000000,
        value: 10000000
    })
        .then((trytes) => {
            console.log('Successfully prepared transaction trytes:', trytes)
        })
        .catch(err => {
            // Handle errors here...
        });
    ```

添付ルーチンは、確定されるまで未確定のトランザクションを添付し続けます。
<!-- The attachment routine will keep on attaching unconfirmed transactions until they are confirmed. -->

`stopAttaching()`メソッドと`startAttaching()`メソッドを呼び出すことで、添付ルーチンを開始または停止できます。
<!-- You may start or stop the attachment routine by calling the `startAttaching()` and -->
<!-- `stopAttaching()` methods. -->

    ```js

    account.startAttaching({
        depth: 3,
        minWeightMagnitude: 9,
        delay: 30 * 1000

        // How far back in the Tangle to start the tip selection
        depth: 3,

        // The minimum weight magnitude is 9 on the Devnet
        minWeightMagnitude: 9,

        // How long to wait before the next attachment round
        delay: 1000 * 30,

        // The depth at which transactions are no longer promotable
        // Those transactions are automatically re-attached
        maxDepth: 6
    });

    account.stopAttaching();

    ```

2. **オプション：** CDAをマグネットリンクとして使用するには、CDAを`parseCDAMagnet()`メソッドに渡してから、結果を`sendToCDA()`メソッドに渡します。
  <!-- 2. **Optional:** To use a CDA as a magnet link, pass it to the `parseCDAMagnet()` method, then and pass the result to the`sendToCDA()` method -->

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
        value: 1 // Value to send to the CDA
    })
        .then((trytes) => {
            console.log('Successfully prepared transaction trytes:', trytes)
        })
        .catch(err => {
            // Handle errors here...
        });

    account.startAttaching({
        depth: 3,
        minWeightMagnitude : 9,
        delay: 30 * 1000 // 30 second delay
    });
    ```

## CDAを配布する
<!-- ## Distribute a CDA -->

`generateCDA()`メソッドは以下のフィールドを持つCDAオブジェクトを返します。 CDAを送信者に配布する前に、任意の形式にシリアル化することができます。
<!-- The `generateCDA()` method returns a CDA object with the following fields. You can serialize a CDA into any format before distributing it to senders: -->

```js
{
   address, // The last 9 trytes are the checksum
   timeoutAt,
   multiUse,
   expectedAmount
}
```

1. CDAをマグネットリンクにシリアル化するには、`@iota/cda`モジュールの`serializeCDAMagnet()`メソッドを使用します。
  <!-- 1. To serialize a CDA into a magnet link, use the `serializeCDAMagnet()` method in the `@iota/cda` module -->

    ```js
    const CDA = require('@iota/cda');

    const magnetLink = CDA.serializeCDAMagnet(cda);
    // iota://MBREWACWIPRFJRDYYHAAME…AMOIDZCYKW/?timeout_at=1548337187&multi_use=1
    ```

## 次のステップ
<!-- ## Next steps -->

[イベントリスナーを作成する](root://iota-js/0.1/how-to-guides/listen-to-events.md)。
<!-- [Create an event listener](root://iota-js/0.1/how-to-guides/listen-to-events.md). -->
