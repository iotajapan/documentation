# ドキュメントの不変性概要
<!-- # Document immutability overview -->

**ドキュメントは，当事者間で情報や契約を転送するための重要な手段です．ドキュメントが確立された状態から変更されていないことを確実に証明できることは，当事者を保護するだけでなく，当事者間の信頼を確保するのに役立ちます．この設計図は，IOTA タングルを使用して以前に署名された文書の署名を自動的にチェックするソリューションを説明しています．**
<!-- **Documents are an important means of transporting information and contracts between parties. Being able to reliably prove that a document has not been changed from an established state helps ensure trust between parties. As well as protect the parties. This blueprint describes a solution that automatically checks the signature of a previously signed document using the IOTA Tangle.** -->

## ビジネスケース
<!-- ## Business case -->

インターネット上のファイルは，ダウンロード中に傍受および変更される可能性があります．ダウンロードしたファイルが完全で正しいことを確認できるように，Web サイトではファイルのハッシュが表示され，ダウンロードしたファイルのハッシュと比較できます．
<!-- Files on the Internet can be intercepted and changed while you're downloading them. To help you to make sure the file you downloaded to complete and correct, websites display a hash of the file for you to compare against the hash of the one you downloaded. -->

たとえば，2016年に，ハッカーは Mint と呼ばれる Linux ディストリビューション用の ISO ファイルに[バックドアをうまく挿入しました](https://blog.linuxmint.com/?p=2994)．また，これらの改ざんされたファイルにリンクするように Web サイトを変更しました．さらに，ハッカーは新しく生成したファイルのハッシュ値を Web サイトに公開することにも成功しました．
<!-- For example, in 2016, hackers [successfully put a backdoor](https://blog.linuxmint.com/?p=2994) into an ISO file for a Linux distribution called Mint, they also changed the website to link to these tampered files. Additionally, they managed to publish newly generated hashes of the files on the website. -->

### 課題
<!-- ### Challenges -->

ほとんどの人は，ファイルをハッシュする方法を知らないか，それを行う時間がありません．
<!-- Most people either don't know how to hash a file or don't have time to do it. -->

データベース所有者や Google ドライブなどのサードパーティサービスでは，ユーザーがドキュメントが人間やウイルスによって変更されていないことを簡単に確認できるようにしたい場合があります．
<!-- Database owners or third-party services such as Google Drive may want to allow users to easily check that their document hasn’t been changed by human or a virus. -->

### 解決策
<!-- ### Solution -->

この設計図では，IOTA テクノロジーを使用して，信頼できるソース（タングル）からダウンロードしたファイルを自動的にチェックし，ファイルが異なる場合はユーザーに警告します．
<!-- In this blueprint, we use IOTA technology to automatically check downloaded files from a trusted source (the Tangle) and alert the user if the files are different. -->

## デモ
<!-- ## Demo -->

存在証明アプリケーションのデモンストレーションについては，[この Web サイト](https://iota-poex.dag.sh/)を参照してください．
<!-- See this website for a [demonstration of a proof of existence application](https://iota-poex.dag.sh/). -->

残念ながら，このデモの自分自身のローカルバージョンをデプロイすることはできません．ただし，この設計図を使用して，IOTA 財団がビルドした[存在の証明 JavaScript ライブラリ](root://utils/0.1/official/proof-of-existence/overview.md)をテストできます．
<!-- Unfortuntely, you can't deploy your own local version of this demo. However, you can test the [proof of existence JavaScript library](root://utils/0.1/official/proof-of-existence/overview.md) that we built, using this blueprint. -->

## その他のリソース
<!-- ## Additional resources -->

---------------
#### GitHub リポジトリ ####
[Link](https://github.com/iotaledger/poc-document-immutable-blueprint)

この設計図をテストするには，コードとクイックスタートの手順を読んでください．
<!-- Read the code and some quickstart instructions to test this blueprint. -->
---
#### 存在証明の npm ライブラリ ####
[Link](https://www.npmjs.com/package/@iota/poex-tool)

存在証明のシナリオに使用できるライブラリ．存在証明アプリケーションに使用されます．
<!-- A library that can be used for proof of existence scenarios. Used for the proof of existence application. -->
---
---------------
