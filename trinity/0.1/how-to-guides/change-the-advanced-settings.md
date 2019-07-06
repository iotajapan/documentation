# 詳細設定を変更する
<!-- # Change the advanced settings -->

**詳細設定は、トリニティの機能に影響します。IOTAの仕組みに慣れている場合は、詳細設定を変更します。**
<!-- **Advanced settings affect the functionality of Trinity. Change the advanced settings if you're familiar with how IOTA works.** -->

## プルーフオブワークの設定を変更する
<!-- ## Change the proof-of-work settings -->

トランザクションを送信する前に、トランザクションを有効にするためにプルーフオブワークを行う必要があります。プルーフオブワークには、計算を行うためにエネルギーを消費するコンピュータが必要です。デフォルトでは、プルーフオブワークはトリニティを実行しているコンピュータ上で行われます。
<!-- Before you send a transaction, proof of work must be done to make it valid. Proof of work requires a computer to use energy to do computations. By default proof of work is done on the computer that is running Trinity. -->

トリニティが接続しているIRIノードによってプルーフオブワークが行われるようにするには、この設定を**リモート**に変更します。
<!-- To have the proof of work be done by the IRI node that Trinity is connected to, change this setting to **Remote**. -->

## 自動促進の設定を変更する
<!-- ## Change the auto-promotion settings -->

トランザクションが確定されたことを確認できるまでに、トランザクションを自動促進（促進と再添付）することが必要かもしれません。デフォルトでは、トリニティは確定されるまでトランザクションを自動促進します。
<!-- To make sure that transactions are confirmed, it may be necessary to auto-promote (promote and reattach) them. By default, Trinity auto-promotes transactions until they're confirmed. -->

トリニティの自動促進トランザクションを停止するには、この設定を**無効**に変更します。
<!-- To stop Trinity from auto-promoting transactions, change this setting to **Disabled**. -->

:::warning:
トリニティは、最小化されている（開かれていない）場合、トランザクションを自動促進しません。
:::
<!-- :::warning: -->
<!-- Trinity does not auto-promote transactions if it's minimized (not open). -->
<!-- ::: -->
