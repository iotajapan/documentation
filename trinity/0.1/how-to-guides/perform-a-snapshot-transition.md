# スナップショット移行を実行する
<!-- # Perform a snapshot transition -->

**グローバルスナップショット中に、ノードは自身の台帳から古いトランザクションデータを削除してメモリを解放します。グローバルスナップショットの後、ノードは少なくとも1iの残高を持つアドレスのみを持ちます。グローバルスナップショットの後に正しい残高が表示されない場合は、トリニティが最新の残高をリクエストできるように、スナップショット移行を実行する必要があります。**
<!-- **During a global snapshot, nodes remove old transaction data from their ledgers to free memory. After a global snapshot, nodes have only the addresses with a balance of at least 1i. If you don't see your correct balance after a global snapshot, you must perform a snapshot transition to allow Trinity to request the latest balance of your addresses.** -->

:::info:
トリニティはステートフルです。つまり、トランザクション履歴のローカルコピーをデバイスに保存するということです。その結果、グローバルスナップショット後もトランザクション履歴を見ることができます。
:::
<!-- :::info: -->
<!-- Trinity is stateful, which means that it stores a local copy of your transaction history on your device. As a result, you can still see your transaction history after a global snapshot. -->
<!-- ::: -->

1. **アカウント設定** > **ツール** > **スナップショットの移行**に移動します。
  <!-- 1. Go to Account management > **Tools** > **Transition**. -->

    モバイル端末のアカウント設定にアクセスするには、**設定** > **アカウント管理**に移動します。デスクトップでアカウント設定にアクセスするには、バーガーメニュー > **アカウント** > **アカウント設定**に移動します。
    <!-- To access Account management on a mobile device, go to **Settings** > **Account management**. To access Account management on a desktop, go to the burger menu >  **Account** > **Account management**. -->

![photo of snapshot transition](../images/transition.jpg)
