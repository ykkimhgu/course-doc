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
#include "ecGPIO2.h"
#include "ecRCC2.h"

// TO BE MODIFIED SOON

#define LED_pin PA_5;
uint32_t count = 0;

void TIM2_init_tutorial();

void setup(void);
	
int main(void) { 
	// Initialiization --------------------------------------------------------
	setup();
	
	// Inifinite Loop ----------------------------------------------------------
	while(1){}
}

// Initialiization 
void setup(void)
{	
	RCC_PLL_init();                       // System Clock = 84MHz
	GPIO_init(LED_pin, OUTPUT);    // calls RCC_GPIOA_enable()	
	TIM2_init_tutorial();
}

// YOUR CODE GOES HERE
void TIM2_init_tutorial(){
	TIM_TypeDef* timerx;
	timerx = TIM2;
	RCC->APB1ENR |= RCC_APB1ENR_TIM2EN;
	
	timerx->PSC =______ 			// Timer counter clock: 1MHz(1us)
	timerx->ARR =______ 			// Set auto reload register to maximum (count up to 65535)
	timerx->DIER |=______           // Enable Interrupt
	timerx->CR1 |=______            // Enable counter
	
	NVIC_SetPriority(___ );       	// TIM2_IRQHandler Set priority as 2
	NVIC_EnableIRQ(___);			// TIM2_IRQHandler Enable
}

// YOUR CODE GOES HERE
void TIM2_IRQHandler(void){
	if((TIM2->SR & TIM_SR_UIF) ==__________      ){ // update interrupt flag
		//Create the code to toggle LED by 1000ms

		TIM2->SR &=____________________             // clear by writing 0
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
// TO BE MODIFIED SOON

#include "stm32f411xe.h"
#include "ecGPIO2.h"
#include "ecRCC2.h"
#include "ecSysTick2.h"

#define LED_PIN    PA_5

void setup(void);
void PWM_init_tutorial();


int main(void) { 
	// Initialiization --------------------------------------------------------	
	setup();
	
	// Inifinite Loop ----------------------------------------------------------
	while(1){
			//Create the code to change the brightness of LED as 10kHZ (use "delay(1000)")
		}
	}



// Initialiization 
void setup(void)
{	
	RCC_PLL_init();       // System Clock = 84MHz
	SysTick_init();       // for delay_ms()
	// YOUR CODE GOES HERE
	GPIO_init(LED_PIN, ______);     // GPIOA 5 ALTERNATE function
	// YOUR CODE GOES HERE
	GPIO_ospeed(LED_PIN, ______);   // GPIOA 5 HIGH SPEED
	PWM_init_tutorial();
}

// YOUR CODE GOES HERE
void PWM_init_tutorial(){	
	// TEMP: TIMER Register Initialiization --------------------------------------------------------		
	TIM_TypeDef *TIMx;
	TIMx = TIM2;
	
	// GPIO: ALTERNATIVE function setting
	GPIOA->AFR[0]	 =           				// AF1 at PA5 = TIM2_CH1 (p.150)
	
	// TIMER: PWM setting
	RCC->APB1ENR |=               				// Enable TIMER clock
	
	TIMx->CR1 &= 				              	// Direction Up-count
	
	TIMx->PSC = 						         // Set Timer CLK = 100kHz : (PSC + 1) = 84MHz/100kHz --> PSC = ?
	
	TIMx->ARR = 								 // Auto-reload: Upcounting (0...ARR). 
												// Set Counter CLK = 1kHz : (ARR + 1) = 100kHz/1kHz --> ARR = ?
	
	TIMx->CCMR1 &= ~TIM_CCMR1_OC1M;  			// Clear ouput compare mode bits for channel 1
	TIMx->CCMR1 |=                   			// OC1M = 110 for PWM Mode 1 output on ch1
	TIMx->CCMR1	|= TIM_CCMR1_OC1PE;    			// Output 1 preload enable (make CCR1 value changable)
	
	TIMx->CCR1 =       							// Output Compare Register for channel 1 	
	
	TIMx->CCER &= ~TIM_CCER_CC1P;    			// select output polarity: active high	
	TIMx->CCER |= 												// Enable output for ch1
	
	TIMx->CR1  |= TIM_CR1_CEN;      			// Enable counter
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

