# 3進法
<!-- # Trinary -->

**IOTAは、3進数値システムに従ってデータを表します。2進数と比較すると、2進数計算ではなく3つの状態でデータを表すことができるため、2進数計算より効率的です。**
<!-- **IOTA represents data according to the trinary numeric system. Compared to binary, trinary computing is more efficient as it can represent data in three states rather then just two.** -->

2進数では、データは1または0として表すことができます。これらの値はビットと呼ばれます。8ビットは1バイトに相当し、256（2<sup>8</sup>）個の値を取ります。
<!-- In binary, data can be represented as either 1 or 0. These values are called bits. Eight bits is equal to one byte, which can have 256 (2<sup>8</sup>) possible values. -->

平衡3進法では、データは1、0、または-1として表すことができます。これらの値はトリットと呼ばれます。3トリットは1トライトに相当します。これは27（3<sup>3</sup>）の可能な値を持つことができます。
<!-- In balanced trinary, data can be represented as 1, 0, or -1. These values are called trits. Three trits is equal to one tryte, which can have 27 (3<sup>3</sup>) possible values. -->

IOTAは、27文字（A〜Zと9）をトライトに対応付けた[トライト文字列表](../references/tryte-alphabet.md)に従って、データをトライトにエンコードされた文字列で表現します。
<!-- IOTA represents data as tryte-encoded characters, according to the [tryte alphabet](../references/tryte-alphabet.md) where each of the 27 characters consists of one tryte. -->

幸い、自分でプログラムを書いてデータをバイトからトライトへ変換する必要はありません。[IOTAクライアントライブラリ](root://client-libraries/0.1/introduction/overview.md)には、データを変換するための組み込み関数があります。あるいは、[IOTAツール](https://laurencetennant.com/iota-tools/index.html)などのサービスを使用することもできます。
<!-- Luckily, you don't have to convert data from bytes to trytes yourself. The [IOTA client libraries](root://client-libraries/0.1/introduction/overview.md) have built-in functions for converting data. Or, you can use a service such as [IOTA tools](https://laurencetennant.com/iota-tools/index.html). -->
