# Tutorial: arduino-stm32 Part1



## Preparation

MCU board: Nucleo-F401RE

[HUINS experiment kit](../../stm32-m4-programming/hardware/experiment-hardware/huins-embedded-kit.md#pin-map)



## **GPIO Digital In/Out**

We are going to create a simple program that turns LED(LD2) on and off by pressing the user button(BT1).

### [pinMode()](https://www.arduino.cc/reference/en/language/functions/digital-io/pinmode/)
Configures the specified pin to behave either as an input or an output.

```cpp
pinMode(pin, mode)
```
* pin: the pin number to set the model of.
* mode: INPUT, OUTPUT or INPUT_PULLUP.
> Look up for pinMode() function in arduino reference for detail description.

### Example

Create a new program named as ‘**TU\_arduino\_GPIO\_LED\_button**’.

Write the following source code:  [source code](https://github.com/ykkimhgu/EC-student/tree/main/stm32duino-tutorial).

```cpp
const int btnPin = 3;
const int ledPin = 13;

int btnState = HIGH;

void setup() {
  pinMode(ledPin, OUTPUT);
  pinMode(btnPin, INPUT);
}

void loop() {
  btnState = digitalRead(btnPin);

  if (btnState == HIGH) 
    digitalWrite(ledPin, LOW);
  
  else 
    digitalWrite(ledPin, HIGH);
  
}
```
The user button pin is `PC13`, but this pin cannot be used in arduino. So, you should connect `PC13` to `pinName` `D3` by using wire.

![button pin connection](https://user-images.githubusercontent.com/91526930/186584565-3dc47e19-e5c5-43a2-b4c4-d4de8916a2c8.png)

Click on **upload** button. Push the reset button(black) and check the performance. The LED(LD2) should be turned on when the button is pressed.





## **External Interrupt**

We are going to create a simple program that turns LED(LD2) on triggered by **External Interrupt** of user button(BT1).

### [attachInterrupt()](https://www.arduino.cc/reference/en/language/functions/external-interrupts/attachinterrupt/)
```cpp
attachInterrupt(digitalPinToInterrupt(pin), ISR, mode)
```
* digitalPinToInterrupt(pin): translate the digital pin to the specific interrupt number.
* ISR: a function called whenever the interrupt occurs.
* mode: defines when the interrupt should be trggered. (LOW, CHANGE, RISING, FALLING)

### Example

Create new program as ‘**TU\_arduino\_ExtIn**’.

Write the following code.

```cpp
const int btnPin = 3;
const int ledPin =  13; 

int btnState = HIGH;

void setup() {
    pinMode(ledPin, OUTPUT);
    pinMode(btnPin, INPUT_PULLUP);
    attachInterrupt(digitalPinToInterrupt(btnPin), blink, CHANGE);
}

void loop() {
}

void blink(){
    btnState = digitalRead(btnPin);

    if (btnState == HIGH)
        digitalWrite(ledPin, LOW);
    else 
        digitalWrite(ledPin, HIGH);
}
```

Click on **upload** button. 

Whenever the user button(BT1) is pressed (at fall), then the LED should be ON. When the button is released then the LED should be off.



### Exercise

**Motion Detection Exercise**

- Use External interrupt to get the digital in data from the motion sensor
- When the userbutton is pressed, it should turn-off the LED.



The experiment kit has IR motion sensor(HD-SEN0018) that detects a motion of an object nearby. It is often used in automatic lighting system at the front door. It is connected to `PinName` `D5` as DigitalIn.

![image](https://user-images.githubusercontent.com/91526930/186357289-990c7152-b4cb-4fd5-bd83-62b4a841d9c4.png)

* Hint:

```cpp
// set pin numbers
const int motionPin = 5;    // the number of the Motion detection sensor
const int btnPin = 3;       // the number of the push button pin
const int ledPin = 13;      // the number of the LED pin

void setup() {
  // initialize the LED pin as an output:
  // your code 
  
  // initialize the Motion detection sensor pin as an interrupt input:
  // your code

  // initialize the push button pin as an interrupt input:
  // your code
}

void loop() {

}

void motionDetected(){
  // LED on
  // your code
}

void btnPressed(){
  // LED off
  // your code
}
```



## Ticker (SysTick interrupt)

We are going to create a simple program that uses System Timer Tick Interrupt that occurs periodically. Lets turn LED on and off at 1 sec of period.

### Add STM32Timer Interrupt Library

Download the library as lastest version. 
> [STM32Timer Interrupt Library](https://www.arduino.cc/reference/en/libraries/stm32_timerinterrupt/)

Add the library as .zip file.

> On the menu bar, select **sketch > Include Library > Add .ZIP Library**. Then, open 'STM32_TimerInterrupt-(version).zip'.



### STM32Timer class

* STM32Timer
  ``` cpp
  STM32Timer ITimer(TIMx);
  ITimer.attachInterruptInterval(period_us, TimerHandler)
  ```
  - TIMx : Timer number (TIM1, TIM2, ...)
  - period_us : Period of Timer (unit: microseconds)
  - TimerHandler : a function handling the timers run


* STM32_ISR_Timer
  ``` cpp
  STM32_ISR_Timer ISR_Timer;
  ISR_Timer.setInterval(period_ms, timerInterrupt);
  ```
  - period_ms : Period of ISR Timer (unit: miliseconds)
  - timerInterrupt : a function excuted whenever the period of ISR Timer.

Use the STM32TimerInterrupt interface to set up a recurring interrupt; it calls a function repeatedly and at a specified rate.



### Example

Create a new program named as ‘**TU\_arduino\_TimerInterrupt**’.

You can make LED blink every second, even though there is no infinite loop. This is also called as the ‘Timer interrupt’ or ‘SysTick interrupt’.

```cpp
#include "STM32TimerInterrupt.h"
#include "STM32_ISR_Timer.h"

#define HW_TIMER_INTERVAL_MS  100
#define TIMER_INTERVAL        1000L

STM32Timer ITimer(TIM1);    // Init STM32 timer TIM1
STM32_ISR_Timer ISR_Timer;  // Init STM32_ISR_Timer

const int ledPin =  13;

void setup() {
  pinMode(ledPin, OUTPUT);

  ITimer.attachInterruptInterval(HW_TIMER_INTERVAL_MS * 1000, TimerHandler);
  ISR_Timer.setInterval(TIMER_INTERVAL, timerInterrupt);
}

void loop(){
}

void TimerHandler(){
  ISR_Timer.run();
}

void timerInterrupt(){
  digitalWrite(ledPin, !digitalRead(ledPin));
}
```

Click on **Upload** button. 

LED(LD2) should blink every second.



### Exercise

**Buzz output exercise**

* Buzz the sound for about 1 second that repeats for every 3 seconds.
* e.g:  1 sec of buzzing then 2 secs of silence and repeat



This experiment kit has a digital buzzer (MCKPI-G1410).

It is connected at DigitalOut `PA13`. (But, you should wire `PA13` to `pinName` `D8`.)

A simple example of using a buzzer: 

```cpp
void loop(){
  if (buzzState == HIGH)
    tone(buzzPin, 100);
    
  else if (buzzState == LOW)
    noTone(buzzPin);
}
```
![buzzer pin connection](https://user-images.githubusercontent.com/91526930/186584724-e7159e35-670e-4225-8580-a07b23eb2d79.png)

[Hint:](https://github.com/ykkimhgu/EC-student/tree/main/stm32duino-tutorial/exercise)

```cpp
#include "STM32TimerInterrupt.h"
#include "STM32_ISR_Timer.h"

#define HW_TIMER_INTERVAL_MS  100
#define TIMER_INTERVAL        1000L

STM32Timer ITimer(TIM1);    // Init STM32 timer TIM1

STM32_ISR_Timer ISR_Timer;  // Init STM32_ISR_Timer

// constants won't change. They're used here to set pin numbers:
const int ledPin =  13;   // the number of the LED pin
const int buzzPin = 8;    // the number of the buzzer pin

// variables will change:
int buzzState = LOW;
int cnt = 0;

void setup() {
  // initialize the LED pin as an output:
  // your code

  // Interval in microsecs
  // your code

  // Timer interrupt every 1sec
  // your code
}

void loop(){
  if (buzzState == HIGH)
    tone(buzzPin, 100);
    
  else if (buzzState == LOW)
    noTone(buzzPin);
}

// Timer handler controls ISR Timer(timer interrupt).
void TimerHandler(){
  // your code
}

// Whenever ISR Timer interrupts, this function is excuted.
void timerInterrupt(){
  // Use 'cnt' and 'buzzState' variables
  // your code 
  
}
```