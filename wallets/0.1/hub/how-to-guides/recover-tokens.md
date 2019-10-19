# スウィープアドレスからIOTAトークンをリカバーする
<!-- # Recover IOTA tokens from a swept address -->

**ユーザーは、既にスウィープされた使用済みの預け入れアドレスにIOTAトークンを送信する場合があります。この場合、攻撃者が署名をブルートフォースしてIOTAトークンを盗もうとするリスクがあります。スウィープされたアドレスからIOTAトークンを回復するために、潜在的な攻撃者ができる前にIOTAトークンを新しいアドレスに転送しようとすることができます。これを行うと、アドレスの秘密鍵がより多く公開されますが、IOTAトークンを安全なアドレスに転送するためには仕方がありません。**
<!-- **Sometimes users send IOTA tokens to spent deposit addresses that have already been swept. In this case, the address is at risk of an attacker trying to brute force its signature to steal its tokens. To recover the tokens from the swept address, you can try to transfer them to a new address before a potential attacker can. Doing so exposes more of the address's private key, but this is inevitable to transfer the tokens to a safe address.** -->

:::info:
このガイドでは、使用済みアドレスからIOTAトークンをリカバーするために、`signBundle()` gRPC API呼び出しを使用します。このメソッドは、使用済みアドレスの合計残高を1つ以上の出力アドレスに預け入れるカスタムバンドルを作成するのに役立ちます。

