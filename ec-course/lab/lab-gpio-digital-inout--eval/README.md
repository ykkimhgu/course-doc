# LAB: GPIO Digital InOut 7-segment(eval board)

## LAB: GPIO Digital InOut 7-segment

**Date:** 2025-09-02

**Author/Partner:**

**Github:** repository link

**Demo Video:** Youtube link

**PDF version:**

## Introduction

In this lab, you are required to create a simple program to control a 7-segment display to show a decimal number (0\~9) that increases by pressing a push-button.

You must submit

* LAB Report (\*.pdf)
* Zip source files(lab\*\*\*.c, ecRCC2.h, ecGPIO2.h etc...).
  * Only the source files. Do not submit project files

#### Requirement

**Hardware**

* MCU
  * NUCLEO-F411RE
* Actuator/Sensor/Others:
  * eval board

**Software**

* Keil uVision, CMSIS, EC\_HAL library

## Exercise

Fill in the table

| **Port/Pin**    | **Description**              | **Register setting**                      |
| --------------- | ---------------------------- | ----------------------------------------- |
| Port B Pin 5    | Clear Pin5 mode              | GPIOB->MODER &=\~(3<<(5\*2))              |
| Port B Pin 5    | Set Pin5 mode = Output       | GPIOB->MODER \|=\_\_\_\_\_\_\_\_\_\_\_\_  |
| Port B Pin 6    | Clear Pin6 mode              | GPIOB->MODER &=\~\_\_\_\_\_\_\_\_\_\_\_   |
| Port B Pin 6    | Set Pin6 mode = Output       | GPIOB->MODER \|=\_\_\_\_\_\_\_\_\_\_\_\_  |
| Port B Pin Y    | Clear PinY mode              | GPIOB->MODER &=\~\_\_\_\_\_\_\_\_\_\_\_   |
| Port B Pin Y    | Set PinY mode = Output       | GPIOB->MODER \|=\_\_\_\_\_\_\_\_\_\_\_\_  |
| Port B Pin 5\~9 | Clear Pin5\~9 mode           | GPIOB->MODER &=\~\_\_\_\_\_\_\_\_\_\_\_   |
|                 | Set Pin5\~9 mode = Output    | GPIOB->MODER \|=\_\_\_\_\_\_\_\_\_\_\_\_  |
| Port X Pin Y    | Clear Pin Y mode             | GPIOX->MODER &=\~\_\_\_\_\_\_\_\_\_\_\_   |
|                 | Set Pin Y mode = Output      | GPIOX->MODER \|=\_\_\_\_\_\_\_\_\_\_\_\_  |
| Port B Pin5     | Set Pin5 otype=push-pull     | GPIOB->OTYPER =\_\_\_\_\_\_\_\_\_\_\_\_   |
| Port B PinY     | Set PinY otype=push-pull     | GPIOB-> OTYPER =\_\_\_\_\_\_\_\_\_\_\_\_  |
| Port B Pin5     | Set Pin5 ospeed=Fast         | GPIOB->OSPEEDR =\_\_\_\_\_\_\_\_\_\_\_\_  |
| Port B PinY     | Set PinY ospeed=Fast         | GPIOB-> OSPEEDR =\_\_\_\_\_\_\_\_\_\_\_\_ |
| Port B Pin 5    | Set Pin5 PUPD=no pullup/down | GPIOB->OTYPER =\_\_\_\_\_\_\_\_\_\_\_\_   |
| Port B Pin Y    | Set PinY PUPD=no pullup/down | GPIOB-> OTYPER =\_\_\_\_\_\_\_\_\_\_\_\_  |

***

## Problem 0: Check FND 7-Segment Display

### Procedure

Review 7-segment Decoder and Display from Digital Logic lecture.

