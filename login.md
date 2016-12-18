# slider へのログイン

slider からのデータの取り出しや起動設定など日常の作業は Raspberry Pi 上で動作する slider の Linux にログインすることなしに、単に slider の SD カードを PC に挿入して PC だけで行うことが可能です  

しかし、slider への新規センサーモジュールの追加等、いくつかの設定は Raspberry Pi 上で動作する slider の Linux へのログインが必要になります

## slider へのログイン方法
slider へのログインには以下の方法があります

- [**Raspberry Pi 単体での login**](rpi_login.md): slider の動作する Raspberry Pi にキーボード、マウス及びディスプレイを装着して、Raspberry Pi 単体で入出力の手段を用意し、ログインする方法です
- [**PC からの login**](pc_login.md): PC のディスプレイとキーボードを使って slider にログインする方法です。PC と Raspberry Pi との接続方法によって [LANケーブル直結による login](direct_login.md)、[有線、無線によるネットワークを介した login](lan_login.md)、[シリアルケーブルを利用した login](serial_login.md) などの方法があります

## 起動シーケンスの確認
下記の接続方法は起動シーケンスが正常に終了した後に接続を開始します
- [LANケーブル直結による login](direct_login.md)
- [有線、無線によるネットワークを介した login](lan_login.md)、

<img src="pic/ss.2016-12-18 19.08.38.png" width="75%">

なんらかの起動の問題がある場合など、起動シーケンスのログを表示させる必要がある場合は、起動シーケンスの開始前から接続を行い、そのログの表示を行う下記の接続方法が必要です
- [Raspberry Pi 単体での login](rpi_login.md)
- [シリアルケーブルを利用した login](serial_login.md)
