# ネットワーク機能

slider は通常のネットワーク環境の中で使う他に、ネットワークの存在しない環境で一台のもしくは複数の slider を組み合わせた簡易ネットワークを構築して利用したり、軽量なプロトコルを利用して通信したり等、ネットワークに関するいろいろな機能を備えています。この節ではそれらについて詳細に説明します

##<u> [Access Point mode](ap.md)</u>
slider を WiFi のアクセスポイントにして使用する [Access Point mode](ap.md)が利用できます  
Raspberry Pi をいろいろな場所で、そこのネットワークに参加させて利用するためにはその都度、煩雑な登録が必要となるので、むしろ Raspberry Pi 自身にアクセスポイント機能をもたせたほうが便利なことがあります

そのようなときに slider の[Access Point mode](ap.md)を利用すると便利です

##<u>[addhoc mode と MANET(Mobile Adhoc NETwork)](adhoc.md)</u>
slider はアクセスポイントを利用せず、個々の Raspberry Pi 同士で **相互に通信** させる WiFi の adhoc モードを利用することができます

このモードに設定された複数の slider は直接に通信を行うだけではなく、どの slider が他のどの slider と直接に繋がっているかの情報をバケツリレーで接続した全体の slider で共有し、直接に接続されていない slider に対してもマルチホップで情報を伝達させるメッシュネットを形成します

詳細は[addhoc mode と MANET(Mobile Adhoc NETwork)](adhoc.md)を参照してください

##<u>[簡易 monitor 機能](internal_monitor.md)</u>
外部の monitor サービスが利用できない場合でも、slider は自身のサーバ機能をつかって[簡易 monitor 機能](internal_monitor.md)を提供し、自身や他の slider が取得したセンサデータや撮影画像を表示することができます

前述の[Access Point mode](ap.md)や[addhoc mode と MANET(Mobile Adhoc NETwork)](adhoc.md)のslider の一台を[簡易 monitor 機能](internal_monitor.md)としたり、または[Access Point mode](ap.md)に通常接続する siider を[簡易 monitor 機能](internal_monitor.md)とすることで、外部ネットワークの monitor サービスをつかうことなく、slider に閉じたネットワーク内で他のPCやスマフォ等にデータや画像を表示させることができます

##<u> [MQTT Brocker の提供](mqtt.md)</u>
slider は http ベースの通信の他に軽量な通信プロトコルである mqtt ベースの通信も提供します
詳細は [MQTT Brocker の提供](mqtt.md) をご参照ください
