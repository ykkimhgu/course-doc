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

{% hint style="info" %}
DO NOT use `ecEXTI2_student.h`  for this tutorial.
{% endhint %}

> You MUST write your name on the source file inside the comment section

```c
//#include "ecSTM32F4v2.h"
#include "ecRCC2.h"
#include "ecGPIO2.h"

#define LED_PIN   PB_12 		//EVAL board JKIT
#define BUTTON_PIN PA_4			//EVAL board JKIT

void LED_toggle(PinName_t pinName);

// Initialiization 
void setup(void)
{
	RCC_PLL_init();                         // System Clock = 84MHz
	// Initialize GPIOB_12 for Output
	GPIO_init(LED_PIN, OUTPUT);    // LED for EVAL board	
	// Initialize GPIOA_4 for Input Button
	GPIO_init(BUTTON_PIN, INPUT);  // OUTPUT for EVAL borad
	EXTI_init_tutorial(PA_4);
		
}

// MAIN  ----------------------------------------
int main(void) {
	setup();
	while (1);
}


// EXTI Initialiization ------------------------------------------------------
// YOUR CODE GOES HERE	
void EXTI_init_tutorial(PinName_t pinName){
	GPIO_Typedef *Port;
	unsigned int pin;
	ecPinmap(pinName,&Port,&pin);
	

	// SYSCFG peripheral clock enable
	RCC->APB2ENR |= __________________

	// Connect External Line to the GPIO
	// Button: PA_4 -> EXTICR2(EXTI4)
	SYSCFG->EXTICR[____] &= ~SYSCFG_EXTICR2_EXTI4;
	SYSCFG->EXTICR[____] |= ______________________;

	// Falling trigger enable (Button: pull-up)
	EXTI->FTSR |= 1UL << __________;

	// Unmask (Enable) EXT interrupt
	EXTI->IMR |= 1UL << ___________;

	// Interrupt IRQn, Priority
	NVIC_SetPriority(EXTI4_IRQn, 0);  		// Set EXTI priority as 0	
	NVIC_EnableIRQ(EXTI4_IRQn); 			// Enable EXTI 
	
}

// YOUR CODE GOES HERE
void EXTI4_IRQHandler(void) {
	if ((EXTI->PR & EXTI_PR_PR4) == _________) {
		LED_toggle(LED_PIN);
		EXTI->PR |= EXTI_PR_PR4; // cleared by writing '1'
	}
}


void LED_toggle(PinName_t pinName){
	GPIO_Typedef *Port;
	unsigned int pin;
	ecPinmap(pinName,&Port,&pin);
	// YOUR CODE GOES HERE
}
```
