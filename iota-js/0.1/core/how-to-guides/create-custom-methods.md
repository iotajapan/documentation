# カスタムメソッドを作成する
<!-- # Create custom methods -->

**`create`プレフィックスを持つメソッドを使用して、独自のカスタムメソッドを作成できます。**
<!-- **You can use the methods that have the `create` prefix to create your own custom methods.** -->

1. HTTPクライアントパッケージをインストールします。
  <!-- 1. Install the HTTP client package -->

    ```bash
    npm install @iota/http-client
    ```

2. APIメソッドを作成します。
  <!-- 2. Create an API method -->

    ```js
    import { createHttpClient } from '@iota/http-client'
    import { createGetNodeInfo } from '@iota/core'

    const client = createHttpClient({
      // IRIノードアドレスに置き換えます。
      //またはテストのためにDevnetノードに接続します：'https://nodes.devnet.iota.org:443'
      provider: 'http://localhost:14265'
    })

    const getNodeInfo = createGetNodeInfo(client)
    ```
