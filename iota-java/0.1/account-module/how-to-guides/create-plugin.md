# プラグインを作成する
<!-- # Create a plugin -->

**プラグインはアカウントの機能を拡張します。すべてのプラグインは`EventListener`インターフェースを実装しており、`EventManager`に追加されています。故に、プラグインを作成した時は、アカウントに別々の`EventListener`を登録する必要はありません。**
<!-- **Plugins extend the functionality of an account. All plugins implement the `EventListener` interface and are added to the `EventManager`. As a result, when you create a plugin, you don't need to register a separate `EventListener` with your account.** -->

## 手順1. スクリーンにイベントを出力するプラグインを作成する
<!-- ## Step 1. Create a plugin that prints events to the screen -->

このガイドでは、プラグインの作成方法を説明するために、イベントが発生したときにイベントをスクリーンに出力するプラグインを作成します。
<!-- To explain how to create a plugin, this guide helps you to create one that prints events to the screen as they happen. -->

プラグインクラスを作成するには、次のいずれかを実行します。
<!-- To create a plugin class you can do one of the following: -->

* `AccountPlugin`クラスを拡張する
<!-- * Extend the `AccountPlugin` class -->
* `Plugin`インターフェースを実装する
<!-- * Implement the `Plugin` interface -->

### AccountPluginクラスを拡張する
<!-- ### Extend the AccountPlugin class -->

プラグインを作成する最も簡単な方法は`AccountPlugin`クラスを拡張するクラスを作成することです。
<!-- The easiest way to create a plugin is to create a class that extends the `AccountPlugin` class. -->

```java
public class TestPlugin extends AccountPlugin {

    @Override
    public void load() throws Exception {
        // ファイルの読み取り、メモリを集中的に使用するリソースの生成など、プラグインが必要とするデータをロードします。
    }

    @Override
    public boolean start() {
        // 継続的に実行したいプロセスを開始します。

        // すべてうまくいった場合はtrueを返し、そうでない場合はfalseを返します。
        return true;
    }

    @Override
    public void shutdown() {
        // 実行中のプロセスをここで停止します。
    }

    @Override
    public String name() {
        return "AwesomeTestPlugin";
    }

    @AccountEvent
    public void confirmed(EventTransferConfirmed e) {
        System.out.println("account: " + account.getId());
        System.out.println("confimed: " + e.getBundle().getBundleHash());
    }

    @AccountEvent
    public void promoted(EventPromotion e) {
        System.out.println("account: " + account.getId());
        System.out.println("promoted: " + e.getPromotedBundle());
    }
}
```

### Pluginインターフェースを実装する
<!-- ### Implement the Plugin interface -->

クラスを拡張できない場合、またはそうしたくない場合は、`Plugin`インターフェースを実装することができます。このオプションはプラグインが動作するアカウントオブジェクトのための`getter`と`setter`メソッドを必要とします。
<!-- If you can't extend a class, or you don't want to, you can implement the `Plugin` interface. This option requires `getter` and `setter` methods for the account object with which the plugin will work. -->

```java
public class TestPlugin implements Plugin {

    private Account account;

    @Override
    public void setAccount(Account account) {
        this.account = account;
    }

    @Override
    public Account getAccount() {
        return account;
    }

    @Override
    public void load() throws Exception {
        // このプラグインに必要なデータをロードします。ストレージの読み取り、メモリ集約型リソースの生成などを考えてください。
    }

    @Override
    public boolean start() {
        // Start any processes that you want to have running continuously
        // 継続的に実行したいプロセスを開始します。

        // すべてうまくいった場合はtrueを返し、そうでない場合はfalseを返します。
        return true;
    }

    @Override
    public void shutdown() {
        // 実行中のプロセスをここで停止します。
    }

    @Override
    public String name() {
        return "AwesomeTestPlugin";
    }

    @AccountEvent
    public void confirmed(EventTransferConfirmed e) {
        System.out.println("account: " + account.getId());
        System.out.println("confimed: " + e.getBundle().getBundleHash());
    }

    @AccountEvent
    public void promoted(EventPromotion e) {
        System.out.println("account: " + account.getId());
        System.out.println("promoted: " + e.getPromotedBundle());
    }
}
```

## 手順2. アカウントオブジェクトにプラグインクラスを追加する
<!-- ## Step 2. Add the plugin class to your account object -->

プラグインクラスを作成したら、それを使ってアカウントを作成できます。
<!-- After you've created a plugin class, you can build your account with it. -->

```java
Plugin myPlugin = new TestPlugin();
IotaAccount account = new IotaAccount.Builder(SEED)
        .plugin(myPlugin)
        .build();
```

:::success:おめでとうございます！:tada:
アカウントがプラグインをロードすると、次のメッセージが表示されます：`Loaded plugin AwesomeTestPlugin`

これで、アドレスへの預け入れまたは取り出しが確定または促進されるたびに、プラグインからメッセージが届きます。
:::
<!-- :::success: -->
<!-- When the account loads the plugin, you'll see the following message: `Loaded plugin AwesomeTestPlugin`. -->
<!--  -->
<!-- Now, whenever a deposit or withdrawal is confirmed or promoted for your account, you'll receive a message from the plugin. -->
<!-- ::: -->
