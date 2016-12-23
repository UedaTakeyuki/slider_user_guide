# シリアルケーブルを利用した login
##<u>物理的な接続方法</u>
USB Serial ケーブルで USB と PC、シリアル と Raspberry Pi の GPIO を直結させる方法です

<img src="pic/ss.2016-12-18 19.08.30.png" width="75%">

Raspberry Pi の Raspbian OS はデフォルトで Raspberry Pi の GPIO に対して tty を開いてデバッグ接続を用意します。この tty を使って接続する方法です。この tty は /boot/config.txt の設定変更で閉じる事もできます

USB シリアルケーブルは、シリアル側の接点インターフェースが 3.3V のものを利用してください。それ以上の電圧のものを利用した場合、Raspberry Pi が破壊される場合があります

##<u>tty の利用</u>
シリアルケーブルを接続後、PC から tty に対して screen コマンド等で下記のようにコンソールを開きます

```bash:
screen /dev/USBシリアルのtty名 通信速度
```

USBシリアルのttyデバイス名と通信速度はケーブルによって異なりますので、個別に確認してくださ

下記は Mac から シリアルケーブルで接続する例です

<img src="pic/ss.2016-12-16 11.34.57.png" width="75%">

上の例では、下記コマンドで USB シリアルのttyデバイスを探し

```bash:
  ls /dev/tty.usb*
```

そこに 115200bouで接続しています

```bash:
  screen /dev/tty.usbserial-FTH9AV1T 115200
```

接続して Raspberry Pi の電源を入れると、Raspberry Pi の起動シーケンスから表示が始まります（コンソールタイプの gc16 のみ。gc15 の pixcel は起動シーケンスを表示しない）

<img src="pic/ss.2016-12-16 11.40.09.png" width="75%">

起動シーケンスの確認は WiFi が有効にならない等、起動時の問題の確認に有効です

<img src="pic/ss.2016-12-16 11.41.19.png" width="75%">

起動シーケンスが正常に終了すると slider のログイン画面になるので、ログインして利用してください
