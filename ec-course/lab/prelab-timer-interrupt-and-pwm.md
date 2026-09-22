# PreLAB: Timer Interrupt & PWM

## Introduction

In this tutorial, you will learn how to set up MCU Timers for Timer Interrupt and PWM output.



## Part 1:  Timer Interrupt

### A. Register List

List of TIMx registers for Timer Interrupt

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Type</td><td valign="top">Register Name</td><td valign="top">Description</td></tr><tr><td valign="top">TIMx</td><td valign="top">TIMx_ CR1</td><td valign="top">TIMx control register 1</td></tr><tr><td valign="top"> </td><td valign="top">TIMx_ PSC</td><td valign="top">TIMx prescaler register</td></tr><tr><td valign="top"> </td><td valign="top">TIMx_ARR</td><td valign="top">TIMx auto-reload register</td></tr><tr><td valign="top"> </td><td valign="top">TIMx_DIER</td><td valign="top">TIMx DMA Interrupt Enable register</td></tr></tbody></table>



### B. Register Setting

1\. **System Clock setting**

* RCC setting (PLL)



2\. **Timer Counter setting**

* Enable Timer Peripheral Clock         (RCC->APB1ENR)
* Set Timer Clock Pre-scaler value     (TIMx->PSC : PSC\[15:0])
* Set Auto-reload value                       (TIMx->ARR : ARR)
* Set Counting Direction                      (TIMx->CR1 : DIR)
* Enable counter                                   (TIMx->CR1 : CEN)

3\. **Timer Interrupt setting**
* Enable Timer DMA/Interrupt.            (TIMx->DIER : UIE)

4\. **NVIC setting**

* Set Interrupt Priority                             NVIC\_SetPriority(TIMx\_IRQn,2)
* Enable TIMx Interrupt:                         NVIC\_EnableIRQ(TIMx\_IRQn)

&#x20;



### C. Exercise

This is an example code for Timer Interrupt that turns LED on/off at 1 second period.

•          Use Counter of TIM2: Up-counting, Timer2\_CLK = 100 kHz,  COUNT\_CLK = 1 kHz

•          Use timer interrupt “void TIM2\_IRQHandler(void)”



#### **Procedure**

* Download the header library files and save under `include\`.
  * `ecTIM2_student.h, ecTIM2_student.c`: [Click here to download](https://github.com/ykkimhgu/EC-student/tree/main/include/lib-student)
*   Rename the files as `ecTIM2.h, ecTIM2.c`

    > Write your name and modified date in the comment box
* Create a new project under the directory `EC\tutorial\`
  * Environment : `env:TU_TIM_Interrupt`
  * Source file: `TU_TIM_Interrupt_student.c`
* Modify the `platformio.ini` **,** to add new environment.
* Fill in the empty spaces in the downloaded library:  `ecTIM2.c`
* Run the program and check your result.
* No need to submit this tutorial report 



{% tabs %}
{% tab title="TU_TIM_Interrupt_student.c" %}
{% code expandable="true" %}
```c
/*----------------------------------------------------------------\
@ Embedded Controller by Young-Keun Kim - Handong Global University
Author           : [ YOUR NAME GOES HERE !!!!!]
Created          : 05-03-2021
Modified         : 09-03-2026 [WRITE THE DATE!!!!]
Language/ver     : C++ in VS Code

Description      : [Write description here!!]
/----------------------------------------------------------------*/


// #include "ecSTM32F411.h"
#include "stm32f411xe.h"
#include "ecRCC2.h"
#include "ecGPIO2.h"
#include "ecSysTick2.h"
#include "ecTIM2.h"


#define LD2_PIN	PA_5
uint32_t _count = 0;
// led_toggle() function should have been defined in ecGPIO2.c


// Initialization
void setup(void){
	RCC_PLL_init();				
    SysTick_init();
	GPIO_init(LD2_PIN, OUTPUT);	
	TIM_UI_init(TIM2, 1);		// TIM2 Update-Event Interrupt every 1 msec 
	TIM_UI_enable(TIM2);        // Enable TIM2 Update Interrupt (Optional, already done in TIM_UI_init())
}

