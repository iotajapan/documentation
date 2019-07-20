# アカウントのイベントをリッスンする
<!-- # Listen to events in an account -->

**アカウントオブジェクトは、イベントが発生したときにイベントを発行します。イベントの例としては、支払いをするときや受け取るときがあります。あなたはこれらのイベントをリッスンして行動することができます。**
<!-- **An account object emits events when they happen. An example of an event is when you make or receive a payment. You can listen for these events and act on them.** -->

## 前提条件
<!-- ## Prerequisites -->

[アカウントを作成します](../how-to-guides/create-account.md)。
<!-- [Create an account](../how-to-guides/create-account.md). -->

## 入金と出金についてアカウントを監視する
<!-- ## Monitor your account for incoming and outgoing payments -->

アカウントの接続ノードが残高に影響するバンドルを受け取ると、アカウントは2種類のイベントをトリガできます。1つはバンドルが**ペンディング**状態にあるとき、もう1つは**含まれた**（確定済み）状態にあるときです。
<!-- When your account's connected nodes receive a bundle that affects your balance, your account can trigger two types of event: One when the bundle is in a **pending** state, and one when it's in an **included** (confirmed) state. -->

アカウントへのすべての入金は預け入れと呼ばれ、出金は取り出しと呼ばれます。
<!-- Any incoming payments to your account are called deposits, and outgoing payments are called withdrawals. -->

1. アカウントイベントをリッスンするクラスを作成します。
  <!-- 1. Create a class that listens to account events -->

    ```java
    private class AccountListener implements EventListener {
        private IotaAccount account;

        public AccountListener(IotaAccount account) {
            this.account = account;
        }

         @AccountEvent
        public void sent(EventSentTransfer e) {
            System.out.println("Account: " + account.getId());
            System.out.println("Outgoing payment is pending: " + e.getBundle().getBundleHash());
        }

         @AccountEvent
        public void promoted(EventPromotion e) {
            System.out.println("Account: " + account.getId());
            System.out.println("Promoting a pending bundle: " + e.getPromotedBundle());
        }

        @AccountEvent
        public void reattach(EventReattachment e) {
            System.out.println("Account: " + account.getId());
            System.out.println("Reattaching a pending bundle: " + e.getNewBundle());
        }

        @AccountEvent
        public void confirmed(EventTransferConfirmed e) {
            System.out.println("Account: " + account.getId());
            System.out.println("Outgoing payment confirmed: " + e.getBundle().getBundleHash());
        }

        @AccountEvent
        public void received(EventReceivedMessage e) {
            System.out.println("Account: " + account.getId());
            System.out.println("Received a new message: " + e.getMessage());
        }

        @AccountEvent
        public void received(EventReceivingDeposit e) {
            System.out.println("Account: " + account.getId());
            System.out.println("Receiving a new payment: " + e.getBundle());
        }

        @AccountEvent
        public void received(EventReceivedDeposit e) {
            System.out.println("Account: " + account.getId());
            System.out.println("Received a new payment: " + e.getBundle());
        }
    }
    ```

2. アカウントに`AccountListener`オブジェクトを登録します。
  <!-- 2. Register the `AccountListener` object with your account -->

    ```java
    account.getEventManager().registerListener(new AccountListener(account));
    ```

:::success:おめでとうございます！:tada:
アカウントはリッスンして行動することができるイベントを発信しています。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You're account can now emit events that you can listen to and act on. -->
<!-- ::: -->

## 次のステップ
## Next steps

イベントリスナを作ったので、これをテストするために[アカウントとの間で支払いを行います](../how-to-guides/create-and-manage-cda.md)。
Now that you have an event listener, start [making payments to/from your account](../how-to-guides/create-and-manage-cda.md) to test it.
