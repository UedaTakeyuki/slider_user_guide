# LANケーブル直結による login
##<u>接続方法</u>
LAN ケーブルで PC と直結させる方法です

<img src="pic/ss.2016-12-16 14.43.45.png" width="75%">

Raspberry Pi の Eathernet Port は LAN ケーブルのストレート/クロスを認識して自動的に切り替える **Auto-MDIX** を備えているのでケーブルはストレートでもクロスでもどちらでもかまいません  

この接続方法では Raspberry Pi と PC がお互いに調整し、169.254.255.255 のサブネット内の衝突しない IP アドレスを自動的に割り当てる **APIPA（Automatic Private IP Addressing)** と呼ばれる **zeroconf** のプロトコルによって作成された一時的なネットワークを使って両者の通信が可能になります  

##<u>ログインの手段</u>
この方法は APIPA で作成された一時的なネットワークを使って通信を行うことを除けば、普通に管理されたネットワークを経由して通信を行う[有線、無線によるネットワークを介した login](lan_login.md)と技術的には全く同じです   
そのため、[有線、無線によるネットワークを介した login](lan_login.md)同様に下記全ての手段が利用可能です

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
