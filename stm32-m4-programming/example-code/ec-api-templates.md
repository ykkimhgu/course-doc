# Code Templates

## Example Code Structure for   main.c&#x20;

```cpp
/**
******************************************************************************
* @course   	Embedded Controller- HGU
* @author	iiLAB
* @mod		2023-12-05 by YKKIM
* @brief	Example Code Structure
* 
******************************************************************************
*/

#include “ecSTM32F411.h”	// includes all necessary library

// Initialization
void setup()
{
	// RCC Configuration (HSI: 16MHz, PLL: 84MHz)
	RCC_HSI_init();
	RCC_PLL_init();
    
	// SysTick Configuration (delay_ms)
	SysTick_init();
    
	// GPIO Configuration
	GPIO_init();
    
	// External Interrupt Configuration
	EXTI_init();
    
	// Timer Configuration
	TIM_init();
	TIM_period();
    
	// Timer IRQ Interrupt Configuration
	TIM_UI_init();
    
	// PWM Configuration
	PWM_init();
	PWM_period();
    
	// Stepper Motor Configuration
	Stepper_init();
	Stepper_setSpeed();
    
	// Input Capture Configurtion
	ICAP_init();
    
	// ADC Configuration
	ADC_init();
	ADC_sequence();
    
	// Injected ADC Configuration
	JADC_init();
	JADC_sequence();
    
	// UART Configuration
	UART1_init();
	UART1_baud();
	UART2_init();
	UART2_baud();
}

// Main - Polling
void main()
{
	setup( );
	while(1)
	{
	// polling logic goes here
	}
}


// For Interrupts,
// 1. Set the priority of each type of interrupt handler
// 2. Make sure interrupts is as short as possible. 
// 3. For a periodic interrupt, check the calculation time within the interrupt handler. It should not go beyond the interrupt period

void TIM2_IRQHanlder(void){   // TIM1~TIM8
	if(is_UIF(TIM2)){
		// Periodic tasks. Such as
		// Sensor read
		// motor controller out, etc 
		// make it as short as possible
        
		// clear UIF
		clear_UIF(TIM2);
	}
}

void SysTick_Handler(void){
	// Periodic tasks. Such as time measurement etc. 
	// make it as short as possible
}

void EXTI15_10_IRQHandler(void){   // EXTI0~15
	if(is_pending_EXTI(PIN)){
		/* USER CODE BEGIN EXTI0_IRQn 0 */
		/* USER CODE END EXTI0_IRQn 0 */
		/* USER CODE BEGIN EXTI0_IRQn 1 */
		/* USER CODE END EXTI0_IRQn 1 */
		
		// Clear pending
		clear_pending_EXTI(PIN);
	}
}

void ADC_IRQHandler(void){
	// ADC Overflow flag
	if(is_ADC_OVR()){
		clear_ADC_OVR();
	}
    
	// ADC finishing sequence
	if(is_ADC_EOC()){
		// Periodic ADC acquisition. 
		// Configure sampling rate with the period of
		// triggering Timer
	}

	// JADC finishing sequence
	if(is_ADC_JEOC()){
		// Periodic ADC acquisition. 
		// Configure sampling rate with the period of
		// triggering Timer
        
		// clear ADC JEOC
		clear_ADC_JEOC();
	}
}

void USART1_IRQHandler(void){   // USART1~USART2
	if(is_USART1_RXNE()){
		// USART interrupt call whenever data is received
	}
}
```
