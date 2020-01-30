# シード
<!-- # Seeds -->

**シードとは，[アドレス](../clients/addresses.md)に保持されているメッセージや [IOTA トークン](../clients/token.md)の所有権を証明できる一意のパスワードです．**
<!-- **A seed is a unique password that gives you the ability to prove your ownership of either messages and/or any [IOTA tokens](../clients/token.md) that are held on your [addresses](../clients/addresses.md).** -->

## シードの形式
<!-- ## Seed format -->

シードは，81[トライト](../introduction/ternary.md)（または[チェックサム](../clients/checksums.md)付きの90トライト）の文字列です．
<!-- A seed is a string of 81 [trytes](../introduction/ternary.md) (or 90 trytes with a [checksum](../clients/checksums.md)). -->

```bash
PUEOTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX
```

## シードの最大数
<!-- ## Maximum number of seeds -->

可能なシードの総数はほぼ無制限です（27^81 = 8.72 x 10<sup>115</sup>）．その結果，2つのシードが同じになる可能性はほとんどありません．
<!-- The total number of possible seeds is almost unlimited (8.7 x 10<sup>115</sup>). As a result, the chances of two seeds being the same is very unlikely. -->

## シードの生成方法
<!-- ## How seeds are generated -->

自分自身のシードを作成する必要があります．シードを失った場合，回復することはできません．
<!-- You are responsible for creating your own seed. If you lose your seed, you can't recover it. -->

シードにアクセスできる人は誰でもトランザクションに署名できるため，シードのアドレスに属する IOTA トークンにアクセスできます．
<!-- Anyone who has access to a seed is able to sign transactions and therefore has access to any IOTA tokens that belong to the seed's addresses. -->

## ユーティリティ
<!-- ## Utilities -->

シードの保護，トランザクションの送信，残高管理などのプロセスを簡素化するために，IOTA 財団によって管理されている[公式のオープンソースウォレット](root://wallets/0.1/introduction/overview.md)のいずれかを使用できます．
<!-- To simplify the process of securing your seed, sending transactions, and managing your balance, you can use one of the [official open-source wallets](root://wallets/0.1/introduction/overview.md), which are maintained by the IOTA Foundation. -->

:::warning:
シードを入力する場所に注意してください．一部の非公式のウォレットは信頼できない場合があります
:::
<!-- :::warning: -->
<!-- Be careful where you enter your seed. Some unofficial wallets may not be trustworthy -->
<!-- ::: -->

## 関連ガイド
<!-- ## Related guides -->

[シードを作成する](../tutorials/create-a-seed.md)．
<!-- [Create a seed](../tutorials/create-a-seed.md). -->
