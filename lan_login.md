# 有線、無線によるネットワークを介した login
##<u>接続方法</u>
有線、無線等のネットワークに slider と PC を参加させる方法です  
slider の Eathernet Port は DHCP で IP アドレスやゲートウェイの情報を取得します  
slider の WiFi はアクセスポイントに接続して IP アドレスやゲートウェイの情報を取得する [Access Point mode](ap.md) と、固定IPアドレスでアドホックネットワークに参加する [addhoc mode と MANET(Mobile Adhoc NETwork)](adhoc.md) が可能です  
また、slider への WiFi アクセスポイント情報の登録には [WiFiアクセスポイント設定](addwpa.md) 機能が利用できます


##<u>ログインの手段</u>
下記全の手段が利用可能です

- [ssh 接続](ssh.md)
-	[X window によるデスクトップ共有](x.md)
-	[Remote Desktop Service によるデスクトップ共有](remotedesktop.md)

##<u>ホスト名の指定</u>
Apple bonjour プロトコルがインストールされた PC （Mac, Windows10 及び iTune のインストールされた Windows）では **mDNS** プロトコルで名前の解決ができるので、下記のように **ホスト名.local** で接続できます

```bash:
   ssh pi@gc15.local # gc15 の場合
   ssh pi@gc16.local # gc16 の場合
```

それ以外の PC の場合、APIPA で割り振られた IP Address を直接指定して接続します

```bash:
   ssh pi@169.254.167.131
```

[データの表示](show.md) を有効にしておくと、slider は APIPA で割り振られた IP アドレスを表示します

<img src="pic/ss.2016-12-16 14.43.45.png" width="75%">
