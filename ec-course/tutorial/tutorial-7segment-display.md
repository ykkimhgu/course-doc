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

Detail information about 7 segment display - [click here](https://www.electronics-tutorials.ws/combination/comb_6.html)

* Common anode: (common pin is connected to VCC)
* Giving ‘LOW’ to the pin -> LED ON
* Needs a load resistor for each pin

![7-segment circuit](https://user-images.githubusercontent.com/91526930/192131110-2fc8e880-10ef-4034-a4a2-9de59ecd42c1.png)

![image](https://user-images.githubusercontent.com/91526930/192942501-63b87284-7c94-4863-8200-106baa02b907.png)

![image](https://github.com/user-attachments/assets/5ca0e7ba-2620-4615-9aea-07aaf7e2aadb)

Please check the difference between the common cathode and common anode.

> We will use common anode.

###

### BCD 7-segment decoder

Model: [74LS47N (datasheet download)](https://pdf1.alldatasheet.com/datasheet-pdf/download/5724/MOTOROLA/SN74LS47N.html)

* All output pins are active low

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

###

### Array resistor (B331J)

![array resistor](https://user-images.githubusercontent.com/91526930/192131231-c6ae1c48-a236-43f8-9577-010ccd46eccc.png)

## Circuit Configuration

### Connecting with BCD 7-segment decoder

![image](https://github.com/user-attachments/assets/9f83a231-a63f-415f-b22c-07ebabcc86b6)

![image](https://github.com/user-attachments/assets/26b5a3a5-b0f0-406b-801a-4c0fd7cc930b)

### Connecting 7-segment without decoder

![image](https://github.com/user-attachments/assets/5ca0e7ba-2620-4615-9aea-07aaf7e2aadb)

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

![circuit on breadbord](https://user-images.githubusercontent.com/91526930/192194707-c62df336-9869-4de1-9d72-cb2355166989.png)

## Code

Tutorial code : "TU\_GPIO\_LED\_7segment\_student.c" [here](https://github.com/ykkimhgu/EC-student/tree/main/tutorial/tutorial-student)

* Creating 7-segment decoder (without using decoder chip)

If you want to use 7-segment decoder chip: [read about 7-segment and Decoder](https://ykkim.gitbook.io/ec/stm32-m4-programming/hardware/electronic-chips#7-segment-and-decoder)

```cpp
#include "stm32f4xx.h"
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
		GPIO_write(PA_7, HIGH);
		GPIO_write(PB_6, HIGH);
		GPIO_write(PC_7, HIGH);
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


## 7-Segment Display (eval board)

### Overview

Display decimal number (0~9) on a 7-segment display (JKIT - Nucleo 64)

* Inputs: display number, choosing 7-segment // ex) 9, 3 -> choosing 7-segment 3, display 9
* Outputs:
  * 7-segment display: decimal number 0~9
  * Using 7-segment: The selected one of the four 7-segments
  * If you want to display different numbers on the four 7-segments, you need to use a very short delay to display the numbers

## Hardware Specification

* 7-Segment Display (JKIT - Nucleo 64): [link](https://www.devicemart.co.kr/goods/view?no=14123215&srsltid=AfmBOooT3zgaxGXZ_q0fvy4uZxHesTbwpqNprE6P3YXMk9V0EGoGrhLM)

<img src="https://raw.githubusercontent.com/LeeJunjae1/EC_22000573/main/img/connect.jpg" alt="Connection" style="zoom: 7%;" />







## 7-Segment Display (JKIT - Nucleo 64)

* Connect directly to the STM board

* Giving 'High' to the pin -> LED on



<img src="https://raw.githubusercontent.com/LeeJunjae1/EC_22000573/main/img/7seg.png" alt="config" style="zoom:33%;" />

<img src="https://raw.githubusercontent.com/LeeJunjae1/EC_22000573/main/img/LED.png" alt="LED Choose" style="zoom:33%;" />



| Digital Out for 7-Segment(number)              | Digital Out for 7-Segment(LED)                |
| ---------------------------------------------- | --------------------------------------------- |
| Digital Out                                    | Digital Out                                   |
| PB_7, PB_6, PB_5, PB_4, PB_3, PB_2, PB_1, PB_0 | PC_3, PC_4, PA_11, PA_10                      |
| Push-Pull, No Pull-up-Pull-down, Medium Speed  | Push-Pull, No Pull-up-Pull-down, Medium Speed |



## Code

Tutorial code:

```c	
#include "../../include/ecGPIO2.h"
#include "../../include/ecSysTick2.h"
#include "stm32f4xx.h"
#include "../../include/ecRCC2.h"
#include "../../include/ecEXTI2.h"


void setup(void);
void Seven_Seg_FND_init(void);
void Seven_Seg_FND_display(long long num);

int main(void) {
	setup();

	while (1) {
		Seven_Seg_FND_display(8);
		

	}
}

void Seven_Seg_FND_init(void){
	
	//pinname array
	char PinName[12]={PB_7, PB_6, PB_5, PB_4, PB_3, PB_2, PB_1, PB_0, PC_3, PC_4, PA_11, PA_10};
	
    //fill output setting
	
}

void Seven_Seg_FND_display(long long num) {

}

// Initialiization
void setup(void) {
	RCC_PLL_init();
	SysTick_init(1000);

	// initialize the pushbutton pin as an input:
	Seven_Seg_FND_init();
}
```