使用済みアドレスの合計残高を単一の出力アドレスに転送するには、[`recoverFunds()`メソッド](../references/grpc-api-reference.md#hub.rpc.RecoverFundsRequest)を使用する方が簡単です。
:::
<!-- :::info: -->
<!-- In this guide, we use the `signBundle()` gRPC API call to recover IOTA tokens from a spent address. This method is useful for creating a custom bundle that deposits any amount of the spent address's total balance into one or more output addresses. -->

<!-- To transfer the total balance of a spent address into a single output address, it's easier to use the [`recoverFunds()` method](../references/grpc-api-reference.md#hub.rpc.RecoverFundsRequest). -->
<!-- ::: -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完了するには、次のものが必要です。
<!-- To complete this guide, you need the following: -->

* Node.js 8、またはNode.js 10以降。 [最新のLTS](https://nodejs.org/en/download/)をお勧めします。
<!-- * Node.js 8, or Node.js 10 or higher. We recommend the [latest LTS](https://nodejs.org/en/download/). -->
* [Visual Studio Code](https://code.visualstudio.com/Download)などのコードエディター
<!-- * A code editor such as [Visual Studio Code](https://code.visualstudio.com/Download) -->
* コマンドプロンプトへのアクセス
<!-- * Access to a command prompt -->
* [`@iota/bundle`](https://github.com/iotaledger/iota.js/tree/next/packages/bundle), [`@iota/core`](https://github.com/iotaledger/iota.js/tree/next/packages/core), [`@iota/converter`](https://github.com/iotaledger/iota.js/tree/next/packages/converter), および[`@iota/transaction`](https://github.com/iotaledger/iota.js/tree/next/packages/transaction)パッケージ
<!-- * The [`@iota/bundle`](https://github.com/iotaledger/iota.js/tree/next/packages/bundle), [`@iota/core`](https://github.com/iotaledger/iota.js/tree/next/packages/core), [`@iota/converter`](https://github.com/iotaledger/iota.js/tree/next/packages/converter), and [`@iota/transaction`](https://github.com/iotaledger/iota.js/tree/next/packages/transaction) packages -->
* ハブの[`SignBundle_enabled`フラグ](../references/command-line-flags.md#signBundle)が`true`に設定されていることを確認します。
<!-- * Make sure that Hub's [`SignBundle_enabled` flag](../references/command-line-flags.md#signBundle) is set to `true`. -->

:::info:
IOTAクライアントライブラリを使用したことがない場合は、[入門チュートリアル](root://getting-started/0.1/tutorials/send-a-zero-value-transaction-with-nodejs.md)を完了することをお勧めします。
:::
<!-- :::info: -->
<!-- If you've never used the IOTA client libraries before, we recommend completing [the getting started tutorial](root://getting-started/0.1/tutorials/send-a-zero-value-transaction-with-nodejs.md) -->
<!-- ::: -->

## 手順1. 未署名のバンドルを作成する
<!-- ## Step 1. Create an unsigned bundle -->

ハブがバンドルに署名する前に、未署名のバンドルを作成する必要があります。
<!-- Before Hub can sign a bundle, you need to create an unsigned one. -->

:::info:
このガイドでは、JavaScriptクライアントライブラリを使用してバンドルを作成および送信しますが、Go、Java、Pythonなどの他の[公式およびコミュニティライブラリ](root://client-libraries/0.1/introduction/overview.md)もあります。
:::
<!-- :::info: -->
<!-- <!-- In this guide, we use the JavaScript client library to create and send the bundle, but we also have other [official and community libraries](root://client-libraries/0.1/introduction/overview.md), including Go, Java, and Python. --> -->
<!-- ::: -->

1. パッケージを要求します。
  <!-- 1. Require the packages -->

    ```js
    const Iota = require('@iota/core');
    const Bundle = require('@iota/bundle');
    const Transaction = require('@iota/transaction');
    const Converter = require('@iota/converter');
    ```

2. 署名されていないバンドルを作成し、ファイルに保存する関数を作成します。
  <!-- 2. Write a function that creates an unsigned bundle and saves it to a file -->

    ```js
    async function createUnsignedBundle({ outputAddress, inputAddress, securityLevel, value }) {
    let bundle = new Int8Array();
    const issuanceTimestamp = Converter.valueToTrits(Math.floor(Date.now() / 1000));

    bundle = Bundle.addEntry(bundle, {
      address: outputAddress,
      value: Converter.valueToTrits(value),
      issuanceTimestamp
    });

    // セキュリティレベルごとに、新しいゼロトークントランザクションを作成します。このトランザクションに後で残りの署名フラグメントを追加できます。
    for (let i = 0; i < securityLevel; i++) {
      bundle = Bundle.addEntry(bundle, {
        address: inputAddress,
        value: Converter.valueToTrits(i == 0 ? -value : 0),
        issuanceTimestamp
      });
    }

    const result = await Bundle.finalizeBundle(bundle);

    // バンドル配列をバイナリファイルに保存します。
    fs.writeFileSync('bundle', result, (error) => {
      if(!error) {
        console.log('Bundle details saved to file');
      } else{
        console.log(`Error writing file: ${error}`);
      }});

    const bundleHash = Converter.tritsToTrytes(Transaction.bundle(result));

    console.log(bundleHash);
    }
    ```

3. ハブから次の値を取得し、それらを`parameters`オブジェクトに追加します。
  <!-- 3. Get the following values from Hub and add them to the `parameters` object: -->

    | **フィールド** | **説明** | **メモ** |
    | :------------- | :------- | :------- |
    | `outputAddress` | スウィープアドレス上のIOTAトークンを転送する新しい81トライトのアドレス（チェックサムなし） | このアドレスは、ハブアドレスである必要はありません。たとえば、IOTAトークンをハードウェアウォレットのアドレスに送信できます。 |
    | `inputAddress` | 回復する必要があるIOTAトークンを含む、スウィープした81トライトのアドレス（チェックサムなし） | [`balanceSubscription()`メソッド](../references/grpc-api-reference.md#hub.rpc.BalanceSubscriptionRequest)を使用して、受信アドレスへの預け入れをチェックすることをお勧めします。[`getUserHistory()`メソッド](../references/grpc-api-reference.md#hub.rpc.GetUserHistoryRequest)を使用して、どの使用済みアドレスに正のバランスがあるかを確認することもできます。 |
    | `securityLevel` | スウィープアドレスのセキュリティレベル | デフォルトのセキュリティレベルは2です。[`keySecLevel`コマンドラインフラグ](../references/command-line-flags.md#keySec)のセキュリティレベルを変更した場合は、必ずそれを使用します。 |
    | `value` | `inputAddress`フィールドのスウィープアドレスの合計残高 | [thetangle.org](https://thetangle.org/)などのタングルエクスプローラーでアドレスの残高を確認できます。 |

    <!-- |**Field**|**Description**|**Notes**| -->
    <!-- |:----|:----------|:-----------| -->
    <!-- |`outputAddress`|The new 81-tryte address (without a checksum) to which you want to transfer the tokens on the swept address|This address does not need to be a Hub address. For example, you may want to send the tokens to an address on a hardware wallet. | -->
    <!-- |`inputAddress`|The swept 81-tryte address (without a checksum) that contains the IOTA tokens that you need to recover|It's best practice to use the [`balanceSubscription()` method](../references/grpc-api-reference.md#hub.rpc.BalanceSubscriptionRequest) to check for incoming deposits into swept addresses. You can also use the [`getUserHistory()` method](../references/grpc-api-reference.md#hub.rpc.GetUserHistoryRequest) to check which spent addresses have a positive balance.| -->
    <!-- |`securityLevel`| The security level of the swept address|The default security level is 2. If you changed the security level in the [`keySecLevel` command-line flag](../references/command-line-flags.md#keySec), make sure you use that one. | -->
    <!-- |`value`|The total balance of the swept address in the `inputAddress` field|You can check the balance of any address on a Tangle explorer such as [thetangle.org](https://thetangle.org/) | -->

    :::info:
    `createUnsignedBundle()`関数が[signature fragment](root://dev-essentials/0.1/concepts/addresses-and-signatures.md#signatures)を追加できる十分なゼロトークントランザクションを作成できるように、スウィープアドレスのセキュリティレベルが必要です。
    :::
    <!-- :::info: -->
    <!-- You need the security level of the swept address so that the `createUnsignedBundle()` function can create enough zero-value transactions to which you can add the [signature fragments](root://dev-essentials/0.1/concepts/addresses-and-signatures.md#signatures). -->
    <!-- ::: -->

    ```js
    let outputAddress = Converter.trytesToTrits('ADDRESS...');
    let inputAddress = Converter.trytesToTrits('ADDRESS...');

    const params = {
      outputAddress: outputAddress,
      inputAddress: inputAddress,
      securityLevel: 2,
      value: 1
    }
    ```

2. `createUnsignedBundle()`関数を呼び出し、ハブから取得したパラメーターを渡します。
  <!-- 2. Call the `createUnsignedBundle()` function, and pass it the parameters you got from Hub -->

    ```js
    createUnsignedBundle(parameters);
    ```

    この関数は、現在のディレクトリにある`bundle`と呼ばれるバイナリファイルに未署名のバンドルを保存します。
    <!-- This function saves the unsigned bundle to a binary file called `bundle` in your current directory. -->

3. ファイルを実行し、未署名のバンドルハッシュをコンソールからクリップボードにコピーします。ハブは、署名を作成するためにこのバンドルハッシュを必要とします。
  <!-- 3. Execute the file, then copy the unsigned bundle hash from the console to the clipboard. Hub needs this bundle hash to create the signature. -->

## 手順2. ハブを使用して未署名のバンドルに署名する
<!-- ## Step 2. Use Hub to sign the unsigned bundle -->

Hubには`signBundle()`gRPCメソッドがあり、ハブユーザーのアドレスから取り出しバンドルに署名できます。
<!-- Hub has a `signBundle()` gRPC method that allows you to sign bundles that withdraw from a Hub user's address. -->

:::info:
`signBundle()`メソッドを使用する前に、ハブの[`SignBundle_enabled`フラグ](../references/command-line-flags.md#signBundle)が`true`に設定されていることを確認します。
:::
<!-- :::info: -->
<!-- Before you use the `signBundle()` method, make sure that Hub's [`SignBundle_enabled` flag](../references/command-line-flags.md#signBundle) is set to `true`. -->
<!-- ::: -->

1. ハブで、未署名のバンドルハッシュとスウィープしたアドレスを`signBundle()`メソッドに渡します。
  <!-- 1. In Hub, pass the unsigned bundle hash and the swept address to the `signBundle()` method -->

    ```bash
    Hub@localhost:50051> client.signBundle({address:'ADDRESS...',bundleHash:'BUNDLEHASH...',authentication:'',validateChecksum:false},pr)
    ```

2. 返された署名をコピーし、`trytesToTrits()`メソッドにペーストして、トライトに変換します。
  <!-- 2. Copy the returned signature and paste it into the `trytesToTrits()` method to convert it to trits -->

    ```js
    const signature = Converter.trytesToTrits('SIGNATURE...')
    ```

3. 前に保存した`bundle`ファイルから未署名のバンドルを読み取ります。
  <!-- 3. Read the unsigned bundle from the `bundle` file you saved earlier -->

    ```js
    bundle = new Int8Array(fs.readFileSync('bundle'));
    ```

4. 署名を未署名のバンドルに追加します。
  <!-- 4. Add the signature to the unsigned bundle -->

    ```js
    bundle.set(Bundle.addSignatureOrMessage(bundle, signature, 1));
    ```

    :::info:
    3番目の引数は、署名フラグメントの追加を開始するバンドル内の最初のトランザクションを指定します。この例では、最初のトランザクションは出力トランザクションであるため、署名は必要ありません。
    :::
    <!-- :::info: -->
    <!-- The third argument specifies the first transaction in the bundle to which to start adding the signature fragments. In our example, the first transaction is the output transaction, so it doesn't need a signature. -->
    <!-- ::: -->

## 手順3. 署名済みバンドルをノードに送信する
<!-- ## Step 3. Send the signed bundle to a node -->

バンドル内の入力トランザクションに署名フラグメントを追加すると、バンドルは署名され、ノードに送信する準備が整います。
<!-- After adding the signature fragments to the input transactions in your bundle, it's now signed and ready to be sent to a node. -->

1. 署名済みバンドル内のトランザクションをトライトに変換し、それらを新しい配列にプッシュします。
  <!-- 1. Convert the transactions in the signed bundle to trytes and push them into a new array -->

    ```js
    const trytes = []
    for (let offset = 0; offset < bundle.length; offset += Transaction.TRANSACTION_LENGTH) {
      trytes.push(Converter.tritsToTrytes(bundle.subarray(offset, offset + Transaction.TRANSACTION_LENGTH)));
    }
    ```

2. `provider`フィールドにURLを追加して、ノードに接続します。
  <!-- 2. Connect to a node by adding the URL of one to the `provider` field -->

    ```js
    const iota = Iota.composeAPI({
      provider: ''
    });
    ```

3. ノード構成オプションを設定します。
  <!-- 3. Set the node configuration options -->

    ```js
    const depth = 3;
    const minWeightMagnitude = 14;
    ```

    :::info:Depth
    `depth`引数は[チップ選択](root://node-software/0.1/iri/concepts/tip-selection.md)に影響します。`depth`が大きいほど、タングルの奥に戻り、重み付きランダムウォークが始まります。
    :::
    <!-- :::info:Depth -->
    <!-- The `depth` argument affects [tip selection](root://node-software/0.1/iri/concepts/tip-selection.md). The greater the depth, the farther back in the Tangle the weighted random walk starts. -->
    <!-- ::: -->

    :::info:最小重量値（Minimum weight magnitude）
    [`最小重量値`](root://dev-essentials/0.1/concepts/minimum-weight-magnitude.md)（MWM）引数は、プルーフオブワーク（PoW）の難易度に影響します。MWMが大きいほど、PoWは難しくなります。

    すべてのIOTAネットワークは、独自のMWMを強制します。Devnetでは、MWMは9です。しかし、Mainnetでは、MWMは14です。小さすぎるMWMを使用すると、トランザクションは有効にならず、確定されません。
    :::
    <!-- :::info:Minimum weight magnitude -->
    <!-- The [`minimum weight magnitude`](root://dev-essentials/0.1/concepts/minimum-weight-magnitude.md) (MWM) argument affects the difficulty of proof of work (PoW). The greater the MWM, the more difficult the PoW. -->
    <!--  -->
    <!-- Every IOTA network enforces its own MWM. On the Devnet, the MWM is 9. But, on the Mainnet the MWM is 14. If you use a MWM that's too small, your transactions won't be valid and will never be confirmed. -->
    <!-- ::: -->

4. バンドルのトランザクショントライトをノードに送信します。
  <!-- 4. Send the bundle's transaction trytes to the node -->

    ```js
    iota.sendTrytes(trytes.reverse(), depth, minWeightMagnitude)
    .then(bundle => {
      console.log(`Sent bundle: ${JSON.stringify(bundle, null, 1)}`)
    })
    .catch(error => {
      console.log(error);
    });
    ```

    :::info:
    ここで、ノードはバンドルの先頭トランザクションから先に送信されることを期待しているため、トランザクショントライト配列を逆にします。
    :::
    <!-- :::info: -->
    <!-- Here, we reverse the transaction trytes array because nodes expect a bundle to be sent head first. -->
    <!-- ::: -->

:::success:おめでとうございます:tada:
スウィープしたアドレスからIOTAトークンを安全なアドレスに預け入れしてリカバーする署名付きバンドルを送信しました。
:::
<!-- :::success: -->
<!-- You've sent a signed bundle that recovers IOTA tokens from a swept address by depositing them into a safe one. -->
<!-- ::: -->

:::warning:
バンドルが確定されるまで、IOTAトークンは攻撃者によって取り出される危険性があります。

バンドルが確定される可能性を高めるには、[促進と再添付](root://dev-essentials/0.1/how-to-guides/confirm-pending-bundle.md)を行います。
:::
<!-- :::warning: -->
<!-- Until the bundle is confirmed, the tokens are still at risk of being withdrawn by an attacker. -->
<!--  -->
<!-- To increase the chances of your bundle being confirmed, you can [promote and reattach it](root://dev-essentials/0.1/how-to-guides/confirm-pending-bundle.md). -->
<!-- ::: -->

## サンプルコード
<!-- ## Sample code -->

これは、このチュートリアルのテストに使用したスウィープアドレスの例のコードです。
<!-- This is the code for an example swept address that we used to test this tutorial. -->

タングルエクスプローラーで[このアドレスの履歴を表示](https://thetangle.org/address/LIQJBJRBSTGYWHYRPCLLCZUMP9SLHCBBWGQ9YRFWYDFF9FMXIAELYLTTBXCPVIDWWZYIOJIFLUFYVZIBD)できます。
<!-- You can [see the history of this address](https://thetangle.org/address/LIQJBJRBSTGYWHYRPCLLCZUMP9SLHCBBWGQ9YRFWYDFF9FMXIAELYLTTBXCPVIDWWZYIOJIFLUFYVZIBD) on a Tangle explorer. -->

```js
const Iota = require('@iota/core');
const Bundle = require('@iota/bundle');
const Transaction = require('@iota/transaction');
const Converter = require('@iota/converter');
const fs = require('fs');

async function createUnsignedBundle({ outputAddress, inputAddress, securityLevel, value }) {
  let bundle = new Int8Array();
  const issuanceTimestamp = Converter.valueToTrits(Math.floor(Date.now() / 1000));

  bundle = Bundle.addEntry(bundle, {
    address: outputAddress,
    value: Converter.valueToTrits(value),
    issuanceTimestamp
  });

  // セキュリティレベルごとに、残りの署名フラグメントを追加する新しいゼロトークントランザクションが必要です。
  for (let i = 0; i < securityLevel; i++) {
    bundle = Bundle.addEntry(bundle, {
      address: inputAddress,
      value: Converter.valueToTrits(i == 0 ? -value : 0),
      issuanceTimestamp
    });
  }

  const result = await Bundle.finalizeBundle(bundle);

  fs.writeFileSync('bundle', result, (error) => {
    if(!error) {
      console.log('Bundle details saved to file');
    } else{
      console.log(`Error writing file: ${error}`);
    }});

  // バンドルハッシュのトライトを取得します。
  const bundleHash = Converter.tritsToTrytes(Transaction.bundle(result));

  // ハブでバンドルハッシュを使用して署名を取得します。
  console.log(bundleHash);
}

// これらのパラメーターの値をハブから取得します。

let outputAddress = Converter.trytesToTrits('LYRGKMBCVZMTPRRMPDNNRXFVKIXTZCJTZDOAMTHZXQNESSLYGVQ99PGAW9OCAPRGSSSDMFCMZ9NAJTWUX');
let inputAddress = Converter.trytesToTrits('LIQJBJRBSTGYWHYRPCLLCZUMP9SLHCBBWGQ9YRFWYDFF9FMXIAELYLTTBXCPVIDWWZYIOJIFLUFYVZIBD');

const params = {
  // Address in trits to send the tokens from the swept address (without checksum)
  // スウィープされるアドレスからIOTAトークンを送信するためのトライトのアドレス（チェックサムなし）
  outputAddress: outputAddress,
  // スウィープされるアドレスのトライト（チェックサムなし）
  inputAddress: inputAddress,
  // スウィープされるアドレスのセキュリティレベル
  securityLevel: 2,
  // Total amount of IOTA tokens to withdraw from the swept address
  // スウィープされるアドレスから取り出されるIOTAトークンの合計量
  value: 1
}

createUnsignedBundle(params);

// Replace with the signature from Hub in trits
// トライトでハブからの署名に置き換えます。
const signature = Converter.trytesToTrits("DRFFHLSWJBLEBNHXMSLIGUKBQJIJRNHGMGVDFKKPF9DYABMQXDVLWPHXT9LEPSXVBIYKZEWCT9U99NJ9DQJVJJMBGCUHTZQBCYWEIN9DCSDPJXRKGQQBEHPBZZWRFNBIENYV9UNSSNXZHGATAWRTHEW9FQVTXUJLFCQEPLVWATFRXFAUSNFKVVYFISDULSVIQNEQVPNIKZSBJQHWAZMGOIPZSKY9QDVMCFXUS9UGUDK9BOZFAWXQFHFBEYUJBESIFMBWNXVFCKHTGXTWKLSMZ9FHYFUZKE9GTLFWXPCLFPQKCJQAZFSTGWJILTACIXLRRJWDGIGJOCTUCEBFLWFQBQKAHTJMAEWZSDMOTQ9GPALHYYRGTFYJCDRFKZFUPECKUEVZJPVQDYIDJAIHSFSWZSQLQIJSCKJPJPLFVHXLUMLZKUHJWRVPBIOESDIFCUCKZAGANIATOUCSF9IOUA9EQWCSOFMETZEADESOADJGLCDKUQIBEPMXBWNIHJMJHCYUSM9LXMEBTHORZDGYST9VJLUUSXTSAURNUZAUPNWIUSRVL9KGFCEIAIWTNIH9BIRCSLWDAPXYKSZAOFERYFGPBWYUIEEENGMMYVPRRRIXULRJWJCLUBBNMNLFMNPYDHVBFZUFRCD9PCZAQLDBJFFKGQJW9LFUUPIZPKUCBAJFUEUTMXKWUKFXKQISDXYIHEJSQTCEHVABAUWGGANWZCNBOLGZDCLXSZIUKRMFG9THEZTULVZ9CFGNUWCTQVSACVYSZASONJIGUSXZAZNVCBIWRHLOKCXPXFONAWNMWRMGAXXJNPYRNDKAKIEWNSDRSNTSTMDPUYNPSIJTWERECFKSNTWOKCMDIUEKEYXORVIQVQXKWOWNMVBLWVJIWBWKAHHECGLNZAADFVSATZDMLYXBGRCEATLTCMQACYQQDQOXXMSUWSZVZKEXQUYM9DZMMOMBIVCPJU9UUKAXOSQQX9QRPTFXSZPBZFFJLGOBVZZOT9LNWMA9LTRUZUJBDGH9GILESQLNYDYPCHHYICVMAPDVDILEOZDNZMVSERSDWLZ9ZXFKJXISY9BLXZZ9DJYIRPXNLGAWMDZOXOWBUDZRUROHKPREZJTXNSUNC9LDHGFLGYOXBAQVGYUZXECCKUUFOPSLPH9QRLAZUOWGBQBVEJBAZKUEWSUIVGLEULZKDXFQSGQNZYXXZCYPRZUSBPMVPLWMYDWEGXANNXLOTME9HQGQW9POKCSQWHLHUUUWNFFHXZYDYUYBMSJVWSAKMEFJDJNJZHIYP9NBLFWVKBSDIUXLAVLMYLLCRX9BZZZQEESERVDIHKKPVAM9UEOXR9CFW9OAXLCLMJHWEOSTCYYHPWHJZEKAXPBGNGVMCGNHOUWMJX9GEV9BBGYVNEG9AXSNPFEGOZNZWXMNXVV9CVJ9Z9DGLYOUWZKFJYIIWRUEYPGDFSJGRXZDS9RKFELFVMASCAPXVCPLNQRJHADACOOGGHOVTTG9DCOCMHTMTMNEVASXHKLBAWKNFTFDAEEHATXGUKBRLDXNLSHZEPO9OZQBSHJRLGLNPKHAX9WRMYARVJETYAPO9SUWZQHAKJAUWBIWQDSQ9FWIEWSJWKQGNIFGGVKZLWZZOVVBGPNRTONMZDLJHGWQUSCILYOBG9YUUUZDXILKBIQ9ZEKXEHURJLFMDB9CUZI9TEJSQZYHMNTX9F9GJBZ9UJNCKZVYAROOVZYZURRWKLYHJEMZDQLH9FWODFZUEZRV9DWNZCOMXNXFWCPCZQDGFP9DKJYUNXIYIRUVWRSWWIGKZWITSYIVEOPHBJGSZNYJSFTXAKLENHCPGOYQSGEZQMRVNFGAGZXFG9GYFIBJRVU9MYDSZSTJQVVODQJJEHYJYCHOHTCOPZJVGMBEIIGYFMNAPZATAHQGFVWSGYODTOHRE9FVYXSLESASDMMAPBZZVPLIWGIRORAGWNFA9JPSUWJCTNBYRDJANSYMNDGQJNSFQRSVMRIAXDWQONWSAYUYFXJUTWYLPXVV9YGJBMQUXODSEFAEGYXSYYEQTVRAWNCKKSWPRYYINMBOBNZJSANFLKDFMGTNUDU9SFQRBUJM9XDNWIECTJPEICLVXTMCMGQJT9BTRZ9HQCHHBYXSPJBGTSTJDIOUQ9SLRVHTENSCLLGBFMXOYFKYB9SAKWTPGHEXPU9ERFYGEMEPSLJUPRJNRRWQLA9XVSWBEGJHLLDXGDTESDPRBRQHLCIOPWBDYJVCLOHHDYFNQSXBOVUVICSVBMEZYJJZHGDKAHFRJHBDWUSFSGC9UCLVH9ZNVMPYMRMMJTSWAFDRGNALYLRRVPLGMMKGZR9LXEV9ZXRPFKYDBLIUEVEJKVDANOBKFLRSWQPMOMTEJA9WVHFGBNTAJIPWNUWKJMQAOKBCACLYGRCBZRVFKIOMCXYXGUTNODHMLOZRETEPDONTAPVQEQAWCAH9CSTPYRFUXPKBDVONWFNOZHA9N9YUTEIGAHAPEKNWGCXNAMVJVLPCAPDPNTVVSKROYWXDKVLOAHPCCJBGBPSMQEDFDXSGNMIMLEZPBVCKYKVZFFIZVECYQKQWDHXRVEAOL9QIDZK9JFBRYL9JOSAWMBZAXJWNBTLKRSIYHZEILOPRZVLOFACJGIPABPFZILVQMUAYMPABXEBDYOSYBZKUXCFVXUEQJJOUDMBZZGOBVBF9N9ENHSCTDVFASKXHOLLFYMMWSDFHJKHODXYPWMEOQZKIECIBHYXF9PXZOCBMIBI9PLVDVBGALAWBT9FLIZWRNENVLSUCSOATNVRUKTTWZNTZRCMTLALKYGOUKVSO9BBUDVUCFCIKQZWJIUBMXTSNQSFRETWAHPQ9MTYOUKX9BLA9QUUHPNLNINNIPWQVUPUCXHTMURNWYSUGOKWPYMZ9ILDJWEOD9D99YWKQTKKOOWQOLEZMTZ99AYQPWUVVZUNVORK9XRAE9GQSHLVIZLCEDWHSWYNVZKELOI9AVGUWFSJYHA9WZNVNZYFEZYJAXKUOPINHR9OEN9NVLNIDEJLPLVEMIANSGFXXXL9IKQAFLFPOPWO9SAEMNETZMUM9NEHAA9JOGLHOBHYHXASFWHGNKCRNWZSCDPIFGFHOFLWCOMNCPRBECDEVSNXGKNF9IOYIVZRBJJHCREKOFHMTXXALIWOGOZRIJITCWGKQKCVUPUOFNMEUGSVPYAKRORXQHUXJVCAEWJBLAPDDSCIYMODOMM9GWQSVQUBGBUUDVGSSHAONRHVMILHPMBHKKHUTRZFWNAEEUHCQUGHLSVXAMOGVTXELPGKXBHELPCGYRCLMYGIZYLANN9LSXCQVQPBLIZXJUVRTA9CQAFSWWTIBILERJDTYHJWDPOYHNCHJEWSVXEJCPZVNVLWWOAZHJLLTS9WAEXXSMWHITJYJLFHGGDFNYIGDKJFUZGSJDCXSVNZQYUSAPVJRHSRNLNQDLDBEFPLRXJ9MIZBPNBZVHHLOYC9VYLDKNJAHLNPRCSOZKHWHQKXYWXABCPHYNQYRZJEARZCOLQMBCJCHNNSFGORUOHFOFJMF9MJHHFFVBCS9QITDPGXTJKDDJKHGBOLEVMXRAWUMCXUABILEYVHRTZFFMAM9REMUPNEPUHKWFHOJEFBLXXYHI99UDFMXLXEFWQLPTKJJQEQAXYCQNCZMMOTSKGQCJNFCWGUISHWZLDWJBNNAFDDOMOJ9QRVDEXXNHLKVP9CQJSGZRT9CHDTUJIXWM9WB9RTYQ9EEIJWEVSGKQPFLKG9HKNWSGWZUAGOBENKKT9NUMUIAPWZ9UESGRDITJSOMJAPHIN9CJSSUJCAKCLTQPJOUEMKVLSZZ9MAEROPNSBHPHOFBVVOSNTWPSCRTAYQJQSG9YXPRMAHREBCQKMRFISYDXXNRMGEHCXFNH9SAGFOYAHFCQRNQFRXDIJAIBIRPBNVQZLZNJHBVCZHOIHJURHCFFVHFCPBSKGLQOCXKWXSFINGLYQAH9YGPFWJGQDOUIMFJJOFGNDREMEIMDKOEQONGHTVYWVXWHQQOJGCM9YEHBCVWNJLJXK9HQB9BDYTUJVTHYU9R9DPMUHTLB9NGFEEVKIUANTFHUQRRK9LSSFMUBZLKJTJLPQLBEUSVAFJURBSGFNMBZCDKEUTCNYHBZPUCZGQDAP9JISCORGZVTMLCFRDHKJBCEOYOHEHUDTHMGHMFDTPKGNQPCIBTSOCISWKFSMPDVUKPAWVACALATUSWJTJPXHJWUFDEICUNUDSHGYEGRPMDRABLMFCHXNUA9LHNUMDKXH99XGJKU9XMUXFOXKWLI9AFNDD9CSETSFB9MLEMEY9UMHQQYROBZZEBGHRQRERMHILEVEGNOBSFUIIIJZHRFPOBXHGRSXPYEC");

bundle = new Int8Array(fs.readFileSync('bundle'));

// トランザクション0は出力トランザクションなので、バンドル内の次のトランザクションから署名フラグメントの追加を開始します。
bundle.set(Bundle.addSignatureOrMessage(bundle, signature, 1));

const trytes = []
for (let offset = 0; offset < bundle.length; offset += Transaction.TRANSACTION_LENGTH) {
  trytes.push(Converter.tritsToTrytes(bundle.subarray(offset, offset + Transaction.TRANSACTION_LENGTH)));
}

const iota = Iota.composeAPI({
  // トランザクションを送信するIRIノードのURLに置き換えます。
  provider: 'https://pow.iota.community:443'
});

const depth = 3;
const minWeightMagnitude = 14;

// バンドルは、ノードに送信する前に先頭トランザクションから末尾トランザクションまで順番に並んでいる必要があります。
iota.sendTrytes(trytes.reverse(), depth, minWeightMagnitude)
  .then(bundle => {
    console.log(`Sent bundle: ${JSON.stringify(bundle, null, 1)}`)
  })
  .catch(error => {
    console.log(error);
  });
```
