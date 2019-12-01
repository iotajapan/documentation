# 公開メッセージを発行する
<!-- # Publish public messages -->

**誰でも読むことができるチャネルにメッセージを発行したい場合は、パブリックチャネルを使用できます。パブリックチャネルは、すべてのユーザーに対して開かれているトランザクションのアドレスを使用して、すべてのユーザーがメッセージを見つけて復号化できるように開いています。パブリックチャネルは、データをイミュータブルにする公示またはパブリックソーシャルメディアアプリケーションに役立ちます。**
<!-- **If you want to publish messages to a channel that anyone can read, you can use a public channel. These channels are open for any user to find and decrypt the messages, using the address of the transaction that it's in. This type of channel is useful for public announcements or public social media applications that want the data to be immutable.** -->

ここで、テストに使用できる[IOTAネットワーク](root://getting-started/0.1/network/iota-networks.md)の1つであるDevnetのノードに接続します。Devnetは、トークンが無料であることを除いて、Mainnetに似ています。
<!-- Here, we connect to a node on the Devnet, which is one of the [IOTA networks](root://getting-started/0.1/network/iota-networks.md) that you can use for testing. The Devnet is similar to the Mainnet, except the tokens are free. -->

1. パッケージを`require`します。
  <!-- 1. Require the packages -->

  ```js
  const Mam = require('@iota/mam');
  const { asciiToTrytes, trytesToAscii } = require('@iota/converter');
  ```

2. プライバシーモードをパブリックに設定し、Devnetノードに接続し、ユーザーインターフェイスでメッセージを表示するために使用するMAMエクスプローラーURLを設定します
  <!-- 2. Set the privacy mode to public, connect to a Devnet node, and set the MAM explorer URL to use for seeing messages in a user interface -->

  ```js
  const mode = 'public';
  const provider = 'https://nodes.devnet.iota.org';

  const mamExplorerLink = `https://mam-explorer.firebaseapp.com/?provider=${encodeURIComponent(provider)}&mode=${mode}&root=`;
  ```

3. MAMステートを初期化します。
  <!-- 3. Initialize the MAM state -->

  ```js
  let mamState = Mam.init(provider);
  ```

4. ASCIIデータをトライトに変換し、MAMチャネルに発行する前にJSONに保存する関数を作成します。
  <!-- 4. Create a function to convert ASCII data to trytes and store it in JSON before publishing it to a MAM channel -->

  ```js
  const publish = async packet => {
    // トライとの文字列としてMAMメッセージを作成します。
    const trytes = asciiToTrytes(JSON.stringify(packet));
    const message = Mam.create(mamState, trytes);

    // 新しいmamStateを保存します。
    mamState = message.state;
    // タングルにメッセージを添付します。
    await Mam.attach(message.payload, message.address, 3, 9)

    console.log('Published', packet, '\n');
    return message.root
  }
  ```

  :::info:
  `message.root`プロパティは、このMAMストリームのMAMチャネルIDです。
  :::
  <!-- :::info: -->
  <!-- The `message.root` property is the MAM channel ID for this MAM stream. -->
  <!-- ::: -->

5. 3つのMAMメッセージを発行する関数を作成します。
  <!-- 5. Create a function that publishes three MAM messages -->

  ```js
  const publishAll = async () => {
    const root = await publish({
      message: 'Message from Alice',
      timestamp: (new Date()).toLocaleString()
    })

    await publish({
      message: 'Message from Bob',
      timestamp: (new Date()).toLocaleString()
    })

    await publish({
      message: 'Message from Charlie',
      timestamp: (new Date()).toLocaleString()
    })

    return root
  }
  ```

6. メッセージを発行し、取得し、復号化します。
  <!-- 6. Publish the messages, fetch them, and decrypt them -->

  ```js
  // フェッチからデータを渡すために使用されるコールバック
  const logData = data => console.log('Fetched and parsed', JSON.parse(trytesToAscii(data)), '\n')

  publishAll()
    .then(async root => {

      // "log Data"コールバック関数を使用して非同期に出力します。
      await Mam.fetch(root, mode, null, logData)

      // フェッチが完了すると同期して出力します。
      const result = await Mam.fetch(root, mode)
      result.messages.forEach(message => console.log('Fetched and parsed', JSON.parse(trytesToAscii(message)), '\n'))
  ```

7. コンソールへリンクを出力して、MAMエクスプローラーでこれらのメッセージを確認します。
  <!-- 7. Print the link to the console to see these messages in the MAM Explorer -->

  ```js
      console.log(`Verify with MAM Explorer:\n${mamExplorerLink}${root}\n`);
    })
  ```

コンソールに次のようなものが表示されるはずです。
<!-- You should see something like the following in the console: -->

```bash
Published { message: 'Message from Alice',
  timestamp: '9/12/2019, 10:07:53 AM' }

Published { message: 'Message from Bob',
  timestamp: '9/12/2019, 10:07:59 AM' }

Published { message: 'Message from Charlie',
  timestamp: '9/12/2019, 10:08:08 AM' }

Fetched and parsed { message: 'Message from Alice',
  timestamp: '9/12/2019, 10:07:53 AM' }

Fetched and parsed { message: 'Message from Bob',
  timestamp: '9/12/2019, 10:07:59 AM' }

Fetched and parsed { message: 'Message from Charlie',
  timestamp: '9/12/2019, 10:08:08 AM' }

Fetched and parsed { message: 'Message from Alice',
  timestamp: '9/12/2019, 10:07:53 AM' }

Fetched and parsed { message: 'Message from Bob',
  timestamp: '9/12/2019, 10:07:59 AM' }

Fetched and parsed { message: 'Message from Charlie',
  timestamp: '9/12/2019, 10:08:08 AM' }

Verify with MAM Explorer:
https://mam-explorer.firebaseapp.com/?provider=https%3A%2F%2Fnodes.devnet.iota.org&mode=public&root=9LBMBRUAJIRNASMNJP99ZMNVKNOER9XGVSLJLECEXBTNADHPWGO9FMBRRAGZPKEPSRLJ9SZYQU9EVLMPC
```

:::success:おめでとうございます:tada:
制限付きMAMチャネルにメッセージを公開しました。このチャネルに公開するものはすべて、サイドキーを付与したユーザーのみが復号化できます。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've published messages to a restricted MAM channel. Anything you publish to this channel can be decrypted only by those to whom you give the side key. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

緑色のボタンをクリックしてサンプルコードを実行し、Webブラウザーで結果を確認します。
<!-- Click the green button to run the sample code and see the results in the web browser. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/MAM-public?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>
