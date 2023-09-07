# Tutorial: mbed-Part 1

## Overview

In this tutorial, you will use ‘mbed’ online compiler to handle several peripherals of MCU (ARM-Cortex M4). Using the given platform, you will perform some tasks about GPIO, timer and interrupt.

The objectives of this lab are

* Practice to use ‘mbed’ library in online compiler.
* Understand digital in/out peripheral in MCU.
* Understand timer and interrupt function of MCU.
* Handle GPIO, timer and interrupt using ‘mbed’ library.

## Hardware

![​ Figure 1. Pin configuration for NUCLEO-F411RE board (same pin configuration with NUCLE-F401Re)](https://user-images.githubusercontent.com/79825525/129155781-83639c1d-bb1f-4cc9-b3d5-3a080426d382.jpg)

## Tutorial

## Creating account

Open [https://www.mbed.com/en/](https://www.mbed.com/en/)

Create new account

![image-20210811204723487](https://user-images.githubusercontent.com/79825525/129155672-82f51b2d-1bdd-4016-a4f6-3ffd60923ec9.png)

If you already have an account, then click on **'Compiler'**

![](<../../.gitbook/assets/image (17).png>)

### \*\*\*\*

## **Creating New Program**

On menu bar, \*\*\*\* select **New>Create New Program.**

For the first project, it can ask for ‘add new platform’.

#### \*\*\*\*

### **Hardware Setting**

For the Platform, select your MCU board.

1. For NUCLEO-F411RE:

* Click ‘**Add Platform**’ > Search for ‘**NUCLEO-F411RE**’ board
* Click ‘**Add to your Mbed Compiler**’ and ‘**Open Mbed Complier**’

![add\_platform](https://user-images.githubusercontent.com/79825525/129156475-64577741-2f1d-4a5d-9872-d7a27abe9b8e.png)

![mbed\_compiler](https://user-images.githubusercontent.com/79825525/129157119-ac6bd034-428c-4981-80be-7b7b0cbbcd9c.png)

**2.** For NUCLEO-F401RE:

* Click ‘**Add Platform**’ > Search for ‘**NUCLEO-F401RE**’ board
* Click ‘**Add to your Mbed Compiler**’ and ‘**Open Mbed Complier**’

![401](https://user-images.githubusercontent.com/79825525/129155462-259c330a-493f-478f-a050-3acf474d7708.png)

### New program

Set the Program Name as: **‘Tutorial1\_GPIO\_LED’**.

![](<../../.gitbook/assets/image (2) (1) (1).png>)

Right-Click on the Program name in **Program Workspace.** Then, create new file. Name the source file as ‘**main.cpp’**.

![](<../../.gitbook/assets/image (22).png>)

## Import 'mbed' Library

Right-Click on the Program name in **Program Workspace.** Select **Import Library.**

On the **Import Wizard Window > Libraries** tab : Search 'mbed'. Choose the mbed created by 'mbed\_official'.

![](<../../.gitbook/assets/image (20).png>)

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

Then, compile the program by clicking on ‘**Compile’** button.

![compile](https://user-images.githubusercontent.com/79825525/129156625-5a31b30a-51b4-4b34-b229-4c14fd642b29.png)

If the compilation is successful, the binary file of the project "\*\*.bin" will be created and downloaded on your computer.

![](<../../.gitbook/assets/image (21).png>)

## Import to MCU

Connect the MCU board to your PC via USB cable and check if a new memory drive of “NODE\_F401RE (E:)” is created in your computer.

To load the binary program onto the MCU, copy the downloaded binary file to the drive “NODE\_F401RE (E:)”.

If the program is loaded successfully then LED(LD1) will be green light.

> Click reset button on MCU if nothing happens'

#### \*\*\*\*

#### \*\*\*\*

#### \*\*\*\*
