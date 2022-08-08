# TIMER

## TIMx TypeDef

`#include stm32f11xe.h`

\`\`

```cpp
/*!< APB2 peripherals */
#define TIM1_BASE           (APB2PERIPH_BASE + 0x0000UL)
#define TIM1                ((TIM_TypeDef *) TIM1_BASE)

/*!< APB1 peripherals */
#define TIM2_BASE           (APB1PERIPH_BASE + 0x0000UL)
#define TIM2                ((TIM_TypeDef *) TIM2_BASE)

typedef struct
{
  __IO uint32_t CR1;         /*!< TIM control register 1,              Address offset: 0x00 */
  __IO uint32_t CR2;         /*!< TIM control register 2,              Address offset: 0x04 */
  __IO uint32_t SMCR;        /*!< TIM slave mode control register,     Address offset: 0x08 */
  __IO uint32_t DIER;        /*!< TIM DMA/interrupt enable register,   Address offset: 0x0C */
  __IO uint32_t SR;          /*!< TIM status register,                 Address offset: 0x10 */
  __IO uint32_t EGR;         /*!< TIM event generation register,       Address offset: 0x14 */
  __IO uint32_t CCMR1;       /*!< TIM capture/compare mode register 1, Address offset: 0x18 */
  __IO uint32_t CCMR2;       /*!< TIM capture/compare mode register 2, Address offset: 0x1C */
  __IO uint32_t CCER;        /*!< TIM capture/compare enable register, Address offset: 0x20 */
  __IO uint32_t CNT;         /*!< TIM counter register,                Address offset: 0x24 */
  __IO uint32_t PSC;         /*!< TIM prescaler,                       Address offset: 0x28 */
  __IO uint32_t ARR;         /*!< TIM auto-reload register,            Address offset: 0x2C */
  __IO uint32_t RCR;         /*!< TIM repetition counter register,     Address offset: 0x30 */
  __IO uint32_t CCR1;        /*!< TIM capture/compare register 1,      Address offset: 0x34 */
  __IO uint32_t CCR2;        /*!< TIM capture/compare register 2,      Address offset: 0x38 */
  __IO uint32_t CCR3;        /*!< TIM capture/compare register 3,      Address offset: 0x3C */
  __IO uint32_t CCR4;        /*!< TIM capture/compare register 4,      Address offset: 0x40 */
  __IO uint32_t BDTR;        /*!< TIM break and dead-time register,    Address offset: 0x44 */
  __IO uint32_t DCR;         /*!< TIM DMA control register,            Address offset: 0x48 */
  __IO uint32_t DMAR;        /*!< TIM DMA address for full transfer,   Address offset: 0x4C */
  __IO uint32_t OR;          /*!< TIM option register,                 Address offset: 0x50 */
} TIM_TypeDef;
```

## Example Code for Tutorial

#### Tutorial: TIMER Initiation

```cpp
// Some code
/**
******************************************************************************
* @author  SSSLAB
* @Mod		 2021-8-12 by YKKIM  	
* @brief   Embedded Controller:  Tutorial ___
*					 - _________________________________
* 
******************************************************************************
*/

#include "stm32f411xe.h"
#include "ecGPIO.h"
#include "ecRCC.h"
//#include "ecTIM.h"

uint32_t _count=0;

#define LED_PIN 5

void setup(void);
	
int main(void) { 
	// Initialiization --------------------------------------------------------
	setup();
	TIM_TypeDef* timerx;
	timerx = TIM2;
	RCC->APB1ENR |= RCC_APB1ENR_TIM2EN;
	
	timerx->PSC = 83;							// Timer counter clock: 1MHz(1us)
	timerx->ARR = 999;							// Set auto reload register to maximum (count up to 65535)
	timerx->DIER |= TIM_DIER_UIE;
	timerx->CR1 |= TIM_CR1_CEN;	
	
	NVIC_EnableIRQ(TIM2_IRQn);				
	NVIC_SetPriority(TIM2_IRQn ,2);
	
	// Inifinite Loop ----------------------------------------------------------
	while(1){}
}

