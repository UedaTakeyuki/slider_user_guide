# データの表示 の機能
##<u>機能概要</u>
slider は 接続した LCD や OLED に現在値を表示し、phone jack Speaker 等で読み上げる機能があります  
時刻を表示しながら毎分決まった時間に以下の情報を表示します

- 読み取ったセンサーデータ
- slider のIPアドレス

<img src="pic/ss.2016-12-15 18.31.55.png" width="30%">
<img src="pic/ss.2016-12-15 18.31.20.png" width="30%">
<img src="pic/ss.2016-12-15 18.30.01.png" width="30%">


表示動作の実際はこの[動画](https://youtu.be/i9my6MqdXHM)のようになります

##<u>LCD表示</u>
下記の LCD に対応しています

1. YmRobot LCM1602 IIC V1
2. SPI SSD1306 128X64 OLED

###動作設定コマンド
コンソールより下記コマンドを実行することで動作設定を変更できます  
デフォルト設定では自動実行解除状態になっています

####YmRobot LCM1602 IIC V1
1. 起動時自動実行設定: sudo systemctl enable clock_note.service
2. 起動時自動実行解除: sudo systemctl disable clock_note.service

####SPI SSD1306 128X64 OLED
1. 起動時自動実行設定: sudo systemctl enable clock_note2.service
2. 起動時自動実行解除: sudo systemctl disable clock_note2.service

##<u>データの読み上げ</u>
### 概要

phone jack に接続されたスピーカーや、HDMI や RCA で接続された TV のスピーカー等に現状値の読み上げを行うことができます
読み上げ設定には音声合成エンジンのインストールが必要になります  
下記の音声合成エンジンに対応しています

|| [AquesTalk](http://www.a-quest.com/products/aquestalkpi.html) | [Open JTalk](http://open-jtalk.sourceforge.net) |
|:-----------|:------------:|:------------:|
| 特徴 | 合成が高速、自然なアクセント |  |
| ライセンス | 商用（試供版あり） | modified BSD |

### エンジンの選択
エンジンの選択は slider にログインし、下記ファイルの編集でおこないます

```bash:
  /var/www/html/SCRIPT/say/config.ini
```

上記ファイルの command_path に、エンジンへのパスを設定してください

```bash:config.ini
command_path=/home/pi/install/aquestalkpi/AquesTalkPi
#command_path=/home/pi/SCRIPT/slider/vendor/jsay.mei.sh
```

### エンジンのインストール
open-jtalk のエンジンはすでにイントールしてありますので、上記の設定で使用できます  
aquestalkpi を使用する場合は /home/pi/install 配下にエンジンをインストールし、エンジンの実行ファイルへのフルパスを上記ファイルの cmmand_path に設定してください
