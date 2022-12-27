# Tutorial: arduino-stm32  Installation





## Overview

In this tutorial, you will use arduino IDE compiler to handle several peripherals of MCU (ARM-Cortex M4). Using the given platform, you will perform some tasks about GPIO, timer and interrupt.

The objectives of this lab are

* Connect STM32F (MCU board) to arduino IDE.
* Understand digital in/out peripheral in MCU.
* Understand timer and interrupt function of MCU.
* Handle GPIO, timer and interrupt using arduino library.



## Hardware

![ ​ Figure 1. Pin configuration for NUCLEO-F411RE board (same pin configuration with NUCLE-F401Re)](https://user-images.githubusercontent.com/79825525/129155781-83639c1d-bb1f-4cc9-b3d5-3a080426d382.jpg)



## Tutorial

## Installing arduino IDE

Open [https://www.arduino.cc/en/software](https://www.arduino.cc/en/software)

Click "Windows Win 7 and newer" in Download options. Then, you should click "JUST DOWNLOAD" for free.

![image](https://user-images.githubusercontent.com/91526930/186331900-ee06a945-723a-4210-8dc9-dd6079d16288.png)



## Installing STM32 MCU based boards in arduino IDE

### Add STM32 MCU board manager URL

On menu bar, select  **File > Preferences**

Then, add board manager URL 
>https://github.com/stm32duino/BoardManagerFiles/raw/main/package_stmicroelectronics_index.json


![image](https://user-images.githubusercontent.com/91526930/186333899-c7f1ee61-c4a3-42b3-898c-0108ae1c3b0e.png)

### Install STM32 MCU based boards

On menu bar, select **Tools > Board: " " > Boards Manager...**

Install STM32 MCU based boards.

![image](https://user-images.githubusercontent.com/91526930/186336101-53603bcc-e7d2-4fd4-8c86-078f703154e4.png)



## **Hardware Setting**


### Install ST-Link Driver

[Option 1: Download **en.stsw-link009\_v2.0.2.zip** from github](https://github.com/ykkimhgu/EC-student/blob/main/tutorial/(ST-Link)en.stsw-link009_v2.0.2.zip)

[Option 2: Download driver (STSW-LINK009) from ST website](https://www.st.com/en/development-tools/stsw-link009.html)

> ST-LINK, ST-LINK/V2, ST-LINK/V2-1, STLINK-V3 USB driver signed for Windows10. This USB driver (STSW-LINK009) is for ST-LINK/V2, ST-LINK/V2-1 and STLINK-V3 boards(STM8/STM32 discovery boards, STM8/STM32 evaluation boards and STM32 Nucleo boards).
>
> It declares to the system the USB interfaces possibly provided by the ST-LINK: ST Debug, Virtual COM port and ST Bridge interfaces. **The driver must be installed prior to connecting the device, in order to have a successful enumeration.**

After you download "**en.stsw-link009\_v2.0.2.zip**", unzip the file.





Connect MCU board to PC with USB cable.

Update the usb driver:   윈도우 **장치 관리자 > 범용직렬버스장치> ST-Link Debug >드라이버 업데이트>**
* Select the folder where en-stsw-link009 is unzipped 



{% hint style="info" %}
MCU board (STM32F411) must be connected to your PC to install the USB driver
{% endhint %}

![](<../../.gitbook/assets/image (3).png>)

> What is ST Link utiliy? [https://m.blog.naver.com/ansdbtls4067/221510252896](https://m.blog.naver.com/ansdbtls4067/221510252896)



### Connect STM32F401RE through USB port

Check the port number and settings in "device manager".
 - baudrate: 9600
 - data bits: 8
 - parity: None
 - Stop bit: 1
 - Flow control: None

![image](https://user-images.githubusercontent.com/91526930/186338119-272e8119-cfc5-411e-bce7-7118e94aea96.png)



### Connect STM32F401RE to arduino IDE

Arduino Setting for STM32F401RE
- Board: Nucleo-64
- Board part number: Nucleo F401RE
- Port: USB port number connected with STM32 Board

![image](https://user-images.githubusercontent.com/91526930/186340825-658a6bba-8c2a-43ea-aaae-3124205f33b5.png)



## Example - Blink

There is a simple example, that LED blinks every 1 sec.

On menu bar of Arduino IDE,  select **File > Examples > 01.Basics > Blink**

Then, new window will be opened. If you click **upload** button, the example code will be loaded on the MCU.

> If the program is loaded successfully then LED(LD1) will be green light.

![image](https://user-images.githubusercontent.com/91526930/186355557-1e9e137b-03f1-4c8b-8b87-05a4d1c87077.png)


