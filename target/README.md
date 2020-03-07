# 【コンパイル済みファームウェア】TTGO T-Camera ESP32 OV2640  
forked by Wataru KUNINO  

Wi-Fi＋カメラ搭載ボードTTGO T-Camera ESP32が、人体などの動きを検出すると、Raspberry Piへ写真を保存します。

* Wi-Fi マイコンESP32と、JPEGカメラPV2640を搭載したTTGO T-Camera用のファームウェアです。  

STAモード、SSID=1234ABCD、パスワード=passwordを設定しました。  
その他の機能については TTGO-Camera-Series (原作者：lewisxhe)のものなどを流用しています。

## コンパイル済みファームウェアの種類

以下の2種類があります。TTGO T-Cameraボードのバージョン（基板の裏面にシルク印字あり）に合わせて使用してください。

* iot-camera_V05.ino.bin: TTGO T-Camera OV2640_V05用(マイクロホンなし初期バージョン用)
* iot-camera_V16.ino.bin: TTGO T-Camera OV2640_V1.6用(マイクロホン付きバージョン用)

## ファームウェアの書き込み方法

下記の IoT Sensor Core の情報を参考にしてください（iot-sensor-coreの部分をiot-cameraに読み替えてください。）  

* <https://github.com/bokunimowakaru/sens/blob/master/README.pdf>

## SSIDの変更方法

ソースコードの変更が必要です。一つ上のディレクトリ内の[README.md](https://github.com/bokunimowakaru/iot-camera/blob/master/README.md)をご覧ください。

## M5Stackの液晶ディスプレイへ表示する

Wi-Fiカメラからの画像をM5Stackへ表示することが出来るフォトフレーム用ソフトも公開中です。  

TTGO T-Camera+M5StackでWi-Fiカメラ  
* ブログ記事：https://bokunimo.net/blog/esp/420/
* レポジトリ：https://github.com/bokunimowakaru/iot-photo

## 関連情報

* [2000円のTTGO T-Camera：Wi-Fi＋広角レンズ付カメラ＋OLED＋人感センサ](https://bokunimo.net/blog/esp/12/)(当方ブログ)
* 製品の販売サイト：<https://www.aliexpress.com/item//32968683765.html>
* 販売者GitHub：<https://github.com/Xinyuan-LilyGO/esp32-camera-bme280>
* 開発者GitHub：<https://github.com/lewisxhe/esp32-camera-series>

## このファームウェアには、下記の製作物が含まれます。

* カメラ管理部：https://github.com/lewisxhe/esp32-camera-series  
* ボタン操作部：https://github.com/mathertel/OneButton  
* Wi-Fiカメラ部：https://github.com/espressif/arduino-esp32  

## 本ファームウェアをコンパイルしたときの条件

### 最新版コンパイル日

* 2020年3月7日

### IDEおよびライブラリ

* Arduino IDE 1.8.9  
* arduino-esp32 1.0.4  
* esp8266-oled-ssd1306: 3398c97

### ESP32ボード設定

* Board: ESP32 Wrover Module
* Flash Frequency: 80MHz
* Flash Mode: QIO
* Partition Scheme: Default 4MB with spiffs (1.2MB APP/1.5MB SPIFFS)
* Core Debug Level: None  

### define設定

* define ENABLE_SSD1306
* (undef) //define SOFTAP_MODE                  // STAモード用
* (undef) //define ENABLE_BME280                // 環境センサBME280用
* define ENABLE_SLEEP
* define ENABLE_IP5306

* define TTGO_OV2640_V16 // OV2640_V1.6用 ※ V05のときはundef

* define SSID "1234ABCD"  
* define PASS "password"  
* define SENDTO "255.255.255.255"            // 送信先のIPアドレス
* define PORT 1024                           // 送信のポート番号

* define DEVICE_PIR "pir_s_5,"               // デバイス名(人感センサ)
* define DEVICE_CAM "cam_a_5,"               // デバイス名(カメラ)

## 注意事項

一つ上のフォルダ内の[README.md](https://github.com/bokunimowakaru/iot-camera/blob/master/README.md)の注意事項をよく読んでからご利用ください。

by <https://bokunimo.net>
