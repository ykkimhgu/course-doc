# Tutorial: arduino-stm32 Part3

## Tutorial: arduino-stm32 Part3

##

### **U(S)ART (Universal Asynchronous/synchronous Receiver and Transmitter)**

We are going to create a simple program that links MCU-PC via UART communication. MCU can receive and transmit 8-bit character data through UART communication.

NUCLEO-F401RE board offers UART2 channel with USB connector.

#### [Serial class](https://www.arduino.cc/reference/en/language/functions/communication/serial/)

> Used for communication between the board and a computer or other devices.

```cpp
Serial.begin(baudrate); // Initialize the serial port.
Serial.available();     // Get the number of bytes (characters) available for reading from the serial port
Serial.read();          // Reads incoming serial data.
Serial.print(text);     // Prints data to the serial port as human-readable ASCII text.
```

* baudrate : the data rate in bits per second (9600, ...)

Create a new program named as ‘**TU\_arduino\_UART**’.

Write the following source code.

#### Example 1: Print the Input Key

```cpp
char keyIn;

void setup() {
  Serial.begin(9600);
  Serial.print("Hello Nucleo\r\n");
}

void loop() {
  if (Serial.available() > 0){
    keyIn = Serial.read();
    if (keyIn == '\n')
      Serial.print("\r\n");
    else if (keyIn)
      Serial.print(keyIn);
  }
}
```

#### Example 2: Printing every 3 seconds

```cpp
#include "STM32TimerInterrupt.h"
#include "STM32_ISR_Timer.h"

#define HW_TIMER_INTERVAL_MS  100
#define TIMER1_INTERVAL       1000L
#define TIMER2_INTERVAL       3000L

STM32Timer ITimer(TIM1);

STM32_ISR_Timer ISR_Timer1;
STM32_ISR_Timer ISR_Timer2;

const int ledPin = 13;
int bPrint = 0;

void setup() {
  Serial.begin(9600);
  Serial.print("Hello Nucleo\r\n");

  pinMode(ledPin, OUTPUT);
  
  ITimer.attachInterruptInterval(HW_TIMER_INTERVAL_MS * 1000, TimerHandler);

  ISR_Timer1.setInterval(TIMER1_INTERVAL, ledInterrupt);
  ISR_Timer2.setInterval(TIMER2_INTERVAL, printInterrupt);
}

void loop() {

  if (bPrint){
    Serial.print("LED = ");
    Serial.println(digitalRead(ledPin));
    bPrint = 0;
  }
}

void TimerHandler(){
  ISR_Timer1.run();
  ISR_Timer2.run();
}

void ledInterrupt(){
  digitalWrite(ledPin, !digitalRead(ledPin));
}

void printInterrupt(){
  bPrint = 1;
}
```

Click on **Upload** button.

#### Installing Serial Monitor

