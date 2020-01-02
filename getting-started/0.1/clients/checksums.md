# チェックサム
<!-- # Checksums -->

**チェックサムは、アドレスまたはシードの末尾に追加される[トライト](../introduction/ternary.md)に付けられた名前です。多くの場合、ユーザーインターフェイスのアドレスとシードにチェックサムが追加され、タイプミスを検出しやすくなります。**
<!-- **A checksum is the name given to the [trytes](../introduction/ternary.md) that are appended to the end of an address or a seed. Checksums are often appended to addresses and seeds in user interfaces to help you to detect typos.** -->

## チェックサムの形式
<!-- ## Checksum format -->

チェックサムの長さは、アドレス用かシード用かによって異なります。アドレスチェックサムは9トライト長ですが、シードチェックサムは3トライト長です。この違いの理由は、アドレスの入力ミスにより IOTA トークンが間違ったアドレスに送信される可能性があるため、チェックサムは[衝突](https://en.wikipedia.org/wiki/Collision_(computer_science))に対してより耐性がなければならないためです。
<!-- The length of a checksum depends on whether it's for an address or a seed. Address checksums are 9 trytes long, whereas seed checksums are 3 trytes long. The reason for this difference is that mistyping an address can lead to sending IOTA tokens to the wrong address, so the checksum must be more resistant to [collisions](https://en.wikipedia.org/wiki/Collision_(computer_science)). -->

|             |**チェックサム無し**|**チェックサム有り**|
|-------------|:----------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------|:
| **シード**    | PUEOTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX | PUEOTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX**XTY**       |
| **アドレス** | GWQQYWCUFLDR9LIWDITVHTPYYO9BAMOADFLNBIHYLIFDTORUCFCOGRQFK9IXEHVEMDVZH9RYOXAFIVUOA | GWQQYWCUFLDR9LIWDITVHTPYYO9BAMOADFLNBIHYLIFDTORUCFCOGRQFK9IXEHVEMDVZH9RYOXAFIVUOA**DAYDSMFZW** |

:::info:
セキュリティ上の予防措置として、[トリニティ](root://wallets/0.1/trinity/introduction/overview.md)などの一部のアプリケーションでは、チェックサムを含むアドレスのみを入力します。
:::
<!-- :::info: -->
<!-- As a security precaution, some applications such as [Trinity](root://wallets/0.1/trinity/introduction/overview.md) allow you to enter only addresses that include a checksum. -->
<!-- ::: -->

## チェックサムの生成方法
<!-- ## How checksums are generated -->

最初に、[Kerl](https://github.com/iotaledger/kerl) [ハッシュ関数](https://en.wikipedia.org/wiki/Hash_function)を使用して、アドレスまたはシードがトリットに変換され、ハッシュされます。
<!-- First, the address or the seed is converted to trits and hashed, using the [Kerl](https://github.com/iotaledger/kerl) [hash function](https://en.wikipedia.org/wiki/Hash_function). -->

次に、そのハッシュ結果の最後の9または3のトライトがアドレスまたはシードの最後に追加されます。
<!-- Then, the last 9 or 3 trytes of the resulting hash are appended to the end of the address or seed. -->

## 関連ガイド
<!-- ## Related guides -->

[JavaScript でアドレスを生成する](root://client-libraries/0.1/how-to-guides/js/generate-an-address.md)。
<!-- [Generate an address in JavaScript](root://client-libraries/0.1/how-to-guides/js/generate-an-address.md). -->
