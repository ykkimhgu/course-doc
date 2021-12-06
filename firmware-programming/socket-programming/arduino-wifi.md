# Arduino WiFi



## Add WiFi(ESP8266) to Arduino UNO

{% embed url="https://it-g-house.tistory.com/entry/%EC%95%84%EB%91%90%EC%9D%B4%EB%85%B8Arduino-%EC%9D%B8%ED%84%B0%EB%84%B7-%ED%95%98%EA%B8%B0-Wifi-ESP-01%EC%97%B0%EA%B2%B0%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95" %}

If ESP-01 Adapter is used, Connect VCC=5V.&#x20;

If ESP9266 module is used, Connect VCC=3.3V

![](https://blog.kakaocdn.net/dn/I8PbU/btqzRMPhoso/OIRORf9Ke8lYQhLmKKi9NK/img.png)

[아두이노(Arduino) 인터넷 연결하기: ESP-01(ESP8266) 와이파이 모듈(Wifi module) 어댑터 배선](https://it-g-house.tistory.com/entry/%EC%95%84%EB%91%90%EC%9D%B4%EB%85%B8Arduino-%EC%9D%B8%ED%84%B0%EB%84%B7-%ED%95%98%EA%B8%B0-Wifi-ESP-01%EC%97%B0%EA%B2%B0%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95)\
\


### Arduino sketch code&#x20;

```c
#include <SoftwareSerial.h>

SoftwareSerial ESPserial(2, 3); // RX | TX

void setup()

{

//Serial.begin(115200); // communication with the host computer
Serial.begin(9600); // communication with the host computer

//while (!Serial) { ; }

// Start the software serial for communication with the ESP8266

ESPserial.begin(115200);
//ESPserial.begin(9600); // communication with the host computer

Serial.println("");

Serial.println("Remember to to set Both NL & CR in the serial monitor.");

Serial.println("Ready");

Serial.println("");

}

void loop()

{

// listen for communication from the ESP8266 and then write it to the serial monitor

if ( ESPserial.available() ) { 
  //Serial.println("received"); 
  Serial.write( ESPserial.read() ); }

// listen for user input and send it to the ESP8266

if ( Serial.available() ) { 
  //Serial.println("read");
  ESPserial.write( Serial.read() ); }

}
```



First use UNO-PC Serial Baud 9600, UNO-ESP Serial Baud 115200

In Serial Monitor (Baud 9600) : Type

```
AT                          // should get OK response
AT+UART_DEF=9600,8,1,0,0    // to change UNO-ESP baudrate to 9600*
```

Now, we have changed ESP8266 Serial to 9600.

Modify the Arduino Sketch to `ESPserial.begin(115200);` and upload again.

```
AT+CWMODE?                  // Current Connection Mode
AT+CWMODE=1                 // Change Connection Mode to Mode=1 (Station mode)
AT+CWLAP                    // Show List of Avaiable WiFi
AT+CWJAP="SSID","비밀번호" // Connect with SSID and Password
AT+CIFSR                    // Check IP and MAC
```

ESP8266 와이파이 모듈은 네트워크 연결에 대해 3가지 모드를 제공합니다.

1. Station mode: ESP8266 모듈이 client로 wifi 기능만 함 (본 예제에서 모드 1 사용)
2. AP mode: ESP8266 모듈이 Access Point가 됨
3.  AP + Station mode: AP와 client 둘 다 됨

    \


![](<../../.gitbook/assets/image (115) (1).png>)

### Reference

{% embed url="https://controllerstech.com/esp8266-webserver-using-stm32-hal" %}

{% embed url="https://blog.naver.com/PostView.nhn?blogId=chcbaram&categoryNo=43&from=search&isShowPopularPosts=true&logNo=222279235752&parentCategoryNo=&viewDate=" %}

{% embed url="http://stm32f4-discovery.net/2015/02/library-52-ethernet-peripheral-on-stm32f4xx" %}

{% embed url="https://www.arduino.cc/en/Reference/WiFi" %}

{% embed url="https://www.instructables.com/Add-WiFi-to-Arduino-UNO" %}

{% embed url="https://github.com/imjeffparedes/iot-esp8266-arduino-interface" %}
