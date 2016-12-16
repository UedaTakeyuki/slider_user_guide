# slider SD カードの構造

##<u>通常の Raspberry Pi の SD カード</u>
Raspberry Pi の SD カードは、Linux を起動するための小さな **boot領域** と、ほとんどを占める Linux の領域(EXT4領域)から構成されます

<img src="pic/ss.2016-12-16 15.07.10.png" width="60%">

この SD カードを PC の SDカードドライブや USB SDカードドライブに挿入した時に見えるのは **boot** 領域だけで、EXT4 の領域は PC からは見えません  
取得したセンサデータや撮影画像は PC からは見えず、PCに取り出すためには Raspberry Pi を起動してネットワークを経由して FTP 等でダウンロードする必要があります  


##<u>slider の SD カード</u>
slider ではこの boot 領域を **拡張** し、Linux 起動だけではなく取得したセンサデータや撮影画像の保存、頻繁な変更が必要な設定の窓口としても利用しています

<img src="pic/ss.2016-12-16 15.07.40.png" width="60%">

これによって slider では取得したデータを PC で直接操作したり、slider の基本的な設定起動に関する設定ファイルを PC で直接変更したりすることが可能です

<img src="pic/ss.2016-12-11 17.09.16.png" width="60%">

詳細は、[拡張boot領域](extended_boot.md) をご参照ください
