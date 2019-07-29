# SSHでRaspberry Piに接続する
<!-- # Connect to the Raspberry Pi through SSH -->

**モニターやキーボードなしでAstroPiOTAを実行したい場合は、SSHを介して対話するためにAstroPiOTAのネットワークアドレスが必要です。Raspberry PiのIPアドレスを見つけるには、Pythonスクリプトを使用できます。**
<!-- **If you want to run AstroPiOTA without a monitor and keyboard, you need it's network address to interact with it over SSH. To find the IP address of the Raspberry Pi, you can use a Python script.** -->

Python 3はRaspberry Piにプレインストールされています。
<!-- Python 3 comes pre-installed on Raspberry Pi. -->

1. ホームフォルダに`senseIP.py`という名前の新しいファイルを作成します。
  <!-- 1. In your home folder, create a new file called `senseIP.py` -->

    ```bash
    sudo nano senseIP.py
    ```

2. ファイルに次のコードを追加します。
  <!-- 2. Add the following code to the file: -->

    ```python
    import socket
    from sense_hat import SenseHat
    sense = SenseHat()

    def getIP():
            s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
            try:
                    s.connect(('192.168.255.255',1))        #lab network used for this example
                    IP = s.getsockname()[0]
            except:
                    IP = '127.0.0.1'
            finally:
                    s.close()
            return IP

    senseIP = getIP()
    for x in range(3):
            sense.show_message(senseIP, scroll_speed=0.2)
    ```

3. ファイルを保存して新しいシェルスクリプトを作成します。
  <!-- 3. Save the file and create a new shell script -->

    ```bash
    sudo nano senseBoot.sh
    ```

4. `senseIP.py`スクリプトを実行します。
  <!-- 4. Run the senseIP.py script -->

    ```bash
    python3 senseIP.py
    ```

    ![Screen capture of shell script described in text](../RasSenseIP.png)

5. シェルスクリプトを保存し、Raspberry Piの再起動時に実行するようにcronジョブをスケジュールします。
  <!-- 5. Save the shell script and schedule a cron job to run it when Raspberry Pi reboots -->

    ```bash
    crontab -e
    ```

6. `/tmp/crontab.0QNkTW/crontab`ファイルの末尾に次のコマンドを追加します。
  <!-- 6. Add the following command at the bottom of the /tmp/crontab.0QNkTW/crontab file: -->

    ```bash
    @reboot /home/pi/senseBoot.sh
    ```

    ![Screen capture of crontab file update described in text](../RasCron.png)

7. Raspberry Piを再起動してcronジョブを開始します。数秒待ってから、Raspberry PiのIPアドレスがSense HATのLEDパネルを横切ってスクロールするのを確認します。
  <!-- 7. Restart Raspberry Pi to start the cron job. Wait a second or two, then see its IP address scrolling across the Sense HAT LED panel. -->

8. SSH接続を開き、Raspberry PiのIPアドレスに接続します。
  <!-- 8. Open an SSH connection and connect to the IP address of your Raspberry Pi -->

Raspberry Piをシャットダウンするには、以下行います。
<!-- To shut down the Raspberry Pi, do the following: -->

```bash
sudo shutdown now
```
