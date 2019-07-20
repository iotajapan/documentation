# アカウントへ/から支払いをする
<!-- # Make payments to/from your account -->

**支払いをするには、送信者と受信者の両方にCDAが必要です。送信者はIOTAトークンを含む期限切れのCDAを必要とし、受信者はアクティブなCDAを必要とします。**
<!-- **To make payments, both the sender and the receiver need to have a CDA. The sender needs an expired CDA that contains IOTA tokens, and the receiver needs an active CDA.** -->

:::info:
CDAはアカウント内でのみ使用でき、汎用クライアントライブラリメソッドでは使用できません。故に、CDAを使用するには、送信者と受信者の両方にアカウントが必要です。
:::
<!-- :::info: -->
<!-- CDAs can be used only in an account and not in the generic client library methods. As a result, both you and the sender must have an account to be able to use CDAs. -->
<!-- ::: -->

1. CDAの有効期限を定義します。
  <!-- 1. Define an expiration time for the CDA -->

    ```java
    // In this case, after 72 hours
    Date hours = new Date(System.currentTimeMillis() + 72000 * 60 * 60);
    ```

2. 有効期限と予想量で新しいCDAを作成します。
  <!-- 2. Create a new CDA with an expiration time and expected amount -->

    ```java
    Future<ConditionalDepositAddress> cda = account.newDepositAddress(hours, false, 1000).get();
    ```

