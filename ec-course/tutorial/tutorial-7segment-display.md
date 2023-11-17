# Tutorial: 7-Segment Display

##

## Overview

Display decimal number (0\~9) on a 7-segment display

* Inputs: 4-bit numbers (D C B A) // (00001111)
* Output:
  * 7-segment decoder: 7-bit numbers ( a to g)
  * 7-segment display: decimal number 0\~9

#### Hardware Specification

* 7-segment display: common anode (5101ASR)
* 7-segment decoder: [74LS47N (datasheet download)](https://pdf1.alldatasheet.com/datasheet-pdf/download/5724/MOTOROLA/SN74LS47N.html)

Hardware

### 7-segment display (5101ASR)

Detail information about 7 segment display - [click here](https://www.electronics-tutorials.ws/combination/comb\_6.html)

* Common anode: (common pin is connected to VCC)
* Giving ‘LOW’ to the pin -> LED ON
* Needs a load resistor for each pin

![7-segment circuit](https://user-images.githubusercontent.com/91526930/192131110-2fc8e880-10ef-4034-a4a2-9de59ecd42c1.png)

![image](https://user-images.githubusercontent.com/91526930/192942501-63b87284-7c94-4863-8200-106baa02b907.png)

Please check the difference between the common cathode and common anode.

> We will use common anode.

###

### BCD 7-segment decoder

Model: [74LS47N (datasheet download)](https://pdf1.alldatasheet.com/datasheet-pdf/download/5724/MOTOROLA/SN74LS47N.html)

* All output pins are active low

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt="" width="375"><figcaption></figcaption></figure>

###

### Array resistor (B331J)

![array resistor](https://user-images.githubusercontent.com/91526930/192131231-c6ae1c48-a236-43f8-9577-010ccd46eccc.png)

## Circuit Configuration

### Connecting with BCD 7-segment decoder

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

### Connecting 7-segment without decoder

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

![circuit on breadbord](https://user-images.githubusercontent.com/91526930/192194707-c62df336-9869-4de1-9d72-cb2355166989.png)

## Code

Tutorial code : "TU\_GPIO\_LED\_7segment\_student.c" [here](https://github.com/ykkimhgu/EC-student/tree/main/tutorial/tutorial-student)

* Creating 7-segment decoder (without using decoder chip)

If you want to use 7-segment decoder chip: [read about 7-segment and Decoder](https://ykkim.gitbook.io/ec/stm32-m4-programming/hardware/electronic-chips#7-segment-and-decoder)

```cpp
#include "stm32f4xx.h"
#include "ecRCC.h"
#include "ecGPIO.h"

void setup(void);
	
int main(void) {	
	// Initialiization --------------------------------------------------------
	setup();
	
	// Inifinite Loop ----------------------------------------------------------
	while(1){
		GPIO_write(GPIOA, 5, LOW);
		GPIO_write(GPIOA, 6, LOW);
		GPIO_write(GPIOA, 7, HIGH);
		GPIO_write(GPIOB, 6, HIGH);
		GPIO_write(GPIOC, 7, HIGH);
		GPIO_write(GPIOA, 9, LOW);
		GPIO_write(GPIOA, 8, LOW);
		GPIO_write(GPIOB, 10, LOW);
	}
}

void setup(void){
	RCC_HSI_init();
	GPIO_init(GPIOA, 5, OUTPUT);
	GPIO_init(GPIOA, 6, OUTPUT);
	GPIO_init(GPIOA, 7, OUTPUT);
	GPIO_init(GPIOB, 6, OUTPUT);
	GPIO_init(GPIOC, 7, OUTPUT);
	GPIO_init(GPIOA, 9, OUTPUT);
	GPIO_init(GPIOA, 8, OUTPUT);
	GPIO_init(GPIOB, 10, OUTPUT);
}
```
