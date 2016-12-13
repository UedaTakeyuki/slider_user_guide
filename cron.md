# 処理間隔の設定

## <u>各種機能の処理間隔</u>
slider が定期的に繰り返す各種処理のデフォルトの間隔は以下のようになっています

- IoT 基本機能：　１分間隔
- FOTA機能：　５分間隔
- 状態通知機能：　５分間隔

## <u>処理間隔の設定変更</u>
slider SD カードを PC 等に挿すと "boot" という名前で認識されます  
その中の crontab.slider.txt で、これらの処理間隔は設定されており、このファイルを変更することで設定変更が可能です

/boot/crontab.slider.txt の内容は以下のようになっています

```bash:/boot/crontab.slider.txt
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').#
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
#
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
#
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
#
# For more information see the manual pages of crontab(5) and cron(8)
#
# m h  dom mon dow   command
# 分 時 日 月 曜 実行コマンド
*/5 * * * * python /home/pi/SCRIPT/fota.py
*/5 * * * * python /home/pi/SCRIPT/slider/statelog.py
*/1 * * * * sudo python /home/pi/SCRIPT/slider/read.py
```

下３行が、それぞれ下記機能の処理間隔設定になります

- FOTA機能：　fota
- 状態通知機能：　statelog
- IoT 基本機能：　read

設定ファイルの基本は以下になります

```
#分 時 日 月 曜日
*　 *  *  *  * 　　コマンド
```

最初の＊の変わりに＊/1 とすると、１分間隔でのコマンド実行になります  
同様に、＊/5 で５分間隔、＊/30 で 30分間隔になります
＊を付けずに 1 と書くと、毎時 1分（0時1分、1時1分、...）に起動されます  
同様に、0 で毎時0分、30 で毎時30分に起動されます
また、行の先頭に # を置いて行全体をコメントアウトすることで、コマンドを起動しないようにすることもできます  
ルールをまとめると以下のようになります

```
#分  時   日  月 曜日
*/1　*   *  *  * 　　１分間隔で起動
*/5　*   *  *  * 　　5分間隔で起動
0　  *   *  *  * 　　毎時0分に起動
30　 *   *  *  * 　　毎時30分に起動
#*　 *   *  *  * 　　起動しない

0　  */1 *  *  * 　　１時間間隔で0分に起動
0  　*/5 *  *  * 　　5時間間隔で0分に起動
0　  0   *  *  * 　　毎日0時0分に起動
*/1　7   *  *  * 　　毎日７時代に１分間隔で起動
```

##<u>crontab.slider.txt の仕組み</u>
前述の crontab.slider.txt の記述ルールは Linux の定時起動設定 crontab の記述ルールと同じです  
slider は起動時に crontab.slider.txt の内容でコマンドの定時起動設定を行います

##<u>起動間隔設定のヒント</u>
コマンドの終了前に同じコマンドが起動されると正常に終了せず、システムリソースが開放されずにシステムがフリーズする等の原因になることがあります  
センサーやカメラを大量に接続すると、全てのセンサーデータや撮影写真を保存して送信するのに数分の時間がかかることがあります  
そのように、デフォルトの1分間隔では IoT基本機能が終わらない場合は crontab.slider.txt の設定変更で間隔を長く調整してください
