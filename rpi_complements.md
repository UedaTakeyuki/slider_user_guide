# Raspberry Pi 補完機能

slider は Raspberry Pi を補完して情報システムの一部として利用する際に便利ないくつかの機能を用意しています

- [拡張boot領域](extended_boot.md)：  
Raspberry Pi の SD カードに PC から読み書きできるデータ領域を提供し、slider で取得したセンサデータや撮影画像を PC から読み書きできるようにし、slider の起動設定の一部を PC から編集可能にします
- [WiFiアクセスポイント設定](addwpa.md)：
WiFi のアクセスポイント情報を拡張 boot 領域のファイルに準備しておくことで、slider の起動時に自動的に そのアクセスポイントを slider から利用可能にします
- [Webインターフェース](webif.md)：  
slider 自身のサーバ機能を使い、いくつかの便利な Web サービス機能を提供します
  - [バックアップ](backup.md)：  
  他の Raspberry Pi の SD カードのバックアップ／リストアを簡単に行うことができます  
  - [簡易 monitor 表示](internal_monitor_view.md)：  
  [簡易 monitor server](internal_monitor_server.md) 機能を使い自身や他の slider から送信されたセンサデータや撮影画像を表示することができます
  - [簡易 スピーチ機能](speech.md)：
  slider に接続した phone スピーカーや、TV のスピーカーにテキストを送信して slider を喋らせる機能です