int main(void) {
	// Initialization --------------------------------------------------
	setup();
	
	// Infinite Loop ---------------------------------------------------
	while(1){}
}


// TIM2 Update Interrupt Handler
void TIM2_IRQHandler(void){
	if(is_UIF(TIM2)){			// Check UIF(update interrupt flag)
        _count++;
		if (_count > 1000) {
		    led_toggle(LD2_PIN);	// Toggle every 1000 msec
			_count = 0;
		}	
        clear_UIF(TIM2); 		// Clear TIM_UI flag 
	}
}
```
{% endcode %}
{% endtab %}

{% tab title="ecTIM2.h" %}
```c
// Timer Period setup
void TIM_init(TIM_TypeDef *TIMx, uint32_t msec);
void TIM_period(TIM_TypeDef* TIMx, uint32_t msec);
void TIM_period_ms(TIM_TypeDef* TIMx, uint32_t msec);
void TIM_period_us(TIM_TypeDef* TIMx, uint32_t usec);

// Timer Interrupt setup
void TIM_UI_init(TIM_TypeDef* TIMx, uint32_t msec);
void TIM_UI_enable(TIM_TypeDef* TIMx);
void TIM_UI_disable(TIM_TypeDef* TIMx);


// Timer Interrupt Flag 
uint32_t is_UIF(TIM_TypeDef *TIMx);
void clear_UIF(TIM_TypeDef *TIMx);
```


{% endtab %}
{% tab title="ecTIM2.c" %}

```c

// Default Setting:  1 msec of TimerUEV with Counter_Clk 100kHz / PSC=840-1, ARR=100-1
void TIM_init(TIM_TypeDef* TIMx){     

    // 1. Enable Timer CLOCK
	if(TIMx ==TIM1) RCC->APB2ENR |= RCC_APB2ENR_TIM1EN;
	else if(TIMx ==TIM2) RCC->APB1ENR |= __________________;
	else if(TIMx ==TIM3) __________________________________;
	// repeat for TIM4, TIM5, TIM9, TIM11
    // YOUR CODE GOES HERE
	// YOUR CODE GOES HERE
	
	
    // 2. Set CNT period
	 uint32_t msec=1;
	TIM_period_ms(TIMx, msec); 
	
	
    // 3. CNT Direction
	TIMx->CR1 _________________;					// Upcounter	
	
    // 4. Enable Timer Counter
	TIMx->CR1 |= TIM_CR1_CEN;		
}


// Timer Update Event Period  1~600 msec  with 100kHz Couter / ARR=100*msec
void TIM_period_ms(TIM_TypeDef* TIMx, uint32_t msec){ 
    // YOUR CODE GOES HERE
	// YOUR CODE GOES HERE
}



// Update Event Interrupt
void TIM_UI_init(TIM_TypeDef* TIMx, uint32_t msec){
    // YOUR CODE GOES HERE
	// YOUR CODE GOES HERE
}