3. CDAがまだアクティブであることを確認した後で、CDAに預け入れを送信します。
  <!-- 3. After making sure that the CDA is still active, send a deposit to it -->

    ```java
    Future<Bundle> bundle = account.send(
            cda.getDepositAddress().getHashCheckSum(),
            cda.getRequest().getExpectedAmount(),
            Optional.of("Thanks for that pizza!"), Optional.of("OMNOMNOM"));
    bundle.get();
    ```

    :::info
    このサンプルコードを無料のテストトークンでテストしたい場合は、[Devnet蛇口](root://getting-started/0.1/tutorials/receive-test-tokens.md)からテストトークンを取得してください。
    :::
    <!-- :::info -->
    <!-- If you want to test this sample code with free test tokens, [request some from the Devnet faucet](root://getting-started/0.1/tutorials/receive-test-tokens.md). -->
    <!-- ::: -->

    :::info:
    CDAの最後の9文字はチェックサムで、アドレスとそのアドレスのすべての条件のハッシュ値です。トリニティやDevent蛇口はCDAをまだサポートしていないため、このチェックサムは、トリニティやDeventの蛇口とは互換性がありません。

    トリニティやDevent蛇口の入力フィールドに自分のアドレスを貼り付ける前に、チェックサムを削除してください。
    :::
    <!-- :::info: -->
    <!-- The last 9 characters of a CDA are the checksum, which is a hash of the address and all of its conditions. This checksum is not compatible with Trinity or the Devent faucet because they don't yet support CDAs. -->
    <!--  -->
    <!-- Remove the checksum before pasting your address into the input field of either of these applications. -->
    <!-- ::: -->

## 利用可能な残高全体を1つのCDAに転送する
<!-- ## Transfer your entire available balance to one CDA -->

できるだけ少ない数のCDAで残高の大部分を維持することをお勧めします。こうすることで、支払いをより速く、そしてより少ないトランザクションですみます。そのためには、利用可能残高を新しいCDAに転送することができます。
<!-- You may want to keep the majority of your balance on as few CDAs as possible. This way, making payments is faster and requires fewer transactions. To do so, you can transfer you available balance to a new CDA. -->

:::info:
利用可能残高は、すべての期限切れCDAの総残高です。この残高は取り出しても安全です。

アカウントの合計残高には、まだアクティブで期限切れではないCDAが含まれています。この残高を取り出すのは危険です。
:::
<!-- :::info: -->
<!-- Available balance is the total balance of all expired CDAs. This balance is safe to withdraw. -->
<!--  -->
<!-- Your account's total balance includes CDAs that are still active as well as expired. This balance is unsafe to withdraw. -->
<!-- ::: -->

1. アカウントの利用可能な残高の合計を予想量とするCDAを作成します。
  <!-- 1. Create a CDA that has your account's total available balance as its expected amount -->

    ```java
    Date hours = new Date(System.currentTimeMillis() + 10000 * 60 * 60);
    ConditionalDepositAddress cda = account.newDepositAddress(hours, false, account.availableBalance()).get();
    ```

2. 使用可能な残高の合計をCDAに転送します。
  <!-- 2. Transfer your total available balance to the CDA -->

    ```java
    Future<Bundle> bundle = account.send(
            address.getDepositAddress().getHashCheckSum(),
            address.getRequest().getExpectedAmount(),
            Optional.of("Sweep of all addresses"),
            Optional.of("IOTA9SWEEP"));
    bundle.get();
    ```

## CDAを送信者に送信する
<!-- ## Send someone your CDA -->

送信者にあなたのアカウントにIOTAトークンを送って欲しい場合、送信者にあなたのCDAを送信する必要があります。
<!-- If you want a depositer to send IOTA tokens to your account, you need to send them your CDA. -->

CDAは記述的なオブジェクトなので、送信する前にCDAを任意の形式にシリアル化できます。`generateCDA()`メソッドは以下のフィールドを持つCDAオブジェクトを返します。CDAを送信者に配布する前に、任意の形式にシリアル化することができます。
<!-- Because CDAs are descriptive objects, you can serialize them into any format before sending them. The `generateCDA()` method returns a CDA object with the following fields. You can serialize a CDA into any format before distributing it to senders. -->

<!-- This is the -->

```js
{
   address, // The last 9 trytes are the checksum
   timeout_at,
   multi_use,
   expected_amount
}
```

例えば、これらのフィールドをシリアル化してマグネットリンクを作成できます。
<!-- For example, you can serialize these fields to create a magnet link. -->

### CDAをマグネットリンクにシリアル化する
<!-- ### Serialize a CDA into a magnet link -->

CDAをシリアル化するための組み込みメソッドを使って、マグネットリンクを作成できます。
<!-- The built-in method for serializing a CDA is to create a magent link. -->

1. CDAをマグネットリンクにシリアル化するには、次の手順に従います。
  <!-- 1. To serialize a CDA to a magnet link, do the following: -->

    ```java
    String magnet = DepositFactory.get().build(cda.get(), MagnetMethod.class);

    System.out.println(magnet);
    // iota://YWEQLREFJQORXXKKEBBBDKOPAXHXJRGVPBUTBJFSRPPYVWWYUWSBDJTIUBJVFREXEAUZWRICKH9VBSQE9KPNLTCLNC/?timeout_at=1554472983208&multi_use=false&expected_amount=1000
    ```

2. マグネットリンクにシリアル化されたCDAにトランザクションを送信するには、マグネットリンクをCDAに逆シリアル化します。
  <!-- 2. To send a transaction to a CDA that's been serialized into a magnet link, first deserialize the magent link into a CDA -->

    ```java
     String magnet = "iota://YWEQLREFJQORXXKKEBBBDKOPAXHXJRGVPBUTBJFSRPPYVWWYUWSBDJTIUBJVFREXEAUZWRICKH9VBSQE9KPNLTCLNC/?timeout_at=1554472983208&multi_use=false&expected_amount=1000";
    ConditionalDepositAddress cda = DepositFactory.get().parse(magnet, MagnetMethod.class);
    Future<Bundle> bundle = account.send(
            cda.getDepositAddress().getHashCheckSum(),
            cda.getRequest().getExpectedAmount(),
            Optional.of("Thanks for that pizza!"), Optional.of("OMNOMNOM"));
    bundle.get();
    ```
