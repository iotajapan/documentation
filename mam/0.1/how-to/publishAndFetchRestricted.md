# 制限されたメッセージの公開
<!-- # Publishing restricted messages -->

**この例を使用して、制限されたメッセージを公開できます。制限モードは、発行者がメッセージを表示できる購読者を制限するために発行者が使用するMAMプライバシーモードです。**
<!-- **Using this example you can publish a restricted message.** Restricted is the MAM privacy mode used by publishers to limit which subscribers may view their messages.** -->

MAMクライアントライブラリとASCIIからトライトへのコンバーター（`asciiToTrytes`）とトライトからASCIIへのコンバーター（`trytesToAscii`）をインポートします。
<!-- First, import the MAM client library and the ascii to tryte and tryte to ascii converters -->

```js
const Mam = require('../lib/mam.client.js')
const { asciiToTrytes, trytesToAscii } = require('@iota/converter')
```

プライバシーモードを制限モードに設定します。`secretKey`を設定します。この例では、`VERYSECRETKEY`です。プロバイダーを構成します。この例では、`Devnet`と呼ばれるIOTAテストネットワークを使用します。mamExplorerLinkを構成します。この例では、IOTA MAMエクスプローラーを使用します。
<!-- Set the privacy mode to restricted.  Set the secretKey.  In this example, it is 'VERYSECRETKEY'.  Configure the provider.  This example uses the IOTA testbed, called "Devnet".  Configure the mamExplorerLink.  This example uses the IOTA MAM explorer. -->

```js
const mode = 'restricted'
const secretKey = 'VERYSECRETKEY'
const provider = 'https://nodes.devnet.iota.org'

const mamExplorerLink = `https://mam-explorer.firebaseapp.com/?provider=${encodeURIComponent(provider)}&mode=${mode}&key=${secretKey.padEnd(81, '9')}&root=`
```

MAMにプロバイダーを使用するように指示します。
<!-- Tell MAM to use the provider -->

```js
// Initialise MAM State
let mamState = Mam.init(provider)
```

メッセージを暗号化するときに秘密鍵を使用するようにMAMに指示します。
<!-- Tell MAM to use the secret key when encrypting the message -->

```js
// Set channel mode
mamState = Mam.changeMode(mamState, mode, secretKey)
```

ASCIIデータをトライトに変換し、jsonに保存します。 MAMメッセージを作成します。
<!-- Convert the ascii data to trytes and store it in json.  Create your MAM message -->

```js
// Publish to tangle
const publish = async packet => {
    // Create MAM Payload - STRING OF TRYTES
    const trytes = asciiToTrytes(JSON.stringify(packet))
    const message = Mam.create(mamState, trytes)

    // Save new mamState
    mamState = message.state
```

タングルにメッセージを送信します。このMAMストリームのMAMチャネルIDであるメッセージルートを取得します。
<!-- Send your message to the Tangle. You will get the message root which is the MAM channel ID for this MAM stream -->

```js
    // Attach the payload
    await Mam.attach(message.payload, message.address, 3, 9)

    console.log('Published', packet, '\n');
    return message.root
}
```

3つのメッセージを公開します。
<!-- Publish three messages -->

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

各メッセージを取得します。
<!-- Fetch each message -->

```js
// Callback used to pass data out of the fetch
const logData = data => console.log('Fetched and parsed', JSON.parse(trytesToAscii(data)), '\n')

publishAll()
  .then(async root => {

    // Output asyncronously using "logData" callback function
    await Mam.fetch(root, mode, secretKey, logData)

    // Output syncronously once fetch is completed
```

メッセージをデコードするには`secretKey`が必要です。
<!-- You need the secretKey to decode the message -->

```js
    const result = await Mam.fetch(root, mode, secretKey)
    result.messages.forEach(message => console.log('Fetched and parsed', JSON.parse(trytesToAscii(message)), '\n'))
```

MAMエクスプローラーでこれらのメッセージを表示するためのコマンドを出力します。
<!-- Print the command for viewing these messages in the MAM Explorer -->

```js
    console.log(`Verify with MAM Explorer:\n${mamExplorerLink}${root}\n`);
  })
```