```
{% endtab %}
{% endtabs %}



<br>

&#x20;

## &#x20;Part 2:  PWM

### A. Register List

List of TIMx registers for PWM

<figure><img src="../../.gitbook/assets/image (170).png" alt=""><figcaption></figcaption></figure>

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Type</td><td valign="top">Register Name</td><td valign="top">Description</td></tr><tr><td valign="top">TIMx</td><td valign="top">TIMx _CCMRy</td><td valign="top">TIMx capture/compare mode register for yth channel</td></tr><tr><td valign="top"> </td><td valign="top">TIMx _CCRy</td><td valign="top">TIMx capture/compare register for yth channel</td></tr><tr><td valign="top"> </td><td valign="top">TIMx _CCER</td><td valign="top">TIMx capture/compare output enable register</td></tr></tbody></table>

&#x20;

### B. Register Setting

**1\. System Clock setting**

* Same as above&#x20;



**2. Timer Counter setting**

* Same as above&#x20;


3\. **Timer Interrupt setting**
* If necessary 

**4. GPIO (AF) Out**

* mode=AF(TIMx)  for  Pin\_y in GPIOx&#x20;



**5. PWM Out setting**

* Select PWM Output mode                     (TIMx->CCMR : OCyM)
* Select CompareCapture value              (TIMx->CCRy : CCR)
* Select Output Polarity                            (TIMx->CCER : CCyP)
* Enable CompareCaptureOutput            (TIMx->CCER : CCyE)



### C. Exercise

This is an example code for PWM Out ( 0 / 50% / 100% )

* Give PWM output of 0%, 50%, 100% of 1kHz PWM
* Check the brightness of LD2 (PA5) for each different Duty ratio



#### Configuraiton

* System CLK is PLL 84MHz&#x20;
* Timer Counter:&#x20;
  * TIM2: Up-counting, Timer2\_CLK = 100 kHz,  COUNT\_CLK=1 kHz

•  PWM Output:&#x20;

* PWM Out on PA\_5 : TIM2\_CH1&#x20;
* PWM Mode 1 / Period 1kHz / Duty ratio 50%.

> &#x20;There are several pins for TIM2\_CH1. We will use GPIO Pin5 (LD2) for tutorial
>
> [ MCU pins for TIMERS](https://ykkim.gitbook.io/ec/resource/nucleo-f411re)

• GPIO

* mode= Alternate function (AF) for TIM2
* High speed



&#x20;

#### **Procedure**

* Download the header library files and save under `include\`.
  * `ecPWM2_student.h, ecPWM2_student.c`: [Click here to download](https://github.com/ykkimhgu/EC-student/tree/main/include/lib-student)
*   Rename the files as `ecPWM2.h, ecPWM2.c`

    > Write your name and modified date in the comment box
* Create a new project under the directory `EC\tutorial\`
  * Environment : `env:TU_TIM_PWM`
  * Source file: `TU_TIM_PWM_student.c`
* Modify the `platformio.ini` **,** to add new environment.
* Fill in the empty spaces in the downloaded library:  `ecPWM2.c`
* Run the program and check your result.
* No need to submit this tutorial report 





{% tabs %}
{% tab title="TU_TIM_PWM_student.c" %}
{% code expandable="true" %}
```c
#include "stm32f411xe.h"
#include "math.h"

// #include "ecSTM32F411.h"
#include "ecPinNames.h"
#include "ecGPIO.h"
#include "ecSysTick.h"
#include "ecRCC.h"
#include "ecTIM.h"
#include "ecPWM.h"   // ecPWM2.h


// Definition Button Pin & PWM Port, Pin
#define BUTTON_PIN PC_13
#define PWM_PIN PA_5
void setup(void);


int main(void) {
	// Initialization --------------------------------------------------
	setup();	
	
	// Infinite Loop ---------------------------------------------------
	while(1){
		LED_toggle();		
		for (int i=0; i<5; i++) {						
			PWM_duty(PWM_PIN, (float)0.2*i);			
			delay_ms(1000);
		}		
	}
}


// Initialiization 
void setup(void) {	
	RCC_PLL_init();
	SysTick_init();
		
	// PWM of 20 msec:  TIM2_CH1 (PA_5 AFmode)
	GPIO_init(GPIOA, 5, EC_AF);
	PWM_init(PWM_PIN);	
	PWM_period(PWM_PIN, 20);   // 20 msec PWM period
}
```
{% endcode %}
{% endtab %}

{% tab title="ecPWM2.h" %}
```c
/* PWM Configuration using PinName_t Structure */

/* PWM initialization */
// Default: 84MHz PLL, 1MHz CK_CNT, 50% duty ratio, 1msec period
void PWM_init(PinName_t pinName);
void PWM_pinmap(PinName_t pinName, TIM_TypeDef **TIMx, int *chN);


/* PWM PERIOD SETUP */
// allowable range for msec:  1~2,000
void PWM_period_ms(PinName_t pinName,  uint32_t msec);	// same as PWM_period()
// allowable range for usec:  1~1,000
void PWM_period_us(PinName_t pinName, uint32_t usec);


/* DUTY RATIO SETUP */
// High Pulse width in msec
void PWM_pulsewidth_ms(PinName_t pinName, uint32_t pulse_width_ms);  // same as void PWM_pulsewidth
// Duty ratio 0~1.0
void PWM_duty(PinName_t pinName, float duty);
```


{% endtab %}
{% endtabs %}

