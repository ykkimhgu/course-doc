# PreLAB: Timer Interrupt & PWM

## Introduction

In this tutorial, you will learn how to set up MCU Timers for Timer Interrupt and PWM output.



## Part 1:  Timer Interrupt

### A. Register List

List of TIMx registers for Timer Interrupt

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Type</td><td valign="top">Register Name</td><td valign="top">Description</td></tr><tr><td valign="top">TIMx</td><td valign="top">TIMx_ CR1</td><td valign="top">TIMx control register 1</td></tr><tr><td valign="top"> </td><td valign="top">TIMx_ PSC</td><td valign="top">TIMx prescaler register</td></tr><tr><td valign="top"> </td><td valign="top">TIMx_ARR</td><td valign="top">TIMx auto-reload register</td></tr><tr><td valign="top"> </td><td valign="top">TIMx_DIER</td><td valign="top">TIMx DMA Interrupt Enable register</td></tr></tbody></table>

&#x20;

### B. Register Setting

1\. **System Clock setting**

* RCC setting (PLL)

&#x20;

2. **Timer setting**

* Enable Timer Peripheral Clock         (RCC->APB1ENR)
* Set Timer Clock Pre-scaler value     (TIMx->PSC : PSC\[15:0])
* Set Auto-reload value                       (TIMx->ARR : ARR)
* Set Counting Direction                      (TIMx->CR1 : DIR)
* Enable Timer DMA/Interrupt.            (TIMx->DIER : UIE)
* Enable counter                                   (TIMx->CR1 : CEN)

&#x20;

3. **NVIC setting**

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
* Your tutorial report must be submitted to LMS



Use the given source code of ‘TU\_TimerInterrupt\_student.c’  [Click to download](https://github.com/ykkimhgu/EC-student/tree/main/tutorial/tutorial-student)

{% tabs %}
{% tab title="TU_TIM_Interrupt_student.c" %}
{% code expandable="true" %}
```c
#include "stm32f411xe.h"
#include "ecGPIO.h"
#include "ecRCC.h"
#include "ecTIM.h"


#define LED_PIN	PB_15
uint32_t _count = 0;
void setup(void);


int main(void) {
	// Initialization --------------------------------------------------
	setup();
	
	// Infinite Loop ---------------------------------------------------
	while(1){}
}


// Initialization
void setup(void){
	RCC_PLL_init();				// System Clock = 84MHz
	GPIO_init(GPIOA, LED_PIN, OUTPUT);	// calls RCC_GPIOA_enable()
	TIM_UI_init(TIM2, 1);			// TIM2 Update-Event Interrupt every 1 msec 
	TIM_UI_enable(TIM2);
}

void TIM2_IRQHandler(void){
	if(is_UIF(TIM2)){			// Check UIF(update interrupt flag)
		_count++;
		if (_count > 1000) {
			LED_toggle();		// LED toggle every 1 sec
			_count = 0;
		}
		clear_UIF(TIM2); 		// Clear UI flag by writing 0
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

1\. **System Clock setting**

* Same as above&#x20;



2. **Timer setting**

* Same as above&#x20;



**3. GPIO (AF) Out**

* mode=AF(TIMx)  for  Pin\_y in GPIOx&#x20;



4. **PWM Out setting**

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
* Your tutorial report must be submitted to LMS





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

