# Tutorial: mbed - Part 1

## Preparation

MCU board: Nucleo-F401RE



[Refer to HUINS mbed experiment kit: Pin Map](../experiment-hardware/huins-embedded-kit.md#pin-map)

## **GPIO Digital In/Out**

We are going to create a simple program that turns LED(LD2) on and off by pressing the user button(BT1).&#x20;

### mbed class

* [DigitalOut ](https://os.mbed.com/docs/mbed-os/v6.13/apis/digitalout.html)
* [Digital In](https://os.mbed.com/docs/mbed-os/v6.13/apis/digitalin.html)

> Look up for DigitalOut and DigitalIn in mbed documentation for the fulll list of methods

Create a** **new program named as  ‘**TU\_mbed\_GPIO\_LED\_button**’.

Write the following source code on ‘main.cpp’

```cpp
#include "mbed.h"

DigitalIn  button(USER_BUTTON);
DigitalOut led(LED1);

int main() {
    while(1) {
        if(!button) led = 1;
        else led = 0;
    }
}
```

Click on **Compile** button. Then, the binary files will be created and downloaded. Copy the binary file to MCU board via USB cable.&#x20;

Push the reset button(black) and check the performance. The LED(LD2)  should be turned on when the button is pressed.



## **External **Interrupt

We are going to create a simple program that turns LED(LD2) on triggered by **External Interrupt **of user button(BT1).&#x20;

### mbed class

* [InterruptIn](https://os.mbed.com/docs/mbed-os/v6.13/apis/interruptin.html)



Create new program as ‘**TU\_mbed\_ExtIn**’.

Write the following code on ‘mbed’ complier.&#x20;

We have created user defined functions of  `void pressed()` and `void released()`.&#x20;



```cpp
#include "mbed.h"

InterruptIn button(USER_BUTTON); 
DigitalOut  led(LED1);

void pressed()
{
    led = 1; 
}

void released(){
    led = 0;
}

int main()
{
    button.fall(&pressed);
    button.rise(&released);
    while (1);
}
```

Click on **Compile** button. Then, the binary files will be created and downloaded. Copy the binary file to MCU board via USB cable.&#x20;

Whenever the user button(BT1) is pressed (at fall), then the LED should be ON. When the button is released then the LED should be off.&#x20;



### Exercise

The experiment kit has IR motion sensor(HD-SEN0018) that detects a motion of an object nearby. It is often used in automatic lighting system at the front door.  It is connected to `PinName D5` as DigitalIn

![](<../../.gitbook/assets/image (95).png>)

* Use External interrupt to get the digital in data from the motion sensor
* When the userbutton is pressed,  it should turn-off the LED.
* Hint: &#x20;

```cpp
InterruptIn motion(D5)
// InterruptIN for button input

void motionDetected(void)
{
    // code to light up the LED
}

//  Interrupt handler function for Button to turn off LED
// Refer to void pressed(void)

int main(void)
{
    // other codes
    while(1){
    // other codes
    motion.rise(&motionDetected);
    // other codes
}

```



## Ticker (SysTick interrupt)

We are going to create a simple program that uses System Timer Tick Interrupt that occurs periodically. Lets turn LED on and off at 1 sec of period.&#x20;

### mbed class

* [Ticker](https://os.mbed.com/docs/mbed-os/v6.13/apis/ticker.html)

Use the Ticker interface to set up a recurring interrupt; it calls a function repeatedly and at a specified rate.

Create new program as ‘**TU\_mbed\_SysTick**’.

`tick.attach( )`  makes periodic interrupt of second unit.&#x20;

You can make LED blink every second, even though there is no infinite loop in main(). This is also called  as the ‘ SysTIck interrupt’.&#x20;

```cpp
#include "mbed.h"

Ticker     tick;
DigitalOut led(LED1);

void INT(){
    led = !led;      
}
int main(void){
    tick.attach(&INT, 1); // 1초마다 LED blink

    while(1);
}
```

Click on **Compile** button. Then, the binary files will be created and downloaded. Copy the binary file to MCU board via USB cable.&#x20;

LED(LD2) should blink every second.



### Exercise

This experiment kit has a digital buzzer (MCKPI-G1410).

It is connected at DigitalOut  `PinName`  `PA_13`

* Buzz the sound for about 1second that repeats for every 3 seconds.&#x20;

> You can also may use  wait(sec)&#x20;

To use the buzzer, square digital signals such as

```cpp
while(1){
    buzzer=1;
    wait(0.01);
    buzzer=0;
    wait(0.01);
}
```

## ****
