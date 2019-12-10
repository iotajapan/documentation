# Create an account plugin in Java

**Plugins extend the functionality of an account. In this guide, you create a plugin that prints your account's events to the console.**

## IOTA network

In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet).

## Step 1. Create a plugin that prints events to the console

To create a plugin you can do one of the following:

- Extend the `AccountPlugin` class (easiest option)
- Implement the `Plugin` interface

### AccountPluginクラスを拡張する
<!-- ### Extend the AccountPlugin class -->

プラグインを作成する最も簡単な方法は`AccountPlugin`クラスを拡張するクラスを作成することです。
<!-- The easiest way to create a plugin is to create a class that extends the `AccountPlugin` class. -->

```java
public class TestPlugin extends AccountPlugin {

	@Override
	public void load() throws Exception {
		// Load data that the plugin needs such as reading a file, generating memory intensive resources, etc..
	}

	@Override
	public boolean start() {
		// Start any processes that you want to run continuously

		// Return true if all went well, otherwise return false
		return true;
	}

	@Override
	public void shutdown() {
		// Stop any running processes here
	}

	@Override
	public String name() {
		return "AwesomeTestPlugin";
	}

	@AccountEvent
	public void confirmed(EventTransferConfirmed e) {
	    System.out.println("account: " + this.getAccount().getId());
	    System.out.println("confimed: " + e.getBundle().getBundleHash());
	}

	@AccountEvent
	public void promoted(EventPromotion e) {
	    System.out.println("account: " + this.getAccount().getId());
	    System.out.println("promoted: " + e.getPromotedBundle());
	}
}
```

### Pluginインターフェースを実装する
<!-- ### Implement the Plugin interface -->

Instead of extending the `AccountPlugin` class, you can implement the `Plugin` interface. This option requires `getter` and `setter` methods for the account object with which the plugin will work.

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

After you've created a plugin class, build and start your account with it.

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

<!-- Now, whenever a deposit or withdrawal is confirmed or promoted for your account, you'll receive a message from the plugin. -->
<!-- ::: -->

## Run the code

To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device.

You also need a Java development environment that uses the [Maven](https://maven.apache.org/download.cgi) build tool. If this is your first time using the Java client library, complete our [getting started guide](../../getting-started/java-quickstart.md), and follow the instructions for installing the library with Maven.

In the command-line, do the following:

--------------------
### Linux and macOS
```bash
git clone https://github.com/JakeSCahill/iota-samples.git
cd iota-samples/java/account-module
mvn clean install
mvn exec:java -Dexec.mainClass="com.iota.CreatePluginAccount"
```
---
### Windows
```bash
git clone https://github.com/JakeSCahill/iota-samples.git
cd iota-samples/java/account-module
mvn clean install
mvn exec:java -D"exec.mainClass"="com.iota.CreatePluginAccount"
```
--------------------

You should see that the event logger starts when your account does.

## Next steps

[Generate a conditional deposit address](../java/generate-cda.md).
