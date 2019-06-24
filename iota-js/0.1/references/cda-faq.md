# 条件付預け入れアドレスに関するFAQ
<!-- # Conditional deposit address FAQ -->

## CDAのタイムアウトにはどのくらいの期間を指定すべきですか？
<!-- ## How long should I specify in a CDA's timeout? -->

`timeout_at`フィールドに指定する値は、送信者が預け入れをするのにどれだけ早くかかるかによって異なります。あなたが送信者と直接連絡を取り合っていて、両方ともが清算の転送を待っている場合は、より短いタイムアウトを指定できます。
<!-- The value that you specify in the `timeout_at` field depends on how fast you expect the depositor to make a deposit. If you are in direct contact with the depositor and you are both waiting to settle the transfer, you can specify a shorter timeout. -->

:::danger:重要
`timeout_at`フィールドのみを使用してCDAを作成した場合は、期限切れになっていなくてもゼロではない残高になるとすぐにIOTAトークンを取り出すことができます。そのため、使用済みアドレスからの2重取り出しを避けるために、可能な限り`multi_use`フィールドまたは`expected_amount`フィールドのいずれかを`timeout_at`フィールドと併用してCDAを作成することを強くお勧めします。
:::
<!-- :::danger:Important -->
<!-- If a CDA was created with only the `timeout_at` field, it can be used in withdrawals as soon as it has a non-zero balance even if it hasn't expired. So, to avoid withdrawing from a spent address, we recommend creating CDAs with either the `multi_use` field or with the `expected_amount` field whenever possible. -->
<!-- ::: -->

## multi_useフィールドを持つCDAはいつ使えば良いか？
<!-- ## When should I create a multi-use CDA? -->

次のような場合には、`multi_use`フィールドを持つCDAを使用することをお勧めします。
<!-- We recommend that you use multi-use CDAs in case when the following is true: -->

- 複数の送信者からの任意の量の複数の預け入れを予想する場合。
<!-- - You expect multiple payments of arbitrary value from multiple depositors. -->
- Webサイトなどで、CDAの作成と共有を完全に管理する場合。
<!-- - You fully control the creation and sharing of the CDA, for example on your website. -->
- 個別に設定された`expected_amount`フィールド値を持つCDAを各送信者と共有できない場合。
<!-- - When you cannot share a CDA with the `expected_amount` field value set with each depositor individually. -->

または、`timeout_at`フィールドの値が足りなくなったときに確実に新しいCDAを要求する送信者と通信するとき。
<!-- Or when communicating with depositors who reliably request a new CDA when the `timeout_at` field value is running out. -->

`multi_use` CDAアドレスの1つのシナリオは、Webサイトまたはスクリーンなどの他のデジタル媒体で寄付アドレスを共有する場合です。このシナリオでは、任意の量の複数の預け入れを受け取ることができ、CDAの共有を完全に管理できます。`timeout_at`値がなくなる前に、72時間経過するたびに、WebサイトまたはスクリーンでCDAを更新することができます。これにより、CDAを更新する前に、送信者が送った支払いを確定するのに十分な時間が送信者に与えられます。
<!-- One scenario for `multi_use` CDA addresses is sharing a donation address on a website or other digital medium, such as a screen. In this scenario, you can receive multiple deposits of arbitrary value and you fully control the sharing of the CDA. You can refresh the CDA on the website or screen each time the CDA is 72 hours before its `timeout_at` value runs out. This gives depositors enough time to finalize the payments they may have sent before you refreshed the CDA. -->

## expected_amountフィールドを持つCDAはいつ使えば良いか？
<!-- ## When should I create a CDA with an expected amount? -->

預け入れの値が送信者と受信者の両方の観点から明らかである場合は、`expected_amount`フィールドの値を指定する必要があります。たとえば、あなたが取引所からIOTAトークンを取り出したいときなどです。あなたは取引所に予想される量を持つCDAを与えることができます。
<!-- You should specify the value of the `expected_amount` field when the value of the deposit is clear from both the depositor's and the receiver's point of views. For example, when you want to withdraw from an exchange. You can give a CDA with an expected amount to the exchange. -->

または、直接連絡を取れる単一の送信者からの、特定のサービスまたは商品に対する支払いを要求する場合です。
<!-- Or when you request a payment for a specific service or good from a single deposit with whom you have direct communication. -->
