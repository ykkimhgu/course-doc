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

* PlatformIO, CMSIS, EC\_HAL library

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

## Problem 0: Prelab

### Procedure

Complete the Tutorial: 7-segment Display.

{% embed url="https://ykkim.gitbook.io/ec/ec-course/tutorial/tutorial-7segment-display#option-3.-without-using-a-7-segment-decoder-on-jkit-evaluation-board" %}

You must check the 7-segment display can show all the number from 0 to 9.

* Give 'HIGH' signal to each 7-segment pin of 'a'\~'g'
* Observe if that LED is turned ON or OFF
* Check another 7-segment display leds
  * Example: Connect VCC to all 'a'\~'g' pins

Complete the required functions that displays numbers on 7-segment FND.

These functions must be moved to `ecGPIO2.h,ecGPIO2.c`

Update your library header

* **ecGPIO2.h, ecGPIO2.c**

```c
// Initialize 7 DOUT pins for 7 segment leds
void seven_seg_FND_init(void); 

// Select display: 0 to 3
// Display a number 0 - 9 only
void seven_seg_FND_display(uint8_t  num, uint8_t select);

```

## Problem 1: Display a Number with Button Press <a href="#problem-1-display-a-number-with-button-press" id="problem-1-display-a-number-with-button-press"></a>

### Procedure <a href="#procedure-1" id="procedure-1"></a>

Create a new project under the directory `\EC\lab\`

* The project name is “**LAB\_GPIO\_7segment”.**
* Create a new source file named as “**LAB\_GPIO\_7segment.c”**
* Update `platformio.ini` for VS.Code : [Read here for detail](../../tutorial/tutorial-platformio-in-vscode.md)

\
Create a code that increases the displayed number from 0 to 9 with each button press.

* After the number '9', it should start from '0' again.

***

### Configuration

Configure the MCU GPIO

| Function                 | Port - Pin                                                                                                          | Configuration                                 |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------- | --------------------------------------------- |
| **Button (SW2) on JKIT** | PA\_4                                                                                                               | Pull-Up                                       |
| **7-Segment DOUT**       | <p>PB7,PB6,PB5,PB4,PB3,PB2,PB1,PB0('a'~'h', respectively)<br>PC3,PC4,PA11,PA10<br>('FND_0'~FND_3, respectively)</p> | Push-Pull, No Pull-up-Pull-down, Medium Speed |

####

### Code

[**Sample Code**](https://ykkim.gitbook.io/ec/stm32-m4-programming/example-code#seven-segment).

```c
#include "stm32f4xx.h"
#include "ecGPIO2.h"
#include "ecRCC2.h"

#define BUTTON_PIN PA_4

void setup(void){
    // Intialize System Clock
    RCC_HSI_init();
    GPIO_init(BUTTON_PIN, INPUT);  // calls RCC_GPIOC_enable()
    // and Others
    // [YOUR CODE GOES HERE]    
    seven_seg_FND_init(); 
};

int main(void) {
    setup();
    uint8 numDisplay=8;
    uint8 selectFND=0;

    while (1) {
        // [YOUR CODE GOES HERE]    
        seven_seg_FND_display(numDisplay,selectFND);
        // [YOUR CODE GOES HERE]    
        // [YOUR CODE GOES HERE]    
    }
}
	


```

Your code goes here: [ADD Code LINK such as github](https://github.com/ykkimhgu/EC-student/)

> Explain your source code with necessary comments.

```
// YOUR MAIN CODE ONLY
// YOUR CODE
```

Your code goes here: [ADD Code LINK such as github](https://github.com/ykkimhgu/EC-student/)

* [Example Code for MCU configuration](https://github.com/ykkimhgu/EC-student/blob/main/tutorial/tutorial-student/TU_GPIO_LED_7segment_student.c)
* [Example code of 7-segment decoder control](https://os.mbed.com/users/ShingyoujiPai/code/7SegmentDisplay/file/463ff11d33fa/main.cpp/)

> Explain your source code with the necessary comments.

```
// YOUR MAIN CODE ONLY
// YOUR CODE
```

### Connection Diagram

Circuit diagram (if needed)

> You need to include the circuit diagram

![image](https://user-images.githubusercontent.com/38373000/192134563-72f68b29-4127-42ac-b064-2eda95a9a52a.png)

### Results

Experiment images and results

> Show experiment images /results

Add [demo video link](https://github.com/ykkimhgu/course-doc/blob/master/course/lab/link/README.md)

### Discussion

1. Analyze the result and explain any other necessary discussion.
2. Draw the truth table for the BCD 7-segment decoder with the 4-bit input.

> Answer discussion questions

```
** YOUR Truth-table  goes here**
```

3. What are the common cathode and common anode of 7-segment display?

> Answer discussion questions

4. Does the LED of a 7-segment display (common anode) pin turn ON when 'HIGH' is given to the LED pin from the MCU?

> Answer discussion questions

***

## Reference

Complete list of all references used (github, blog, paper, etc)

```
```

***

## Troubleshooting

(Option) You can write Troubleshooting section
