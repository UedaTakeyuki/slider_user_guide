# Access Point mode
slider を WiFi のアクセスポイントにして利用することができます  
このモードで起動すると slider は事前に登録された WiFi アクセスポイントに接続 **するのではなく**、自分自身をアクセスポイントとして下記の WiFi ネットワークを作ります

```bash:
# slider がつくるアクセスポイントの接続情報
  SSID: pi
  PSK:  Raspberry
```

PCやスマフォなどで上記アクセスポイントに接続すると通常のアクセスポイント同様に slider に接続する事ができる他、アクセスポイントに接続している機器間での通信も可能です

<img src="pic/ss.2016-12-07 18.51.04.png" width="50%">

このアクセスポイントに別の slider を参加させて独立したサーバとして利用することも可能です

<img src="pic/ss.2016-12-07 18.50.46.png" width="50%">

##<u>起動設定</u>
slider を Access Point Mode で動作させる設定は **拡張boot 領域** の **gc.ini** ファイルで、network 設定に **pi** を選択することでおこないます

```
# Access Point mode で起動する設定
#
# file: /boot/gc.ini

network=pi
#network=wpa
#network=adhoc
#adhoc_address=172.24.1.3
```

##<u>Access Point Mode 対応 WiFi ドングル</u>
Access Point Mode の機能を持っていて、かつ Raspberry Pi で利用できるドライバが公開されている WiFi デバイスで slider を Access Point Mode で起動させることができます。
現在、下記 WiFi でバイスで Access Point Mode での起動を確認しています
- Raspberry Pi 3 組み込み WiFi
- mac80211 ドライバを使う USB wifi ドングル  
Buffalo WLI-UC-GNM, Buffalo WLI-UC-GNM2 等
- r8188eu ドライバ を利用する USB wifi ドングル  
PLANEX GW-USNANO2A 等

##<u>dhcp serverの設定</u>
Access Point Mode で動作する slider は dhcp server として接続してきた機器に対して下記レンジの IP address を配布します

```
# レンジ　172.24.1.50 - 172.24.1.150
```
リース時間は 12h です  

slider 自身は自身の wlan0 に IP address 172.24.1.0 を設定します
