# Tutorial: Create new project in mbed



## Overview

In this tutorial, you will use ‘mbed’ online compiler to handle several peripherals of MCU \(ARM-Cortex M4\). Using the given platform, you will perform some tasks about GPIO, timer and interrupt.

The objectives of this lab are

* Practice to use ‘mbed’ library in online compiler.
* Understand digital in/out peripheral in MCU.
* Understand timer and interrupt function of MCU.
* Handle GPIO, timer and interrupt using ‘mbed’ library.

## Hardware

![ &#x200B; Figure 1. Pin configuration for NUCLEO-F411RE board \(same pin configuration with NUCLE-F401Re\)](https://user-images.githubusercontent.com/79825525/129155781-83639c1d-bb1f-4cc9-b3d5-3a080426d382.jpg)



## Tutorial

## Creating account 

Open [https://www.mbed.com/en/](https://www.mbed.com/en/)

Create new account 



![image-20210811204723487](https://user-images.githubusercontent.com/79825525/129155672-82f51b2d-1bdd-4016-a4f6-3ffd60923ec9.png)

If you already have an account, then click on **'Compiler'**

![](../../.gitbook/assets/image%20%2817%29.png)

### \*\*\*\*

## **Creating New Program**

On menu bar, ****select  **New&gt;Create New Program.**

For the first project, it can ask for ‘add new platform’.

#### \*\*\*\*

### **Hardware Setting**

For the Platform, select your MCU board.  

1. For NUCLEO-F411RE:

* Click  ‘**Add Platform**’  &gt;  Search for ‘**NUCLEO-F411RE**’ board
*  Click ‘**Add to your Mbed Compiler**’ and ‘**Open Mbed Complier**’

![add\_platform](https://user-images.githubusercontent.com/79825525/129156475-64577741-2f1d-4a5d-9872-d7a27abe9b8e.png)

![mbed\_compiler](https://user-images.githubusercontent.com/79825525/129157119-ac6bd034-428c-4981-80be-7b7b0cbbcd9c.png)

    **2.** For NUCLEO-F401RE: 

* Click  ‘**Add Platform**’  &gt;  Search for ‘**NUCLEO-F401RE**’ board
* Click ‘**Add to your Mbed Compiler**’ and ‘**Open Mbed Complier**’

![401](https://user-images.githubusercontent.com/79825525/129155462-259c330a-493f-478f-a050-3acf474d7708.png)



### New program

Set  the Program Name as:  **‘Tutorial1\_GPIO\_LED’**. 

![](../../.gitbook/assets/image%20%282%29.png)

Right-Click on the Program name in **Program Workspace.**  Then, create new file.  Name the source file as ‘**main.cpp’**.

![](../../.gitbook/assets/image%20%2822%29.png)



## Import 'mbed' Library

Right-Click on the Program name in **Program Workspace.**   Select **Import Library.**

On the **Import Wizard Window &gt; Libraries** tab : Search 'mbed'. Choose the mbed created by 'mbed\_official'.

![](../../.gitbook/assets/image%20%2820%29.png)



## Compile

Copy and paste the following source code on ‘main.cpp’.

```cpp
#include "mbed.h"

DigitalOut led(LED1);

int main() {
    while(1) {
    led = 1;
    }
}
```

Then, compile the program by  clicking on ‘**Compile’** button.

![compile](https://user-images.githubusercontent.com/79825525/129156625-5a31b30a-51b4-4b34-b229-4c14fd642b29.png)

If the compilation is successful, the binary file of the project  "\*\*.bin" will be created and downloaded on your computer. 

![](../../.gitbook/assets/image%20%2821%29.png)

## Import to MCU

Connect the MCU board to your PC via USB cable and check if a new memory drive of “NODE\_F401RE \(E:\)” is created in your computer.

To load the binary program onto the MCU, copy the downloaded binary file to the drive “NODE\_F401RE \(E:\)”. 

If the program is loaded successfully then LED\(LD1\) will be green light. 

> Click reset button on MCU if nothing happens'

#### \*\*\*\*

#### \*\*\*\*

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