// Initialiization 
void setup(void)
{	
	RCC_PLL_init();                         // System Clock = 84MHz
	GPIO_init(GPIOA, LED_PIN, OUTPUT);    // calls RCC_GPIOA_enable()	
}

void TIM2_IRQHandler(void){
	if((TIM2->SR & TIM_SR_UIF) == TIM_SR_UIF){// update interrupt flag
		_count++;
		if (_count >1000) {
			LED_toggle();
			_count=0;}
		TIM2->SR &= ~TIM_SR_UIF;// clear by writing 0
	}
	
}
```

#### Tutorial: PWM Configuration

```
/**
******************************************************************************
* @author  SSSLAB
* @Mod		 2021-8-12 by YKKIM  	
* @brief   Embedded Controller:  Tutorial ___
*					 - _________________________________
* 
******************************************************************************
*/
#include "stm32f411xe.h"
#include "ecGPIO.h"
#include "ecRCC.h"
#include "ecPWM.h"
#include "ecSysTick.h"
#include "ecEXTI.h"


uint32_t _count=0;

#define LED_PIN 	5
#define BUTTON_PIN 13

void setup(void);
	
int main(void) { 
	// Initialiization --------------------------------------------------------
	
	RCC_PLL_init();                         // System Clock = 84MHz
	SysTick_init();                         // for delay_ms()
	GPIO_init(GPIOA, LED_PIN, EC_ALTE);     // GPIOA 5 ALTERNATE function
	GPIO_ospeed(GPIOA, LED_PIN, EC_HIGH);   // GPIOA 5 HIGH SPEED
	
	// TEMP: TIMER Register Initialiization --------------------------------------------------------		
	TIM_TypeDef *TIMx;
	TIMx = TIM2;
	
	// GPIO: ALTERNATIVE function setting
	GPIOA->AFR[0]	 = 1<<(4*LED_PIN);          // AF1 at PA5 = TIM2_CH1 (p.150)

	// TIMER: PWM setting
	RCC->APB1ENR |= RCC_APB1ENR_TIM2EN;			// Enable TIMER clock
	
	TIMx->CR1 &= ~TIM_CR1_DIR;					// Direction Up-count
	
	uint32_t prescaler = 839;
	TIMx->PSC = prescaler;						// Set Timer CLK: (PSC+1)= 84Mhz/0.1MHz --> PSC= 840-1
	
	TIMx->ARR = 99;								// Set ARR:  (ARR+1) = 100kHz/1kHz  --> ARR=100-1.    
	
	TIMx->CCMR1 &= ~TIM_CCMR1_OC1M;                     // Clear ouput compare mode bits for channel 1
	TIMx->CCMR1 |= TIM_CCMR1_OC1M_1 | TIM_CCMR1_OC1M_2; // OC1M = 110 for PWM Mode 1 output on ch1. #define TIM_CCMR1_OC1M_1          (0x2UL << TIM_CCMR1_OC1M_Pos)
	TIMx->CCMR1	|= TIM_CCMR1_OC1PE;                     // Output 1 preload enable (make CCR1 value changable)
			
	TIMx->CCER &= ~TIM_CCER_CC1P;                       // select output polarity: active high	
	TIMx->CCER  |= TIM_CCER_CC1E;						// Enable output for ch1
	TIMx->CCR1  = 99/2; 								// Output Compare Register for channel 1 (default duty ratio = 50%)
	
	TIMx->CR1  |= TIM_CR1_CEN;							// Enable counter

	

	// Inifinite Loop ----------------------------------------------------------
	while(1){
		for (int i=0;i<3;i++){
		TIM2->CCR1 = 99*i/2;
		delay_ms(100);
		}
	}
}
```
