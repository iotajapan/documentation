# セキュリティレベル
<!-- # Security levels -->

**新しい[アドレス](../clients/addresses.md)を生成するとき、セキュリティレベルを指定できます。アドレスのセキュリティレベルは、秘密鍵の長さ、使用済みアドレスが総当り攻撃から保護される度合い、および署名を含めるために必要な[トランザクション](../transactions/transactions.md)の数に影響します。**
<!-- **When generating a new [address](../clients/addresses.md), you can specify a security level for it. The security level of an address affects how long the private key is, how secure a spent address is against brute-force attacks, and how many [transactions](../transactions/transactions.md) are needed to contain the signature.** -->

アドレスは、1、2、または3のセキュリティレベルを持つことができ、それぞれがインデックスごとに異なるアドレスを生成します。
<!-- Addresses can have a security level of 1, 2, or 3, and each one generates a different address for each index. -->

セキュリティレベルが高いほど、アドレスの秘密鍵が長くなり、同じ長さのより長くより安全な署名が得られます。
<!-- The greater the security level, the longer the address's private key, which results in a longer and more secure signature of the same length. -->

| **セキュリティレベル** | **秘密鍵と署名の長さ（トライト）** |
| :--------------------- | :--------------------------------- |
| 1                      | 2,187 (非推奨)                     |
| 2                      | 4,374 (トリニティで使用)           |
| 3                      | 6,561 (最も安全)                   |

アドレスのセキュリティレベルは、署名を含めるために必要なトランザクション数と同じ数になります。
<!-- The security level of an address corresponds to the same number of transactions that are needed to contain the signature. -->

たとえば、アドレスのセキュリティレベルが3の場合、その署名は6,561トライトになりますが、単一トランザクションの[`signatureMessageFragment`フィールド](../transactions/transactions.md#structure-of-a-transaction)には2,187トライトの署名しか入りません。その結果、セキュリティレベルが3の場合、[バンドル](../transactions/bundles.md)内の2つの追加のゼロ価値トランザクションで、残りの署名の4,374トライトをフラグメント化する必要があります。
<!-- For example, when an address has a security level of 3, its signature is 6,561 trytes long, but the [`signatureMessageFragment` field](../transactions/transactions.md#structure-of-a-transaction) of a single transaction can contain only 2,187 trytes. As a result, you would need fragment the other 4,374 trytes of the signature over 2 additional zero-value transactions in the [bundle](../transactions/bundles.md). -->

## セキュリティレベルを選ぶ際のアドバイス
<!-- ## Advice for choosing a security level -->

セキュリティレベルを選択するときは、次の質問を考慮する必要があります。
<!-- When choosing a security level, you should consider the following questions. -->

### 使用済みアドレスにIOTAトークンを受け取る確率がどのくらいか？
<!-- ### What is the probability that the address will be spent and still receive IOTA tokens? -->

ユーザーがIOTAに慣れていないアプリケーションを構築していて、使用済みアドレスへのデポジットが心配な場合は、予防策としてセキュリティレベル3を使用できます。
<!-- If you're building an application where your users aren't familiar with IOTA and you are concerned about them depositing into spent addresses, you may want to use security level 3 as a precaution. -->

### トランザクションに署名できるようになるにはどれくらいの速さが必要か？
<!-- ### How fast do you need to be able to sign transactions? -->

高速トランザクションに依存するアプリケーションを構築している場合は、セキュリティレベル2を使用することをお勧めします。このようにすると、バンドルが小さくなり、プルーフオブワークが少なくなり、トランザクション署名が速くなります。
<!-- If you're building an application that relies on fast transactions, you may want to use security level 2. This way, you can benefit from smaller bundles, less proof of work, and faster transaction signing. -->

### プルーフオブワークやトランザクションの署名を行っているデバイスがどれくらい強力か？
<!-- ### How powerful is the device that is doing proof of work and/or signing transactions? -->

モノのインターネット上のデバイスなどは、多くの場合、電力が制限されています。アプリケーションがこれらのデバイスのいずれかで実行されており、アドレスに少量のIOTAトークンしか含まれていない場合は、セキュリティレベル2を使用して、プルーフオブワークとトランザクションの署名に必要なエネルギー量を削減することができます。
<!-- Devices such as those on the Internet of Things are often power-constrained. If your application is running on one of these devices and the addresses contain only small amounts of IOTA tokens, you may want to use security level 2 to reduce the amount of energy needed to do proof of work and to sign transactions. -->

## 関連ガイド
<!-- ## Related guides -->

[JavaScriptでアドレスを生成する](root://client-libraries/0.1/how-to-guides/js/generate-an-address.md)
<!-- [Generate an address in JavaScript](root://client-libraries/0.1/how-to-guides/js/generate-an-address.md). -->
