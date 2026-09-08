# Tutorial: 7-Segment Display

## Introduction

We will learn how to display a decimal number (0\~9) on a 7-segment display in STM32F4

* Inputs:
  * Decimal number : 0\~9
  * Binary: 4-bit numbers \[D C B A] // (0000\~ 1001)
* Output:
  * 7-segment decoder: 7-bit numbers ( a to g)
  * 7-segment display: decimal number 0\~9

We will learn how to configure the 7-segment display for different options

* Option 1. With 7-segment decoder
* Option 2. Without using 7-segment decoder
* Option 3. Without using 7-segment decoder on JKIT evaluation board

## Hardware Description

#### 7-segment display (5101ASR)

For more detail information about 7 segment display - [click here](https://www.electronics-tutorials.ws/combination/comb_6.html)

* Common anode: (common pin is connected to VCC)
* Giving ‘LOW’ to the pin -> LED ON
* Needs a load resistor for each pin(led)

Check the difference between the common cathode and common anode.

> We will use common anode.

<figure><img src="../../../.gitbook/assets/image (165).png" alt="" width="375"><figcaption></figcaption></figure>

####

#### BCD 7-segment decoder : [74LS47N (datasheet download)](https://pdf1.alldatasheet.com/datasheet-pdf/download/5724/MOTOROLA/SN74LS47N.html)

<figure><img src="../../../.gitbook/assets/image (166).png" alt=""><figcaption></figcaption></figure>

#### Array resistor (B331J)

<img src="https://user-images.githubusercontent.com/91526930/192131231-c6ae1c48-a236-43f8-9577-010ccd46eccc.png" alt="array resistor" width="188">

## Option 1.  With a BCD 7-segment decoder

Connect the 7-segment decoder with the 7-segment display as shown below.



<figure><img src="../../../.gitbook/assets/image (167).png" alt=""><figcaption></figcaption></figure>



## Option 2. Without a 7-segment decoder

### Configuration

Connect the MCU to a 7segment display directly, without using a decoder chip.

> MUST connect load resistors as shown in the figure.

<table><thead><tr><th>Function</th><th width="386.3333740234375">2. Digital Out: Select 7-Segment display</th><th>Configuration</th></tr></thead><tbody><tr><td>7-Segment DOUT</td><td>PA_5, PA_6, PB_6, PC_7, PA_9, PA_8, PB10</td><td>DOUT, Push-Pull</td></tr></tbody></table>



![circuit on breadbord](https://user-images.githubusercontent.com/91526930/192194707-c62df336-9869-4de1-9d72-cb2355166989.png)

### Example Code

Create a new environment and source file named as

* Environment: “**TU\_GPIO\_7segment”.**
* Source File: “**TU\_GPIO\_7segment.c”**



Copy the tutorial code

* It is a simple program that turns on a 7-segment display to show '8'
* You can change any of GPIO outputs to display a different number

```cpp
#include "ecRCC2.h"
#include "ecGPIO2.h"

void setup(void);
	
int main(void) {	
	// Initialiization --------------------------------------------------------
	setup();
	
	// Inifinite Loop ----------------------------------------------------------
	while(1){
		GPIO_write(PA_5, LOW);
		GPIO_write(PA_6, LOW);
		GPIO_write(PA_7, LOW);
		GPIO_write(PB_6, LOW);
		GPIO_write(PC_7, LOW);
		GPIO_write(PA_9, LOW);
		GPIO_write(PA_8, LOW);
		GPIO_write(PB_10, LOW);
	}
}

void setup(void){
	RCC_HSI_init();
	GPIO_init(PA_5, OUTPUT);
	GPIO_init(PA_6, OUTPUT);
	GPIO_init(PA_7, OUTPUT);
	GPIO_init(PB_6, OUTPUT);
	GPIO_init(PC_7, OUTPUT);
	GPIO_init(PA_9, OUTPUT);
	GPIO_init(PA_8, OUTPUT);
	GPIO_init(PB_10, OUTPUT);
}
```



