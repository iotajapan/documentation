# Java でアカウントプラグインを作成する
<!-- # Create an account plugin in Java -->

**プラグインはアカウントの機能を拡張します．このガイドでは，アカウントのイベントをコンソールに出力するプラグインを作成します．**
<!-- **Plugins extend the functionality of an account. In this guide, you create a plugin that prints your account's events to the console.** -->

## IOTA ネットワーク
<!-- ## IOTA network -->

このガイドでは，[デブネット](root://getting-started/0.1/network/iota-networks.md#devnet)の[ノード](root://getting-started/0.1/network/nodes.md)に接続します．
<!-- In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet). -->

## 手順1. コンソールにイベントを出力するプラグインを作成する
<!-- ## Step 1. Create a plugin that prints events to the console -->

プラグインを作成するには，次のいずれかを実行できます．
<!-- To create a plugin you can do one of the following: -->

- `AccountPlugin` クラスを拡張する（最も簡単なオプション）
<!-- - Extend the `AccountPlugin` class (easiest option) -->
- `Plugin` インターフェースを実装する
<!-- - Implement the `Plugin` interface -->

### AccountPlugin クラスを拡張する
<!-- ### Extend the AccountPlugin class -->

プラグインを作成する最も簡単な方法は `AccountPlugin` クラスを拡張するクラスを作成することです．
<!-- The easiest way to create a plugin is to create a class that extends the `AccountPlugin` class. -->

```java
public class TestPlugin extends AccountPlugin {

	@Override
	public void load() throws Exception {
		// ファイルの読み取り，メモリを集中的に使用するリソースの生成など，プラグインが必要とするデータをロードします．
	}

	@Override
	public boolean start() {
		// 継続的に実行したいプロセスを開始します

		// すべてうまくいった場合は true を返し，そうでない場合は false を返します
		return true;
	}

	@Override
	public void shutdown() {
		// 実行中のプロセスをここで停止します
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

### Plugin インターフェースを実装する
<!-- ### Implement the Plugin interface -->

`AccountPlugin` クラスを拡張する代わりに，`Plugin` インターフェースを実装できます．このオプションには，プラグインが動作するアカウントオブジェクトの `getter` および `setter` メソッドが必要です．
<!-- Instead of extending the `AccountPlugin` class, you can implement the `Plugin` interface. This option requires `getter` and `setter` methods for the account object with which the plugin will work. -->

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
        // このプラグインに必要なデータをロードします．ストレージの読み取り，メモリ集約型リソースの生成などを考えてください．
    }

    @Override
    public boolean start() {
        // 継続的に実行したいプロセスを開始します

        // すべてうまくいった場合はtrueを返し，そうでない場合はfalseを返します
        return true;
    }

    @Override
    public void shutdown() {
        // 実行中のプロセスをここで停止します
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

プラグインクラスを作成したら，プラグインクラスを使用してアカウントをビルドおよび開始します．
<!-- After you've created a plugin class, build and start your account with it. -->

```java
Plugin myPlugin = new TestPlugin();
IotaAccount account = new IotaAccount.Builder(SEED)
        .plugin(myPlugin)
        .build();
```

:::success:おめでとうございます:tada:
アカウントがプラグインをロードすると，次のメッセージが表示されます：`Loaded plugin AwesomeTestPlugin`

これで，アドレスへのデポジットまたは取り出しが確定またはプロモートされるたびに，プラグインからメッセージが届きます．
:::
<!-- :::success: -->
<!-- When the account loads the plugin, you'll see the following message: `Loaded plugin AwesomeTestPlugin`. -->

<!-- Now, whenever a deposit or withdrawal is confirmed or promoted for your account, you'll receive a message from the plugin. -->
<!-- ::: -->

## コードを実行する
<!-- ## Run the code -->

これらのコードサンプルは[GitHub](https://github.com/iota-community/account-module) でホストされています．
<!-- These code samples are hosted on [GitHub](https://github.com/iota-community/account-module). -->

開始するには，デバイスに [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) がインストールされている必要があります．
<!-- To get started you need [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed on your device. -->

また，[Maven](https://maven.apache.org/download.cgi) ビルドツールを使用する Java 開発環境も必要です．Java クライアントライブラリを初めて使用する場合は，[スタートガイド](../../getting-started/java-quickstart.md)を完了し，Maven でライブラリをインストールするための指示に従ってください．
<!-- You also need a Java development environment that uses the [Maven](https://maven.apache.org/download.cgi) build tool. If this is your first time using the Java client library, complete our [getting started guide](../../getting-started/java-quickstart.md), and follow the instructions for installing the library with Maven. -->

コマンドラインで，次を実行します．
<!-- In the command-line, do the following: -->

--------------------
### Linux and macOS
```bash
git clone https://github.com/iota-community/account-module.git
cd account-module/java/account-module
mvn clean install
mvn exec:java -Dexec.mainClass="com.iota.CreatePluginAccount"
```
---
### Windows
```bash
git clone https://github.com/iota-community/account-module.git
cd account-module/java/account-module
mvn clean install
mvn exec:java -D"exec.mainClass"="com.iota.CreatePluginAccount"
```
--------------------

アカウントが開始されると，イベントロガーが開始されることがわかります．
<!-- You should see that the event logger starts when your account does. -->

## 次のステップ
<!-- ## Next steps -->

[条件付きデポジットアドレスを生成する](../java/generate-cda.md)．
<!-- [Generate a conditional deposit address](../java/generate-cda.md). -->
