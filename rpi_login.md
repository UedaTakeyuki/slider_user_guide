# Raspberry Pi 単体での login

slider の動作する Raspberry Pi にキーボード、マウス及びディスプレイを装着して、Raspberry Pi 単体で入出力の手段を用意し、ログインする方法です  

<img src="pic/ss.2016-12-18 18.53.51.png" width="75%">

ディスプレイ側の接続インターフェースには最近の TV や Mac などでよく利用される HDMI端子、古いアナログ TV でよく使われていたアナログの RCA端子, PC のディスプレイやプロジェクタでよく使われる DVI端子などが可能です
<img src="pic/ss.2016-12-18 19.08.17.png" width="75%">
つまり、最近の TV、今ではTVとして使えなくなった中古のアナログ TV、Mac や PC のディスプレイ、プロジェクタのどれでも Raspberry Pi のディスプレイとして利用可能です

この方法には以下の利点があります
- 準備が簡単：  
ネットワークの準備や設定なしで開始できます
- 起動シーケンスから確認できる：  
後述する[LANケーブル直結による login](direct_login.md)や、[有線、無線によるネットワークを介した login](lan_login.md)では、電源オンからネットワークを有効にするまでの間に設定ミスなどで起動シーケンスが止まってしまった場合に原因を確認する手段がありませ  
この方法では電源オンからのシーケンスが表示されるので、起動シーケンスが止まってしまう原因を確認できます
- phone jack スピーカーなしで音を出せる：
後述する[X window によるデスクトップ共有](chapter2.md) や [Remote Desktop Service によるデスクトップ共有](chapter2.md)のようなネットワークによるディスプレイの共有は基本的に画面を共有するだけで音声は共有しませんが、この方法では HDMI を使う場合でも RCA ケーブルを使う場合でも基本的に音声信号もディスプレイのスピーカーに接続します  

この方法には以下の欠点があります
- 必要な備品が多い：  
ディスプレイ、キーボード、マウス及びそれらのケーブルが必要になり、特にオフィスを離れた現場での作業の場合に持ち込む機材がおおくなる欠点があります  
特に屋外でも使用することができる小型・軽量・低消費電力のディスプレイがあまりないため、屋外での作業が難しくなります

これらの総合的に勘案してオフィスでの開発時、特に起動シーケンスの確認が必要になるような場合に向いてます