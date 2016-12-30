# 簡易 monitor server

slider 自身を monitor サービスのサーバとして機能させることができ、自分自身や他の slider から送信されたセンサデータや撮影画像を Web に表示させることができます

デフォルトでは、自分自身のデータを表示します

##<u>他の slider からのデータを表示する設定</u>
他の slider から送信されるデータを表示するためには以下の設定が必要です

1. 他の slider の [データの送信](send.md)先の設定を変更
2. 他の slider からのデータを保存する領域の準備

### 他の slider からのデータの保存領域

簡易 monitor のデータの保存領域は下記フォルダになります

```bash:
/var/www/html/SCRIPT/monitor/uploads
```

配下にデフォルトで 0000000000000000 フォルダと org フォルダがあります

0000000000000000 フォルダは自身のデータを受け取る場所、org は他 slider からのデータを受け取るフォルダの雛形になります

データを受け取る slider のシリアルIDを使い、下記のように org をコピーします

```bash:
sudo cp -r /var/www/html/SCRIPT/monitor/uploads/org /var/www/html/SCRIPT/monitor/uploads/シリアルID
```

slider で下記コマンドを実行することで自身のシリアルIDを確認できます

```bash:
/home/pi/SCRIPT/slider/getserialnumber.sh
```
