# LAB: EXTI & SysTick

## LAB: EXTI\_SysTick

**Date:** 2022-09-26

**Author/Partner:**

**Github:** repository link

**Demo Video:** Youtube link

**PDF version:**

### Introduction

In this lab, you are required to create two simple programs: toggling multiple LEDs with a push-button input and displaying the number counting from 0 to 9 at 1 second rate on a 7-segment display.

You must submit

* LAB Report (\*.md & \*.pdf)
* Zip source files(main\*.c, ecRCC.h, ecGPIO.h, ecSysTick.c etc...).
  * Only the source files. Do not submit project files

#### Requirement

**Hardware**

* MCU
  * NUCLEO-F411RE
* Actuator/Sensor/Others:
  * 4 LEDs and load resistance
  * 7-segment display(5101ASR)
  * Array resistor (330 ohm)
  * breadboard

**Software**

* Keil uVision, CMSIS, EC\_HAL library

### Problem 1: LED Toggle with EXTI Button

A program that toggles multiple LEDs with a push-button input using external interrupt.

#### Create HAL library

Declare and Define the following functions in your library. You must

update your header files located in the directory `EC \lib\`.

**ecEXTI.h**

```
void EXTI_init(GPIO_TypeDef *port, int pin, int trig_type, int priority);
void EXTI_enable(uint32_t pin);  // mask in IMR
void EXTI_disable(uint32_t pin);  // unmask in IMR
uint32_t  is_pending_EXTI(uint32_t pin);
void clear_pending_EXTI(uint32_t pin);
```

#### Procedure

1. Create a new project under the directory `\repos\EC\LAB\LAB_EXTI`

* The project name is “**LAB\_EXTI”.**
* Create a new source file named as “**LAB\_EXTI.c”**

> You MUST write your name on the source file inside the comment section.

2\. Include your updated library in `\repos\EC\lib\` to your project.

* **ecGPIO.h, ecGPIO.c**
* **ecRCC.h, ecRCC.c**
* **ecEXTI.h, ecEXTI.c**

1. Connect 4 LEDs externally on a breadboard.
2. Toggle LEDs, turning on one LED at a time by pressing the push button.
   * Example: LED0--> LED1--> …LED3--> …LED0….
3. You must use your library function of EXTI.
4. Refer to the [sample code](https://ykkim.gitbook.io/ec/firmware-programming/example-code#button-interrupt)

#### Configuration

| Button        | LED                              |
| ------------- | -------------------------------- |
| Digital In    | Digital Out                      |
| GPIOC, Pin 13 | PA5, PA6, PA7, PB6               |
| PULL-UP       | Push-Pull, Pull-up, Medium Speed |

#### Circuit Diagram

> You need to include the circuit diagram

![image](https://user-images.githubusercontent.com/38373000/192134563-72f68b29-4127-42ac-b064-2eda95a9a52a.png)

#### Discussion

1. To detect an external signal we can use two different methods: polling and interrupt. What are the advantages and disadvantages of each approach?

> Answer discussion questions

1.  What would happen if the EXTI interrupt handler does not clear the interrupt pending flags? Check with your code

    > Answer discussion questions

#### Code

Your code goes here: [ADD Code LINK such as github](https://github.com/ykkimhgu/EC-student/)

Explain your source code with necessary comments.

```
// YOUR MAIN CODE ONLY
// YOUR CODE
```

#### Results

Experiment images and results

> Show experiment images /results

Add demo video link







### Problem 2: Counting number on 7-Segment

Display the number 0 to 9 on the 7-segment LED at the rate of 1 sec. After displaying up to 9, then it should display ‘0’ and continue counting.

When the button is pressed, the number should be reset ‘0’ and start counting again.

#### Create HAL library

Declare and Define the following functions in your library. You must

update your header files located in the directory `EC \lib\`.

**ecSysTick.h**

```
void SysTick_init(uint32_t msec);
void delay_ms(uint32_t msec);
uint32_t SysTick_val(void);
void SysTick_reset (void);
void SysTick_enable(void);
void SysTick_disable (void)
```

#### Procedure

1.  Create a new project under the directory

    `\repos\EC\LAB\LAB_EXTI_SysTick`

* The project name is “**LAB\_EXTI\_SysTick”.**
* Create a new source file named as “**LAB\_EXTI\_SysTick.c”**

> You MUST write your name on the source file inside the comment section.

2\. Include your updated library in `\repos\EC\lib\` to your project.

* **ecGPIO.h, ecGPIO.c**
* **ecRCC.h, ecRCC.c**
* **ecEXTI.h, ecEXTI.c**
* **ecSysTick.h, ecSysTick.c**

1. First, check if every number, 0 to 9, can be displayed properly on the 7-segment.
2. Then, create a code to display the number counting from 0 to 9 and repeat at the rate of 1 second.
3. When the button is pressed, it should start from '0' again. Use EXTI for this reset.
4. Refer to the [sample code](https://ykkim.gitbook.io/ec/stm32-m4-programming/example-code#systick-interrupt)

#### Configuration

| Digital In for Button (B1) | Digital Out for 7-Segment                                                 |
| -------------------------- | ------------------------------------------------------------------------- |
| Digital In                 | Digital Out                                                               |
| PC13                       | <p>PA5, PA6, PA7, PB6, PC7, PA9, PA8, PB10<br>('a'~'h', respectively)</p> |
| PULL-UP                    | Push-Pull, No Pull-up-Pull-down, Medium Speed                             |

#### Circuit Diagram

> You need to include the circuit diagram

![image](https://user-images.githubusercontent.com/38373000/192134563-72f68b29-4127-42ac-b064-2eda95a9a52a.png)

#### Code

Your code goes here: [ADD Code LINK such as github](https://github.com/ykkimhgu/EC-student/)

Explain your source code with necessary comments.

```
// YOUR MAIN CODE ONLY
// YOUR CODE
```

#### Results

Experiment images and results

> Show experiment images /results

Add demo video link

### Reference

Complete list of all references used (github, blog, paper, etc)

```
```

### Troubleshooting

(Option) You can write Troubleshooting section
