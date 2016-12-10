# データの送信
デフォルト設定で slider は取得したセンサデータや撮影した画像を自分自身が提供する monitor サービスに送信します。送信を行うかどうか、送信先、送信方法等を変更することができます

##<u>デフォルトの送信先</u>
###monitor サービス
monitor サービスは slider から継続的に送信されるセンサーデータや撮影画像を受け取り、直近の状況を表示する Web アプリケーションといくつかの付加機能から構成される Web ベースのサービスです

現場の状況を PC からも スマフォからもわかりやすく表示します

<img src="pic/ss.2016-12-07 13.32.53.png" width="75%">
<img src="pic/2016-12-09 14.31.02.png" width="20%">

###localhost
slider は Web Server をそなえていて、slider と同じネットワークに属する(windows10 以前の Windows を除き) Windows10, Mac, iOS, android などから以下のアドレスにアクセスすると

- gc15 の場合： http://gc15.local
- gc16 の場合： http://gc16.local

下記の画面を開きます
<img src="pic/ss.2016-12-09 15.16.29.png" width="75%">

BackupPi_2 は他の RaspberryPi の SD カードを Web インターフェースでバックアップする機能、say は Raspberry Pi を遠隔で喋らせる機能、そして monitor が **自分自身が提供する monitor サービス** になります

monitor のリンクをクリックすると login 画面が開きます。初期状態でログインアカウントは id, pw 共に **00** です

<img src="pic/ss.2016-12-10 21.34.17.png" width="75%">

ログインすると、slider が送信するデータの最新値の監視がはじまります。左上の **設定変更** ボタンで設定変更画面になり

<img src="pic/ss.2016-12-10 21.34.38.png" width="75%">

グラフ表示データの表示数とアカウント情報の変更が可能です

<img src="pic/ss.2016-12-10 21.38.47.png" width="75%">

###localhost の monitor の設定
monitor は複数の slider を、Raspberry Pi のシリアルIDで区別し、データの保存と表示を行います。そこで先ず

デフォルトの設定で下記の拡張boot 領域の DATA フォルダ配下に .csv ファイルとして保存します

```bash:
/boot/DATA
```

拡張boot 領域は PC から読み書き可能な領域なので、slider のSD カードを Raspberry Pi から抜いて Mac や Windows 等に挿すと **boot** というパーティションとして認識され、中のファイルも PC の通常のファイルと同様にあつかうことができます

技術的に、拡張boot 領域は FAT32 でフォーマットされたパーティションです。Raspbian OS オリジナルな boot 領域は FAT16 でフォーマットされた 53MByte の小さな領域であるのに対して、slider では数GByte のFAT32に拡張してあります。

FAT32 は Windows, Mac, Linux 共通に読み書きができる利点があるのですが、前世紀の Windows95 で主に使われていた古いファイルシステムで、一つのファイルの最大サイズが 4GByte 以下などの制限があります。この制限を越えてデータを保存したい場合、後に述べるように USB にマウントした NTFS(Windows)やHSF+(Mac)にデータを保存するように設定を変更することができます

## センサーデータの保存形式
センサーデーターは保存領域に

```bash:
temp.csv
humidity.csv
humiditydeficit.csv
```

のような名前の .csv ファイルとして保存されます。ファイル名は[センサデータの取得](read.md)で説明する config.toml ファイルの data 宣言で宣言された名前と .csv の拡張子になります

```bash:config.toml
[sensors]
# data [[名前、単位、送信、保存],...]

  [sensors.dht22]
  data = [["temp","℃","gen_sender","gen_saver"],
  				["humidity","%","gen_sender","gen_saver"],
  				["humiditydeficit","g/㎥","gen_sender","gen_saver"]]
```

csv ファイルの内容は、以下のようになります

```bash:/boot/DATA/temp.csv
2016/11/18 23:02:08,23.7,000000004444903e
2016/11/18 23:07:06,23.4,000000004444903e
2016/11/18 23:09:23,23.4,000000004444903e
2016/11/18 23:10:05,23.4,000000004444903e
```

先頭から、データの取得日時、データ値、データを取得した Raspberry Pi のシリアルIDになります

slider では、Raspberry Pi のシリアルIDを下記コマンドで確認できます

```bash:
/home/pi/SCRIPT/slider/getserialnumber.sh
```

slider は config.toml で指定したセンサデータファイルがなければ自動で生成しますので、センサデータファイルの初期化は単にPC で削除するだけでかまいません。slider にログインして削除する場合は sudo が必要になります

```bash:
sudo rm /boot/DATA/temp.csv
```

###撮影画像の保存形式
撮影画像もセンサデータと同じ保存領域に以下のように保存されます

```bash:
video0
 |-20161118
 |  |-20161118231304.jpg
 |  |-20161118231405.jpg
...
video1
 |-20161118
 |  |-20161118231304.jpg
 |  |-20161118231405.jpg
...
```

video0, video1... 等、Raspberry Pi が存在を認識したカメラの番号毎にフォルダを作り、その下に年月日のフォルダを作り、撮影日時分秒 + .jpg のファイル名で保存します

