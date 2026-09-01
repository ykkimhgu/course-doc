# Tutorial: arduino-stm32 Installation

## Overview

In this tutorial, you will use arduino IDE compiler to handle several peripherals of MCU (ARM-Cortex M4). Using the given platform, you will perform some tasks about GPIO, timer and interrupt.

The objectives of this lab are

* Connect STM32F (MCU board) to arduino IDE.
* Understand digital in/out peripheral in MCU.
* Understand timer and interrupt function of MCU.
* Handle GPIO, timer and interrupt using arduino library.

### Official Reference

{% embed url="https://github.com/stm32duino/Arduino_Core_STM32/wiki/Getting-Started" %}

### Hardware

Among many pins availableon NUCLEO board, we can ONLY use Arduino pins for Arduino-STM32

![​ Figure 1. Pin configuration for NUCLEO-F411RE board (same pin configuration with NUCLE-F401Re)](https://user-images.githubusercontent.com/79825525/129155781-83639c1d-bb1f-4cc9-b3d5-3a080426d382.jpg)

## Install & Setup Arduino IDE

### Install Arduino IDE

Open [https://www.arduino.cc/en/software](https://www.arduino.cc/en/software)

Click "Windows Win 7 and newer" in Download options. Then, you should click "JUST DOWNLOAD" for free.

![image](https://user-images.githubusercontent.com/91526930/186331900-ee06a945-723a-4210-8dc9-dd6079d16288.png)

**경로에 한글이름, 띄어쓰기 금지(오직 영어만 있어야 함)**

![image](https://github.com/ckdals915/HESAI_Pandar_XT32_Interface/assets/84508106/c75b4557-0782-4af7-85fe-840cd37817e1)

### Add Board Manager:  STM32 MCU &#x20;

On menu bar, select **File > Preferences**

Then, add board manager URL by copying this link.

{% code expandable="true" %}
```
https://github.com/stm32duino/BoardManagerFiles/raw/main/package_stmicroelectronics_index.json
```
{% endcode %}

![image](https://user-images.githubusercontent.com/91526930/186333899-c7f1ee61-c4a3-42b3-898c-0108ae1c3b0e.png)



On menu bar, select **Tools > Board: " " > Boards Manager...**

Search for STM32.

Install STM32 MCU based boards.

![image](https://user-images.githubusercontent.com/91526930/186336101-53603bcc-e7d2-4fd4-8c86-078f703154e4.png)

## **Hardware Connection Setup : Requires MCU board**

### Install ST-Link Driver&#x20;

[Option 1: Download **en.stsw-link009\_v2.0.2.zip** from github](https://github.com/ykkimhgu/EC-student/blob/main/tutorial/\(ST-Link\)en.stsw-link009_v2.0.2.zip)

* (If the download link of Option 1 fails, try Option 2)
* [Option 2: Download driver (STSW-LINK009) from ST website](https://www.st.com/en/development-tools/stsw-link009.html)

> ST-LINK, ST-LINK/V2, ST-LINK/V2-1, STLINK-V3 USB driver signed for Windows10. This USB driver (STSW-LINK009) is for ST-LINK/V2, ST-LINK/V2-1 and STLINK-V3 boards(STM8/STM32 discovery boards, STM8/STM32 evaluation boards and STM32 Nucleo boards).
>
> It declares to the system the USB interfaces possibly provided by the ST-LINK: ST Debug, Virtual COM port and ST Bridge interfaces. **The driver must be installed prior to connecting the device, in order to have a successful enumeration.**

### Connect MCU to Laptop(PC)

After you download "**en.stsw-link009\_v2.0.2.zip**", unzip the file.

Connect MCU board to PC with USB cable.

Update USB driver:&#x20;

* 윈도우 > **장치 관리자 (WIN key + X) > 범용직렬버스장치> ST-Link Debug >드라이버 업데이트>**
* Select the folder where en-stsw-link009 is unzipped

{% hint style="info" %}
MCU board (STM32F411) must be connected to your PC to install the USB driver
{% endhint %}

![](<../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1).png>)

> What is ST Link utiliy? [https://m.blog.naver.com/ansdbtls4067/221510252896](https://m.blog.naver.com/ansdbtls4067/221510252896)

#### Check COM port for STM32F4xx&#x20;

Check the port number and settings in "device manager".

* baudrate: 9600
* data bits: 8
* parity: None
* Stop bit: 1
* Flow control: None

![image](https://user-images.githubusercontent.com/91526930/186338119-272e8119-cfc5-411e-bce7-7118e94aea96.png)

### Setup STM32F4xx in Arduino IDE

> Check the model number:  STM32F401RE  or F411RE

Arduino Setting for STM32F411RE

* Board: **Nucleo-64**
* Board part number: **Nucleo F411RE**
* Port:  COM port number of STM32 Board (장치관리자에서 확인)

<figure><img src="https://github.com/user-attachments/assets/d2c6d210-a968-49b8-a2a0-cc14f7ca956f" alt=""><figcaption></figcaption></figure>





### Setup for Upload Method: STM32CubeProgrammer(SWD)

#### Download & Install STM32CubeProgrammer 📥

Visit the official [STM32CubeProg download page](https://www.st.com/en/development-tools/stm32cubeprog.html).

Download and Install **STMCubeProgrammer Software**

> Choose the default settings.
>
> You may need to create a free ST account.



![STM32CubeProgrammer Installer](https://github.com/user-attachments/assets/15de450e-5e0a-476b-8f14-780054539ef2)

#### Set Up the Environment Path 🗺️

This step tells your computer where to find the program.

Open Environment Variables Tab:&#x20;

* **내 PC -> 속성 ->고급시스템설정->환경변수**
* In the Windows search bar, type "Edit the system environment variables"&#x20;

Edit the Path:&#x20;

* "System variables" (시스템변수) -->    Path ( double click) -->  새로만들기&#x20;
* Add Path: `C:\Program Files\STMicroelectronics\STM32Cube\STM32CubeProgrammer\bin`

Confirm: Click OK on all windows to close and save the changes.

<figure><img src="https://github.com/user-attachments/assets/1efb4c74-e3e2-4777-a841-3fee99bf5a1b" alt=""><figcaption></figcaption></figure>

#### Configure Arduino IDE Upload Setting 🔄

Restart Arduino IDE:&#x20;

> This is very important! Completely close and reopen the Arduino IDE for the new settings to take effect.



Upload Method: STM32CubeProgrammer (SWD)

<figure><img src="https://github.com/user-attachments/assets/f48ff05d-a16c-472b-b23a-8fa0f9c0bfa6" alt="" width="563"><figcaption></figcaption></figure>

## Example Code&#x20;

There is a simple example, that LED blinks every 1 sec.

On menu bar of Arduino IDE, select **File > Examples > 01.Basics > Blink**

{% code expandable="true" %}
```cpp
/*
  Blink
  Turns an LED on for one second, then off for one second, repeatedly.
*/

// The setup function runs once when you press reset or power the board
void setup() {
  // Initialize digital pin LED_BUILTIN as an output.
  pinMode(LED_BUILTIN, OUTPUT);
}

// The loop function runs over and over again forever
void loop() {
  digitalWrite(LED_BUILTIN, HIGH);   // Turn the LED on (HIGH is the voltage level)
  delay(1000);                       // Wait for a second (1000 milliseconds)
  digitalWrite(LED_BUILTIN, LOW);    // Turn the LED off by making the voltage LOW
  delay(1000);                       // Wait for a second
}

```
{% endcode %}

If you click **upload** button, the example code will be loaded on the MCU.

> If the program is loaded successfully then LED(LD1) will be green light.

![image](https://user-images.githubusercontent.com/91526930/186355557-1e9e137b-03f1-4c8b-8b87-05a4d1c87077.png)
