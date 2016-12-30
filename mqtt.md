# MQTT Broker

slider は MQTT Broker として機能します

##<u>例</u>

```bash:
# サブスクリプション
mosquitto_sub -h ホスト名.local -t "#"
```

```bash:
# パブリケーション
mosquitto_pub -h ホスト名.local -t "topic" -m メッセージ
```

##<u>設定</u>
```bash:
# 起動時の自動起動のオフ
sudo systemctl disable mosquitto
```

```bash:
# 起動時の自動起動のオン（デフォルト）
sudo systemctl enable mosquitto
```

```bash:
# MQTT サービスの停止
sudo systemctl stop mosquitto
```

```bash:
# MQTT サービスの開始
sudo systemctl start mosquitto
```

##<u>websocket</u>
javascript からは port 9001 で websocket に接続できます
