# シードとは？
<!-- # What is a seed? -->

**シードはシードから導出されたすべてのアドレスにアクセスできるようにする一意なパスワードです。これらのアドレスは IOTA トークンを保持しているので残高があります。IOTA トークンを使うには、シードを使って、IOTA トークンを保持しているアドレスを所有していることを証明する必要があります。シードには数字9と大文字のA〜Zのみを含めることができます。**
<!-- **A seed is your unique password that gives you access to all your addresses. These addresses hold your IOTA tokens and as such have a balance. To spend IOTA tokens, you must use your seed to prove that you own the address that holds them. Seeds can include only the number 9 and the uppercase letters A-Z.** -->

[アドレス](root://dev-essentials/0.1/concepts/addresses-and-signatures.md)は、[バンドル](../introduction/what-is-a-bundle.md)内の[トランザクション](../introduction/what-is-a-transaction.md)の送受信に使用できるアカウントのようなものです。
<!-- [Addresses](root://dev-essentials/0.1/concepts/addresses-and-signatures.md) are like  accounts that you can use to send and receive [transactions](../introduction/what-is-a-transaction.md) in a [bundle](../introduction/what-is-a-bundle.md). -->

IOTA プロトコルでは暗号化を使用して、シードからほぼ無制限のアドレスを導出することができます。アドレスは一般公開されているので、データや IOTA トークンを送信したい人と共有できます。
<!-- The IOTA protocol uses cryptography to allow you to derive an almost unlimited amount of addresses from your seed. Addresses are public, and you can share them with anyone who wants to send you data and/or IOTA tokens. -->

各アドレスには対応する秘密鍵もあります。秘密鍵は秘密であり、決して他人と共有してはいけません。IOTA トークンを他の人のアドレスに転送する場合は、秘密鍵を使用してバンドルに署名し、署名を入力トランザクションに含めることで、トークンを保持しているアドレスの秘密鍵を所有していることを証明する必要があります。
<!-- Each address also has a corresponding private key. Private keys are secret, and you should never share your them with anyone. If you want to transfer IOTA tokens to someone else's address, you need to prove that you own the private key to the address that holds the tokens by signing the bundle with your private key and including it in the input transaction. -->

このドキュメントでは、次の**example seed**を使用しています。
<!-- In our documentation, we use the following **example seed**: -->

```bash
PUEOTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX
```

シードは自分で作成しバックアップする責任があります。シードを失うと、シードを回復することはできません。そしてシードを持っている人は誰でも IOTA トークンへのアクセス権も持っています。
<!-- You are responsible for creating and backing up your own seed. If you lose your seed, you can't recover it, and anyone who has your seed also has access to your IOTA tokens. -->
