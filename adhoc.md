# addhoc mode と MANET(Mobile Adhoc NETwork)
## addhoc mode
slider を WiFi のアクセスポイントを利用 **しない** adhoc モードで利用することができます  
このモードに設定された複数の slider は直接に通信を行います

<img src="pic/ss.2016-12-16 20.29.29.png" width="50%">

##<u>起動設定</u>
slider を Adhoc Mode で動作させる設定は **拡張boot 領域** の **gc.ini** ファイルで下記２点の設定が必要です

1. network 設定に **adhoc** を選択します
2. adhoc_address に **172.24.1.255 サブネット内** の、互いに衝突しない IP address を設定します

```
# Access Point mode で起動する設定
#
# file: /boot/gc.ini

#network=pi
#network=wpa
network=adhoc
adhoc_address=172.24.1.3
```

上記の例では、adhoc_address に 172.24.1.3 を設定しています。おなじ adhoc network に参加させる別の slider は、**172.24.1.3** 以外の、例えば **172.24.1.4** や **172.24.1.5** が設定されている必要があります

まとめ：
- slider は **172.24.1.255** のネットワークで adhoc ネットワークを形成します
- adhoc ネットワークに参加する個々の slider は上記 subnet 内で一意な IP アドレスが **adhoc_address に設定されている必要** があります  

##<u>Adhoc Mode 対応 WiFi ドングル</u>
Adhoc Mode の機能を持っていて、かつ Raspberry Pi で利用できるドライバが公開されている WiFi デバイスで slider を Access Point Mode で起動させることができます。
現在、下記 WiFi でバイスで Access Point Mode での起動を確認しています
- Raspberry Pi 3 組み込み WiFi
- mac80211 ドライバを使う USB wifi ドングル  
Buffalo WLI-UC-GNM, Buffalo WLI-UC-GNM2 等  

現在、 下記の WiFi デバイスでは起動が **できていません**
- r8188eu ドライバ を利用する USB wifi ドングル  
PLANEX GW-USNANO2A 等

##<u>MANET(Mobile Adhoc NETwork)</u>
slider を adhoc mode で起動すると、自分から見える範囲の slider の情報を収集し、バケツリレーで届く範囲の全ての slider で共有することで、直接に通信をする事ができない slider 同士を別の slider が仲介してバケツリレーで通信を行うことを可能にします  

<img src="pic/ss.2016-12-07 18.51.12.png" width="50%">

このようにバケツリレーで通信を行うことを **マルチホップ通信** と呼び、自分からは直接見えない slider に情報を送りたい時、自分から見える誰に中継をゆだねれば良いかの情報を **経路情報** と呼びます  

自分から見える範囲の slider の情報を **定期的に収集** し、共有することで **経路情報** を **定期的に更新** する事で、個々のslider が移動する事で slide 間の関係が時間と共に変化しても、常に **誰に委ねれば良いのか** の情報を維持できるようにした **マルチホップ** のネットワークの事を
 **MANET(Mobile Adhoc NETwork)** と呼びます

slider は adhoc mode で起動すると同時に olsrd プロトコルで経路情報の作成と更新をおこない、参加する slider 全体で MANET を構築します  

## <u>MANETの利点</u>
MANET には、ネットワーク構造を事前の設計がなくても単に中継する slider を設置していくだけで自動的にネットワークを構築できるという柔軟な利点があります  
また、WiFi は見通しがきく場所では数百メートルの距離の伝搬も可能なの有線でネットワークを構築するよりも安価に簡単にネットワーク経路を構築できることがあります

MANET のネットワークノード間で自発的なネットワークを形成するという性質により、ネットワークプロバイダが課金しにくいという性質があるためこれまで商用サービスが提供されてこなかった経緯があります。しかし、ボランティアベースで一時的なネットワークを簡易に構築できる事で、これまで不可能であった多くの問題の解決の手段として利用していただけることを期待する次第です
