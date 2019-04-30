# TTGO T-Camera ESP32  
forked by Wataru KUNINO  

TTGO T-Camera�̐l���Z���T���쓮�����Ƃ���UDP�u���[�h�L���X�g�𑗐M���܂��B  
STA���[�h�ASSID=1234ABCD�A�p�X���[�h=password��ݒ肵�܂����B  
���̑��̋@�\�ɂ��Ă͂� TTGO-Camera-Series (����ҁFlewisxhe)�̂��̂Ȃǂ𗬗p���Ă��܂��B

## SSID�̕ύX���@

esp32-camera-series.ino �̉��L�̕�����ύX���Ă��������B  

	/***************************************
	 *  WiFi
	 **************************************/
	#define WIFI_SSID   "1234ABCD"          // your wifi ssid
	#define WIFI_PASSWD "password"          // your wifi password


## �R���p�C�����@
Arduino IDE�ɁA[arduino-esp32](https://github.com/espressif/arduino-esp32/releases)�Ȃ�т�[esp8266-oled-ssd1306](https://github.com/ThingPulse/esp8266-oled-ssd1306) ��g�ݍ���ŁA�R���p�C�����s���܂��Barduino-esp32�̂� 1.0.2 ���g�p���܂����B  
�������ӁF�o�[�W����1.0.0�ɂ͖{�J�����p���C�u�������܂܂�Ă��Ȃ��̂ŁA���삵�܂���B  

�R���p�C�����ɕK�v�ȃ��C�u�����F  
* arduino-esp32�Fhttps://github.com/espressif/arduino-esp32/releases
* esp8266-oled-ssd1306 https://github.com/ThingPulse/esp8266-oled-ssd1306

Arduino IDE��[�c�[��]���j���[��[�{�[�h]����A�uESP32 Wrover Module�v��I�����Ă��������B  
PSRAM���g�p���Ă���̂ŁAPSRAM�̗L���^�����̐ݒ肪�\�����ꂽ�ꍇ�́A�K���u�L���v�ɂ��Ă��������B  

* Arduino IDE�F[�c�[��]��[�{�[�h]��[ESP32 Wrover Module]
* PSRAM : Enable

## �֘A���

* [2000�~��TTGO T-Camera�FWi-Fi�{�L�p�����Y�t�J�����{OLED�{�l���Z���T](https://bokunimo.net/blog/esp/12/)(�����u���O)
* ���i�̔̔��T�C�g�F<https://www.aliexpress.com/item//32968683765.html>
* �̔���GitHub�F<https://github.com/Xinyuan-LilyGO/esp32-camera-bme280>
* �J����GitHub�F<https://github.com/lewisxhe/esp32-camera-series>

## ���̃\�[�X�R�[�h�ɂ́A���L�̐��앨���܂܂�܂��B
* �J�����Ǘ����Fhttps://github.com/lewisxhe/esp32-camera-series  
* �{�^�����암�Fhttps://github.com/mathertel/OneButton  
* Wi-Fi�J�������Fhttps://github.com/espressif/arduino-esp32  

by <https://bokunimo.net>

--------------------------------------------------------------------------------
�ȉ��́A�����README.md�ł��B  

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