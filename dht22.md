# Introduction

##<u>配線</u>

下図のようにセンサモジュールの + 記号のついた端子を Raspberry Pi の 3.3V Pin に、- 記号のついた端子を GND に、真ん中の信号端子端子を GPIO.29 番に接続します

                      ![pic](pic/ss.2016-12-11 16.52.45.png)                                                                  

信号端子は 29番以外のGPIO に変更することも可能です

##<u>接続確認</u>

slider は dht22 の polling に [lol_dht22](https://github.com/technion/lol_dht22) の dht22 polling application を利用しています  
下記コマンドで温度と湿度が表示されれば正しく接続できています

```bash:
pi@gc16:~ $ sudo SCRIPT/slider/vendor/lol_dht22/loldht 29
Raspberry Pi wiringPi DHT22 reader
www.lolware.net
Data not good, skip
Humidity = 43.60 % Temperature = 24.00 *C
```

loldht コマンドのパラメタの 29 は、dht22 センサモジュールの信号端子を繋いだ GPIO の番号です

GPIO29 　以外に信号端子を接続した場合は、接続した GPIO 番号を指定してください

##<u>信号端子を接続する GPIO の変更</u>

設定ファイルを編集して信号端子の接続先を 29番 以外の GPIO に変更する事が可能です

下記、 dht22.ini ファイルの [gpio] セクション gpio パラメータに gpio 端子番号を変更してください

```bash:
# 設定ファイル
# /home/pi/SCRIPT/slider/dht22.ini
[mode]
#run_mode=dummy
run_mode=real

[gpio]
gpio=29
```

##<u>ダミーモード</u>
ダミーモードを利用すると dht22 センサモジュールのかわりにダミーデータを使ってセンサモジュールなしで [基本機能](slider_main_feature.md) の動作確認を行う事ができます

下記、 dht22.ini ファイルの [mode] セクション run_mode パラメータを real から dummy に変更してください

```bash:
# 設定ファイル
# /home/pi/SCRIPT/slider/dht22.ini
[mode]
run_mode=dummy
#run_mode=real

[gpio]
gpio=29
```
