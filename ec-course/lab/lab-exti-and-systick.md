# LAB: EXTI & SysTick

**Date:** 2023-09-26

**Author/Partner:**

**Github:** repository link

**Demo Video:** Youtube link

**PDF version:**

## Introduction

In this lab, you are required to create two simple programs using interrupt:

(1) displaying the number counting from 0 to 9 with Button Press

(2) counting at a rate of 1 second

You must submit

* LAB Report (\*.md & \*.pdf)
* Zip source files(main\*.c, ecRCC2.h, ecGPIO2.h, ecSysTick2.c etc...).
  * Only the source files. Do not submit project files

### Requirement

#### Hardware

* MCU
  * NUCLEO-F411RE
* Actuator/Sensor/Others:
  * 4 LEDs and load resistance
  * 7-segment display(5101ASR)
  * Array resistor (330 ohm)
  * breadboard

#### Software

* Keil uVision, CMSIS, EC\_HAL library

## Tutorial: STM-Arduino

{% embed url="https://ykkim.gitbook.io/ec/ec-course/tutorial/tutorial-arduino-stm32/tutorial-arduino-stm32-part1#external-interrupt" %}

We are going to create a simple program that turns LED(LD2) on triggered by **External Interrupt** of user button(BT1)/

###

#### [attachInterrupt()](https://www.arduino.cc/reference/en/language/functions/external-interrupts/attachinterrupt/) <a href="#attachinterrupt" id="attachinterrupt"></a>

```c
attachInterrupt(digitalPinToInterrupt(pin), ISR, mode)
```

* `digitalPinToInterrupt(pin)`: translate the digital pin to the specific interrupt number.
* `ISR`: a function called whenever the interrupt occurs.
* `mode`: defines when the interrupt should be triggered. (LOW, CHANGE, RISING, FALLING)

###

### Procedure

1. Create a new project under the directory `\EC\LAB\LAB_EXTI`
2. Open _Arduino IDE_ and Create a new program named as ‘**TU\_arduino\_EXTINT.ino**’.
3. Write the following code.

```c
const int btnPin = 3;
const int ledPin =  13; 

int btnState = HIGH;

void setup() {
    pinMode(ledPin, OUTPUT);
    pinMode(btnPin, INPUT_PULLUP);
    attachInterrupt(digitalPinToInterrupt(btnPin), blink, CHANGE);
}

void loop() {
    // blank
}


void blink(){
    btnState = digitalRead(btnPin);

    if (btnState == HIGH)
        digitalWrite(ledPin, LOW);
    else 
        digitalWrite(ledPin, HIGH);
}
```



> The user button pin is `PC13`, but this pin cannot be used in arduino. So, you should connect `PC13` to `pinName` `D3` by using wire.

<figure><img src="https://ykkim.gitbook.io/~gitbook/image?url=https%3A%2F%2Fuser-images.githubusercontent.com%2F91526930%2F186584565-3dc47e19-e5c5-43a2-b4c4-d4de8916a2c8.png&#x26;width=300&#x26;dpr=4&#x26;quality=100&#x26;sign=ff32bff&#x26;sv=1" alt="" width="375"><figcaption></figcaption></figure>

4. Click on **upload** button.
5. Whenever the user button(BT1) is pressed (at fall), LED should be ON. When the button is released, the LED should be OFF.



***

## Tutorial: STM32F4xx
### 1.Tutorial: Managing library header files
Read how to manage library header files for MCU register configurations. Apply it in your LAB.

{% embed url="https://ykkim.gitbook.io/ec/ec-course/tutorial/tutorial-library-header-files#ec-2024" %}


### 2.Tutorial: Custom Initialization
Instead of writing initial setting functions for each registers, you can call a user defined function e.g. `MCU_init()` for the commonly used default initialization.
Follow the tutorial and apply it in your LAB. 

{% embed url="https://ykkim.gitbook.io/ec/ec-course/tutorial/tutorial-custom-initialization" %}


---

## Problem 1: Counting numbers on 7-Segment using EXTI Button

### 1-1. Create HAL library