* Read here: [7-segment tutorial](../../tutorial/tutorial-7segment-display.md)
* Read here: [How to connect 7-segment decoder to MCU](../../hardware/experiment-hardware/electronic-chips.md#7-segment-and-decoder)

#### 1. 7-segment display connection

First, connect the common anode 7-segment display with the given array resistors.

Then, apply VCC and GND to the 7-segment display.

Check that all LEDs of 7-segment work properly

* Give 'H' signal to each 7-segment pin of 'a'\~'g' . Observe if that LED is turned ON or OFF
* Example: Connect VCC to all 'a'\~'g' pins

#### 2. BCD 7-segment decoder connection

The popular BCD 7-segment decoder chip is **74LS47.** With the BCD chip, you need only 4 DOUT pins of MCU to display from 0 to 9.

Connect the decoder chip (**74LS47**) on the bread board.

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Then, Check that the decoder chip works properly

* Supply a combination of VCC/GND to the pins of 'A'\~'D' of the decoder.
* Check if the 7-segment LED display shows the correct number

#### Connection Diagram

Circuit diagram

> You need to include the circuit diagram in the report

![image](https://user-images.githubusercontent.com/38373000/192134563-72f68b29-4127-42ac-b064-2eda95a9a52a.png)

#### Discussion

1. Draw the truth table for the BCD 7-segment decoder with the 4-bit input.

> Answer discussion questions

```
** YOUR Truth-table  goes here**
```

2. What are the common cathode and common anode of 7-segment display?

> Answer discussion questions

3. Does the LED of a 7-segment display (common anode) pin turn ON when 'HIGH' is given to the LED pin from the MCU?

> Answer discussion questions

***

## Problem 2: Program FND-7-segment decoder

Instead of using the decoder chip, we are going to make the 7-segment decoder with the MCU programming.

> Do not use the 7-segment decoder for this problem

<figure><img src="../../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

### Procedure

1. Use the same project and source file.
2. Include your updated library in `\repos\EC\include\` to your project.

* **ecGPIO2.h, ecGPIO2.c**
* **ecRCC2.h, ecRCC2.c**

3. Declare and Define the following functions in your library

```c
void Seven_Seg_FND_init(void); 
void Seven_Seg_FND_display(uint8_t num, uint8_t LED);
```

* num: 0 to 9999 only (unsigned)

4. Configure and connect the MCU to the circuit
5. First, check that every number, 0 to 9, can be displayed properly
6. Then, create a code that increases the displayed number from 0 to 30 with each button press.
   * After the number '30', it should start from '0' again.

### Configuration

Configure the MCU

| Digital In for Button (B1) | Digital Out for 7-Segment                                                 |
| -------------------------- | ------------------------------------------------------------------------- |
| Digital In                 | Digital Out                                                               |
| PA4                        | PB7,PB6,PB5,PB4,PB3,PB2,PB1,PB0<br>('a'~'h', respectively)<br>PC3,PC4,PA11,PA10<br>('LED1'~'LED4', respectively)                                                    |
| PULL-UP                    | Push-Pull, No Pull-up-Pull-down, Medium Speed                             |

### Code

Your code goes here: [ADD Code LINK such as github](https://github.com/ykkimhgu/EC-student/)

* [Example Code for MCU configuration](https://github.com/ykkimhgu/EC-student/blob/main/tutorial/tutorial-student/TU_GPIO_LED_7segment_student.c)
* [Example code of 7-segment decoder control](https://os.mbed.com/users/ShingyoujiPai/code/7SegmentDisplay/file/463ff11d33fa/main.cpp/)

> Explain your source code with the necessary comments.

```
// YOUR MAIN CODE ONLY
// YOUR CODE
```

### Connection Diagram

Circuit diagram

> You need to include the circuit diagram

![image](https://user-images.githubusercontent.com/38373000/192134563-72f68b29-4127-42ac-b064-2eda95a9a52a.png)

### Results

Experiment images and results

> Show experiment images /results

Add [demo video link](../../../course/lab/link/)

### Discussion

Analyze the result and explain any other necessary discussion.

***


## Problem 1: Display a Number with Button Press

### Procedure

1. Create a new project under the directory `\repos\EC\lab\LAB_GPIO_7segment`

* The project name is “**LAB\_GPIO\_7segment”.**
* Create a new source file named as “**LAB\_GPIO\_7segment.c”**
*   Refer to the [sample code](https://github.com/ykkimhgu/EC-student/tree/main/tutorial/tutorial-student)

    > You MUST write your name on the source file inside the comment section.

2. Include your updated library in `\repos\EC\include\` to your project.

* **ecGPIO2.h, ecGPIO2.c**
* **ecRCC2.h, ecRCC2.c**

3. Declare and Define the following functions in your library

```c
void sevensegment_display_init(PinNames_t pinNameA, PinNames_t pinNameB, PinNames_t pinNameC, PinNames_t pinNameD); 
void sevensegment_display(uint8_t  num);
```

* num: 0 to 9 only (unsigned)

4. Configure and connect the MCU to the circuit
5. First, check that every number, 0 to 9, can be displayed properly
6. Then, create a code that increase the displayed number from 0 to 9 with each button press.
   * After the number '9', it should start from '0' again.

### Configuration

Configure the MCU

| Digital In for Button (B1) | Digital Out for 7-Segment                     |
| -------------------------- | --------------------------------------------- |
| Digital In                 | Digital Out                                   |
| PC13                       | <p>PA7, PB6, PC7, PA9<br></p>                 |
| PULL-UP                    | Push-Pull, No Pull-up-Pull-down, Medium Speed |

#### Code

[**Sample Code**](https://ykkim.gitbook.io/ec/stm32-m4-programming/example-code#seven-segment).

```c
#include "stm32f4xx.h"
#include "ecGPIO2.h"
#include "ecRCC2.h"

#define LED_PIN PA_5
#define BUTTON_PIN PC_13

void setup(void);
	
int main(void) { 
	// Initialiization --------------------------------------------------------
	setup();
	unsigned int cnt = 0;
	
	// Inifinite Loop ----------------------------------------------------------
	while(1){
		sevensegment_display(cnt % 10);
		if(GPIO_read(BUTTON_PIN) == 0) cnt++; 
        if (cnt > 9) cnt = 0;
		for(int i = 0; i < 500000;i++){}  // delay_ms(500);
	}
}


// Initialiization 
void setup(void)
{
	RCC_HSI_init();	
	GPIO_init(BUTTON_PIN, INPUT);  // calls RCC_GPIOC_enable()
	sevensegment_display_init(PA_7, PB_6, PC_7, PA_9);  // Decoder input A,B,C,D
}
```

Your code goes here: [ADD Code LINK such as github](https://github.com/ykkimhgu/EC-student/)

> Explain your source code with necessary comments.

```
// YOUR MAIN CODE ONLY
// YOUR CODE
```

### Results

Experiment images and results

> Show experiment images /results

Add [demo video link](../../../course/lab/link/)

### Discussion

Analyze the result and explain any other necessary discussion.

***

## Problem 2: Program BCD-7-segment decoder

Instead of using the decoder chip, we are going to make the 7-segment decoder with the MCU programming.

> Do not use the 7-segment decoder for this problem

<figure><img src="../../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

### Procedure

1. Use the same project and source file.
2. Include your updated library in `\repos\EC\include\` to your project.

* **ecGPIO2.h, ecGPIO2.c**
* **ecRCC2.h, ecRCC2.c**

3. Declare and Define the following functions in your library

```c
void sevensegment_decoder_init(void); 
void sevensegment_decoder(uint8_t  num);
```

* num: 0 to 9 only (unsigned)

4. Configure and connect the MCU to the circuit
5. First, check that every number, 0 to 9, can be displayed properly
6. Then, create a code that increases the displayed number from 0 to 9 with each button press.
   * After the number '9', it should start from '0' again.

### Configuration

Configure the MCU

| Digital In for Button (B1) | Digital Out for 7-Segment                                                 |
| -------------------------- | ------------------------------------------------------------------------- |
| Digital In                 | Digital Out                                                               |
| PC13                       | <p>PA5, PA6, PA7, PB6, PC7, PA9, PA8, PB10<br>('a'~'h', respectively)</p> |
| PULL-UP                    | Push-Pull, No Pull-up-Pull-down, Medium Speed                             |

### Code

Your code goes here: [ADD Code LINK such as github](https://github.com/ykkimhgu/EC-student/)

* [Example Code for MCU configuration](https://github.com/ykkimhgu/EC-student/blob/main/tutorial/tutorial-student/TU_GPIO_LED_7segment_student.c)
* [Example code of 7-segment decoder control](https://os.mbed.com/users/ShingyoujiPai/code/7SegmentDisplay/file/463ff11d33fa/main.cpp/)

> Explain your source code with the necessary comments.

```
// YOUR MAIN CODE ONLY
// YOUR CODE
```

### Connection Diagram

Circuit diagram

> You need to include the circuit diagram

![image](https://user-images.githubusercontent.com/38373000/192134563-72f68b29-4127-42ac-b064-2eda95a9a52a.png)

### Results

Experiment images and results

> Show experiment images /results

Add [demo video link](../../../course/lab/link/)

### Discussion

Analyze the result and explain any other necessary discussion.

***

## Reference

Complete list of all references used (github, blog, paper, etc)

```
```

***

## Troubleshooting

(Option) You can write Troubleshooting section
