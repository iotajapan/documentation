# IOTAエリアコード概要
<!-- # IOTA area codes overview -->

**IOTAエリアコード（IAC）は、IOTAトランザクションを地理的位置でタグ付けするための標準規格として提案されています。**
<!-- **IOTA area codes (IAC) are a proposed standard for tagging IOTA transactions with a geo-location, which allows you to be filter them by location.** -->

タングル上のトランザクションには、サービス広告やセンサー情報など、特定の分野に固有の情報が含まれていることがあります。
<!-- Transactions on the Tangle can sometimes contain information that's specific to a certain area such as service advertisements or sensor information. -->

IACを使用することで、トランザクションにエリアをタグ付けし、他の人が同じようなIACを持つトランザクションを照会することによってトランザクションを見つけることができます。
<!-- By using IACs, you can tag a transaction with an area and allow someone else to find it by querying for transactions with a similar IAC. -->

## IACについて
<!-- ## About IACs -->

IACは[Open Location Code](https://en.wikipedia.org/wiki/Open_Location_Code)（OLC）のクローンです。IACには、[トライエンコーディング](root://dev-essentials/0.1/concepts/trinary.md)との互換性を持たせるためのいくつかの小さな変更が含まれています。
<!-- IACs are a clone of [Open Location Codes](https://en.wikipedia.org/wiki/Open_Location_Code) (OLC), which includes some minor changes to make them compatible with [tryte encoding](root://dev-essentials/0.1/concepts/trinary.md): -->

* コードを構成する数字と文字は_トライト_と呼ばれ、`FGHJKLMNOPQRSTUVXWYZ`が含まれます。
<!-- * The numbers and letters that make up a code are called _trytes_, which include the following: `FGHJKLMNOPQRSTUVXWYZ` -->
* IACの8番目のトライトの後に来る区切り文字は、`+`ではなく`9`です。
<!-- * The separator that comes after the eighth tryte in an IAC is a `9` instead of a `+` -->
* IACをパディングするために`0`の代わりに`A`トライトが使用されます。
<!-- * The `A` tryte is used for padding IACs instead of a `0` -->

## IACを読む
<!-- ## Reading IACs -->

IACは、通常の緯度と経度の形式の座標よりも短くて使いやすい形式で場所をエンコードする方法です。
<!-- IACs are a way of encoding locations into a form that is shorter and easier to use than coordinates in the usual form of latitude and longitude. -->

たとえば、IOTA財団の住所の緯度と経度の座標は`52.529510`、`13.413018`です。これらの座標のIACは、`NPHTQORL9XK`です。
<!-- For example, the latitude and longitude coordinates of the address of the IOTA Foundation are 52.529510, 13.413018. The IAC for these coordinates is : `NPHTQORL9XK`. -->

IACはトライトで構成されているため、IACをトランザクションの`tag`フィールドに簡単に追加して場所をタグ付けすることができます。
<!-- Because an IAC consists of trytes, you can easily add them to the `tag` field of a transaction to tag it with a location. -->

IACは以下の3つの部分で構成されています（区切り文字`9`を除く）。
<!-- An IAC consists of three parts (excluding the `9` separator): -->

- 最初の4つのトライトはエリアコードで、およそ`100 km x 100 km`の地域を表しています。たとえば、`NPHT`はベルリンとポツダムの一部を含む地域を表します。
<!-- - The first four trytes are the area code, describing a region of roughly 100 km x 100 km. For example, `NPHT` represents an area that includes Berlin and parts of Potsdam -->
- 次の6つのトライトはローカルコードで、`14 m x 14 m`という小さな領域を表しています。
<!-- - The next six trytes are the local code, describing an area as small as 14 m x 14 m -->
- 最後の2つのトライトは特別な精度のためのものです。
<!-- - The final two trytes are for extra precision -->

| **IACの長さ（トライト）** | **おおよそのエリア** |
| :------------------------ | :------------------- |
| 2 | 2200 km |
| 4 | 110 km |
| 6 | 5.5 km |
| 8 | 275 m |
| 10 | 14 m |
| 11 | 3.5 m |
| 12 | 3 m未満 |

<!-- | **IAC length (trytes)**   | **Approximate area**| -->
<!-- |:--------------|:---------------------| -->
<!-- |2       |2200 km | -->
<!-- |4      | 110 km | -->
<!-- |6          | 5.5 km     | -->
<!-- |8            | 275 m        | -->
<!-- |10  | 14 m              | -->
<!-- |11    |3.5 m | -->
<!-- |12 |Less than 3 m| -->

## 参考文献
<!-- ## Further reading -->

IACの詳細については、[ブログの投稿](https://blog.iota.org/iota-area-codes-a-proposal-to-geo-tag-iota-transactions-d3c457d1df1b)を参照してください。
<!-- For more information about IACs, [read our blog post](https://blog.iota.org/iota-area-codes-a-proposal-to-geo-tag-iota-transactions-d3c457d1df1b). -->
