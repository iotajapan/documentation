# 秘密鍵からアドレスを導出する
<!-- # Derive addresses from private keys -->

**クライアントライブラリを使用して秘密鍵からアドレスを導出することで、アドレス、プライベートキー、およびセキュリティレベルの間の関係についてより深く理解することができます。**
<!-- **By using a client library to derive addresses from private keys, you can gain a better understanding of the relationship among addresses, private keys, and security levels.** -->

:::info:クライアントライブラリを初めて使用されますか？
公式クライアントライブラリを使い始めるための[クイックスタートガイド](root://getting-started/0.1/tutorials/get-started.md)をお試してください。
:::
<!-- :::info:First time using a client library? -->
<!-- [Try our quickstart guide](root://getting-started/0.1/tutorials/get-started.md) for getting started with the official client libraries. -->
<!-- ::: -->

:::info:
秘密鍵、サブシード、およびキーダイジェストという用語に慣れていない場合は、[アドレスと署名](../concepts/addresses-and-signatures.md)についてを読むことをお勧めします。
:::
<!-- :::info: -->
<!-- If you're unfamiliar with the terms private key, subseed, and key digest, we recommend [reading about addresses and signatures](../concepts/addresses-and-signatures.md). -->
<!-- ::: -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完成するには、次のものが必要です。
<!-- To complete this guide, you need the following: -->

* Node.js 8、またはNode.js 10以上。[最新のLTS](https://nodejs.org/en/download/)をお勧めします。
<!-- * Node.js 8, or Node.js 10 or higher. We recommend the [latest LTS](https://nodejs.org/en/download/). -->
* [Visual Studio Code](https://code.visualstudio.com/Download)などのコードエディタ
<!-- * A code editor such as [Visual Studio Code](https://code.visualstudio.com/Download) -->

## 秘密鍵からアドレスを導出する
<!-- ## Derive addresses from private keys -->

この例では、iota.jsライブラリの[`signing`パッケージ](https://github.com/iotaledger/iota.js/tree/next/packages/signing)を使用して、シードから秘密鍵を導出し、次に秘密鍵からアドレスを導出します。
<!-- In this example, we use the [`signing` package](https://github.com/iotaledger/iota.js/tree/next/packages/signing) of the iota.js library to derive private keys from a seed and then to derive addresses from those private keys. -->

1. パッケージを`require`します。
  <!-- 1. Require the packages -->

    ```js
    const Iota = require('@iota/core');
    const Sign = require('@iota/signing');
    const Converter = require('@iota/converter');
    ```

2. `subseed`メソッドに、トライトに変換したシードとインデックスを渡してサブシードを導出します。
  <!-- 2. Derive a subseed by passing a seed in trits and an index to the `subseed()` method -->

    ```js
    const seed = "PUETTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX";

    var subseed = Sign.subseed(Converter.trytesToTrits(seed), 0 /*index*/);
    ```

    :::info:
    シードを使用するコードはすべてクライアント側で実行されます。シードがご使用中のデバイスから離れることはありません。
    :::
    <!-- :::info: -->
    <!-- Any code that uses a seed is executed on the client side. Your seed never leaves your device. -->
    <!-- ::: -->

3. `key`メソッドに同じサブシードと異なるセキュリティレベルを渡して、3つのセキュリティレベルそれぞれに対して1つの秘密鍵を導出します。
  <!-- 3. Derive one private key for each of the three security levels by passing the same subseed and a different security level to the `key()` method -->

    ```js
    var privateKey1 = Sign.key(subseed, 1 /*security level*/);

    console.log('Private key length for security level 1: ' + Converter.tritsToTrytes(privateKey1).length);

    var privateKey2 = Sign.key(subseed, 2 /*security level*/);

    console.log('Private key length for security level 2: ' + Converter.tritsToTrytes(privateKey2).length);

    var privateKey3 = Sign.key(subseed, 3 /*security level*/);

    console.log('Private key length for security level 3: ' + Converter.tritsToTrytes(privateKey3).length);
    ```

    ファイルを実行すると、各秘密鍵の長さがトライトで表示されます。
    <!-- When you execute the file, you should see the length of each private key in trytes: -->

    ```console
    Private key length for security level 1: 2187

    Private key length for security level 2: 4374

    Private key length for security level 3: 6561
    ```

    :::info:
    セキュリティレベルの詳細については[こちら](../references/security-levels.md)。
    :::
    <!-- :::info: -->
    <!-- [Find out more about security levels](../references/security-levels.md). -->
    <!-- ::: -->

4. それぞれの秘密鍵を`digests()`メソッドに渡して、秘密鍵ごとにキーダイジェストを導出します。
  <!-- 4. Derive the key digests for each private key by passing each one to the `digests()` method -->

    ```js
    var privateKey1Digests = Sign.digests(privateKey1);

    console.log(`Total key digests for security level 1: ` + Converter.tritsToTrytes(privateKey1Digests).length/81);

    var privateKey2Digests = Sign.digests(privateKey2);

    console.log(`Total key digests for security level 2: ` + Converter.tritsToTrytes(privateKey2Digests).length/81);

    var privateKey3Digests = Sign.digests(privateKey3);

    console.log(`Total key digests for security level 3: ` + Converter.tritsToTrytes(privateKey3Digests).length/81);
    ```

    ファイルを実行すると、各秘密鍵のキーダイジェスト量がわかります。
    <!-- When you execute the file, you should see the amount of key digests for each private key: -->

    ```console
    Total key digests for security level 1: 1

    Total key digests for security level 2: 2

    Total key digests for security level 3: 3
    ```

    :::info:
    キーダイジェストの詳細については[こちら](../concepts/addresses-and-signatures.md)。
    :::
    <!-- :::info: -->
    <!-- [Find out more about key digests](../concepts/addresses-and-signatures.md). -->
    <!-- ::: -->

5. ダイジェストを`address()`メソッドに渡して、各秘密鍵のアドレスを導出します。
  <!-- 5. Derive an address for each private key by passing the digests to the `address()` method -->

    ```js
    var privateKey1Address = Sign.address(privateKey1Digests);

    console.log('Address with security level 1: ' + Converter.tritsToTrytes(privateKey1Address));

    var privateKey2Address = Sign.address(privateKey2Digests);

    console.log('Address with security level 2: ' + Converter.tritsToTrytes(privateKey2Address));

    var privateKey3Address = Sign.address(privateKey3Digests);

    console.log('Address with security level 3: ' + Converter.tritsToTrytes(privateKey3Address));
    ```

    ファイルを実行すると、各セキュリティレベルごとのアドレスが表示されます。
    <!-- When you execute the file, you should see the addresses for each security level: -->

    ```console
    Address with security level 1: ZWENNY9JOIQRJIRHV9PCQMCHKBXVZTTKMVRSZSKQNQCQCTZMTMUPEWE9DPCVBVZOVGFFI9JYLTIFXGJAX
    Address with security level 2: ECMHBSFPVUWHSUXZBXTWSKNMBGNTW9GAFVJUUSSJYFBOKHNFJBPEKJNMQMCSAIBXVUJNQKUBFUXPEIY9B
    Address with security level 3: LJGSYD9N9JEAQ9AVN9BJCAOW9LFVZGFHOXFVFVLQEBKVZFGBIDJJIRK9FBJUKRS9VMUXTCXBRIOOEMQJ9
    ```

6. 同じアドレスがIOTA core ライブラリから返されることを確認するために、以下を実行します。
  <!-- 6. To check that the same addresses would be returned from the IOTA core library, do the following: -->

    ```js
    console.log(Iota.generateAddress(seed, 0 /*index*/, 1 /*security level*/));
    console.log(Iota.generateAddress(seed, 0 /*index*/, 2 /*security level*/));
    console.log(Iota.generateAddress(seed, 0 /*index*/, 3 /*security level*/));
    ```

    ステップ5のアドレスと同じアドレスが標準出力に表示されます。
    <!-- You should see the same addresses in the output as those from step 8. -->

:::success:おめでとうございます:tada:
IOTA core ライブラリの内部で、アドレスは特定のインデックスとセキュリティレベルを持つ秘密鍵から導出されていることを証明しました。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've proven that, under the hood of the IOTA core library, addresses are derived from private keys with a certain index and security level. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

このガイドのサンプルコードを実行してWebブラウザに結果を表示するには, 緑色のボタンをクリックしてください。
<!-- Click the green button to run the sample code in this guide and see the results in the web browser. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Derive-addresses-from-private-keys?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>
