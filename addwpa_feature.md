# WiFi アクセスポイントへの簡単登録

## <u>Raspberr Pi に WiFi アクセスポイントを設定する際の問題</u>

WiFi を利用する機器は一般にアクセスポイント情報の登録が必要です。
正当な権限のないユーザによる悪用を防ぐために WiFi アクセスポイントは一般にパスコードで守られています。
アクセスポイントに接続するために、機器はアクセスポイントの名前(SSID)とパスコード（psk）を知っている必要があります。

PC やスマフォは単にキーボードから設定すればよいのですが、そもそも **WiFi に接続して使う事を前提** として **キーボードもディスプレイも持たない Raspberry Pi** では **その手段がありません**。

Raspberry Pi に WiFi のアクセスポイント情報を登録する手段としては以下の方法がありますが、

- ディスプレイとキーボードを接続してディスクトップアプリやコンソールコマンドで登録する
- PC と LANでの直結、もしくは USB-Serial コンバータで直結し、コンソールを開いて登録する
- EXT4 file system のドライバをインストールしてある Mac で (Windows は SDカードの先頭のパーティションしか認識しない)　Raspberry Pi の SD カードを開き、wpa_supplicant.conf ファイルを直接編集する

これらはあまり簡単でとは言い難く、特に **必要な機材のない出先** や **作業現場などの屋外** では大変な不便を生じます。

## <u>slider の解決策</u>

この問題を解決するために、slider では [addwpa](https://github.com/UedaTakeyuki/addwpa)という、Raspberry Pi 自身に WiFi アクセスポイントを設定させる仕組みを用意しました。
slider の SD カードに PC 等でアクセスポイントの SSID と psk を書いておけば、初回の起動時に自動的にアクセスポイント情報の設定を行います。
設定方法は[WiFi アクセスポイントの設定](addwpa_howto.md)をご参照ください。

## <u>設定方法</u>
slider SD カードを PC 等に挿すと "boot" という名前で認識されます
ここに登録したいアクセスポイントの SSID と psk のみの２業だけのテキストファイルを addwpa.txt という名前で作ります

```bash:/boot/addwpa.txt
SSID
psk
```

例えば、SSID が "Buffalo-G-E854"、psk が　"ppppjjjj333tj"の場合、addwpa.txt は以下のようになります

```bash:/boot/addwpa.txt
Buffalo-G-E854
ppppjjjj333tj
```

このSDカードで Raspberry Pi を起動すると、起動時の最初にまずこのアクセスポイント情報を登録してから、アクセスポイントの検索を始めます

アクセスポイントの登録に成功すると addwpa.txt ファイルは自動的に削除されますので、アクセスポイントの登録情報がいつまでも PC から見える場所に残る心配はありません

## <u>参考：Linux の WiFi アクセスポイント情報の管理</u>
Linux では、WiFi のアクセスポイントへの接続は wpa_supplicant というプログラムで行われます。アクセスポイント情報は /etc/wpa_supplicant/wpa_supplicant.conf というテキストファイルに以下のような形式で保存されます

```bash:/etc/wpa_supplicant/wpa_supplicant.conf
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=GB

network={
	ssid="Buffalo-G-E854"
	psk="ppppjjjj333tj"
}

network={
	ssid="MNG6200-49C2-G"
	psk="qap85j46323hk"
}
```

slider の addwpa は、起動時の最初に addwpa.txt ファイルを読んで、/etc/wpa_supplicant/wpa_supplicant.conf に追加することで、WiFi アクセスポイント情報の登録を実現しています