[Download 'TeraTerm'](https://osdn.net/projects/ttssh2/releases/)

Open ‘Tera Term’ . and make New Connection.

Choose ‘**Serial**’ tab -> Select ‘**COMx: STMicroelectronics STLink**’ port

> COMx, x is the port number and it can be different for each connection.

![teraterm](https://user-images.githubusercontent.com/79825525/129156752-893e425d-1653-496f-a4fa-13cbebe2a271.png)

​ Open Serial port(시리얼 포트) in Setup(설정) tab, check if the baud rate is set as **9600** \[bps].

![teraterm2](https://user-images.githubusercontent.com/79825525/129156774-2bfe2509-d5e2-4ba1-b3bc-b06d53dacd52.png)

Press the reset button(black) and verify the operation. If you put any letter in Tera Term, MCU will receive it and transmit it to PC immediately, so you can see the pushed letters showed in Tera Term.

#### Exercise

**Object Proximity Detection**

* Print only when it detects the presence of an object.
* Use your hand as the object and put it near the sensor
  * It should print " **Warning! An object is too close**"
* Do not print anything when the object is not near.

The experiment kit has an _IR proximal sensor_ that can detect the presence of an object.

* It is connected at `PinName D4` DigitalIn

![IR proximity sensor (NS-IRPSM)](https://user-images.githubusercontent.com/91526930/186362637-4f27b88f-52fa-457c-b3a9-5c1e0dfed238.png)

### Timer

We are going to create a simple program that measures the time to count 100 starting from 0. Print the result through UART communication.

#### Example: time measure

Create a new program named as ‘**TU\_arduino\_Timer**’.

Write the following source code.

```cpp
int cnt = 0;
unsigned long beginTime, endTime;

void setup() {
  Serial.begin(9600);
  Serial.print("Program START\r\n");
}

void loop() {
  cnt = 0;
  beginTime = micros();
  while(cnt < 100) 
    cnt ++;
  endTime = micros();

  Serial.printf("Counting %d takes %d [us]\r\n", cnt, endTime - beginTime);
  delay(500);
}
```

Click on **Upload** button.

Open ‘Tera Term’ and make New Connection.

![teraterm result](https://user-images.githubusercontent.com/79825525/129156799-0f1e7781-71f9-4294-94e3-6304c150d7e5.png)

Push the reset button(black), and verify the time taken in counting 100.

You can measure time taken in any other processes like toggling LED, multiplication or division, etc. If the process takes long time, you can also measure time in \[ms] unit using ‘millis()’ command.

#### Exercise

**Time delay polling**

* Create a time delay function named as `delaycnt(uint cnt)`
* It receives the number of counts that it should wait.
* **cnt** should be from 0\~ 255
*   Print how many micro-seconds it took during the waiting

    * It should print " **Counting 000 took 00 us**"

    ​

### PWM (**Pulse Width Modulation**) Ultra Sonic Sensor

We are going to create a simple program that measure distance by using ultrasonic sensor ‘HC-SR04’ and print out result through UART communication.

#### Example: ultrasonic sensor distance

Create a new program named as ‘**TU\_mbed\_PWM1**’.

Write the following source code.

```cpp
const int trigPin = 10;   // Trigger pin : PWM out
const int echoPin = 7;    // Echo pin : Interrupt in

unsigned long duration;
float distance;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  
  Serial.begin(9600);
}

void loop() {
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  delayMicroseconds(10);

  duration = pulseIn(echoPin, HIGH);
  distance = (float)duration / 58.0;
  
  Serial.printf("duration : %d Distance : ", duration);
  Serial.print(distance);
  Serial.println(" [cm]");
  delay(500);
}
```

Click on **Upload** button.

Open ‘Tera Term’ and make New Connection.

Ultrasonic sensor ‘HC-SR04’ get trigger signal as 10\[us] pwm through trig pin which generate on **D10 pin**. Also, you should capture the echo signal on **D7 pin** and measure its pulse-width to calculate the distance.

![HC-SR04](https://user-images.githubusercontent.com/91526930/186363383-244a393f-7e4c-4e76-940e-06ca1bb7e5fb.png)

Press the reset button(black) and verify the operation. The distance between ultrasonic sensor and obstacle will be shown in Tera Term.

![pwm3](https://user-images.githubusercontent.com/79825525/129156878-fe9e5a5a-869d-4f36-a17e-6f12305c4d08.png)

Click on **Upload** button.

Press the reset button(black) and verify the operation. The distance between ultrasonic sensor and obstacle will be shown in Tera Term.

#### Exercise1:

Measure the distance (cm) and the pulse width (msec) and print both values.

* What is the maximum and minimum distance it can measure
* What is the accuracy of the distance you have measured with the ultrasonic sensor?
* Show your experiment result and compare it with the exact distance measured by a ruler.

#### Exercise 2:

Generate a square pulse of 1\~2Hz by using a function generator.

* Measure the time period of the pulse in msec.
*   What is the accuracy when measuring the period? What can you do to improve the measurement accuracy?

    ​

### PWM (**Pulse Width Modulation**) DC - Motor

We are going to create a simple program that run DC - Motor by giving pwm signal as input.

Create a new program named as ‘**TU\_arduino\_PWM2**’.

Write the following source code.

{% tabs %}
{% tab title="ver1" %}
```cpp
const int pwmPin = 11;   // PWM pin
const int buttonPin = 3;  // button pin

int buttonState = HIGH;

void setup() {
  pinMode(pwmPin, OUTPUT);
  pinMode(buttonPin, INPUT);
}

void loop() {

  buttonState = digitalRead(buttonPin);

  if (buttonState == LOW){
    for (int i = 0; i < 10; i++){
      analogWrite(pwmPin, 40 + 10*i);
      delay(100);
    }
  
    for (int i = 10; i > 0; i--){
      analogWrite(pwmPin, 40 + 10*i);
      delay(100);
    }
  }
  else{
    analogWrite(pwmPin, 0);
  }
}
```
{% endtab %}

{% tab title="ver2" %}
```cpp
const int pwmPin = 11;   // PWM pin
const int buttonPin = 3;  // button pin

int buttonState = HIGH;

void setup() {
  pinMode(pwmPin, OUTPUT);
 
 // initialize the pushbutton pin as an input:
  pinMode(buttonPin, INPUT_PULLUP);
  attachInterrupt(digitalPinToInterrupt(buttonPin), motorOperation, CHANGE);
}

void loop() {

  if (buttonState == LOW){
    for (int i = 0; i < 10; i++){
      analogWrite(pwmPin, 40 + 10*i);
      delay(100);
    }
  
    for (int i = 10; i > 0; i--){
      analogWrite(pwmPin, 40 + 10*i);
      delay(100);
    }
  }
  else{
    analogWrite(pwmPin, 0);
  }
}

void motorOperation(){
  buttonState = digitalRead(buttonPin);
}
```
{% endtab %}
{% endtabs %}

Click on **Upload** button.

Press the reset button(black) and verify the operation. If you press the user button, DC-Motor will turn on.

#### Exercise

Control Motor direction and speed with following configuration

* As button B1 is pressed, increase PWM duty ratio by interval of 25%
* 0 - 0.25(CW) - 0.5(CW) - 0.75(CW) - 1(CW) - 0 - 0.25(CCW) - 0.5(CCW) and so on
* Direction pin is PC\_8 (But, you should wire `PC8` to `D9` and use `D9` pin in arduino example.)
  * DIR=0, CW
  * DIR=1, CCW
* PWM period is 1msec

![direction pin connection](https://user-images.githubusercontent.com/91526930/186585480-9be2fc70-e21c-485c-85d7-ea27ed07e986.png)

### ADC

We are going to create a simple program that measures the output voltage of photo-resistor and print out the result through UART communication.

\###Exercise: analog photodetector

Create a new program named as ‘**TU\_arduino\_AnalogIn**.

Write the following source code.

```cpp
const int ledPin = 13;
const int cdsPin = 0;
float measure;

int ledState = LOW;

void setup() {
  pinMode(ledPin, OUTPUT);
 
  Serial.begin(9600);
}

void loop() {
  
  measure = analogRead(cdsPin);     // mapping(0 ~ 3.3V -> 0 ~ 1023)
  measure = measure * 3300 / 1024;  // [mV] (0 ~ 1023 -> 0 ~ 3300[mV])
  Serial.print("measure = ");
  Serial.print(measure);
  Serial.println(" [mV]");

  if (measure > 2500) ledState = HIGH;
  else                ledState = LOW;

  digitalWrite(ledPin, ledState);
  delay(200);
}
```

Click on **Upload** button.

Open ‘Tera Term’ and make New Connection.

Photo-resistor module outputs low voltage under a bright condition, and vice versa.

![ADC](https://user-images.githubusercontent.com/79825525/129156915-b3ca7031-c459-428e-be9b-dbe6acce91b2.png)

Push the reset button(black) and verify the operation.

If you turn on the flashlight at the photo-resistor with your phone, the sensor output voltage will be decreased. When the output voltage is below 0.2V, which means high brightness is given, the LED(LD2) will be turned on.

#### Exercise 1: photodetection

* Change different values for measure threshold to change the sensitivity of detection.
*   Under the brightness near the thresholding voltage, the led may flicker on and off. How can you change your code to avoid this flicker?

    ​

#### Exercise 2: sound sensor

The experiment kit has a sound sensor (microphone)\[SZH-EK033]. You can change the sensitivity of the sound sensor by turning the variable resistor.

It is connected as `AnalogIn` `PinName` `A5`

![SZH-EK033](https://user-images.githubusercontent.com/91526930/186364708-c1c69d31-c030-4ddb-8aa8-9276c0b07db0.png)

* For every second, print the value of the sound sensor
* Check the max value the sensor can print.
* Turn LED on/off by clapping your hand.

### Bluetooth

We are going to create a simple program that links MCU-PC via Bluetooth communication. MCU can receive and transmit 8-bit character data through the Bluetooth communication.

The experiment kit has Bluetooth HC-05.

```cpp
#include "mbed.h"
 
/* Private typedef -----------------------------------------------------------*/
/* Private define ------------------------------------------------------------*/
/* Private variables ---------------------------------------------------------*/
Serial pc(USBTX, USBRX);
Serial bt(PA_11, PA_12);
 
/* Private function prototypes -----------------------------------------------*/
 
/* Private functions ---------------------------------------------------------*/
/**
   * @brief  Main Function
   * @param  None
   * @retval None
   */
int main(void)
{
    char ch;
    pc.baud(115200);
    bt.baud(115200);
    pc.printf("Hello World!\n\r");
    bt.printf("Hello World!\r\n");
    
    while(1)
    {
        if(bt.readable())
        {
            ch=bt.getc();
            pc.printf("%c",ch);
            bt.printf("%c",ch);
        }
        
        else if(pc.readable())
        {
            ch=pc.getc();
            bt.printf("%c",ch);
            pc.printf("%c",ch); 
        }
    }
}
```

You can connect the MCU to PC via bluetooth or an APP.

* Android App Terminal Multi ([Google Play Download](https://play.google.com/store/apps/details?id=com.edodm85.terminalmulti.free))
* Bluetooth SPP Manager [(Google Play Download)](https://play.google.com/store/apps/details?id=at.rtcmanager)

Refer [here for more information about bluetooth](https://os.mbed.com/docs/mbed-os/v6.14/apis/bluetooth.html)

For example code, refer here
