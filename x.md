# X window によるデスクトップ共有

Mac のように X Window をもつ PC から X のディスプレイ共有を利用して　slider に接続することができます

X 経由で slider に接続する場合は、下記のように -X オプションつきで[ssh 接続](ssh.md) をおこないます

```bash:
ssh -X pi@ホスト名.local
```

<img src="pic/ss.2016-12-22 14.47.01.png" width="80%">
<img src="pic/ss.2016-12-22 14.47.42.png" width="80%">

ログイン後、そのコンソールで lxsession コマンドを実行すると、slider の Linux OS のデスクトップが Mac のデスクトップの中に開かれます

<img src="pic/ss.2016-12-22 14.50.32.png" width="45%">

この接続方法では X window　をホストする PC のデスクトップと slider のデスクトップが完全に融合し、両者の間でテキストのコピー＆ペーストが可能になる等、使い勝手がよいです

この方法は　X window を持つ Mac の他、Cygwin 等で X Window をもつ WIndows PC、 Linux PC などで使うことができます
