# Python クイックスタート
<!-- # Python quickstart -->

**このクイックスタートでは，開発環境のセットアップからタングルでのライブトランザクションのリッスンまで，Python での IOTA 開発の基本を学びます．**
<!-- **In this quickstart, you learn the basics of IOTA development in Python, from setting up a development environment to listening for live transaction on the Tangle.** -->

このクイックスタートでは，次の方法を学習します．
<!-- In this quickstart, you will learn how to: -->

1. 開発環境をセットアップする
<!-- 1. Set up a developer environment -->

2. パッケージをインストールする
<!-- 2. Install packages -->

3. ノードに接続する
<!-- 3. Connect to a node -->

## 手順1. 開発環境をセットアップする
<!-- ## Step 1. Set up a developer environment -->

Python クライアントライブラリを使用するには，開発環境を構成するプログラミングツールのセットが必要です．
<!-- To use the Python client library, you need a set of programming tools, which make up a development environment. -->

必須ではありませんが，ここでのガイドにあるように[仮想環境](https://realpython.com/python-virtual-environments-a-primer/)を使用することをお勧めします．
<!-- Although it's not necessary, we recommend using [virtual environments](https://realpython.com/python-virtual-environments-a-primer/) like we do in this guide. -->

1. [サポートされているPythonのバージョンのいずれかをインストールします](https://www.python.org/downloads/)．
<!-- 1. [Install one of the supported version of Python](https://www.python.org/downloads/): -->

- Python 2.7,
- Python 3.5,
- Python 3.6,
- Python 3.7

2. Python 2.7をインストールした場合，仮想環境パッケージをインストールします．
  <!-- 2. If you installed Python 2.7, install the virtual environment package -->

    ```bash
    pip install virtualenv
    ```

3. コードエディターをインストールします．[Visual Studio Code](https://code.visualstudio.com/Download) をお勧めしますが，さらに多くのものが利用可能です．
<!-- 3. Install a code editor. We recommend [Visual Studio Code](https://code.visualstudio.com/Download), but many more are available. -->

4. コマンドラインインターフェースを開きます．
  <!-- 4. Open a command-line interface -->

  オペレーティングシステムに応じて，コマンドラインインターフェイスは [Windows の PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/getting-started/getting-started-with-windows-powershell?view=powershell-6)，[Linux ターミナル](https://www.howtogeek.com/140679/beginner-geek-how-to-start-using-the-linux-terminal/)，または[macOS のターミナル](https://macpaw.com/how-to/use-terminal-on-mac)になります．
  <!-- Depending on your operating system, a command-line interface could be [PowerShell in Windows](https://docs.microsoft.com/en-us/powershell/scripting/getting-started/getting-started-with-windows-powershell?view=powershell-6), the [Linux Terminal](https://www.howtogeek.com/140679/beginner-geek-how-to-start-using-the-linux-terminal/) or [Terminal for macOS](https://macpaw.com/how-to/use-terminal-on-mac). -->

5. 新しい仮想環境を作成します．
  <!-- 5. Create a new virtual environment -->

    ```bash
    # Python 2:
    virtualenv myNewEnv

    # Python 3
    python3 -m venv myNewEnv
    ```

6. 仮想環境ディレクトリに移動します．
  <!-- 6. Change into your virtual environment's directory -->

    ```bash
    cd myNewEnv
    ```

7. 仮想環境をアクティブにします．
  <!-- 7. Activate your virtual environment -->

    ```bash
    Scripts/activate
    ```

これで，パッケージのインストールを開始する準備ができました．
<!-- Now you are ready to start installing packages. -->

## 手順2. パッケージをインストールする
<!-- ## Step 2. Install packages -->

Python クライアントライブラリ，または一般に PyOTA として知られているパッケージは，関連するメソッドを含むパッケージで構成されています．
<!-- The Python client library, or commonly known as PyOTA, is organized in packages, which contain related methods. -->

[PyPi](https://pypi.org/project/PyOTA/) からライブラリパッケージをインストールするには，[PIP パッケージマネージャー](https://pip.pypa.io/en/stable/)を使用します．
<!-- To install the library packages from [PyPi](https://pypi.org/project/PyOTA/), use the [PIP package manager](https://pip.pypa.io/en/stable/). -->

```bash
pip install pyota
```

すべてがうまくいった場合は，標準出力に次のようなものが表示されるはずです．
<!-- If everything went well, you should see something like the following in the output: -->

```shell
Successfully installed certifi-2019.9.11 cffi-1.13.2 chardet-3.0.4 class-registry-2.1.2 cryptography-2.8 enum34-1.1.6 filters-1.3.2 idna-2.8 ipaddress-1.0.23 py2casefold-1.0.1 pyOpenSSL-19.1.0 pycparser-2.19 pyota-2.1.0 pysha3-1.0.2 python-dateutil-2.8.1 pytz-2019.3 regex-2019.11.1 requests-2.22.0 six-1.13.0 typing-3.7.4.1 urllib3-1.25.7
```

パッケージをインストールすると，クライアントライブラリコードとその依存関係が得られます．
<!-- After installing a package, you'll have the client library code and its dependencies. -->

これで，コーディングを開始できます．
<!-- Now you can start coding. -->

## 手順3. ノードに接続する
<!-- ## Step 3. Connect to a node -->

トランザクションの送信を開始する前に，[同期済みノード](root://getting-started/0.1/network/nodes.md#synchronized-nodes)に接続していることを確認することをお勧めします．同期済みノードに接続することで，[タングル](root://getting-started/0.1/network/the-tangle.md)の最新の概観を知ることが出来ます．
<!-- It's best practice to make sure that you're connected to a [synchronized node](root://getting-started/0.1/network/nodes.md#synchronized-nodes) before you start sending transactions to it. This way, you know that it has an up-to-date view of [the Tangle](root://getting-started/0.1/network/the-tangle.md). -->

ノードに接続するたびに，どの[IOTAネットワーク](root://getting-started/0.1/network/iota-networks.md)に接続しているかを知る必要があります．ここでは，テストに使用できるIOTAネットワークであるDevnetのノードに接続します．
<!-- Whenever you connect to a node, you need to know which [IOTA network](root://getting-started/0.1/network/iota-networks.md) it's in. Here, we connect to a node on the Devnet, which is the IOTA networks that you can use for testing. -->

1. IOTA財団の[公式 Discord] に移動し，`botbox` チャンネルに **!milestone** と入力します．
<!-- 1. Go to the IOTA Foundation [Discord](https://discord.iota.org) and enter **!milestone** in the `botbox` channel -->

  ![Entering !milestone on Discord](../images/discord-milestone-check.PNG)

  Discord ボットは，[ノードクォーラム](root://getting-started/0.1/network/nodes.md#node-quorum)から現在の `latestMilestoneIndex` フィールドを返します．
  <!-- The Discord bot should return the current `latestMilestoneIndex` field from a [node quorum](root://getting-started/0.1/network/nodes.md#node-quorum). -->

2. プロジェクトを初期化したディレクトリで，`index.py` という新しいファイルを作成します．
<!-- 2. In the directory where you initialized your project, create a new file called `index.py` -->

3. ノードが同期しているかどうかを確認するには，次のコードをコピーして，`index.py` ファイルにペーストします．
  <!-- 3. To check if your node is synchronized, copy and paste the following code into the `index.py` file -->

    ```python
    from iota import Iota

    # IOTA API オブジェクトの新しいインスタンスを作成します
    # 接続するノードを指定します
    api = Iota(adapter = 'https://nodes.devnet.iota.org:443')

    # ノードとタングルについての情報を得るために `get_node_info()` メソッドを呼び出します
    response = api.get_node_info()

    print(response)
    ```

4. ファイルを実行します
  <!-- 4. Execute the file -->

    ```bash
    python index.py
    ```

ノードは次のようなものを返します．
<!-- The node returns something like the following: -->

```json
{
    "appName": "IRI Testnet",
    "appVersion": "1.5.6-RELEASE",
    "jreAvailableProcessors": 8,
    "jreFreeMemory": 12052395632,
    "jreVersion": "1.8.0_181",
    "jreMaxMemory": 22906667008,
    "jreTotalMemory": 16952328192,
    "latestMilestone": "FPRSBTMKOP9JTTQSHWRGMPT9PBKYWFCCFLZLNWQDFRCXDDHZEFIEDXRIJYIMVGCXYQRHSZQYCTWXJM999",
    "latestMilestoneIndex": 1102841,
    "latestSolidSubtangleMilestone": "FPRSBTMKOP9JTTQSHWRGMPT9PBKYWFCCFLZLNWQDFRCXDDHZEFIEDXRIJYIMVGCXYQRHSZQYCTWXJM999",
    "latestSolidSubtangleMilestoneIndex": 1102841,
    "milestoneStartIndex": 434525,
    "neighbors": 3,
    "packetsQueueSize": 0,
    "time": 1549482118137,
    "tips": 153,
    "transactionsToRequest": 0,
    "features": ["snapshotPruning", "dnsRefresher", "testnet", "zeroMessageQueue", "tipSolidification", "RemotePOW"],
    "coordinatorAddress": "EQQFCZBIHRHWPXKMTOLMYUYPCN9XLMJPYZVFJSAY9FQHCCLWTOLLUGKKMXYFDBOOYFBLBI9WUEILGECYM",
    "duration": 0
}
```

### レスポンスオブジェクトの内容
<!-- ### Reading the response object -->

レスポンスオブジェクトの `latestMilestoneIndex` フィールドが Discord から取得した `latestMilestoneIndex` フィールドと `latestSolidSubtangleMilestoneIndex` フィールドに等しい場合，ノードは同期しています．
<!-- If the `latestMilestoneIndex` field is equal to the one you got from Discord and the `latestSolidSubtangleMilestoneIndex` field, the node is synchronized. -->

そうでない場合は，別のノードに接続してみてください．[iota.dance Web サイト](https://iota.dance/)には，メインネットノードの一覧が含まれています．または，[自分自身のノードを実行](root://node-software/0.1/iri/how-to-guides/quickstart.md)できます．
<!-- If not, try connecting to a different node. The [iota.dance website](https://iota.dance/) includes a list of Mainnet nodes. Or, you can [run your own node](root://node-software/0.1/iri/how-to-guides/quickstart.md). -->

レスポンスオブジェクトの `features` 配列で，このノードは[リモートプルーフオブワーク](root://getting-started/0.1/transactions/proof-of-work.md)（RemotePOW）もサポートしていることがわかります．その結果，このノードを使用して，ローカルデバイスでプルーフオブワークを実行する代わりに，ノードでプルーフオブワークを行うことができます．
<!-- In the `features` array, you can see that this node also support [remote proof of work](root://getting-started/0.1/transactions/proof-of-work.md) (RemotePOW). As a result, you can use this node to do proof of work instead of doing it on your local device. -->

また，このノードではゼロメッセージキュー（ZMQ）が有効になっているため，ZMQ を使用して[ライブトランザクションをリッスン](../how-to-guides/python/listen-for-transactions.md)できます．
<!-- Also, this node has its zero message queue (ZMQ) enabled, so you can use it to [listen for live transactions](../how-to-guides/python/listen-for-transactions.md). -->

これらのフィールドの詳細については，[IRI API リファレンス](root://node-software/0.1/iri/references/api-reference.md#getNodeInfo)を参照してください．
<!-- For more information about these fields, see the [IRI API reference](root://node-software/0.1/iri/references/api-reference.md#getNodeInfo). -->

:::success:おめでとうございます:tada:
同期済みノードへの接続を確認できました．
:::
<!-- :::success: Congratulations :tada: -->
<!-- You've confirmed your connection to a synchronized node. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

[REPL.it ツール](https://repl.it)を使用して，ブラウザーでサンプルコードを実行できるようにします．
<!-- We use the [REPL.it tool](https://repl.it) to allow you to run sample code in the browser. -->

緑色のボタンをクリックしてコードを実行し，ウィンドウで結果を確認します．
<!-- Click the green button to run the code and see the results in the window. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Connect-to-a-node-Python?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## 議論に参加する
<!-- ## Get involved -->

[Discord チャンネル](https://discord.iota.org)では，次のことができます．
<!-- [Join our Discord channel](https://discord.iota.org) where you can: -->

- IOTA 開発者およびコミュニティとの議論に参加することができます．
<!-- - Take part in discussions with IOTA developers and the community -->
- 助けを求めることができます．
<!-- - Ask for help -->
- 他の人を助けるためにあなたの知識を共有することができます．
<!-- - Share your knowledge to help others -->

次のような多くのチャネルがあります．
<!-- We have many channels, including the following: -->

- `-dev`：これらのチャンネルは読み取り専用であり，開発者が互いにトピックを議論したり，GitHub からのコードの更新を確認したりできます．
<!-- - `-dev`: These channels are read-only and are where developers discuss topics with each other and where you can see any code updates from GitHub. -->

- `-discussion`：これらのチャンネルはあなたが参加できる場所です．
<!-- - `-discussion`: These channels are where you can participate. -->

## 次のステップ
<!-- ## Next steps -->

[Python ワークショップ](../how-to-guides/python/get-started.md)で学習を続ける．
<!-- Continue learning with our [Python workshop](../how-to-guides/python/get-started.md). -->

[開発者ハンドブック](root://getting-started/0.1/references/quickstart-dev-handbook.md)を読んで，自分自身のノードを実行する必要があるかどうか，プライベート IOTA ネットワークが必要かどうか，および両方について考慮する必要があるかについてのガイダンスを参照してください．
<!-- Read our [developer's handbook](root://getting-started/0.1/references/quickstart-dev-handbook.md) for guidance on whether you should run your own node, whether you need a private IOTA network, and what you need to consider for both. -->

[PyOTA ドキュメントページ](https://pyota.readthedocs.io/en/latest/)で詳細な Python ライブラリドキュメントを見つけてください．
<!-- Find the in-depth Python library documentation on the [PyOTA documentation page](https://pyota.readthedocs.io/en/latest/). -->
