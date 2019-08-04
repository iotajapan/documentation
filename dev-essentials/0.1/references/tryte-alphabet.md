# トライト文字
<!-- # Tryte alphabet -->

**この表は、トライトからトライトがエンコードされた文字への変換と、それに相当する10進数の値を表しています。**
<!-- **This table displays the conversion from trytes to tryte-encoded characters, and the equivalent values in decimal numbers.** -->

IOTAは[3進数](../concepts/trinary.md)システムを使用しています。そのため、アドレス、シード、署名などのデータはトライトに変換されます。読みやすくするために、これらの文字は27個ある文字コードの1つとして表示されます。
<!-- IOTA uses a [trinary](../concepts/trinary.md) numeric system. As such, data, such as addresses, seeds, and signatures, is converted to trytes. To make the trytes easy to read, these trytes are represented as one of 27 possible tryte-encoded characters. -->

|トライトがエンコードされた文字|トライト|10進数|
|:-----------------------------|:-------|:-----|
|                               9|  0,  0,  0 |     0|
|                               A|  1,  0,  0 |     1|
|                               B| -1,  1,  0 |     2|
|                               C|  0,  1,  0 |     3|
|                               D|  1,  1,  0 |     4|
|                               E| -1, -1,  1 |     5|
|                               F|  0, -1,  1 |     6|
|                               G|  1, -1,  1 |     7|
|                               H| -1,  0,  1 |     8|
|                               I|  0,  0,  1 |     9|
|                               J|  1,  0,  1 |    10|
|                               K| -1,  1,  1 |    11|
|                               L|  0,  1,  1 |    12|
|                               M|  1,  1,  1 |    13|
|                               N| -1, -1, -1 |   -13|
|                               O|  0, -1, -1 |   -12|
|                               P|  1, -1, -1 |   -11|
|                               Q| -1,  0, -1 |   -10|
|                               R|  0,  0, -1 |    -9|
|                               S|  1,  0, -1 |    -8|
|                               T| -1,  1, -1 |    -7|
|                               U|  0,  1, -1 |    -6|
|                               V|  1,  1, -1 |    -5|
|                               W| -1, -1,  0 |    -4|
|                               X|  0, -1,  0 |    -3|
|                               Y|  1, -1,  0 |    -2|
|                               Z| -1,  0,  0 |    -1|