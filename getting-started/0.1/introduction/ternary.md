# 三進数
<!-- # Ternary -->

**すべてのトランザクションには，トライトと呼ばれる文字が含まれています．これらのトライトは，[3進数システム](https://en.wikipedia.org/wiki/Ternary_numeral_system)の一部です．IOTAはこの3進数システムを使用します．なぜなら，[バイナリ](https://en.wikipedia.org/wiki/Binary_number)と比較して，3進コンピューティングは2つではなく3つの状態でデータを表すことができるため，より効率的であると考えられているためです．**
<!-- **All transactions contain characters called trytes. These trytes are part of the [ternary numeral system](https://en.wikipedia.org/wiki/Ternary_numeral_system). IOTA uses this system because, compared to [binary](https://en.wikipedia.org/wiki/Binary_number), ternary computing is considered to be more efficient as it can represent data in three states rather then just two.** -->

IOTA では，データは1，0，または-1で構成される平衡三進法で表されます．これらの値はトリットと呼ばれ，3トリットで1トライトに相当し，表現可能な27（3<sup>3</sup>）の値を持つことができます．
<!-- In IOTA, data is represented in balanced ternary, which consists of 1, 0, or -1. These values are called trits, and three of them are equal to one tryte, which can have 27 (3<sup>3</sup>) possible values. -->

## トライトエンコーディング
<!-- ## Tryte encoding -->

トライトを読みやすくするために，トライトは数字9と大文字A〜Zのみで構成される27のトライトがエンコードされた文字の1つとして表されます．
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

## ASCII 文字をトライトに変換する方法
<!-- ## How ASCII characters are converted to trytes -->

IOTA クライアントライブラリでは，[ASCII 文字](https://en.wikipedia.org/wiki/ASCII)をトライトとの間で変換できます．
<!-- In the IOTA client libraries, you can convert [ASCII characters](https://en.wikipedia.org/wiki/ASCII) to and from trytes. -->

この機能は，`Your coffee is ready` などの ASCII メッセージをトライトに変換するのに役立ちます．そして変換したトライトをトランザクションに追加して，タングルにアタッチします．
<!-- This feature is useful for converting an ASCII message such as `Your coffee is ready` to trytes, which you can add to a transaction and attach to the Tangle. -->

各 ASCII 文字は，以下のことを行うことで2つのトライトとして表されます．
<!-- Each ASCII character is represented as 2 trytes by doing the following: -->

1. ASCII 文字の10進 Unicode 値を見つけます．
  <!-- 1. Find the decimal Unicode value of an ASCII character -->

  たとえば， `Z` の10進 Unicode 値は90です．
  <!-- For example, the decimal Unicode value of `Z` is 90. -->

2. 次の方程式で10進数の Unicode 値を使用します．
  <!-- 2. Use the decimal Unicode value in the following equations: -->

  ```
  decimal % 27
  (decimal - 9) / 27
  ```

  たとえば，`Z` の場合，結果は次のようになります．
  <!-- For example, for `Z`, the result would be -->

  ```
  90 % 27 = 9
  (90 - 9) / 27 = 3
  ```

3. 方程式の結果をインデックスとして使用して，ASCII 文字のトライト値を見つけます．
  <!-- 3. Use the results of the equations as indices to find the character's tryte value -->

    | **インデックス** | **トライト** |
    |------------------|--------------|
    | 0                | 9            |
    | 1                | A            |
    | 2                | B            |
    | **__3__**        | **__C__**    |
    | 4                | D            |
    | 5                | E            |
    | 6                | F            |
    | 7                | G            |
    | 8                | H            |
    | **__9__**        | **__I__**    |
    | 10               | J            |
    | 11               | K            |
    | 12               | L            |
    | 13               | M            |
    | 14               | N            |
    | 15               | O            |
    | 16               | P            |
    | 17               | Q            |
    | 18               | R            |
    | 19               | S            |
    | 20               | T            |
    | 21               | U            |
    | 22               | V            |
    | 23               | W            |
    | 24               | X            |
    | 25               | Y            |
    | 26               | Z            |

  たとえば，ASCII 文字 `Z` は，トライトでは `IC` として表されます．
  <!-- For example, the ASCII character `Z` is represented as `IC` in trytes. -->

## ユーティリティ
<!-- ## Utilities -->

次の IOTA タングルユーティリティを三進法データで使用できます．
<!-- You can use the following IOTA Tangle Utilities with ternary data: -->

- [データをトライトへ変換 / データへトライトを変換する](https://utils.iota.org/text-conversion)．
<!-- - [Convert data to/from trytes](https://utils.iota.org/text-conversion) -->

- [ストレージのトライトを圧縮する](https://utils.iota.org/compress)．
<!-- - [Compress trytes for storage](https://utils.iota.org/compress) -->

## 関連ガイド
<!-- ## Related guides -->

[JavaScript を用いてデータをトライトへ，トライトをデータへ変換する](root://client-libraries/0.1/how-to-guides/js/convert-data-to-trytes.md)．
<!-- [Convert data to/from trytes in JavaScript](root://client-libraries/0.1/how-to-guides/js/convert-data-to-trytes.md) -->
