# マークル木計算時間
<!-- # Merkle tree computation times -->

**マークル木の計算にかかる時間を把握するために、Amazon Web Services 上の仮想プライベートサーバのインスタンス（[T2.Xlarge](https://aws.amazon.com/ec2/instance-types/)）でテストを行いました。**
<!-- **To give you an idea of how long it takes to compute a Merkle tree, we tested it on an instance of a virtual private server on Amazon Web Services with the following specifications: [T2.Xlarge](https://aws.amazon.com/ec2/instance-types/)** -->

| **マークル木の`depth`** | **秘密鍵/アドレスペアの数** | **計算時間（分）** | **ネットワーク稼働時間** |
| :---------------------- | :-------------------------- | :----------------- | :------------------------- |
| 14 | 16,384 | 3.75 | 11日 9時間 |
| 15 | 32,768 | 7.5 | 22日 18時間 |
| 16 | 65,536 | 15 | 45日 12時間 |
| 17 | 131,072 | 30 | 91日|
| 18 | 262,144 | 60 | 182日 1時間 |
| 19 | 524,288 | 120 | 364日 2時間 |
| 20 | 1,048,576 | 240 | 1年 63日 4時間 |
| 21 | 2,097,152 | 480 | 3年 361日 8時間 |
| 22 | 4,194,304 | 960 | 7年 357日 17時間 |
| 23 | 8,388,608 | 1,920 | 15年 350日 10時間 |

<!-- |**Merkle tree depth**|**Number of private key/address pairs**|**Computation time (minutes)**|**Network uptime\***| -->
<!-- |:--------|:----------------------|:------------------------------|:----------| -->
<!-- |:--------|:----------------------|:------------------------------|:----------| -->
<!-- |14|16,384|3.75|11 days 9 hours| -->
<!-- |15|32,768|7.5|22 days 18 hours| -->
<!-- |16|65,536|15|45 days 12 hours| -->
<!-- |17|131,072|30|91 days| -->
<!-- |18|262,144|60|182 days 1 hours| -->
<!-- |19|524,288|120|364 days 2 hours| -->
<!-- |20|1,048,576|240|1 year 363 days 4 hours| -->
<!-- |21|2,097,152|480|3 years 361 days 8 hours| -->
<!-- |22|4,194,304|960|7 years 357 days 17 hours| -->
<!-- |23|8,388,608|1,920|15 years 350 days 10 hours| -->

- 60秒のマイルストーン間隔で
<!-- - With 60-second milestone intervals -->
