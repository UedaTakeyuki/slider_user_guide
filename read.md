# センサデータの取得

slider は標準的なセンサモジュールやカメラモジュールについてはデータを取得するための標準的なライブラリと設定を事前に用意されており、モジュール名を指定するだけで利用することができます

また、slider でライブラリと設定を用意していないモジュールについても簡単な設定と外部ライブラリの取り込みで slider の一部として上記の一連の処理で利用することができます

## <u>センサモジュールの物理的な接続</u>
標準的なセンサモジュールの接続方法については[センサー等の slider への接続](connect_sensor.md)をご参照ください

その他のセンサモジュールについては、物理的に接続した後、まずそのセンサモジュールの実績のあるライブラリを利用して、値が正しく読めるまでを準備しｔください。以下、標準IoT機能の設定ではこの接続とライブラリをそのまま利用します。

## <u>標準IoT機能の設定ファイル</u>

### config.toml ファイル
標準IoT 機能の設定は以下の toml ファイルで行います

```bash:
/home/pi/SCRIPT/slider/config.toml
```

toml は ini ファイルなどよりも豊かな構造を表現することができる設定ファイルの形式です。slider は toml の表現力を利用することで、多様なセンサーを同じ形式で設定することを可能にしています

toml の構文の詳細についてはこちらの [言語仕様](https://github.com/toml-lang/toml)などが参考になりますが、特に詳細な知識がなくても問題ないように以下で詳細にご説明させていただきます

内容は初期状態で以下のようになっています

```bash:config.toml
[sensors]
# data [[名前、単位、送信、保存],...]

  [sensors.mh_z19]
  data = [["CO2","ppm","gen_sender","gen_saver"]]

  [sensors.dht22]
  data = [["temp","℃","gen_sender","gen_saver"],
  				["humidity","%","gen_sender","gen_saver"],
  				["humiditydeficit","g/㎥","gen_sender","gen_saver"]]

[imaging]
# data [[イメージ種別、デバイス数、送信、保存],...]
# イメージ種別 = [ "pic" | "movie" ]
# デバイス数 = [ "one" | "all"]

	[imaging.uvc]
	data =["pic","all","gen_pic_sender", "gen_pic_saver"]
```

前半の [sensor] テーブルがセンサーの定義部分、後半の [image]テーブルがカメラデバイスの定義部分になります

```bash:
[sensors]
  [sensor.ラッパークラス名]
  data = [[名前、単位、送信、保存],...]
```

### ラッパーモジュールの宣言

[sensor] テーブルの内部テーブル [sensor.ラッパーモジュール名] で、各々のセンサモジュールを定義します。上記の例では [sensor.mh_z19] で mh_Z19 二酸化炭素センサモジュールのラッパーモジュール mh_z19 と、dht22 温湿度センサモジュールのラッパーモジュール dht22 の **名前を宣言** しています。

slider はこちらで宣言されたラッパーモジュール名と同じ名前の python ファイルを使ってセンサーのデータを取得します。同じ内容を技術的に正確に表現すると、slider はラッパーモジュールのダイナミックインポートを行い、read() メソッドを呼び出すことで data を取得します

ラッパーモジュールとは、センサーモジュールの実績のあるライブラリにラップして、出力形式を slider の標準IoT 機能で扱える用に変換する glue code の役割を果たす小さな python のクラスです。作り方については後ほど詳細にご説明いたします

### data 構造の宣言

次に続く **data宣言** で、ラッパーモジュールから返されるデータの構造を宣言します

構文は以下のようになります

```bash:
data = [[名前、単位、送信、保存],...]
```
例えば前述の mh_z19 二酸化炭素センサーの場合は以下のように指定しています

| 要素 | 値 | 意味|
|:-----------|:------------:|:------------|
| 名前 | CO2 | データの名前、ラッパーモジュールが返す python ディクショナリの key、この名前で保存され、送信される |
| 単位 | ppm | このデータの物理的単位 |
| 送信 | gen_sender | データの送信を行うモジュールの指定。なにも指定しないと送信しない |
| 保存 | gen_saver | データの保存を行うモジュールの指定。なにも指定しないと保存しない |

前述の dht22 温湿度センサーのように一回のスキャンで温度と湿度の２種類のデータを返し、さらにその値から計算して作成される飽差のような副次的な値もセンサーのスキャンデータ値と同様に扱いたい場合は、上の例のように必要な数だけ data 内のテーブルを用意します。ここでは下記の３つを用意しています

| 要素 | 値 | 意味|
|:-----------|:------------:|:------------|
| 名前 | temp | データの名前、ッパーモジュールが返す python ディクショナリの key、この名前で保存され、送信される |
| 単位 | ℃ | このデータの物理的単位 |
| 送信 | gen_sender | データの送信を行うモジュールの指定。なにも指定しないと送信しない |
| 保存 | gen_saver | データの保存を行うモジュールの指定。なにも指定しないと保存しない |

| 要素 | 値 | 意味|
|:-----------|:------------:|:------------|
| 名前 | humidity | データの名前、ッパーモジュールが返す python ディクショナリの key、この名前で保存され、送信される |
| 単位 | % | このデータの物理的単位 |
| 送信 | gen_sender | データの送信を行うモジュールの指定。なにも指定しないと送信しない |
| 保存 | gen_saver | データの保存を行うモジュールの指定。なにも指定しないと保存しない |

| 要素 | 値 | 意味|
|:-----------|:------------:|:------------|
| 名前 | humiditydeficit | データの名前、ッパーモジュールが返す python ディクショナリの key、この名前で保存され、送信される |
| 単位 | g/㎥ | このデータの物理的単位 |
| 送信 | gen_sender | データの送信を行うモジュールの指定。なにも指定しないと送信しない |
| 保存 | gen_saver | データの保存を行うモジュールの指定。なにも指定しないと保存しない |

## <u>ラッパーモジュールの構造</u>

例として mh_z19 のラッパーモジュールを見てみます。

```python:/home/pi/SCRIPT/slider/mh_z19.py
# -*- coding: utf-8 -*-
# refer http://eleparts.co.kr/data/design/product_file/SENSOR/gas/MH-Z19_CO2%20Manual%20V2.pdf
#
# © Takeyuki UEDA 2015 -

import serial
import time
import subprocess
import slider_utils as slider
import getrpimodel

# setting

if getrpimodel.model() == "3 Model B":
  serial_dev = '/dev/ttyS0'
  stop_getty = 'sudo systemctl stop serial-getty@ttyS0.service'
  start_getty = 'sudo systemctl start serial-getty@ttyS0.service'
else:
  serial_dev = '/dev/ttyAMA0'
  stop_getty = 'sudo systemctl stop serial-getty@ttyAMA0.service'
  start_getty = 'sudo systemctl start serial-getty@ttyAMA0.service'

def mh_z19():
  try:
    ser = serial.Serial(serial_dev,
                        baudrate=9600,
                        bytesize=serial.EIGHTBITS,
                        parity=serial.PARITY_NONE,
                        stopbits=serial.STOPBITS_ONE,
                        timeout=1.0)
    while 1:
      result=ser.write("\xff\x01\x86\x00\x00\x00\x00\x00\x79")
      s=ser.read(9)
      if len(s) >= 4 and s[0] == "\xff" and s[1] == "\x86":
        return {'co2': ord(s[2])*256 + ord(s[3])}
      break
  except IOError:
    slider.io_error_report()
  except:
    slider.unknown_error_report()

def read():
  p = subprocess.call(stop_getty, stdout=subprocess.PIPE, shell=True)
  result = mh_z19()
  p = subprocess.call(start_getty, stdout=subprocess.PIPE, shell=True)
  if result is not None:
    return {'CO2': result["co2"]}

if __name__ == '__main__':
#  value = mh_z19()
#  print "co2=", value["co2"]
  value = read()
  if value is not None:
    print "co2=", value["CO2"]
  else:
    print "None"
```

ラッパモジュールとして必須なメソッドは read() だけです。read の戻り値は前述の data 宣言で指定した **名前** と同じキー名（上の例では **CO2**）を持つディクショナリでなければなりません

上の例のように

```pyton:
if __name__ == '__main__':
```

を用意しておくと、ラッパーモジュール自体をテストするのに便利です

上の例でメソッド mh_z19() がセンサーモジュールから値を読み込む処理の本体です。メソッド名はなんでもよいです。mh_z19 二酸化炭素センサモジュールは元々 Raspberry Pi 用のライブラリが存在しないので、python の serial モジュールを使ってシリアルから直に値を読んでいます

もう少し複雑な例として、dht22 のラッパーモジュールを見てみます。
こちらは既存のライブラリを利用し、かつラッパモジュール内で副次的な値を生成してセンサデータと共にかえしています

```python:/home/pi/SCRIPT/slider/mh_z19.py
# coding:utf-8
# Copy Right Atelier Grenouille  © 2015 -
#
# require: 'lol_dht' https://github.com/technion/lol_dht22
# return:  {"temp": , "humidity":}

import os
import sys
import subprocess32 as subprocess
import re
import slider_utils as slider

def dht22(gpio):
  try:
    p = subprocess.Popen(os.path.abspath(os.path.dirname(__file__))+"/vendor/lol_dht22/loldht " + str(gpio) + " |grep Hum", stdout=subprocess.PIPE, stderr=subprocess.PIPE, shell=True)
    std_out, std_err = p.communicate(None, timeout=10)
    result = std_out.strip()

    # read result
    match = re.match(r'Humidity = (.*) % Temperature = (.*) \*C',result)
    temp = float(match.group(2))
    humidity =float(match.group(1))

    # drop bad value.
    if temp < -1000  or temp > 1000:
        temp = None
    if humidity < -1000 or humidity > 1000:
        humidity = None
    result = {"temp":temp, "humidity":humidity}
    return result
  except IOError:
    slider.io_error_report()
  except:
    slider.unknown_error_report()

# http://d.hatena.ne.jp/Rion778/20121203/1354546179
def HumidityDeficit(t,rh): # t: 温度, rh: 相対湿度
    ret = AbsoluteHumidity(t, 100) - AbsoluteHumidity(t, rh)
#    print "HD = " + str(ret)
    return ret;

# http://d.hatena.ne.jp/Rion778/20121203/1354461231
def AbsoluteHumidity(t, rh):
    ret = 2.166740 * 100 * rh * tetens(t)/(100 * (t + 273.15))
#    print "AH = " + str(ret)
    return ret


#  飽和水蒸気圧
#  function GofGra(t){};
# http://d.hatena.ne.jp/Rion778/20121126/1353861179
def tetens(t):
    ret = 6.11 * 10 ** (7.5*t/(t + 237.3))
#    print "tetens = " + str(ret)
    return ret

def read():
  result = dht22(29)
  if result is not None:
    result["humiditydeficit"] = ('%.1f' % HumidityDeficit(result["temp"],result["humidity"]))
    return result

if __name__ == '__main__':
    print dht22(29)
```

メソッド dht22(gpio) 内で、以下のように 外部ライブラリ loldht を python の subprocess モジュールを使って呼び出します

```python:
p = subprocess.Popen(os.path.abspath(os.path.dirname(__file__))+"/vendor/lol_dht22/loldht " + str(gpio) + " |grep Hum", stdout=subprocess.PIPE, stderr=subprocess.PIPE, shell=True)
std_out, std_err = p.communicate(None, timeout=10)
result = std_out.strip()
```

そして、外部ライブラリ loldht の戻り値の文字列から温度と湿度をパターンマッチングで取り出し

```python:
# read result
match = re.match(r'Humidity = (.*) % Temperature = (.*) \*C',result)
temp = float(match.group(2))
humidity =float(match.group(1))
```

センサーモジュール内部のコントローラーチップでは一般に異常値の破棄や再取得などは一般にあまりおこなわれていないようです。異常値をチェッkして捨てる処理もラッパー内で実装します

```python:
# drop bad value.
if temp < -1000  or temp > 1000:
    temp = None
if humidity < -1000 or humidity > 1000:
    humidity = None
```

そして、先程同様に data テーブルで宣言したのと同じ key 名で python ディクショナリを作成して返します

```python:
result = {"temp":temp, "humidity":humidity}
return result
```

read() メソッドでは、前述の dht22(gpio) メソッドの戻り値から humiditydeficit を計算し、data 宣言で宣言したとおりディクショナリの配列に追加して３要素にして返します

```python:
result["humiditydeficit"] = ('%.1f' % HumidityDeficit(result["temp"],result["humidity"]))
return result
```

## <u>外部ライブラリのインストール</u>
外部ライブラリはラッパーモジュールから利用できさえすればどのようなものであっても、どこにあっても、どのようにインストールしてもかまいません。例として lol_dht22 のインストーラを紹介します

```bash:/home/pi/SCRIPT/slider/vendor/dht22.setup.sh
# lol_dht22: Setup to use dht22 on Raspbian
git clone https://github.com/technion/lol_dht22
cd lol_dht22
./configure
make
cd ..
```

lol_dht22 は、C のコードを github から clone してビルドしたバイナリです。python のラッパーモジュールからは subprocess モジュールで外部プロセスとして呼び出し、その返り値を加工して使っています

このようにセンサモジュールの外部ライブラリはバイナリやシェルスクリプトであってもかまいませんし、もちろん import して利用できる python のモジュールであってもかまいません

## <u>センサモジュールを追加する手順のまとめ</u>
以上の、新たなセンサモジュールを slider に追加し、IoT の基本機能にマージして利用する手順をまとめるとこのようになります

1. センサモジュールを Raspberry Pi に物理的に接続
2. センサモジュールの値を読むライブラリをインストールし、動作確認
3. config.toml ファイルにラッパーモジュール名と、戻り値の構造や保存、送信の設定を定義
4. ラッパーモジュールを作成
