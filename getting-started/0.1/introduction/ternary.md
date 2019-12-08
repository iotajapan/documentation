# 三進数
<!-- # Ternary -->

**IOTAでは、データはトリットとトライトで構成される[三進数システム](https://en.wikipedia.org/wiki/Ternary_numeral_system)に従って表されます。[二進法](https://en.wikipedia.org/wiki/Binary_number)と比較して、三進法コンピューティングは2つではなく3つの状態のデータを表すことができるため、理論上はより効率的であると考えられています。**
<!-- **In IOTA, data is represented according to the [ternary numeral system](https://en.wikipedia.org/wiki/Ternary_numeral_system), which consists of trits and trytes. Compared to [binary](https://en.wikipedia.org/wiki/Binary_number), ternary computing is considered to be more efficient as it can represent data in three states rather then just two.** -->

IOTAでは、データは1、0、または-1で構成される平衡三進法で表されます。これらの値はトリットと呼ばれ、3トリットで1トライトに相当し、表現可能な27（3<sup>3</sup>）の値を持つことができます。
<!-- In IOTA, data is represented in balanced ternary, which consists of 1, 0, or -1. These values are called trits, and three of them are equal to one tryte, which can have 27 (3<sup>3</sup>) possible values. -->

## トライトエンコーディング
<!-- ## Tryte encoding -->

トライトを読みやすくするために、トライトは数字9と大文字A〜Zのみで構成される27のトライトがエンコードされた文字の1つとして表されます。
<!-- To make trytes easier to read, they are represented as one of 27 possible tryte-encoded characters, which consist of only the number 9 and the uppercase letters A-Z. -->

| **トライトがエンコードされた文字**  | **トリット** | **10進数** |
| :---------------------------------- | :----------- | :--------- |
|                                   9 | 0,  0,  0    |     0      |
|                                   A | 1,  0,  0    |     1      |
|                                   B | -1,  1,  0   |     2      |
|                                   C | 0,  1,  0    |     3      |
|                                   D | 1,  1,  0    |     4      |
|                                   E | -1, -1,  1   |     5      |
|                                   F | 0, -1,  1    |     6      |
|                                   G | 1, -1,  1    |     7      |
|                                   H | -1,  0,  1   |     8      |
|                                   I | 0,  0,  1    |     9      |
|                                   J | 1,  0,  1    |     10     |
|                                   K | -1,  1,  1   |     11     |
|                                   L | 0,  1,  1    |     12     |
|                                   M | 1,  1,  1    |     13     |
|                                   N | -1, -1, -1   |    -13     |
|                                   O | 0, -1, -1    |    -12     |
|                                   P | 1, -1, -1    |    -11     |
|                                   Q | -1,  0, -1   |    -10     |
|                                   R | 0,  0, -1    |    -9      |
|                                   S | 1,  0, -1    |    -8      |
|                                   T | -1,  1, -1   |    -7      |
|                                   U | 0,  1, -1    |    -6      |
|                                   V | 1,  1, -1    |    -5      |
|                                   W | -1, -1,  0   |    -4      |
|                                   X | 0, -1,  0    |    -3      |
|                                   Y | 1, -1,  0    |    -2      |
|                                   Z | -1,  0,  0   |    -1      |

## ユーティリティ
<!-- ## Utilities -->

次の IOTA タングルユーティリティを三進法データで使用できます。
<!-- You can use the following IOTA Tangle Utilities with ternary data: -->

- [データをトライトへ変換 / データへトライトを変換する](https://utils.iota.org/text-conversion)
<!-- - [Convert data to/from trytes](https://utils.iota.org/text-conversion) -->

- [ストレージのトライトを圧縮する](https://utils.iota.org/compress)
<!-- - [Compress trytes for storage](https://utils.iota.org/compress) -->

## 関連ガイド
<!-- ## Related guides -->

[JavaScriptを用いてデータをトライトへ、トライトをデータへ変換する](root://client-libraries/0.1/how-to-guides/js/convert-data-to-trytes.md)
<!-- [Convert data to/from trytes in JavaScript](root://client-libraries/0.1/how-to-guides/js/convert-data-to-trytes.md) -->
