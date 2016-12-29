# 簡易時刻設定

##<u>Raspberry Pi の時刻の問題</u>
Raspberry Pi は、電源がオフの状態でも内部バッテリーで現在時刻を保持し続ける Real Time Clock を内部にもたず、起動直後の内部の時計は SD カードに保存されている前回の終了時刻が設定されます

Raspberry Pi の時刻を合わせるためには、起動時に外付けの Real Time Clock モジュールから時刻情報を受け取るか、Network Time Protocol サーバから時刻情報を受け取ることで内部の時計の時刻をあわせます

それらの方法が使えない場合、Raspberry Pi の時計を手動で合わせる必要があります  
手動で Raspberry Pi の時計を操作するコマンドは以下になります

```bash:
# 現在時刻の確認
  date

# 時刻の設定
  sudo date -s hh:mm:ss

  # hh: 時
  # mm: 分
  # ss: 秒
```

date コマンドで時刻を設定することができますが、あまり便利ではありません

##<u>簡易時刻設定</u>
通常のスマフォやPCは Real Time Clock を持っているので、時刻が同期できています。この時刻を自動的に Raspberry Pi に通知して同期させることができると便利です

簡易時刻設定に Web でアクセスすると、Web クライアント側の時刻情報を自動的に slider に送信し、slider の時計を Web クライアントの時計と動機します
