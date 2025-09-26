# PreLAB: External Interrupt

Name:

ID:

## I. Introduction

In this tutorial, we will learn how to use External Interrupt. We will create functions that capture the falling edge trigger by pushing a button using an external interrupt.

The objectives of this tutorial are how to

* Configure External input (EXTI) interrupt with NVIC
* Create your own functions for configuration of interrupts

### Hardware

* NUCLEO -F411RE

### Software

* VS code, CMSIS, EC\_HAL

### Documentation

* [STM32 Reference Manual](https://ykkim.gitbook.io/ec/stm32-m4-programming/hardware/nucleo-f411re#manual-documentation)

## II.Basics of External Interrupt (EXTI)

### A. Register List

List of external interrupt (EXTI) registers used in this tutorial \[Reference Manual ch7, ch10.2]

![Register List](https://raw.githubusercontent.com/LeeJunjae1/EC_22000573/main/img/exti.png)

### B. Register Setting

**(Digital Input Setting)**

* Enable GPIO peripheral clock **RCC->AHB1ENR**
* Configure DigitalIn pin

**(EXTI Setting)**

* Enable SYSCFG peripheral clock. **RCC->APB2ENR**
* Connect the corresponding external line to GPIO **SYSCFG->EXTICR**
* Configure the trigger edge. **EXTI->FTSR/RTSR**
* Configure Interrupt mask **EXTI->IMR**
* Enable EXTI. **EXTI->IMR**

**(NVIC Setting)**

* Configure the priority of EXTI interrupt request. **NVIC\_SetPriority(**)
* Enable EXTI interrupt request. **NVIC\_EnableIRQ(**)

**(EXTI Use)**

* Create user codes in handler **EXTIx\_IRQHandler()**
* Clear pending bit after interrupt call

##

## III. Tutorial

### A. Register Configuration

Fill in the blanks below



1. **Pin Initialization & Set LED and Push-button**

* LED Pin : Port B Pin 12 / Output / Push-Pull / No Pull-Up & No Pull-Down
* Push-Button:  Port A Pin 4 / Input / No Pull-Up & No Pull-Down

```
// Use your library  GPIO


```



2. **Enable Peripheral Clock:** SYSCFGEN

* **RCC\_APB2ENR:** Enable SYSCFG

![RCC\_APB2ENR](https://raw.githubusercontent.com/LeeJunjae1/EC_22000573/main/img/RCC.png)

3. **EXTI Initialization & Connect Push-button to EXTI line**

* **SYSCFG\_EXTICR2:** Connect PA\_4(push-button) to EXTI4 line

![EXTICR](https://raw.githubusercontent.com/LeeJunjae1/EC_22000573/main/img/EXTI_BUTTON.png)

* **EXTI\_FTSR:** Enable Falling Trigger

![FTSR](https://raw.githubusercontent.com/LeeJunjae1/EC_22000573/main/img/FTSR.png)

* **EXTI\_IMR:** Interrupt NOT masked (Enable)

![IMR](https://raw.githubusercontent.com/LeeJunjae1/EC_22000573/main/img/IMR.png)

### B. Programming

This is an example code for toggling LED on/off with the button input trigger (EXTI)&#x20;

Fill in the empty spaces in the code.

#### Procedure

* Name the project as `TU_EXTI` by creating a new folder as `tutorial\TU_EXTI`
* Download the template code
  * `TU_EXTI_student.c` [Click here to download](https://github.com/ykkimhgu/EC-student/tree/main/tutorial/tutorial-student)



* Fill in the empty spaces in the code.
* Run the program and check your result.
* Your tutorial report must be submitted to LMS



> You MUST write your name on the source file inside the comment section

