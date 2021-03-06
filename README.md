# TTGO T-Camera ESP32 OV2640  
forked by Wataru KUNINO  

Wi-Fi ＋カメラ搭載ボード TTGO T-Camera ESP32 が、人体などの動きを検出すると、写真を撮影し、 Raspberry Pi へ保存します。

* Wi-Fi マイコン ESP32 と、JPEGカメラ OV2640 を搭載した TTGO T-Camera 用のファームウェアです。  
* TTGO T-Camera の人感センサが作動したときに UDP ブロードキャストを送信します。  
* UDP ブロードキャストを受けた Raspberry Pi などに撮影した写真を保存することが出来ます。  

STAモード、SSID=1234ABCD、パスワード=passwordを設定しました。  
その他の機能については TTGO-Camera-Series (原作者：lewisxhe)のものなどを流用しています。  
  
コンパイル済みファームウェアも提供しています。詳しくは、targetフォルダ内の[README.md](https://github.com/bokunimowakaru/iot-camera/blob/master/target/README.md)をご覧ください。  


## SSIDの変更方法

iot-camera.ino の下記の部分を変更してください。  

	/***************************************
	 *  WiFi
	 **************************************/
	#define WIFI_SSID   "1234ABCD"          // your wifi ssid
	#define WIFI_PASSWD "password"          // your wifi password


## コンパイル方法
Arduino IDEに、[arduino-esp32](https://github.com/espressif/arduino-esp32/releases)ならびに[esp8266-oled-ssd1306](https://github.com/ThingPulse/esp8266-oled-ssd1306) を組み込んで、コンパイルを行います。arduino-esp32のバージョンは 1.0.2 を使用しました。  
※ご注意：バージョン1.0.0には本カメラ用ライブラリが含まれていないので、動作しません。  

コンパイル時に必要なライブラリ：  
* arduino-esp32：https://github.com/espressif/arduino-esp32/releases
* esp8266-oled-ssd1306 https://github.com/ThingPulse/esp8266-oled-ssd1306

TTGO T-Cameraボードのバージョン（基板の裏面にシルク印字あり）に合わせて#defineの設定変更が必要です。

コンパイル時に必要なdefine設定：  
* #undef TTGO_OV2640_V16	// TTGO T-Camera OV2640_V05用(マイクロホンなし初期バージョン用)
* #define TTGO_OV2640_V16	// TTGO T-Camera OV2640_V1.6用(マイクロホン付きバージョン用)

Arduino IDEの[ツール]メニュー⇒[ボード]から、「ESP32 Wrover Module」を選択してください。  
PSRAMを使用しているので、PSRAMの有効／無効の設定が表示された場合は、必ず「有効」にしてください。  

* Arduino IDE：[ツール]⇒[ボード]⇒[ESP32 Wrover Module]
* PSRAM : Enable

## Raspberry Pi側の実行方法

同じWi-Fiに接続したRaspberry Piで下記のコマンドを実行するとサーバが起動します。

	cd
	git clone https://github.com/bokunimowakaru/iot-camera
	cd ~/iot-camera
	./iot-cam_serv.sh

サーバが起動した状態で、TTGO T-Camera ESP32のボタン（IO34またはRST）を押すと、カメラのIPアドレスがサーバへ送信され、以降、人感センサが反応するたびに、写真を撮影し、Raspberry Piへ保存します。

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

## このソースコードには、下記の製作物が含まれます。

* カメラ管理部：https://github.com/lewisxhe/esp32-camera-series  
* ボタン操作部：https://github.com/mathertel/OneButton  
* Wi-Fiカメラ部：https://github.com/espressif/arduino-esp32  

# ご注意・外部から不正に侵入されると、室内の様子などが流出してしまいます。

本ソフトウェアの配布にあたり、SSIDとパスワードを公開しています。Wi-Fiカメラが動作する周囲では、当該パスワードを利用することで、本カメラへアクセスすることが出来、実験中の室内の様子などが外部に流出してしまう場合があります。
また、うっかり電源を切り忘れたままにしてしまうと、常に室内の様子が閲覧可能な状態になってしまいます。十分にご注意ください。
玄関に設置した場合であっても、カメラが死角となる範囲の情報が(反射や影の映り込みなどによって)得られたり、撮影した写真を別の写真で上書きするといったことも可能なので、運用時はSSIDやパスワードの変更が必要です。

by <https://bokunimo.net>

--------------------------------------------------------------------------------
以下は、原作のREADME.mdです。  

--------------------------------------------------------------------------------
TTGO-Camera-Series
=====================

<img src="image/1.png" width="400" height="300">

- Now Arduino officially supports the camera, you need to update the Arduino to the latest, see [arduino-esp32](https://github.com/espressif/arduino-esp32/releases) for details.I am using the `1.0.1rc2` version when writing this code.
  
- In order to support BME280, I will use [Adafruit_BME280_Library](https://github.com/adafruit/Adafruit_BME280_Library), but this library conflicts with <esp_camera.h> and enters [Adafruit_BME280_Library](https://github.com/adafruit/Adafruit_BME280_Library) In Adafruit_BME280_Library change <Adafruit_BME280.h> --> 29 lines comment #include <Adafruit_Sensor.h>, BME280 this library does not use this header file, so comment out and no problem

- OLED requires [esp8266-oled-ssd1306](https://github.com/ThingPulse/esp8266-oled-ssd1306) library support
  
- Buttons require [OneButton](https://github.com/mathertel/OneButton) library support

## Board Modify
- The Camera version sold by TTGO will not have the BME280 sensor, because the temperature on the board affects the accuracy of the sensor. The default program does not enable the BME280 function. If necessary, turn on the `ENABLE_BME280` (on esp32-camera-bme280.ino line 12)
- Add OV2640 microphone version board support, need to enable `TTGO_OV2640_V16` macro for pin conversion, no microphone test, need microphone test please use [T-Camera](https://github.com/Xinyuan-LilyGO/T-Camera)




## TTGO CAM PINS
|  Name  | BME280/NoBME280-Version | Microphone-Version |
| :----: | :---------------------: | :----------------: |
|   Y9   |           39            |         36         |
|   Y8   |           36            |         15         |
|   Y7   |           23            |         12         |
|   Y6   |           18            |         39         |
|   Y5   |           15            |         35         |
|   Y4   |            4            |         14         |
|   Y3   |           14            |         13         |
|   Y2   |            5            |         34         |
|  VSNC  |           27            |         5          |
|  HREF  |           25            |         27         |
|  PCLK  |           19            |         25         |
|  PWD   |           26            |        N/A         |
|  XCLK  |           32            |         4          |
|  SIOD  |           13            |         18         |
|  SIOC  |           12            |         23         |
| RESET  |           N/A           |        N/A         |
|  SDA   |           21            |         21         |
|  SCL   |           22            |         22         |
| Button |           34            |         0          |
|  PIR   |           33            |         19         |

* BUTTON Click: Reverse camera ~~Currently the camera reverse color will not be normal, see [issues#9](https://github.com/espressif/esp32-camera/issues/9)~~

* BUTTON LongPress : Enter sleep Mode , ~~Sleep current 6.9mA~~
  
* RESET BUTTON: When the battery is powered, click to turn on the power

* PIR: Detecting human motion and will display the first screen
