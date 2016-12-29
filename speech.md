# 簡易スピーチ機能

簡易スピーチ機能を使って、slider を喋らせる事ができます

phrase に喋らせたい文字列を入れて say ボタンを押すと、slider は受け取った文字列をスピーチエンジンに渡し、HDMI や phone jack 等につながれたスピーカーを使って喋ります

<img src="pic/ss.2016-12-29 19.20.48.png" width="75%">

##<u>スピーチエンジンの設定</u>
スピーチエンジンとして商用の **AquesTalkPi**、オープンソースの **Open JTalk**、45ヶ国語対応の **eSpeak** に対応しています

デフォルトでは Open JTalk を利用する設定になっています

設定は /var/www/html/SCRIPT/say/config.ini ファイルで変更できます

```bash:
command_path=/home/pi/SCRIPT/slider/vendor/jsay.mei.sh
#command_path=/home/pi/install/aquestalkpi/AquesTalkPi
#command_path=/home/pi/SCRIPT/slider/vendor/espeak.fr.sh
#command_path=/home/pi/SCRIPT/slider/vendor/espeak.zh.sh
```


### Open JTalk を利用する場合
デフォルトの設定で利用できます


### AquesTalkPi を使用する設定
AquesTalkPi のライセンスを購入し、適当な場所（例えば /home/pi/install）に AquesTalkPi エンジンをインストールしてください

/var/www/html/SCRIPT/say/config.ini ファイルを変更し、command_path にインストールした AquesTalkPi のパスを指定してください

```bash:
#command_path=/home/pi/SCRIPT/slider/vendor/jsay.mei.sh
command_path=/home/pi/install/aquestalkpi/AquesTalkPi
#command_path=/home/pi/SCRIPT/slider/vendor/espeak.fr.sh
#command_path=/home/pi/SCRIPT/slider/vendor/espeak.zh.sh
```

### eSpeak を使用する設定
eSpeak エンジンはすでにインストールしてあるので、espeak を起動する shell を作成して /var/www/html/SCRIPT/say/config.ini ファイルの command_path に指定してください

```bash:
#command_path=/home/pi/SCRIPT/slider/vendor/jsay.mei.sh
#command_path=/home/pi/install/aquestalkpi/AquesTalkPi
command_path=/home/pi/SCRIPT/slider/vendor/espeak.fr.sh
#command_path=/home/pi/SCRIPT/slider/vendor/espeak.zh.sh
```

eSpeak をフランス語で喋らせるシェル espeak.fr.sh と 中国語の espeak.zh.sh を用意しています

それ以外の言語を利用する場合や、音声オプションを変更する場合は、第一引数を echo して、パイプラインで espeak に渡す以下のようなワンライナーを作成して利用してください

```bash:espeak.fr.sh
# espeak.fr.sh
echo $1 | espeak -vfr+f3
```
