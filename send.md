# データの送信
デフォルト設定で slider は取得したセンサデータや撮影した画像を自分自身が提供する monitor サービスに送信します。送信を行うかどうか、送信先、送信方法等を変更することができます

##<u>デフォルトの送信先</u>
デフォルトの設定で送信先は自分自身の[簡易 monitor 表示](internal_monitor_view.md)になります

###monitor サービス
monitor サービスは slider から継続的に送信されるセンサーデータや撮影画像を受け取り、直近の状況を表示する Web アプリケーションといくつかの付加機能から構成される Web ベースのサービスです

現場の状況を PC からも スマフォからもわかりやすく表示します

<img src="pic/ss.2016-12-07 13.32.53.png" width="75%">
<img src="pic/2016-12-09 14.31.02.png" width="20%">

###localhost
ブラウザから slider の [Webインターフェース](webif.md) に接続し、[簡易 monitor 表示](internal_monitor_view.md) を開くと、センサデータや撮影画像を確認することができます

##<u>送信の抑止</u>
取得したセンサデータや撮影データを送信したくない場合は[センサデータの取得](read.md)で説明する config.toml ファイルの data 宣言の第３要素の **送信** の指定を "" にすることで保存の抑止が指定できます

```bash:config.toml
[sensors]
# data [[名前、単位、送信、保存],...]

  [sensors.dht22]
  data = [["temp","℃","","gen_saver"],
  				["humidity","%","","gen_saver"],
  				["humiditydeficit","g/㎥","","gen_saver"]]

          [imaging]
          # data [[イメージ種別、デバイス数、送信、保存],...]
          # イメージ種別 = [ "pic" | "movie" ]
          # デバイス数 = [ "one" | "all"]

          [imaging.uvc]
	        data =["pic","all","gen_pic_sender", ""]
```

##<u>送信先、送信方法の変更</u>
### gen_sender モジュールと gen_pic_sender モジュール

まず、config.toml ファイルでの送信の指定を再確認します。それはセンサーデータ、撮影画像共にdata宣言の第３要素に指定する、それぞれ gen_sender と gen_pic_sender でした

```bash:config.toml
[sensors]
# data [[名前、単位、送信、保存],...]

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

ここで、gen_sender と gen_pic_sender はそれぞれセンサーデータと画像の送信をおこなう save() というメソッドを持つ python のモジュールの名前です。slider は data 宣言の第４引数で指定される名前の sender モジュールのダイナミックインポートを行い、send()メソッドに必要なデータを渡し、送信処理を sender モジュールに委譲します

### 送信先の変更
センサデータの generic な sender モジュール **gen_sender** と、撮影画像の generic な sender モジュール **gen_pic_sender** の設定ファイルはそれぞれ **gen_sender.ini** と **gen_pic_sender.ini** になります

```/home/pi/SCRIPT/slider/gen_sender.ini
[send]
protocol=http
protocol_old=mqtt

[server]
url_base=http://localhost/SCRIPT/monitor/

[mqtt]
host=localhost
topic=slider/
id_base=slider-

[log]
log_file=/home/pi/LOG/gen_sender.log

[monitor]
mode=public
mode_private=private
```

```/home/pi/SCRIPT/slider/gen_sender.ini
[send]
protocol=http
protocol_old=mqtt

[server]
url_base=http://localhost/SCRIPT/monitor/

[mqtt]
host=localhost
topic=slider/
id_base=slider-

[log]
log_file=/home/pi/LOG/gen_sender.log

[monitor]
mode=public
mode_private=private
```

[server] セクションの url_base がデータの送信先、[log] セクションの log_file が送信処理のログファイルの送信先になります

[protocol]セクション、及び[mqtt]セクションは **現在は未使用** です。monitor の mqtt 対応等に合わせて将来使用予定です

[monitor]セクションは monitor への送信モードです。private モードでは monitor が Raspberry Pi のシリアルID 毎に受け取ったデータを変更できるようにデータを送信します。

###　クラウド上の monitor サービスへの送信
クラウド上の monitor サービスを利用される場合は、下記２点の変更が必要です

- [server]セクションの url_base に指定された url を指定する
- [monitor]セクションの mode を private に変更

```bash:
[monitor]
mode=private
```

### Xivelay, M2X, AWS Kinesis 等の Paas への送信
gen_sender, gen_pic_sender はそれぞれセンサーデータと画像の generic に定義された sender モジュールです。独自の sender モジュールを用意することで、Xivelay, M2X, AWS Kinesis 等の Paas へ送信するように変更することができます

独自 sender モジュールの名前は generic な sender モジュールとおなじでなければどのような名前であってもかまいません

独自 sender モジュールは slider が検索できるように下記フォルダに .py か .pyc で保存してください

```bash:
/home/pi/SCRIPT/slider
```

独自sender には下記の send()メソッドを実装してください
slider は全てのパラメタを str型で渡します

```python:
def send(serialid, name, value):
    """センサーデータ
         serialid: Raspberry Pi のシリアルID
         name:  データ名
         value: データ値
    """
def send(serialid, device, picfilepath):
    """撮影画像
         serialid: Raspberry Pi のシリアルID
         device: カメラデバイス名 ex. "video0"
         pictfilepath: 撮影画像ファイル名をフルパスで
    """
```
