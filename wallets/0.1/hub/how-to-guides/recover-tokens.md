# 使用済みアドレスから IOTA トークンをリカバーする
<!-- # Recover IOTA tokens from a spent address -->

**ユーザーが[スウィープ](../concepts/sweeps.md)に既に含まれている[使用済みアドレス](root://getting-started/0.1/clients/addresses.md#spent-addresses)に IOTA トークンをデポジットする場合があります．この場合，攻撃者が署名をブルートフォースして IOTA トークンを盗もうとするリスクがあります．使用済みアドレスから IOTA トークンをリカバーするために，潜在的な攻撃者ができる前に IOTA トークンを新しいアドレスに転送しようとすることができます．これにより，アドレスの秘密鍵がより多く公開されますが，IOTA トークンを安全なアドレスに転送するためには避けられません．**
<!-- **Sometimes users deposit IOTA tokens into [spent addresses](root://getting-started/0.1/clients/addresses.md#spent-addresses) that have already been included in a [sweep](../concepts/sweeps.md). In this case, the address is at risk of an attacker trying to brute force its signature to steal its tokens. To recover the tokens from the spent address, you can try to transfer them to a new address before a potential attacker can. Doing so exposes more of the address's private key, but this is inevitable to transfer the IOTA tokens to a safe address.** -->

このガイドでは，`signBundle()` gRPC メソッドを使用して，使用済みアドレスから IOTA トークンをリカバーします．この方法は，使用済みアドレスの合計残高を1つ以上の出力アドレスにデポジットするカスタムバンドルを作成するのに役立ちます．
<!-- In this guide, we use the `signBundle()` gRPC method to recover IOTA tokens from a spent address. This method is useful for creating a custom bundle that deposits any amount of the spent address's total balance into one or more output addresses. -->

:::info:
使用済みアドレスの合計残高を単一の出力アドレスに転送するには，`recoverFunds()` メソッドを使用する方が簡単です．
:::
<!-- :::info: -->
<!-- To transfer the total balance of a spent address into a single output address, it's easier to use the `recoverFunds()` API call. -->
<!-- ::: -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完了するには，次のものが必要です．
<!-- To complete this guide, you need the following: -->

- [ハブのインスタンス](../how-to-guides/install-hub.md)
<!-- - An [instance of Hub](../how-to-guides/install-hub.md) -->
- [Node.js の開発者環境](root://client-libraries/0.1/getting-started/js-quickstart.md)
<!-- - [A Node.js developer environment](root://client-libraries/0.1/getting-started/js-quickstart.md) -->
- [`@iota/bundle`](https://github.com/iotaledger/iota.js/tree/next/packages/bundle)，[`@iota/core`](https://github.com/iotaledger/iota.js/tree/next/packages/core)，[`@iota/converter`](https://github.com/iotaledger/iota.js/tree/next/packages/converter)，および[`@iota/transaction`](https://github.com/iotaledger/iota.js/tree/next/packages/transaction) パッケージ
<!-- - The [`@iota/bundle`](https://github.com/iotaledger/iota.js/tree/next/packages/bundle), [`@iota/core`](https://github.com/iotaledger/iota.js/tree/next/packages/core), [`@iota/converter`](https://github.com/iotaledger/iota.js/tree/next/packages/converter), and [`@iota/transaction`](https://github.com/iotaledger/iota.js/tree/next/packages/transaction) packages -->
- [`SignBundle_enabled` flag](../references/command-line-options.md#signBundle) を `true` に設定します．
<!-- - The [`SignBundle_enabled` flag](../references/command-line-options.md#signBundle) set to `true`. -->

## 手順1. 未署名のバンドルを作成する
<!-- ## Step 1. Create an unsigned bundle -->

ハブがバンドルに署名する前に，未署名のバンドルを作成する必要があります．
<!-- Before Hub can sign a bundle, you need to create an unsigned one. -->

:::info:
このガイドでは，JavaScript クライアントライブラリを使用してバンドルを作成および送信しますが，Go，Java，Python などの他の[公式およびコミュニティライブラリ](root://client-libraries/0.1/introduction/overview.md)もあります．
:::
<!-- :::info: -->
<!-- <!-- In this guide, we use the JavaScript client library to create and send the bundle, but we also have other [official and community libraries](root://client-libraries/0.1/introduction/overview.md), including Go, Java, and Python. --> -->
<!-- ::: -->

1. 署名されていないバンドルを作成してファイルに保存する関数を作成します．
  <!-- 1. Write a function that creates an unsigned bundle and saves it to a file -->

    ```js
    async function createUnsignedBundle({ outputAddress, inputAddress, securityLevel, value }) {
    let bundle = new Int8Array();
    const issuanceTimestamp = Converter.valueToTrits(Math.floor(Date.now() / 1000));

    bundle = Bundle.addEntry(bundle, {
      address: outputAddress,
      value: Converter.valueToTrits(value),
      issuanceTimestamp
    });

    // セキュリティレベルごとに，新しいゼロトークントランザクションを作成します．このトランザクションに後で残りの署名フラグメントを追加できます．
    for (let i = 0; i < securityLevel; i++) {
      bundle = Bundle.addEntry(bundle, {
        address: inputAddress,
        value: Converter.valueToTrits(i == 0 ? -value : 0),
        issuanceTimestamp
      });
    }

    const result = await Bundle.finalizeBundle(bundle);

    // バンドル配列をバイナリファイルに保存します．
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

2. ハブから次の値を取得し，それらを `parameters` オブジェクトに追加します．
  <!-- 2. Get the following values from Hub and add them to the `parameters` object: -->

    | **フィールド** | **説明** | **メモ** |
    | :------------- | :------- | :------- |
    | `outputAddress` | 使用済みアドレスから IOTA トークンを転送する新しい81トライトのアドレス（チェックサムなし） | このアドレスは，ハブアドレスである必要はありません．たとえば，IOTA トークンをハードウェアウォレットのアドレスに送信できます． |
    | `inputAddress` | リカバーする必要がある IOTA トークンを含む，81トライトの使用済みアドレス（チェックサムなし） | [`balanceSubscription()` メソッド](../references/grpc-api-reference.md#hub.rpc.BalanceSubscriptionRequest)を使用して，受信アドレスへのデポジットをチェックすることをお勧めします．[`getUserHistory()`メソッド](../references/grpc-api-reference.md#hub.rpc.GetUserHistoryRequest) を使用して，どの使用済みアドレスに正の残高があるかを確認することもできます． |
    | `securityLevel` | 使用済みアドレスのセキュリティレベル | デフォルトのセキュリティレベルは2です．[`keySecLevel` コマンドラインオプション](../references/command-line-options.md#keySecLevel)のセキュリティレベルを変更した場合は，必ず `securityLevel` を使用してください． |
    | `value` | `inputAddress` フィールドの使用済みアドレスの合計残高 | [thetangle.org](https://thetangle.org/) などのタングルエクスプローラーでアドレスの残高を確認できます． |

    <!-- |**Field**|**Description**|**Notes**| -->
    <!-- |:----|:----------|:-----------| -->
    <!-- |`outputAddress`|The new 81-tryte address (without a checksum) to which you want to transfer the IOTA tokens from the spent address|This address does not need to be a Hub address. For example, you may want to send the tokens to an address on a hardware wallet. | -->
    <!-- |`inputAddress`|The spent 81-tryte address (without a checksum) that contains the IOTA tokens that you need to recover|It's best practice to use the [`balanceSubscription()` method](../references/grpc-api-reference.md#hub.rpc.BalanceSubscriptionRequest) to check for incoming deposits into spent addresses. You can also use the [`getUserHistory()` method](../references/grpc-api-reference.md#hub.rpc.GetUserHistoryRequest) to check which spent addresses have a positive balance.| -->
    <!-- |`securityLevel`| The security level of the spent address|The default security level is 2. If you changed the security level in the [`keySecLevel` command-line option](../references/command-line-options.md#keySecLevel), make sure you use that one. | -->
    <!-- |`value`|The total balance of the spent address in the `inputAddress` field|You can check the balance of any address on a Tangle explorer such as [thetangle.org](https://thetangle.org/) | -->

    :::info:
    `createUnsignedBundle()` 関数が `signature fragment` を追加できる十分なゼロトークントランザクションを作成できるように，スウィープアドレスのセキュリティレベルが必要です．
    :::
    <!-- :::info: -->
    <!-- You need the security level of the spent address so that the `createUnsignedBundle()` function can create enough zero-value transactions to which you can add the signature fragments. -->
    <!-- ::: -->

    ```js
    let outputAddress = Converter.trytesToTrits('ADDRESS...');
    let inputAddress = Converter.trytesToTrits('ADDRESS...');

    const parameters = {
      outputAddress: outputAddress,
      inputAddress: inputAddress,
      securityLevel: 2,
      value: 1
    }
    ```

3. `createUnsignedBundle()` 関数を呼び出し，ハブから取得したパラメーターを渡します．
  <!-- 3. Call the `createUnsignedBundle()` function, and pass it the parameters you got from Hub -->

    ```js
    createUnsignedBundle(parameters);
    ```

    この関数は，現在のディレクトリにある `bundle` と呼ばれるバイナリファイルに未署名のバンドルを保存します．
    <!-- This function saves the unsigned bundle to a binary file called `bundle` in your current directory. -->

4. ファイルを実行し，未署名のバンドルハッシュをコンソールからクリップボードにコピーします．ハブは，署名を作成するためにこのバンドルハッシュを必要とします．
  <!-- 4. Execute the file, then copy the unsigned bundle hash from the console to the clipboard. Hub needs this bundle hash to create the signature. -->

## 手順2. ハブを使用して未署名のバンドルに署名する
<!-- ## Step 2. Use Hub to sign the unsigned bundle -->

ハブには `signBundle()` gRPC メソッドがあり，ユーザーのデポジットアドレスから IOTA トークンを取り出すバンドルに署名できます．
<!-- Hub has a `signBundle()` gRPC method, which allows you to sign bundles that withdraw from a user's deposit address. -->

1. ハブで，未署名のバンドルハッシュとスウィープしたアドレスを `signBundle()` メソッドに渡します．
  <!-- 1. In Hub, pass the unsigned bundle hash and the spent address to the `signBundle()` method -->

    ```bash
    Hub@localhost:50051> client.signBundle({address:'ADDRESS...',bundleHash:'BUNDLEHASH...',authentication:'',validateChecksum:false},pr)
    ```

2. 返された署名をコピーし，`trytesToTrits()` メソッドにペーストして，トライトに変換します．
  <!-- 2. Copy the returned signature and paste it into the `trytesToTrits()` method to convert it to trits -->

    ```js
    const signature = Converter.trytesToTrits('SIGNATURE...')
    ```

3. 前に保存した `bundle` ファイルから未署名のバンドルを読み取ります．
  <!-- 3. Read the unsigned bundle from the `bundle` file you saved earlier -->

    ```js
    bundle = new Int8Array(fs.readFileSync('bundle'));
    ```

4. 署名を未署名のバンドルに追加します．
  <!-- 4. Add the signature to the unsigned bundle -->

    ```js
    bundle.set(Bundle.addSignatureOrMessage(bundle, signature, 1));
    ```

    :::info:
    3番目の引数は，署名フラグメントの追加を開始するバンドル内の最初のトランザクションを指定します．この例では，最初のトランザクションは出力トランザクションであるため，署名は必要ありません．
    :::
    <!-- :::info: -->
    <!-- The third argument specifies the first transaction in the bundle to which to start adding the signature fragments. In our example, the first transaction is the output transaction, so it doesn't need a signature. -->
    <!-- ::: -->

## 手順3. 署名済みバンドルをノードに送信する
<!-- ## Step 3. Send the signed bundle to a node -->

バンドル内の入力トランザクションに署名フラグメントを追加すると，バンドルは署名され，ノードに送信する準備が整います．
<!-- After adding the signature fragments to the input transactions in your bundle, it's now signed and ready to be sent to a node. -->

1. 署名済みバンドル内のトランザクションをトライトに変換し，それらを新しい配列にプッシュします．
  <!-- 1. Convert the transactions in the signed bundle to trytes and push them into a new array -->

    ```js
    const trytes = []
    for (let offset = 0; offset < bundle.length; offset += Transaction.TRANSACTION_LENGTH) {
      trytes.push(Converter.tritsToTrytes(bundle.subarray(offset, offset + Transaction.TRANSACTION_LENGTH)));
    }
    ```

2. `provider` フィールドに URL を追加して，ノードに接続します．
  <!-- 2. Connect to a node by adding the URL of one to the `provider` field -->

    ```js
    const iota = Iota.composeAPI({
      provider: ''
    });
    ```

3. ノード構成オプションを設定します．
  <!-- 3. Set the node configuration options -->

    ```js
    const depth = 3;
    const minWeightMagnitude = 14;
    ```

4. バンドルのトランザクショントライトをノードに送信します．
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
    ここで，ノードはバンドルの先頭トランザクションから先に送信されることを期待しているため，トランザクショントライト配列を逆にします．
    :::
    <!-- :::info: -->
    <!-- Here, we reverse the transaction trytes array because nodes expect a bundle to be sent head first. -->
    <!-- ::: -->

:::success:おめでとうございます:tada:
使用済みアドレスから IOTA トークンを安全なアドレスにデポジットしてリカバーする署名付きバンドルを送信しました．
:::
<!-- :::success: -->
<!-- You've sent a signed bundle that recovers IOTA tokens from a spent address by depositing them into a safe one. -->
<!-- ::: -->

:::warning:
バンドルが確定されるまで，IOTA トークンは攻撃者によって取り出される危険性があります．

バンドルが確定される可能性を高めるには，[プロモートと再アタッチ](root://client-libraries/0.1/how-to-guides/js/confirm-pending-bundle.md)を行います．
:::
<!-- :::warning: -->
<!-- Until the bundle is confirmed, the tokens are still at risk of being withdrawn by an attacker. -->
<!--  -->
<!-- To increase the chances of your bundle being confirmed, you can [promote and reattach it](root://client-libraries/0.1/how-to-guides/js/confirm-pending-bundle.md). -->
<!-- ::: -->

## サンプルコード
<!-- ## Sample code -->

これは，このチュートリアルのテストに使用した使用済みアドレスの例のコードです．
<!-- This is the code for an example spent address that we used to test this tutorial. -->

タングルエクスプローラーで[このアドレスの履歴を表示](https://thetangle.org/address/LIQJBJRBSTGYWHYRPCLLCZUMP9SLHCBBWGQ9YRFWYDFF9FMXIAELYLTTBXCPVIDWWZYIOJIFLUFYVZIBD)できます．
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

  // セキュリティレベルごとに，残りの署名フラグメントを追加する新しいゼロトークントランザクションが必要です．
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

  // バンドルハッシュのトライトを取得します．
  const bundleHash = Converter.tritsToTrytes(Transaction.bundle(result));

  // ハブでバンドルハッシュを使用して署名を取得します．
  console.log(bundleHash);
}

// これらのパラメーターの値をハブから取得します．

let outputAddress = Converter.trytesToTrits('LYRGKMBCVZMTPRRMPDNNRXFVKIXTZCJTZDOAMTHZXQNESSLYGVQ99PGAW9OCAPRGSSSDMFCMZ9NAJTWUX');
let inputAddress = Converter.trytesToTrits('LIQJBJRBSTGYWHYRPCLLCZUMP9SLHCBBWGQ9YRFWYDFF9FMXIAELYLTTBXCPVIDWWZYIOJIFLUFYVZIBD');

const parameters = {
   // 使用済みアドレスから IOTA トークンを送信する先のトリットアドレス（チェックサムなし）
   outputAddress: outputAddress,
   // Swept address in trits (without checksum)
   inputAddress: inputAddress,
   // 使用済みアドレスのセキュリティレベル
   securityLevel: 2,
   // 使用済みアドレスから取り出される IOTA トークンの合計量
   value: 1
}

createUnsignedBundle(parameters);

// トライトでハブからの署名に置き換えます．
const signature = Converter.trytesToTrits("DRFFHLSWJBLEBNHXMSLIGUKBQJIJRNHGMGVDFKKPF9DYABMQXDVLWPHXT9LEPSXVBIYKZEWCT9U99NJ9DQJVJJMBGCUHTZQBCYWEIN9DCSDPJXRKGQQBEHPBZZWRFNBIENYV9UNSSNXZHGATAWRTHEW9FQVTXUJLFCQEPLVWATFRXFAUSNFKVVYFISDULSVIQNEQVPNIKZSBJQHWAZMGOIPZSKY9QDVMCFXUS9UGUDK9BOZFAWXQFHFBEYUJBESIFMBWNXVFCKHTGXTWKLSMZ9FHYFUZKE9GTLFWXPCLFPQKCJQAZFSTGWJILTACIXLRRJWDGIGJOCTUCEBFLWFQBQKAHTJMAEWZSDMOTQ9GPALHYYRGTFYJCDRFKZFUPECKUEVZJPVQDYIDJAIHSFSWZSQLQIJSCKJPJPLFVHXLUMLZKUHJWRVPBIOESDIFCUCKZAGANIATOUCSF9IOUA9EQWCSOFMETZEADESOADJGLCDKUQIBEPMXBWNIHJMJHCYUSM9LXMEBTHORZDGYST9VJLUUSXTSAURNUZAUPNWIUSRVL9KGFCEIAIWTNIH9BIRCSLWDAPXYKSZAOFERYFGPBWYUIEEENGMMYVPRRRIXULRJWJCLUBBNMNLFMNPYDHVBFZUFRCD9PCZAQLDBJFFKGQJW9LFUUPIZPKUCBAJFUEUTMXKWUKFXKQISDXYIHEJSQTCEHVABAUWGGANWZCNBOLGZDCLXSZIUKRMFG9THEZTULVZ9CFGNUWCTQVSACVYSZASONJIGUSXZAZNVCBIWRHLOKCXPXFONAWNMWRMGAXXJNPYRNDKAKIEWNSDRSNTSTMDPUYNPSIJTWERECFKSNTWOKCMDIUEKEYXORVIQVQXKWOWNMVBLWVJIWBWKAHHECGLNZAADFVSATZDMLYXBGRCEATLTCMQACYQQDQOXXMSUWSZVZKEXQUYM9DZMMOMBIVCPJU9UUKAXOSQQX9QRPTFXSZPBZFFJLGOBVZZOT9LNWMA9LTRUZUJBDGH9GILESQLNYDYPCHHYICVMAPDVDILEOZDNZMVSERSDWLZ9ZXFKJXISY9BLXZZ9DJYIRPXNLGAWMDZOXOWBUDZRUROHKPREZJTXNSUNC9LDHGFLGYOXBAQVGYUZXECCKUUFOPSLPH9QRLAZUOWGBQBVEJBAZKUEWSUIVGLEULZKDXFQSGQNZYXXZCYPRZUSBPMVPLWMYDWEGXANNXLOTME9HQGQW9POKCSQWHLHUUUWNFFHXZYDYUYBMSJVWSAKMEFJDJNJZHIYP9NBLFWVKBSDIUXLAVLMYLLCRX9BZZZQEESERVDIHKKPVAM9UEOXR9CFW9OAXLCLMJHWEOSTCYYHPWHJZEKAXPBGNGVMCGNHOUWMJX9GEV9BBGYVNEG9AXSNPFEGOZNZWXMNXVV9CVJ9Z9DGLYOUWZKFJYIIWRUEYPGDFSJGRXZDS9RKFELFVMASCAPXVCPLNQRJHADACOOGGHOVTTG9DCOCMHTMTMNEVASXHKLBAWKNFTFDAEEHATXGUKBRLDXNLSHZEPO9OZQBSHJRLGLNPKHAX9WRMYARVJETYAPO9SUWZQHAKJAUWBIWQDSQ9FWIEWSJWKQGNIFGGVKZLWZZOVVBGPNRTONMZDLJHGWQUSCILYOBG9YUUUZDXILKBIQ9ZEKXEHURJLFMDB9CUZI9TEJSQZYHMNTX9F9GJBZ9UJNCKZVYAROOVZYZURRWKLYHJEMZDQLH9FWODFZUEZRV9DWNZCOMXNXFWCPCZQDGFP9DKJYUNXIYIRUVWRSWWIGKZWITSYIVEOPHBJGSZNYJSFTXAKLENHCPGOYQSGEZQMRVNFGAGZXFG9GYFIBJRVU9MYDSZSTJQVVODQJJEHYJYCHOHTCOPZJVGMBEIIGYFMNAPZATAHQGFVWSGYODTOHRE9FVYXSLESASDMMAPBZZVPLIWGIRORAGWNFA9JPSUWJCTNBYRDJANSYMNDGQJNSFQRSVMRIAXDWQONWSAYUYFXJUTWYLPXVV9YGJBMQUXODSEFAEGYXSYYEQTVRAWNCKKSWPRYYINMBOBNZJSANFLKDFMGTNUDU9SFQRBUJM9XDNWIECTJPEICLVXTMCMGQJT9BTRZ9HQCHHBYXSPJBGTSTJDIOUQ9SLRVHTENSCLLGBFMXOYFKYB9SAKWTPGHEXPU9ERFYGEMEPSLJUPRJNRRWQLA9XVSWBEGJHLLDXGDTESDPRBRQHLCIOPWBDYJVCLOHHDYFNQSXBOVUVICSVBMEZYJJZHGDKAHFRJHBDWUSFSGC9UCLVH9ZNVMPYMRMMJTSWAFDRGNALYLRRVPLGMMKGZR9LXEV9ZXRPFKYDBLIUEVEJKVDANOBKFLRSWQPMOMTEJA9WVHFGBNTAJIPWNUWKJMQAOKBCACLYGRCBZRVFKIOMCXYXGUTNODHMLOZRETEPDONTAPVQEQAWCAH9CSTPYRFUXPKBDVONWFNOZHA9N9YUTEIGAHAPEKNWGCXNAMVJVLPCAPDPNTVVSKROYWXDKVLOAHPCCJBGBPSMQEDFDXSGNMIMLEZPBVCKYKVZFFIZVECYQKQWDHXRVEAOL9QIDZK9JFBRYL9JOSAWMBZAXJWNBTLKRSIYHZEILOPRZVLOFACJGIPABPFZILVQMUAYMPABXEBDYOSYBZKUXCFVXUEQJJOUDMBZZGOBVBF9N9ENHSCTDVFASKXHOLLFYMMWSDFHJKHODXYPWMEOQZKIECIBHYXF9PXZOCBMIBI9PLVDVBGALAWBT9FLIZWRNENVLSUCSOATNVRUKTTWZNTZRCMTLALKYGOUKVSO9BBUDVUCFCIKQZWJIUBMXTSNQSFRETWAHPQ9MTYOUKX9BLA9QUUHPNLNINNIPWQVUPUCXHTMURNWYSUGOKWPYMZ9ILDJWEOD9D99YWKQTKKOOWQOLEZMTZ99AYQPWUVVZUNVORK9XRAE9GQSHLVIZLCEDWHSWYNVZKELOI9AVGUWFSJYHA9WZNVNZYFEZYJAXKUOPINHR9OEN9NVLNIDEJLPLVEMIANSGFXXXL9IKQAFLFPOPWO9SAEMNETZMUM9NEHAA9JOGLHOBHYHXASFWHGNKCRNWZSCDPIFGFHOFLWCOMNCPRBECDEVSNXGKNF9IOYIVZRBJJHCREKOFHMTXXALIWOGOZRIJITCWGKQKCVUPUOFNMEUGSVPYAKRORXQHUXJVCAEWJBLAPDDSCIYMODOMM9GWQSVQUBGBUUDVGSSHAONRHVMILHPMBHKKHUTRZFWNAEEUHCQUGHLSVXAMOGVTXELPGKXBHELPCGYRCLMYGIZYLANN9LSXCQVQPBLIZXJUVRTA9CQAFSWWTIBILERJDTYHJWDPOYHNCHJEWSVXEJCPZVNVLWWOAZHJLLTS9WAEXXSMWHITJYJLFHGGDFNYIGDKJFUZGSJDCXSVNZQYUSAPVJRHSRNLNQDLDBEFPLRXJ9MIZBPNBZVHHLOYC9VYLDKNJAHLNPRCSOZKHWHQKXYWXABCPHYNQYRZJEARZCOLQMBCJCHNNSFGORUOHFOFJMF9MJHHFFVBCS9QITDPGXTJKDDJKHGBOLEVMXRAWUMCXUABILEYVHRTZFFMAM9REMUPNEPUHKWFHOJEFBLXXYHI99UDFMXLXEFWQLPTKJJQEQAXYCQNCZMMOTSKGQCJNFCWGUISHWZLDWJBNNAFDDOMOJ9QRVDEXXNHLKVP9CQJSGZRT9CHDTUJIXWM9WB9RTYQ9EEIJWEVSGKQPFLKG9HKNWSGWZUAGOBENKKT9NUMUIAPWZ9UESGRDITJSOMJAPHIN9CJSSUJCAKCLTQPJOUEMKVLSZZ9MAEROPNSBHPHOFBVVOSNTWPSCRTAYQJQSG9YXPRMAHREBCQKMRFISYDXXNRMGEHCXFNH9SAGFOYAHFCQRNQFRXDIJAIBIRPBNVQZLZNJHBVCZHOIHJURHCFFVHFCPBSKGLQOCXKWXSFINGLYQAH9YGPFWJGQDOUIMFJJOFGNDREMEIMDKOEQONGHTVYWVXWHQQOJGCM9YEHBCVWNJLJXK9HQB9BDYTUJVTHYU9R9DPMUHTLB9NGFEEVKIUANTFHUQRRK9LSSFMUBZLKJTJLPQLBEUSVAFJURBSGFNMBZCDKEUTCNYHBZPUCZGQDAP9JISCORGZVTMLCFRDHKJBCEOYOHEHUDTHMGHMFDTPKGNQPCIBTSOCISWKFSMPDVUKPAWVACALATUSWJTJPXHJWUFDEICUNUDSHGYEGRPMDRABLMFCHXNUA9LHNUMDKXH99XGJKU9XMUXFOXKWLI9AFNDD9CSETSFB9MLEMEY9UMHQQYROBZZEBGHRQRERMHILEVEGNOBSFUIIIJZHRFPOBXHGRSXPYEC");

bundle = new Int8Array(fs.readFileSync('bundle'));

// トランザクション0は出力トランザクションなので，バンドル内の次のトランザクションから署名フラグメントの追加を開始します．
bundle.set(Bundle.addSignatureOrMessage(bundle, signature, 1));

const trytes = []
for (let offset = 0; offset < bundle.length; offset += Transaction.TRANSACTION_LENGTH) {
  trytes.push(Converter.tritsToTrytes(bundle.subarray(offset, offset + Transaction.TRANSACTION_LENGTH)));
}

const iota = Iota.composeAPI({
  // トランザクションを送信する IRI ノードの URL に置き換えます．
  provider: 'https://pow.iota.community:443'
});

const depth = 3;
const minWeightMagnitude = 14;

// バンドルは，ノードに送信する前に先頭トランザクションから末尾トランザクションまで順番に並んでいる必要があります．
iota.sendTrytes(trytes.reverse(), depth, minWeightMagnitude)
  .then(bundle => {
    console.log(`Sent bundle: ${JSON.stringify(bundle, null, 1)}`)
  })
  .catch(error => {
    console.log(error);
  });
```
