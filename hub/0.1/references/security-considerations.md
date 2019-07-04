# セキュリティ上の考慮事項
<!-- # Security considerations -->

**ハブの役割がIOTAトークンの管理であることを考えると、そのセキュリティは非常に重要です。ハブを保護して正しく使用するための手順を踏む必要があります。**
<!-- **Given that the role of Hub is to manage IOTA tokens, its security is crucial. You must take steps to secure Hub and use it correctly.** -->

## 預け入れアドレス
<!-- ## Deposit addresses -->

[セキュリティ上の理由](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse)から、預け入れアドレスから一度だけIOTAトークンを取り出すことができます。
<!-- For [security purposes](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse), deposit addresses may only be withdrawn from once. -->

ハブは、IOTAトークンを定期的にユーザーの預け入れアドレスからハブ所有者のアドレスに転送することで、このリスクを軽減します。このプロセスは[スウィープ](../concepts/sweeps.md)内で行われます。
<!-- Hub reduces this risk by transferring IOTA tokens from users' deposit addresses to a Hub owner's address at regular intervals. This process happens in a [sweep](../concepts/sweeps.md). -->

:::danger:重要
使用済みアドレスを保護するためにスイープを頼るべきではありません。ハブがスウィープする前に、攻撃者は資金を盗むことができます。

したがって、使用済みアドレスに2度とトークンを預け入れないようにユーザーに通知することが重要です。
:::
<!-- :::danger:Important -->
<!-- You shouldn't rely on sweeps to protect spent addresses. An attacker could steal the funds before Hub can do a sweep. -->
<!--  -->
<!-- So, it's important that you inform users never to deposit tokens into a spent address. -->
<!-- ::: -->

## ソルト
<!-- ## Salt -->

セキュリティをさらに強化するには、[`salt`](../references/command-line-flags.md)フラグを設定してシード暗号化アルゴリズムの強度を高めます。ソルトは攻撃者がデータベース内のシードUUIDを事前に計算された辞書攻撃に対してチェックする能力を削除します。
<!-- To add an extra layer of security, improve the strength of your seed encryption algortithm by setting the [`salt`](../references/command-line-flags.md) flag. A salt removes the ability for an attacker to check the seed UUIDs in the database against a pre-computed dictionary attack. -->

## 署名サーバー
<!-- ## Signing server -->

盗難防止のために、ハブには2つのサーバーがあります。
<!-- To help prevent theft, Hub offers two servers: -->

* **ハブ：** アカウントとアドレスを管理します。
<!-- * **Hub:** Manages accounts and addresses -->
* **署名サーバー：** ソルトを保管し、スウィープ用のバンドルに署名します。
<!-- * **Signing server:** Stores the salt and signs bundles for sweeps. -->

最大限のセキュリティを確保するために、ハブが危険にさらされた場合に攻撃者が署名サーバーにアクセスせずにIOTAトークンを盗むことができないように、署名サーバーをハブから遠く離れた場所で実行します。
<!-- For maximum security, run the signing server in a remote location so that, if Hub is compromised, attackers can't steal IOTA tokens without access to the signing server. -->

## 履歴ログ
<!-- ## History logs -->

ハブはそのすべてのアクションのログを生成します。このログには、トランザクションレコード、サーバーの再起動、およびその他の詳細情報も含まれています。
<!-- Hub generates a log of all its actions. This log also contains transaction records, server restarts, and other detailed information. -->
