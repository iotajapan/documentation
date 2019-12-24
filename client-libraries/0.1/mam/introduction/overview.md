# マスクされた認証済みメッセージング
<!-- # Masked Authenticated Messaging -->

**マスクされた認証済みメッセージング（Masked Authenticated Messaging, MAM）は、タングルのトランザクションでチャネルと呼ばれる暗号化されたデータストリームを公開できるデータ通信プロトコルです。**
<!-- **Masked Authenticated Messaging (MAM) is a data communication protocol that allows you to publish encrypted data streams, called channels, in transactions on the Tangle.** -->

## MAMの仕組み
<!-- ## How MAM works -->

タングルにゼロトークントランザクションを添付できますが、ゼロトークントランザクションはノードによって署名またはチェックされず、真正性を確認できません。
<!-- The Tangle allows you to attach zero-value transactions to it, but these transactions are not signed or checked by nodes to verify their authenticity. -->

MAMでは、すべてのメッセージはシードの所有者によって署名されます。これにより、チャネルにアクセスできる人は署名を検証し、メッセージを復号化できます。
<!-- With MAM, all messages are signed by the owner of a seed. This way, those with access to a channel can validate the signature and decrypt the messages. -->

[データマーケットプレイス](root://blueprints/0.1/data-marketplace/overview.md)などの多くのユースケースでは、MAMを使用して、ユーザーがチャネルへのアクセス料金を支払い、メッセージを復号化できるようにします。
<!-- Many use cases such as the [Data Marketplace](root://blueprints/0.1/data-marketplace/overview.md) use MAM to allow users to pay for access to channels and decrypt the messages. -->

[このブログ投稿](https://medium.com/coinmonks/iota-mam-eloquently-explained-d7505863b413)でMAMの詳細をご覧ください。
<!-- You can [learn more about the details of MAM](https://medium.com/coinmonks/iota-mam-eloquently-explained-d7505863b413) in this blog post. -->

## チャネル
<!-- ## Channels -->

すべてのMAMメッセージはMAMチャネルに発行されます。MAMチャネルは、シードによって認証される暗号化されたデータストリームです。シードの所有者のみがメッセージをチャネルに公開できます。その結果、MAMチャネルは、特定の人またはデバイスによってメッセージが発行されたことを認証する便利な方法です。
<!-- All MAM messages are published to MAM channels, which are encrypted data streams that are authenticated by a seed. Only the seed owner can publish messages to a channel. As a result, MAM channels are a useful way of authenticating that messages were published by a certain person or device. -->

MAMメッセージは、ゼロトークントランザクションの`signatureMessageFragment`フィールドに送信されます。
<!-- MAM messages are sent in the `signatureMessageFragment` field of zero-value transactions. -->

メッセージをチャネルに公開すると、チャネルIDを受け取ります。チャネルIDは、他の人がそのチャネルを購読してメッセージを取得できるようにする識別子です。**チャネルIDは、MAMメッセージを含むトランザクションのアドレスです**。
<!-- When you publish messages to any channel, you receive a channel ID, which is the identifier that allows others to subscribe to it and fetch your messages. **A Channel ID is the address of the transaction that contains the MAM message**. -->

### チャネルの生成方法
<!-- ### How channels are generated -->

すべてのMAMチャネルはマークル木から生成され、すべてのメッセージはリーフの中の1つの秘密鍵で署名されます。
<!-- All MAM channels are generated from a Merkle tree, where all messages are signed with one of the private keys in the leaves. -->

単一のツリーは短期間しか存続しないため、各メッセージには次のマークル木のルートが含まれるため、購読者はメッセージの取得を継続できます。
<!-- Because a single tree lasts for only a short period of time, each message contains the root of the next Merkle tree to allow the subscriber to continue fetching messages. -->

購読者がMAMストリームからメッセージを取得すると、署名を検証し、署名がマークル木のリーフの1つに属していることを確認することにより、メッセージが最初に認証されます。署名の検証が失敗した場合、メッセージ全体が無効と見なされます。
<!-- When a subscriber fetches messages from a MAM stream, the message is first authenticated by validating the signature and verifying that the signature belongs to one of the Merkle tree's leaves. If the signature verification fails, the entire message is considered invalid. -->

### チャネルタイプ
<!-- ### Channel types -->

次のチャネルモードのいずれかにメッセージを公開できます。
<!-- You can publish messages to any of the following channel modes: -->

- パブリック
<!-- - Public -->
- プライベート
<!-- - Private -->
- 制限付き
<!-- - Restricted -->

#### パブリックチャネル
<!-- #### Public channels -->

パブリックチャネルは、MAMメッセージ（チャネルID）を含むトランザクションのアドレスとしてルートを使用します。その結果、誰でもアドレスを使用してパブリックチャネルのメッセージを復号化できます。
<!-- Public channels use the root as the address of the transaction that contains the MAM message (channel ID). As a result, anyone can decrypt messages in a public channel by using the address. -->

このモードは、公示や組み込みの認証機能を備えたイミュータブルなソーシャルメディアアプリケーションにも使用できます。
<!-- This mode could be used for public announcements or even a immutable social media application with built-in authentication. -->

#### プライベートチャネル
<!-- #### Private channels -->

プライベートチャネルは、MAMメッセージ（チャネルID）を含むトランザクションのアドレスとしてルートのハッシュを使用します。その結果、元のルートを持つユーザーのみがプライベートチャネルのメッセージを復号化できます。
<!-- Private channels use the hash of the root as the address of the transaction that contains the MAM message (channel ID). As a result, only those with the original root can decrypt the messages in a private channel. -->

このモードは、互いにプライベートに通信したいデバイスに役立ちます。
<!-- This mode is useful for devices that want to communicate in private among each other. -->

#### 制限付きチャネル
<!-- #### Restricted channels -->

制限付きチャネルは、サイドキーと呼ばれる認証キーをプライベートモードに追加します。MAMメッセージ（チャネルID）を含むトランザクションのアドレスは、サイドキーとルートのハッシュ値です。その結果、元のルートとサイドキーを持つユーザーのみがプライベートチャネル内のメッセージを復号化できます。
<!-- Restricted channels adds an authorization key, called the side key, to private mode. The address of the transaction that contains the MAM message (channel ID) is the hash of the side key and the root. As a result, only those with the original root and the side key can decrypt messages in a private channel. -->

このモードは、サイドキーを変更してチャンネルへのアクセスを取り消すことができるのに便利です。サイドキーを変更するときは、チャンネルの残りをフォローすることが許可されている関係者に変更するサイドキーを配布するだけです。
<!-- This mode is useful for being able to revoke access to the channel by changing the side key. When you change a side key, you just need to distribute it to the parties that are allowed to follow the rest of the channel. -->
