# バックアップ

slider の Web Ui を使って他の Raspberry Pi の SD カードのバックアップ／リストアを簡単に行うことができます  
slider の SDカード２枚で、相互にバックアップを作成することができます

## <u>バックアップの手順</u>

### 準備
slider の USB ポートに バックアップを取得する SD カードをセットした USB SD カードリーダーを装着し、します

<img src="pic/ss.2016-12-19 18.08.37.png" width="30%">

### Web UI のオープン
slider に接続できる PC、タブレット、スマフォ等の browser で slider に接続します

<img src="pic/ss.2016-12-19 18.33.24.png" width="60%">

BackupPi_2 を開きます

### バックアップ
バックアップとして作成されるイメージファイル名を指定してバックアップを開始します

<img src="pic/ss.2016-12-19 18.32.36.png" width="30%">

デフォルトで日時.gz ファイルで作成されます  
ファイル名の拡張子は .img と .gz が可能です。.img の場合は非圧縮で、.gz の場合は gzip 形式で圧縮したイメージファイルを作成します  

バックアップ中は対象SDのどこまでバックアップが進んでいるか進捗状況を表示します

<img src="pic/ss.2016-12-17 11.07.26.png" width="30%">

終了すると「バックアップ完了」が表示されます

<img src="pic/ss.2016-12-17 12.39.36.png" width="30%">

以後、対象SDカードを抜いてもかまいません

### イメージファイルの作成先
イメージファイルは slider の拡張boot 領域の DATA フォルダ配下に作成されます

```
 /boot/DATA
```

作成したイメージファイルは一般にサイズが大きく拡張boot領域をすぐに使い切ってしまうので、適時 PC等で外部のストレージに移動させてください  

<img src="pic/ss.2016-12-11 17.09.16.png" width="60%">

### リストア
リストア対象のイメージファイルを slider の拡張boot 領域の DATA フォルダ配下に用意し、Web UI から BackupPi_2 を開き、リストアタブを選択します

<img src="pic/ss.2016-12-17 12.40.09.png" width="30%">

「復元ファイル選択」のプルダウンリストから、リストア対象のイメージファイルを選択してリストアを開始します  

<img src="pic/ss.2016-12-17 12.40.24.png" width="30%">
<img src="pic/ss.2016-12-17 12.41.12.png" width="30%">
<!-- <img src="pic/ss.2016-12-17 12.40.57.png" width="60%">
<img src="pic/ss.2016-12-17 12.41.33.png" width="60%"> -->