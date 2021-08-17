# Tutorial: GPIO with mbed Part 1

## Preparation

MCU board: Nucleo-F401RE

## **GPIO Digital In/Out**

We are going to create a simple program that turns LED\(LD2\) on and off by pressing the user button\(BT1\). 

### mbed class

* [DigitalOut ](https://os.mbed.com/docs/mbed-os/v6.13/apis/digitalout.html)
* [Digital In](https://os.mbed.com/docs/mbed-os/v6.13/apis/digitalin.html)

> Look up for DigitalOut and DigitalIn in mbed documentation for the fulll list of methods

Create a ****new program named as  ‘**TU\_mbed\_GPIO\_LED\_button**’.

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

Click on **Compile** button. Then, the binary files will be created and downloaded. Copy the binary file to MCU board via USB cable. 

Push the reset button\(black\) and check the performance. The LED\(LD2\)  should be turned on when the button is pressed.



## **External** Interrupt

We are going to create a simple program that turns LED\(LD2\) on triggered by **External Interrupt** of user button\(BT1\). 

### mbed class

* [InterruptIn](https://os.mbed.com/docs/mbed-os/v6.13/apis/interruptin.html)



Create new program as ‘**TU\_mbed\_ExtIn**’.

Write the following code on ‘mbed’ complier. 

We have created user defined functions of  `void pressed()` and `void released()`. 



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

Click on **Compile** button. Then, the binary files will be created and downloaded. Copy the binary file to MCU board via USB cable. 

Whenever the user button\(BT1\) is pressed \(at fall\), then the LED should be ON. When the button is released then the LED should be off. 

## Ticker \(SysTick interrupt\)

We are going to create a simple program that uses System Timer Tick Interrupt that occurs periodically. Lets turn LED on and off at 1 sec of period. 

### mbed class

* [Ticker](https://os.mbed.com/docs/mbed-os/v6.13/apis/ticker.html)

Use the Ticker interface to set up a recurring interrupt; it calls a function repeatedly and at a specified rate.



Create new program as ‘**TU\_mbed\_SysTick**’.

`tick.attach( )`  makes periodic interrupt of second unit. 

You can make LED blink every second, even though there is no infinite loop in main\(\). This is also called  as the ‘ SysTIck interrupt’. 

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

Click on **Compile** button. Then, the binary files will be created and downloaded. Copy the binary file to MCU board via USB cable. 

LED\(LD2\) should blink every second.

## Timer

* If you want to measure time taken in certain process, you can use ‘Timer’ class. 
* Create new program as ‘**Tutorial1\_Timer**’.
* Write the following source code on ‘main.cpp’. You will measure time to count 100 from 0. 

```cpp
#include "mbed.h"

Timer       timer;
Serial      pc(USBTX, USBRX, 9600); // for using ‘printf()’

int begin, end;
int cnt = 0;

int main(void){

    timer.start();

    begin = timer.read_us();

    while(cnt < 100) cnt++;

    end = timer.read_us();

    pc.printf("Counting 100 takes %d [us]\n", end-begin);
}
```

* Click on **Compile** button. Then, the binary files will be created and downloaded.
* Then, the binary file will be created and downloaded on your computer.
* To load the program onto the MCU, copy the downloaded binary file to the drive “NODE\_F401RE \(E:\)”. If the program is loaded successfully then LED\(LD1\) will be green light.
* To verify the performance, firstly open ‘Tera Term’ and follow the sequences.

  Choose ‘**Serial**’ tab -&gt; Select ‘**COMx: STMicroelectronics STLink**’ port

![teraterm](https://user-images.githubusercontent.com/79825525/129156752-893e425d-1653-496f-a4fa-13cbebe2a271.png)

​ Open Serial port\(시리얼 포트\) in Setup\(설정\) tab, check if the baud rate is set as **9600** \[bps\].

![teraterm2](https://user-images.githubusercontent.com/79825525/129156774-2bfe2509-d5e2-4ba1-b3bc-b06d53dacd52.png)

* Push the reset button\(black\), and verify the time taken in counting 100. You can measure time taken in any other processes like toggling LED, multiplication or division, etc. If the process takes long time, you can also measure time in \[ms\] unit using ‘timer.read\_ms\(\)’ command.

![teraterm3](https://user-images.githubusercontent.com/79825525/129156799-0f1e7781-71f9-4294-94e3-6304c150d7e5.png)

## 

## \*\*\*\*

