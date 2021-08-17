# Tutorial: mbed - Part 2

## **U\(S\)ART \(Universal Asynchronous/synchronous Receiver and Transmitter\)**

We are going to create a simple program that links MCU-PC via UART communication. MCU can  receive and transmit 8-bit character data through UART communication. 

NUCLEO-F401RE board offers UART2 channel with USB connector.

### mbed class

* [BufferedSerial](https://os.mbed.com/docs/mbed-os/v6.13/apis/serial-uart-apis.html)

> Look up in mbed documentation for the fulll list of methods



Create a ****new program named as  ‘**TU\_mbed\_UART**’.

Write the following source code on ‘main.cpp’

{% tabs %}
{% tab title="old version" %}
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
{% endtab %}

{% tab title="new version" %}
```cpp
/*
 * Copyright (c) 2020 Arm Limited and affiliates.
 * SPDX-License-Identifier: Apache-2.0
 */

#include "mbed.h"

// Maximum number of element the application buffer can contain
#define MAXIMUM_BUFFER_SIZE                                                  32

// Create a DigitalOutput object to toggle an LED whenever data is received.
static DigitalOut led(LED1);

// Create a BufferedSerial object with a default baud rate.
static BufferedSerial serial_port(USBTX, USBRX);

int main(void)
{
    // Set desired properties (9600-8-N-1).
    serial_port.set_baud(9600);
    serial_port.set_format(
        /* bits */ 8,
        /* parity */ BufferedSerial::None,
        /* stop bit */ 1
    );

    // Application buffer to receive the data
    char buf[MAXIMUM_BUFFER_SIZE] = {0};

    while (1) {
        if (uint32_t num = serial_port.read(buf, sizeof(buf))) {
            // Toggle the LED.
            led = !led;

            // Echo the input back to the terminal.
            serial_port.write(buf, num);
        }
    }
}

```
{% endtab %}
{% endtabs %}

Click on **Compile** button. Then, the binary files will be created and downloaded. Copy the binary file to MCU board via USB cable. 



[Download 'TeraTerm'](https://osdn.net/projects/ttssh2/releases/)

Open ‘Tera Term’ . and make New Connectino. 

Choose ‘**Serial**’ tab -&gt; Select ‘**COMx: STMicroelectronics STLink**’ port

> COMx, x is the port number and it can be different for each connection.

![teraterm](https://user-images.githubusercontent.com/79825525/129156752-893e425d-1653-496f-a4fa-13cbebe2a271.png)

​ Open Serial port\(시리얼 포트\) in Setup\(설정\) tab, check if the baud rate is set as **9600** \[bps\].

![teraterm2](https://user-images.githubusercontent.com/79825525/129156774-2bfe2509-d5e2-4ba1-b3bc-b06d53dacd52.png)



Press the reset button\(black\) and verify the operation. If you put any letter in Tera Term, MCU will receive it and transmit it to PC immediately, so you can see the pushed letters showed in Tera Term.



##  Timer

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
* 
![teraterm3](https://user-images.githubusercontent.com/79825525/129156799-0f1e7781-71f9-4294-94e3-6304c150d7e5.png)

## 

* Push the reset button\(black\), and verify the time taken in counting 100. You can measure time taken in any other processes like toggling LED, multiplication or division, etc. If the process takes long time, you can also measure time in \[ms\] unit using ‘timer.read\_ms\(\)’ command.



## PWM \(**Pulse Width Modulation**\)

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



