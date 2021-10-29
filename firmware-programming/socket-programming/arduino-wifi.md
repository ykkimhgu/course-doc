# Arduino WiFi



## Add WiFi(ESP8266) to Arduino UNO

{% embed url="https://it-g-house.tistory.com/entry/%EC%95%84%EB%91%90%EC%9D%B4%EB%85%B8Arduino-%EC%9D%B8%ED%84%B0%EB%84%B7-%ED%95%98%EA%B8%B0-Wifi-ESP-01%EC%97%B0%EA%B2%B0%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95" %}

If ESP-01 Adapter is used, Connect VCC=5V.&#x20;

If ESP9266 module is used, Connect VCC=3.3V

###

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

//ESPserial.begin(115200);
ESPserial.begin(9600); // communication with the host computer

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



{% embed url="https://create.arduino.cc/projecthub/imjeffparedes/add-wifi-to-arduino-uno-663b9e" %}

### Reference

{% embed url="https://www.arduino.cc/en/Reference/WiFi" %}

{% embed url="https://www.instructables.com/Add-WiFi-to-Arduino-UNO" %}

{% embed url="https://github.com/imjeffparedes/iot-esp8266-arduino-interface" %}