Raspberry Pi が存在を認識しているカメラの一覧は下記コマンドで確認することができます

```bash:
ls /dev/video*
```

保存先フォルダの容量がいっぱいになると、slider は古い撮影画像を削除して領域を広げます。削除は年月日のフォルダ単位でおこないます

##<u>保存の抑止</u>
取得したセンサデータや撮影データをローカルに保存したくない場合は[センサデータの取得](read.md)で説明する config.toml ファイルの data 宣言の第４要素の **保存** の指定を "" にすることで保存の抑止が指定できます

```bash:config.toml
[sensors]
# data [[名前、単位、送信、保存],...]

  [sensors.dht22]
  data = [["temp","℃","gen_sender",""],
  				["humidity","%","gen_sender",""],
  				["humiditydeficit","g/㎥","gen_sender",""]]

          [imaging]
          # data [[イメージ種別、デバイス数、送信、保存],...]
          # イメージ種別 = [ "pic" | "movie" ]
          # デバイス数 = [ "one" | "all"]

          [imaging.uvc]
	        data =["pic","all","gen_pic_sender", ""]
```

##<u>保存先、保存方法の変更</u>
### gen_saver モジュールと gen_pic_saver モジュール

まず、config.toml ファイルでの保存の指定を再確認します。それはセンサーデータ、撮影画像共にdata宣言の第４要素に指定する、それぞれ gen_saver と gen_pic_saver でした

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

ここで、gen_saver と gen_pic_saver はそれぞれセンサーデータと画像の保存をおこなう save() というメソッドを持つ python のモジュールの名前です。slider は data 宣言の第４引数で指定される名前の saver モジュールのダイナミックインポートを行い、save()メソッドに必要なデータを渡し、保存処理を saver モジュールに委譲します

### 保存先の変更
センサデータの generic な saver モジュール **gen_saver** と、撮影画像の generic な saver モジュール **gen_pic_saver** の設定ファイルはそれぞれ **gen_saver.ini** と **gen_pic_saver.ini** になります

```/home/pi/SCRIPT/slider/gen_saver.ini
[save]
#gc16#data_path=/media/usb0
#gc15#data_path=/media/pi/158717B0012C7F831
data_path=/boot/DATA

[log]
log_file=/home/pi/LOG/gen_saver.log
```

```/home/pi/SCRIPT/slider/gen_saver.ini
[save]
#gc16#data_path=/media/usb0
#gc15#data_path=/media/pi/158717B0012C7F831
data_path=/boot/DATA

[log]
log_file=/home/pi/LOG/gen_pic_saver.log
```

[save] セクションの data_path がデータの保存先、[log] セクションの log_file が保存処理のログファイルの保存先になります

[save] セクションの data_path はデフォルトで拡張boot 領域の /boot/DATA が指定されていますが、この内容を /media フォルダに変更することで Raspberry Pi にオートマウントされる USB 外付けメモリの領域に変更することができます

オートマウンタ は FAT系の格ファイルシステム、Windows の NTFS、Mac の HSF+, Linux の ext系の格ファイルシステムに対応しています

一点、注意が必要なのは desktop を持つ Raspbian jessie をベースとする gc15 と、desktop を持たない Raspbian jessie-lite をベースとする gc16 では /media 以下のマウント先のフォルダ名が異なります
- gc15: 挿入された外部ストレージのパーティション名、無名の場合はuuid
- gc16: Raspberry Pi が認識した順番で usb0, usb1, ...

###　セキュアストレージへの保存
前述の保存先変更で、データをセキュアストレージに保存することで、SDカードの紛失から取得データをガードする事ができます

しかし、デフォルトの slider のセキュアストレージは、通常アプリケーション及び Web アプリケーションをタンパーからガードするために十分なサイズ（それぞれ256MByte）しか用意していないため、長期間の運営を前提とするデータの保存先としては十分なサイズではありません

データ保存用のセキュアストレージが必要な場合、別途その旨のご連絡をいただけますようお願いいたします

### 保存方法の変更
gen_saver, gen_pic_saver はそれぞれセンサーデータと画像の generic に定義された saver モジュールです。独自の saver モジュールを用意することで、例えば csv ファイルではなく sqlite3 の db に保存する等、保存方法を変更することができます

独自 saver モジュールの名前は generic な saver モジュールとおなじでなければどのような名前であってもかまいません

独自 saver モジュールは slider が検索できるように下記フォルダに .py か .pyc で保存してください

```bash:
/home/pi/SCRIPT/slider
```

独自saver には下記の save()メソッドを実装してください
slider は全てのパラメタを str型で渡します

```python:
def save(serialid, name, value):
    """センサーデータ
         serialid: Raspberry Pi のシリアルID
         name:  データ名
         value: データ値
    """
def save(serialid, device, picfilepath):
    """撮影画像
         serialid: Raspberry Pi のシリアルID
         device: カメラデバイス名 ex. "video0"
         pictfilepath: 撮影画像ファイル名をフルパスで
    """
```    
