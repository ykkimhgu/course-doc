# Tutorial: Create new project in mbed



## Tutorial 2021

### 1. Overview

In this tutorial, you will use ‘mbed’ online compiler to handle several peripherals of MCU \(ARM-Cortex M4\). Using the given platform, you will perform some tasks about GPIO, timer and interrupt.

The objectives of this lab are

* Practice to use ‘mbed’ library in online compiler.
* Understand digital in/out peripheral in MCU.
* Understand timer and interrupt function of MCU.
* Handle GPIO, timer and interrupt using ‘mbed’ library.

![STM32duino\_F411](https://user-images.githubusercontent.com/79825525/129155781-83639c1d-bb1f-4cc9-b3d5-3a080426d382.jpg)

​ **Figure 1. Pin configuration for NUCLEO-F411RE board**

### 2. Tutorial

#### Creating program

* Open [https://www.mbed.com/en/](https://www.mbed.com/en/)
* Create new account

  ![image-20210811204723487](https://user-images.githubusercontent.com/79825525/129155672-82f51b2d-1bdd-4016-a4f6-3ffd60923ec9.png)

* Click on yellow **Compiler** button.

**NUCLEO-F411RE**

* Create new program as ‘Tutorial1\_mbed’. If it asks for ‘add new platform’,

  ![add\_platform](https://user-images.githubusercontent.com/79825525/129156475-64577741-2f1d-4a5d-9872-d7a27abe9b8e.png)

  then ‘**Add Platform**’ search for ‘**NUCLEO-F411RE**’ board -&gt; Click ‘**Add to your Mbed Compiler**’ and ‘**Open Mbed Complier**’

![mbed\_compiler](https://user-images.githubusercontent.com/79825525/129157119-ac6bd034-428c-4981-80be-7b7b0cbbcd9c.png)

**NUCLEO-F401RE**

* Create new program as ‘Tutorial1\_mbed’. If it asks for ‘add new platform’,

![add\_platform](https://user-images.githubusercontent.com/79825525/129156475-64577741-2f1d-4a5d-9872-d7a27abe9b8e.png)

​ then ‘**Add Platform**’ search for ‘**NUCLEO-F401RE**’ board -&gt; Click ‘**Add to your Mbed Compiler**’ and ‘**Open Mbed Complier**’

![401](https://user-images.githubusercontent.com/79825525/129155462-259c330a-493f-478f-a050-3acf474d7708.png)

* Then, create new program as ‘**Tutorial1\_GPIO**’

![create](https://user-images.githubusercontent.com/79825525/129156165-9edaafca-4b76-4d4b-8f9d-e58593452705.png)

* Create new file ‘**main.cpp’**.

  ![create3](https://user-images.githubusercontent.com/79825525/129156232-be454692-e8dc-4aff-911e-2f15f7d94a42.png)

* Import ‘mbed’ library to program. Search ‘mbed’ and double-click ‘**mbed**’ by mbed official which is the first one.

  ![mbed](https://user-images.githubusercontent.com/79825525/129156263-d40160c4-5acb-4551-80e5-01018a0d5b16.png)

* Write the source code on ‘main.cpp’.

```cpp
#include "mbed.h"

DigitalOut led(LED1);

int main() {
    while(1) {
    led = 1;
    }
}
```

* To compile the program, click on ‘**Compile’** button.

![compile](https://user-images.githubusercontent.com/79825525/129156625-5a31b30a-51b4-4b34-b229-4c14fd642b29.png)

* Then, the binary file of the project will be created and downloaded on your computer. 
* Connect the MCU board to your PC via USB cable and check if a new memory drive of “NODE\_F401RE \(E:\)” is created.
* To load the program onto the MCU, copy the downloaded binary file to the drive “NODE\_F401RE \(E:\)”. If the program is loaded successfully then LED\(LD1\) will be green light.

#### **GPIO \(General purpose Input/Output\)**

* If you want to use digital input/output pins, you can use ‘DigitalIn, DigitalOut’ classes. You can search many useful class commands of ‘mbed’ library as shown below.

![GPIO](https://user-images.githubusercontent.com/79825525/129156660-6be700e7-9c99-47f7-b097-51c5b3e91109.png)

* Create new program as ‘**Tutorial1\_GPIO**’.

  ![GPIO2](https://user-images.githubusercontent.com/79825525/129156689-5cb53ebc-33b3-4c5e-af01-e29187d4ded6.png)

* Write the following source code on ‘main.cpp’

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

* Click on **Compile** button. Then, the binary files will be created and downloaded.
* Then, the binary file “Tutorial1\_GPIO.NUCLEO\_F401RE.bin” of the project will be created and downloaded on your computer. 
* Connect the MCU board to your PC via USB cable.
* Push the reset button\(black\) and verify the performance. The LED should be turned on when the button is pressed.
* To load the program onto the MCU, copy the downloaded binary file to the drive “NODE\_F401RE \(E:\)”. If the program is loaded successfully then LED\(LD1\) will be green light.

#### Timer

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

#### Ticker \(Timer interrupt\)

* If you want to use call interrupt in every second, you can use ‘Ticker’ class. 
* Create new program as ‘**Tutorial1\_Ticker**’.
* Write the following code on ‘mbed’ complier. You will make LED blink every second, even though there is no specific code in infinite loop syntax. This function is called ‘timer interrupt’ in MCU.

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

* Click on **Compile** button. Then, the binary files will be created and downloaded.
* Then, the binary file will be created and downloaded on your computer.
* To load the program onto the MCU, copy the downloaded binary file to the drive “NODE\_F401RE \(E:\)”. If the program is loaded successfully then LED\(LD1\) will be green light.
* Verify the performance. LED\(LD2\) should blink every second.

#### Interrupt

* If you want to use external interrupt, you can use ‘InterruptIn’ class. 
* Create new program as ‘**Tutorial1\_Interrupt**’.
* Write the following code on ‘mbed’ complier. The performance will be same with that of B.GPIO part, but the main function has empty infinite loop unlike B.GPIO. This represents the advantage of using interrupt function. 

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

* Click on **Compile** button. Then, the binary files will be created and downloaded.
* Then, the binary file will be created and downloaded on your computer. 
* To load the program onto the MCU, copy the downloaded binary file to the drive “NODE\_F401RE \(E:\)”. If the program is loaded successfully then LED\(LD1\) will be green light.
* Verify the performance. LED\(LD2\) should be turned on when user button is pushed.

#### **U\(S\)ART \(Universal Asynchronous/synchronous Receiver and Transmitter\)**

* You can receive and transmit 8-bit character data through UART communication. NUCLEO-F401RE board offers UART2 channel with USB connector.
* Create new program as ‘**Tutorial2\_UART**’.
* Open ‘main.cpp’ and delete the example codes. Write the following source code on ‘main.cpp’.

```cpp
#include "mbed.h"

Serial  uart(USBTX, USBRX, 9600);

int main(){
    char RXD;    
    while(1)
    {        
        if(uart.readable()){
            RXD = uart.getc();
            uart.printf("%c", RXD);
        }
    }
}
```

* Click on ‘**Compile’** button, then the binary file will be created and downloaded on your computer. 
* Connect the MCU board to your PC via USB cable. 
* To load the program onto the MCU, copy the downloaded binary file to the drive “NODE\_F411RE \(E:\)”. If the program is loaded successfully then LED\(LD1\) will be green light.
* Open ‘Tera Term’ and connect serial port\(‘**COMx: STMicroelectronics STLink**’\). Check if the baud rate is selected as 9600 \[bps\].
* Press the reset button\(black\) and verify the operation. If you put any letter in Tera Term, MCU will receive it and transmit it to PC immediately, so you can see the pushed letters showed in Tera Term. 

#### PWM \(**Pulse Width Modulation**\)

* You can generate PWM with ‘PwmOut’ class. In this tutorial, you will use ultrasonic sensor ‘HC-SR04’ and refer its datasheet. You have to generate trigger signal with 10\[us\] pulse-width on D10 pin. Also, you should capture the echo signal on D7 pin and measure its pulse-width to calculate the distance. The calculation formula is shown below.

![pwm1](https://user-images.githubusercontent.com/79825525/129156845-e43c22fc-1041-4dda-a594-7747c5150b26.png)

![pwm2](https://user-images.githubusercontent.com/79825525/129156859-c2f10cd8-f33b-4376-8cae-75e93e3e2128.png)

* Create new program as ‘**Tutorial1\_PWM**’.
* Write the following code on ‘main.cpp’.

```cpp
#include "mbed.h"

Serial      pc(USBTX, USBRX, 9600);
PwmOut      trig(D10); // Trigger 핀
InterruptIn echo(D7);  // Echo 핀
Timer       tim;

int begin = 0;
int end = 0;

void rising(){
    begin = tim.read_us();
}

void falling(){
    end = tim.read_us();
}

int main(void){
    float distance = 0;

    trig.period_ms(60);     // period      = 60ms
    trig.pulsewidth_us(10); // pulse-width = 10us

    echo.rise(&rising);
    echo.fall(&falling);

    tim.start();

    while(1){
        distance =  (float)(end - begin) / 58; // [cm]
        pc.printf("Distance = %.2f[cm]\r\n", distance);
        wait(0.5);
    }
}
```

* Click on **Compile** button. Then, the binary files will be created and downloaded.
* Connect the MCU board to your PC via USB cable and check if a new memory drive of “NODE\_F401RE \(E:\)” is created.
* Open ‘Tera Term’ and connect serial port\(‘**COMx: STMicroelectronics STLink**’\). Check if the baud rate is selected as 9600 \[bps\]
* Press the reset button\(black\) and verify the operation. The distance between ultrasonic sensor and obstacle will be shown in Tera Term.

![pwm3](https://user-images.githubusercontent.com/79825525/129156878-fe9e5a5a-869d-4f36-a17e-6f12305c4d08.png)

#### ADC

* If you want to measure analog voltage, you can use ‘AnalogIn’ class. You will measure the output voltage of photo-resistor\(조도센서\) in this tutorial. This module outputs low voltage in bright condition, and vice versa. 

![ADC](https://user-images.githubusercontent.com/79825525/129156915-b3ca7031-c459-428e-be9b-dbe6acce91b2.png)

* Create new program as ‘**Tutorial1\_ADC**’.
* Write the following code on ‘main.cpp’. 

```cpp
#include "mbed.h"

Serial      pc(USBTX, USBRX, 9600);                                                
AnalogIn    CDS(A0);
DigitalOut  led(LED1);

int main() {
    float measure;

    while(1) {
        measure = CDS.read(); // mapping(0~3.3V -> 0.0~1.0)
        measure = measure * 3300; // [mV] (0.0~1.0 -> 0~3300[mV])
        pc.printf("measure = %f mV\n\r", measure);

        if (measure < 200) led = 1;
        else               led = 0;

        wait(0.2); 
    }
}
```

* Click on **Compile** button. Then, the binary files will be created and downloaded.  Click on **Compile** button. Then, the binary files will be created and downloaded.
* Then, the binary file will be created and downloaded on your computer. 
* To load the program onto the MCU, copy the downloaded binary file to the drive “NODE\_F401RE \(E:\)”. If the program is loaded successfully then LED\(LD1\) will be green light.
* To verify the performance, firstly open ‘Tera Term’ and follow the sequences.
* Push the reset button\(black\) and verify the operation. If you turn on flashlight at photo-resistor with your phone, sensor output voltage will be decreased. When the output voltage is below 0.2V, which means high brightness is given, LED\(LD2\) will be turned on.

