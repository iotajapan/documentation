# コマンドラインフラグ
<!-- # Command-line flags -->

**GoShimmer コードを実行するコマンドでは，以下のフラグを渡してノードのデフォルトの動作を変更できます．たとえば，デフォルトの自動ピアリングポートを変更するように選択できます．**
<!-- **In the command to execute the GoShimmer code, you can pass the following flags to change the default behavior of a node. For example, you can choose to change the default auto-peering port.** -->

:::info:
コードをダウンロードしたのであれば，すべての設定オプションの一覧を見るために `-h` または `--help` フラグを付けて実行することもできます．
:::
<!-- :::info: -->
<!-- If you've downloaded the code, you can also run it with the `-h` or `--help` flag to see a list of all the configuration options. -->
<!-- ::: -->

| **コマンドラインフラグ** | **説明** | **有効な値** | **デフォルト値** | **メモ** |
| :----------------------- | :------- | :----------- | :--------------- | :------- |
| <a name="analysis-server-address"></a> `-analysis-server-address` | ノードが隣接ノードなどの情報を送信する先のサーバを設定します． | string | 82.165.29.179:188 | この情報は，ネットワークの動作を調査するのに役立ちます．たとえば，分析サーバに送信された情報を使用して，[ネットワークの可視化](http://159.69.158.51/)にデータを入力します． |
| <a name="analysis-server-port"></a>`-analysis-server-port` | 自身の分析サーバ用のTCPポートを設定します． | number | 188 | 自身の分析サーバを実行したい場合は，このポートをデバイスに転送します．そうすれば，だれでもデフォルトの代わりにあなたの分析サーバに接続することを許可できます． |
| `--autopeering.acceptRequests` | 受信オートピアリングリクエストを受け入れるかどうか | boolean | true ||
| <a name="autopeering-address"></a> `-autopeering-address` | 着信ピアリングリクエストにバインドするアドレスを設定します． | string | 0.0.0.0 | デフォルトのオプションは任意のアドレスを受け入れます． |
| <a name="autopeering-entry-nodes"></a>`-autopeering-entry-nodes` | 自動ピアリング用の信頼できるエントリノードのリストを設定します． | string | 0d828930890386f036eb77982cc067c5429f7b8f@82.165.29.179:14626 | 複数のエントリノードを追加するには，エントリノードURLのスペース区切りリストを使用し，それを引用符で囲みます． |
| <a name="autopeering-port"></a>`-autopeering-port` | 着信ピアリングリクエスト用のTCPポートを設定します． | number | 14626 |
| `--autopeering.sendRequests` | オートピアリングリクエストを送信するかどうか | boolean | true ||
| <a name="database-directory"></a>`-database-directory` | データベースフォルダへのパスを設定します．  | string | mainnetdb |
| <a name="gossip-port"></a> `-gossip-port` | ゴシップ接続用のTCP/UDPポートを設定します． | number | 14666 |
| <a name="node-disable-plugins"></a>`-node-disable-plugins` | 名前を使ってプラグインを無効にします． | string | "dashboard zeromq spammer" |
| <a name="node-enable-plugins"></a>`-node-enable-plugins` | 名前を使ってプラグインを有効にします． | string | "analysis autopeering bundleprocessor cli gossip gossiponsolidification gracefulshutdown metrics statusscreen statusscreentps tangle tipselection webapi webapigttaendpoint" |
| <a name="node-log-level"></a>`-node-log-level` | 表示されるログメッセージの種類を設定します． | number | 3 | LOG_LEVEL_FAILURE = 0, LOG_LEVEL_WARNING = 1, LOG_LEVEL_SUCCESS = 2, LOG_LEVEL_INFO = 3, LOG_LEVEL_DEBUG = 4 |
| <a name="zeromq-port"></a>`-zeromq-port` | ZMQストリームのTCPポートを設定します． | number | 5556 |

<!-- | **Command-line flags** |  **Description**| **Accepted values** | **Default value**|**Notes** | -->
<!-- | :------------------------ | :--------------- | :--------- | :--------| :------------|:-----| -->
<!-- |<a name="analysis-server-address"></a> `--analysis.serverAddress`|Set the server to which your node sends information such as its neighbors| string | 82.165.29.179:188 |We use this information to help us research how the network behaves. For example, we use the information sent to the analysis server to populate [this visualization of the network](http://159.69.158.51/). -->
<!-- |<a name="analysis-server-port"></a>`--analysis.serverPort` |Set the TCP port for your own analysis server |number |188| If you want to run your own analysis server, forward this port to your device. Then, you can allow anyone to connect to your analysis server instead of the default. -->
<!-- |`--autopeering.acceptRequests`|Whether to accept incoming autopeering requests|boolean|true|| -->
<!-- |<a name="autopeering-address"></a> `--autopeering.address` |Set the address to bind for incoming peering requests|string |0.0.0.0 | The default option accepts any address -->
<!-- |<a name="autopeering-entry-nodes"></a>`--autopeering.entryNodes` |Set a list of trusted entry nodes for auto-peering |string | 0d828930890386f036eb77982cc067c5429f7b8f@82.165.29.179:14626 |To add more than one entry node, use a space-separated list of entry node URLs, and wrap it in quotation marks.| -->
<!-- |<a name="autopeering-port"></a>`--autopeering.port` |Set the TCP port for incoming peering requests |number |14626 | -->
<!-- | `--autopeering.sendRequests`| Whether to send autopeering requests|boolean|true| -->
<!-- |<a name="database-directory"></a>`--database.directory` |Set the path to the database folder  |string |mainnetdb |  -->
<!-- |<a name="gossip-port"></a> `--gossip.port`|Set the TCP/UDP port for the gossip connection | number| 14666 -->
<!-- |<a name="node-disable-plugins"></a>`--node.disablePlugins` |Disable plugins by name | string| "dashboard zeromq spammer"| -->
<!-- |<a name="node-enable-plugins"></a>`--node.enablePlugins`|Enable plugins by name|string|"analysis autopeering bundleprocessor cli gossip gossiponsolidification gracefulshutdown metrics statusscreen statusscreentps tangle tipselection webapi webapigttaendpoint" -->
<!-- |<a name="node-log-level"></a>`--node.logLevel` |Set the type of log messages that are displayed|number |3 |LOG_LEVEL_FAILURE = 0, LOG_LEVEL_WARNING = 1, LOG_LEVEL_SUCCESS = 2, LOG_LEVEL_INFO = 3, LOG_LEVEL_DEBUG = 4| -->
<!-- |<a name="zeromq-port"></a>`-zeromq.port` |Set the TCP port for the ZMQ stream |number |5556 |  -->
