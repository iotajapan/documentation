# バンドル内のメッセージを変更する
<!-- # Change the messages in a bundle -->

**このガイドでは，バンドルのコピーを作成し，テールトランザクション内のメッセージを変更する方法を学習します．**
<!-- **In this guide, you learn how to create a copy of a bundle and change the message in the tail transaction.** -->

トランザクション内のメッセージは， `signatureMessageFragment` フィールドに保存されますが，このフィールドは[バンドルエッセンス](root://getting-started/0.1/transactions/bundles.md#bundle-essence)には含まれません．したがって，バンドルハッシュを変更せずにこのフィールドの値を変更できます．
<!-- Messages in transactions are stored in their `signatureMessageFragment` field, which isn't included in the [bundle essence](root://getting-started/0.1/transactions/bundles.md#bundle-essence). Therefore, you can change the value of this field without changing the bundle hash. -->

## パッケージ
<!-- ## Packages -->

このガイドを完了するには，次のパッケージをインストールする必要があります．
<!-- To complete this guide, you need to install the following packages: -->

--------------------
### npm
```bash
npm install @iota/core @iota/converter @iota/bundle @iota/transaction @iota/transaction-converter
```
---
### Yarn
```bash
yarn add @iota/core @iota/converter @iota/bundle @iota/transaction @iota/transaction-converter
```
--------------------

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは，[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します．
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## コードウォークスルー
<!-- ## Code walkthrough -->

1. パッケージをリクワイアします．
  <!-- 1. Require the packages -->

    ```js
    const Iota = require('@iota/core');
    const Converter = require('@iota/converter');
    const Bundle = require('@iota/bundle');
    const { TRANSACTION_LENGTH, SIGNATURE_OR_MESSAGE_LENGTH } = require('@iota/transaction');
    const { asTransactionTrytes } = require('@iota/transaction-converter');
    ```

2. ノードに接続します．
  <!-- 2. Connect to a node -->

    ```js
    const iota = Iota.composeAPI({
    provider: 'https://nodes.devnet.iota.org:443'
    });
    ```

3. メッセージを変更するテールトランザクションハッシュの値を定義します．
  <!-- 3. Define the hash of the tail transaction whose message you want to change -->

    ```js
    const tail = 'UZSQCOKPEDTIZWLFNJWTPDNYZCYYHAMJAJVVHOHAHSQLPYOYYN9PT9DN9OOCESNS9RPYFIESTOCGCL999'
    ```

    :::info:
    入力である可能性が高いため，テールトランザクションを使用します．これは，署名が含まれないことを意味します．

    署名を変更すると，バンドルは無効になります．ただし，メッセージを変更した場合，新しいトランザクションハッシュを計算するためのプルーフオブワークを行った後でも，バンドルは有効です．
    :::
    <!-- :::info: -->
    <!-- We use a tail transaction because it is more likely to be an input, which means that it won't contain a signature. -->

    <!-- If we were to change a signature, the bundle would be invalid. But, if we change a message, the bundle will still be valid after we do proof of work to calculate the new transaction hash. -->
    <!-- ::: -->

4. [`getBundle()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getBundle) メソッドを使用して，テールトランザクションのバンドル内のすべてのトランザクションを取得します．
  <!-- 4. Use the [`getBundle()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getBundle) method to get all transactions in the tail transaction's bundle -->

    ```js
    iota.getBundle(tail).then(bundle => {

    });
    ```

5. 取得したバンドルのコピーを作成します．
  <!-- 5. Create a copy of the returned bundle -->

    ```js
    // バンドル内のトランザクション数に等しい長さの新しいバンドル配列を作成します
    let newBundle = new Int8Array(bundle.length * TRANSACTION_LENGTH);
    for (let i = 0; i < bundle.length; i++) {
        // 取得したバンドルからのトランザクショントライトで新しいバンドルを埋めます
        newBundle.set(Converter.trytesToTrits(asTransactionTrytes(bundle[i])), i * TRANSACTION_LENGTH);
    }
  ```

6. [`addSignatureOrMessage()`](https://github.com/iotaledger/iota.js/tree/next/packages/bundle#bundleaddsignatureormessagebundle-signatureormessage-index) メソッドを使用して，コピーのテールトランザクションに新しいメッセージを追加します．
  <!-- 6. Use the [`addSignatureOrMessage()`](https://github.com/iotaledger/iota.js/tree/next/packages/bundle#bundleaddsignatureormessagebundle-signatureormessage-index) method to add a new message to the tail transaction of your copy -->

    ```js
    // 新しいメッセージを保持する配列を定義する
    const message = new Int8Array(SIGNATURE_OR_MESSAGE_LENGTH)
    // 新しいメッセージをセットする
    message.set(Converter.trytesToTrits("HIJACKED"))
    // バンドル内のテールトランザクションにメッセージを追加する
    newBundle = Bundle.addSignatureOrMessage(newBundle,message,0);
    ```

7. [`finalizeBundle()`](https://github.com/iotaledger/iota.js/tree/next/packages/bundle#bundlefinalizebundlebundle) メソッドを使用して，コピーのバンドルハッシュを計算します．
  <!-- 7. Use the [`finalizeBundle()`](https://github.com/iotaledger/iota.js/tree/next/packages/bundle#bundlefinalizebundlebundle) method to calculate the bundle hash of your copy -->

    ```js
    newBundle = Bundle.finalizeBundle(newBundle);
    ```

    :::info:
    このバンドルハッシュは，元のバンドルと同じです．
    :::
    <!-- :::info: -->
    <!-- This bundle hash is the same as the original bundle. -->
    <!-- ::: -->

8. コピー内のトランザクションをトリットからトライトへ変換します．
  <!-- 8. Convert the transactions in your copy from trits to trytes -->

    ```js
    const newTrytes = [];
    for (let i = 0; i < bundle.length; i++) {
        // トランザクショントリットをトライトへ変換し，新しい配列に追加する
        newTrytes.push(Converter.tritsToTrytes(newBundle.slice(i * TRANSACTION_LENGTH, (i + 1) * TRANSACTION_LENGTH)))
    }
    ```

9. コピーをノードに送信します．
  <!-- 9. Send your copy to a node -->

    ```js
    iota.sendTrytes(newTrytes.reverse(),3,9).then(transactions => {
    // コンソールに新しいテールトランザクションハッシュを表示する
    console.log(transactions[0].hash);
    })
    ```

    :::info:
    ライブラリはバンドルが先頭に送信されることを想定しているため，バンドル配列を逆にします．
    :::
    <!-- :::info: -->
    <!-- You reverse the bundle array because the library expects bundles to be sent head first. -->
    <!-- ::: -->

これで，タングル上の[新しいテールトランザクションを検索](https://devnet.thetangle.org/)すると，バンドル内に元のバンドルハッシュと同じバンドルハッシュを持っていることがわかります．
<!-- Now, you can [search for your new tail transaction](https://devnet.thetangle.org/) on the Tangle and see that it's in a bundle with the same bundle hash as the original. -->

:::success:おめでとうございます:tada:
バンドル内のテールトランザクションのメッセージを変更し，そのバンドルのコピーをタングルに再アタッチしました．
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just changed the message of a tail transaction in a bundle and reattached a copy of that bundle to the Tangle. -->
<!-- ::: -->

:::warning:
元のテールトランザクションが転送バンドルに属している場合，ノードはコピーまたは元のバンドルを二重支出としてマークします．したがって，そのうちの1つだけが確定されます．
:::
<!-- :::warning: -->
<!-- If your original tail transaction belongs to a transfer bundle, the nodes will mark either your copy or the original bundle as a double spend. Therefore, only one of them will be confirmed. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

[REPL.it ツール](https://repl.it)を使用して，ブラウザーでサンプルコードを実行できます．
<!-- We use the [REPL.it tool](https://repl.it) to allow you to run sample code in the browser. -->

緑色のボタンをクリックして，このガイドのサンプルコードを実行し，ウィンドウで結果を確認できます．
<!-- Click the green button to run the sample code in this guide and see the results in the window. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Change-message-in-a-bundle?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## 次のステップ
<!-- ## Next steps -->

[再アタッチされたバンドルが確定される可能性を高める](../js/confirm-pending-bundle.md)．
<!-- [Increase the chances of your reattached bundle being confirmed](../js/confirm-pending-bundle.md). -->
