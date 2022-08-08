# EXTI\_SysTick

## EC\_HAL for EXTI

{% embed url="https://github.com/ykkimhgu/EC-student/tree/main/tutorial-student" %}

### Header File

`ecEXTI.h`

```cpp
#ifndef __EC_EXTI_H
#define __EC_EXTI_H

#include "stm32f411xe.h"

#define FALL 0
#define RISE 1
#define BOTH 2

#ifdef __cplusplus
 extern "C" {
#endif /* __cplusplus */

void EXTI_init(GPIO_TypeDef *Port, int pin, int trig,int priority);
void EXTI_clearpending(uint32_t pin);
void EXTI_enable(uint32_t pin);
void EXTI_disable(uint32_t pin);
uint32_t  is_pending_EXTI(uint32_t pin);
void clear_pending_EXTI(uint32_t pin);

#ifdef __cplusplus
}
#endif /* __cplusplus */
	 
#endif
```

####

## Example Code for Tutorial

#### Tutorial: EXTI Initiation

```cpp
#include "ecRCC.h"
#include "ecSysTick.h"
#include "ecGPIO.h"

#define LED_PIN 	5
#define BUTTON_PIN 13

void setup(void);

int main(void) {
	
	// System CLOCK, GPIO Initialiization ----------------------------------------
	setup();
	
	
	// EXTI Initialiization ------------------------------------------------------	
	
	// SYSCFG peripheral clock enable
	RCC->APB2ENR |= 	RCC_APB2ENR_SYSCFGEN;	  	
	
	// Connect External Line to the GPIO
	// Button: PC_13 -> EXTICR3(EXTI13)
	SYSCFG->EXTICR[BUTTON_PIN/4] &= ~SYSCFG_EXTICR4_EXTI13;     
	SYSCFG->EXTICR[BUTTON_PIN/4] |=  SYSCFG_EXTICR4_EXTI13_PC;  

	// Falling trigger enable (Button: pull-up)
	EXTI->FTSR |= 1UL << BUTTON_PIN;		    	

	// Unmask (Enable) EXT interrupt
	EXTI->IMR  |= 1UL << BUTTON_PIN;		    	
		
	// Interrupt IRQn, Priority
	NVIC_SetPriority(EXTI15_10_IRQn, 0);  		// Set EXTI priority as 0	
	NVIC_EnableIRQ(EXTI15_10_IRQn); 					// Enable EXTI 
		
	
	while(1);
}


void EXTI15_10_IRQHandler(void) {  
	if ((EXTI->PR & EXTI_PR_PR13) == EXTI_PR_PR13) {
		LED_toggle();
		EXTI->PR |= EXTI_PR_PR13; // cleared by writing '1'
	}
}

// Initialiization 
void setup(void)
{	
	RCC_PLL_init();                         // System Clock = 84MHz
	// Initialize GPIOA_5 for Output
	GPIO_init(GPIOA, LED_PIN, OUTPUT);    // calls RCC_GPIOA_enable()	
	// Initialize GPIOC_13 for Input Button
	GPIO_init(GPIOC, BUTTON_PIN, INPUT);  // calls RCC_GPIOC_enable()
}
```

#### Tutorial: SysTick Initiation

```cpp
#include "ecRCC.h"
#include "ecGPIO.h"
//#include "ecSysTick.h"

#define MCU_CLK_PLL 84000000
#define MCU_CLK_HSI 16000000

volatile uint32_t msTick = 0;
volatile uint32_t curTicks;
volatile uint32_t msDelay=1000;

void setup(void)
{
	RCC_PLL_init();                         // System Clock = 84MHz	
	GPIO_init(GPIOA, LED_PIN, OUTPUT);    // calls RCC_GPIOA_enable()	
}


int main(void) {
	
// System CLOCK, GPIO Initialiization ----------------------------------------
	setup();

// SysTick Initialiization ------------------------------------------------------				
	//  SysTick Control and Status Register
	SysTick->CTRL = 0;											// Disable SysTick IRQ and SysTick Counter

	// Select processor clock
	// 1 = processor clock;  0 = external clock
	SysTick->CTRL |= SysTick_CTRL_CLKSOURCE_Msk;

	// uint32_t MCU_CLK=EC_SYSTEM_CLK
	// SysTick Reload Value Register
	SysTick->LOAD = MCU_CLK_PLL/1000-1;						// 1ms

	// SysTick Current Value Register
	SysTick->VAL = 0;

	// Enables SysTick exception request
	// 1 = counting down to zero asserts the SysTick exception request
	SysTick->CTRL |= SysTick_CTRL_TICKINT_Msk;
		
	// Enable SysTick IRQ and SysTick Timer
	SysTick->CTRL |= SysTick_CTRL_ENABLE_Msk;
		
	NVIC_SetPriority(SysTick_IRQn, 16);		// Set Priority to 1
	NVIC_EnableIRQ(SysTick_IRQn);			// Enable interrupt in NVIC

	
// While loop ------------------------------------------------------	

	msTick = 0;
	while(1){		
	// Method 1		
		curTicks = msTick;
		while ((msTick - curTicks) < 1000);
		LED_toggle();
		msTick = 0;
		
	// Method 2		
		/*
		if ((msTick) > 1000){
			LED_toggle();			
			msTick = 0;
		}
		*/
		
	// Method 3				
		/*
		msDelay=1000;
		while(msDelay !=0);		
		LED_toggle();	
		*/
		
	}
}


void SysTick_Handler(void){
	msTick++;
	msDelay--;
}

```

####

####
