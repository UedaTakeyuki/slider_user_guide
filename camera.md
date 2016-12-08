# 多連装カメラ

slider では、Raspberry Pi 標準のカメラモジュール、及び USB 接続の WebCam を利用することができます。標準カメラモジュールと WebCam を同時に使うことも、また WebCam は USB の制限内で何台でも接続することが可能で、それら全てのカメラから順番に画像を取得します。同時にではなく順番に取得するのは、消費電力やデータバスの物理的な限界を避けるためです

slider では 一個数百円の安価な WebCam と、メートル100円程度の安価な USB extension cable を数十個接続することで、**市販のネットワークカメラ１台程度の安価な値段** で **数十箇所を広範に監視するシステム** を実現することが可能となります

## <u>標準IoT機能の設定ファイル</u>

### config.toml ファイル
カメラの設定もセンサーと同じ config.toml ファイルで設定します

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
[imaging]
# data [[イメージ種別、デバイス数、送信、保存],...]
# イメージ種別 = [ "pic" | "movie" ]
# デバイス数 = [ "one" | "all"]

	[imaging.uvc]
	data =["pic","all","gen_pic_sender", "gen_pic_saver"]
```

現在、イメージ種別の"movie" とデバイス数の "one"は未実装です

gen_pic_sender と gen_pic_saver は撮影画像について slider が用意する標準的な送信モジュールと保存モジュールのファイル名です。この部分を空白にすることで送信や保存をおこなわないようにすることができます

gen_pic_sender と gen_pic_saver の内部の詳細は後ほど[データの送信](chapter2.md)と[データの保存](chapter2.md)でご説明させていただきます
