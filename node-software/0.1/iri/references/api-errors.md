# APIエラーメッセージ
<!-- # API error messages -->

**APIリクエストが失敗すると、レスポンスボディにHTTPエラーコードとJSONオブジェクトが返されます。**
<!-- **If an API request fails you'll receive an HTTP error code and a JSON object in the response body.** -->

## 400: Invalid addresses input

[タングルエクスプローラ](https://thetangle.org/search)でアドレスを検索して、アドレスが有効であることを確認してください。
<!-- Make sure that addresses are valid by searching for them on a [Tangle explorer](https://thetangle.org/search). -->

アドレスは81トライトである必要があります。アドレスが90トライトである場合、最後の9トライトはチェックサムです。9トライトのチェックサムを削除してください。
<!-- Addresses must contain only 81 trytes. If your address contains 90 trytes, the last 9 trytes are the checksum, remove them. -->

## 400: Invalid depth input

`depth`パラメータの値が数値（文字列ではない）であることを確認してください。
<!-- Make sure that the value of the `depth` parameter is a number (not a string). -->

`depth`パラメータの値を減らします。IRIノードは最大許容値を制限することがあります。
<!-- Decrement the value of the `depth` parameter. The IRI node may limit the maximum accepted value. -->

自分のIRIノードにリクエストを出している場合は、[`MAX-DEPTH`](../references/iri-configuration-options.md#maxdepth)設定オプションを確認してください。
<!-- If you're making the request to your own IRI node, check the [`MAX-DEPTH`](../references/iri-configuration-options.md#maxdepth) configuration option. -->

## 400: Invalid parameters

パラメータのスペルを確認してください。
<!-- Check the spelling of the parameters. -->

パラメーターが正しい順序であることを確認してください。
<!-- Make sure that the parameters are in the correct order. -->

## 400: One of the tips is absent

IRIノードの台帳にチップトランザクションがありません。
<!-- The IRI node doesn't have the tip transaction in its ledger. -->

別のチップトランザクションをパラメータとして使用するか、または別のIRIノードにリクエストを出してみてください。
<!-- Use a different tip transaction as a parameter, or try making the request to a different IRI node. -->

## 401: COMMAND is not available on this node

別のIRIノードにリクエストを出してください。現在のIRIノードは、このエンドポイントへのリクエストを制限しています。
<!-- Make the request to a different IRI node. The current IRI node has restricted requests to this endpoint. -->

自分のIRIノードにリクエストを出している場合は、そのコマンドが[`REMOTE-LIMIT-API`](../references/iri-configuration-options.md#remote-limit-api)設定オプションにリストされているかどうかを確認してください。
<!-- If you're making the request to your own IRI node, make sure that the command is not listed in the [`REMOTE-LIMIT-API`](../references/iri-configuration-options.md#remote-limit-api) configuration option. -->