1. [Download sample header files](https://github.com/ykkimhgu/EC-student/tree/main/include/lib-student): **ecEXTI2\_student.h, ecEXTI2\_student.c**
2. Rename these files as **ecEXTI2.h, ecEXTI2.c**
   * You MUST write your name and other information at the top of the library code files.
   * Save these files in your directory `EC \lib\`.
3. Declare and define the following functions in your library : **ecEXTI2.h**

**ecEXTI.h**

```c
void EXTI_init(PinName_t pinName, int trig_type, int priority);
void EXTI_enable(uint32_t pin);  // mask in IMR
void EXTI_disable(uint32_t pin);  // unmask in IMR
uint32_t  is_pending_EXTI(uint32_t pin);
void clear_pending_EXTI(uint32_t pin);
```

### 1-2. Procedure

1. Create a new project under the directory `\EC\LAB\LAB_EXTI`

* The project name is “**LAB\_EXTI”.**
* Create a new source file named as “**LAB\_EXTI.c”**

> You MUST write your name on the source file inside the comment section.

2\. Include your updated library in `\EC\lib\` to your project.

* **ecGPIO2.h, ecGPIO2.c**
* **ecRCC2.h, ecRCC2.c**
* **ecEXTI2.h, ecEXTI2.c**

3.  Use the decoder chip (**74LS47**). Connect it to the breadboard and 7-segment display.

    > Then, you need only 4 Digital out pins of MCU to display from 0 to 9.
4. First, check if every number, 0 to 9, can be displayed properly on the 7-segment.
5. Then, create a code to display the number counting from 0 to 9 and repeating.
   * Count up only by pressing the push button (External Interrupt)
6. You must use your library function of EXTI.
7. Refer to an [sample code](https://ykkim.gitbook.io/ec/firmware-programming/example-code#button-interrupt)

### Configuration

| Digital In for Button (B1) | Digital Out for 7-Segment decoder           |
| -------------------------- | ------------------------------------------- |
| Digital In                 | Digital Out                                 |
| PC13                       | <p>PA7, PB6, PC7, PA9<br></p>               |
| PULL-UP                    | Push-Pull, No PullUp-PullDown, Medium Speed |

### Circuit Diagram

> You need to include the circuit diagram

![image](https://user-images.githubusercontent.com/38373000/192134563-72f68b29-4127-42ac-b064-2eda95a9a52a.png)

### Discussion

1. We can use two different methods to detect an external signal: polling and interrupt. What are the advantages and disadvantages of each approach?

> Answer discussion questions

2.  What would happen if the EXTI interrupt handler does not clear the interrupt pending flag? Check with your code

    > Answer discussion questions

### Code

Your code goes here.

Explain your source code with the necessary comments.

```c
// YOUR MAIN CODE ONLY
// YOUR CODE
```

### Results

Experiment images and results go here

> Show experiment images /results

Add [demo video link](link/)

## Problem 2: Counting numbers on 7-Segment using SysTick

Display the number 0 to 9 on the 7-segment LED at the rate of 1 sec. After displaying up to 9, then it should display ‘0’ and continue counting.

When the button is pressed, the number should be reset ‘0’ and start counting again.

### 2-1. Create HAL library

1. [Download sample header files](https://github.com/ykkimhgu/EC-student/tree/main/include/lib-student): **ecSysTick\_student.h, ecSysTick\_student.c**
2. Rename these files as **ecSysTick2.h, ecSysTick2.c**
   * You MUST write your name and other information at the top of the library code files.
   * Save these files in your directory `EC \lib\`.
3. Declare and define the following functions in your library : **ecSysTick2.h**

**ecSysTick.h**

```c
void SysTick_init(uint32_t msec);
void delay_ms(uint32_t msec);
uint32_t SysTick_val(void);
void SysTick_reset (void);
void SysTick_enable(void);
void SysTick_disable (void)
```

### 2-2. Procedure

1.  Create a new project under the directory

    `\EC\LAB\LAB_EXTI_SysTick`

* The project name is “**LAB\_EXTI\_SysTick”.**
* Create a new source file named as “**LAB\_EXTI\_SysTick.c”**

> You MUST write your name on the source file inside the comment section.

2\. Include your updated library in `\EC\lib\` to your project.

* **ecGPIO2.h, ecGPIO2.c**
* **ecRCC2.h, ecRCC2.c**
* **ecEXTI2.h, ecEXTI2.c**
* **ecSysTick2.h, ecSysTick2.c**

3.  Use the decoder chip (**74LS47**). Connect it to the bread board and 7-segment display.

    > Then, you need only 4 Digital out pins of MCU to display from 0 to 9.
4. First, check if every number, 0 to 9, can be displayed properly on the 7-segment.
5. Then, create a code to display the number counting from 0 to 9 and repeats at the rate of 1 second.
6.  When the button is pressed, it should start from '0' again.

    > Use EXTI for this button reset.

### Configuration

| Digital In for Button (B1) | Digital Out for 7-Segment decoder             |
| -------------------------- | --------------------------------------------- |
| Digital In                 | Digital Out                                   |
| PC13                       | <p>PA7, PB6, PC7, PA9<br></p>                 |
| PULL-UP                    | Push-Pull, No Pull-up-Pull-down, Medium Speed |

### Circuit Diagram

> You need to include the circuit diagram

![image](https://user-images.githubusercontent.com/38373000/192134563-72f68b29-4127-42ac-b064-2eda95a9a52a.png)

### Code

Your code goes here.

Explain your source code with necessary comments.

```
// YOUR MAIN CODE ONLY
// YOUR CODE
```

### Results

Experiment images and results

> Show experiment images /results

Add [demo video link](link/)

## Reference

Complete list of all references used (github, blog, paper, etc)

```
```

## Troubleshooting

(Option) You can write a Troubleshooting section
