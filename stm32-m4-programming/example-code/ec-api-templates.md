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
	// RCC Configuration (PLL: 84MHz)
	RCC_PLL_init();
    
	// SysTick Configuration (delay_ms)
	SysTick_init();
    
	// GPIO Configuration
	GPIO_init(GPIOA, LED_PIN, OUTPUT);
    
	// External Interrupt Configuration
    GPIO_init(GPIOC, BUTTON_PIN, INPUT);
    GPIO_pupd(GPIOC, BUTTON_PIN, EC_PD);
	EXTI_init(GPIOC, BUTTON_PIN, FALL, 0);
    
	// Timer Configuration
	TIM_init(TIM1, 1);
    
	// Timer IRQ Interrupt Configuration
	TIM_UI_init(TIM4, 1);
    
	// PWM Configuration
	PWM_init(PA_1);
	PWM_period(PA_1, 1);
    PWM_duty(PA_1, 0.f);
    
	// Stepper Motor Configuration
	Stepper_init(GPIOB, 10, GPIOB, 4, GPIOB, 5, GPIOB, 3);
	Stepper_setSpeed(5);
    
    // Ultrasonic Configuration
    // Trigger: PWM
    PWM_init(PA_6);
    PWM_period_us(PA_6, 50000);
    PWM_pulsewidth_us(PA_6, 10);
    
	// ECHO: Input Capture Configurtion
	ICAP_init(PB_6);
    ICAP_counter_us(PB_6, 10);
    ICAP_setup(PB_6, 1, IC_RISE);
    ICAP_setup(PB_6, 2, IC_FALL);
    
	// ADC Configuration
    PinName_t seqCHn[2] = {PB_0, PB_1};
	ADC_init(PB_1);
    ADC_init(PB_0)
	ADC_trigger(seqCHn, 2);
    
	// Injected ADC Configuration
    PinName_t seqCHn[2] = {PB_0, PB_1};
	JADC_init(PB_1);
    JADC_init(PB_0);
	JADC_sequence(seqCHn, 2);
    
	// UART Configuration
	UART1_init();
	UART1_baud(BAUD_9600);
	UART2_init();
	UART2_baud(BAUD_38400);
}

// Main - Polling
void main()
{
	setup();
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
	if(is_pending_EXTI(BUTTON_PIN)){
		/* USER CODE BEGIN EXTI0_IRQn 0 */
		/* USER CODE END EXTI0_IRQn 0 */
		/* USER CODE BEGIN EXTI0_IRQn 1 */
		/* USER CODE END EXTI0_IRQn 1 */
		
		// Clear pending
		clear_pending_EXTI(BUTTON_PIN);
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
	if(is_USART_RXNE(USART1)){
		// USART interrupt call whenever data is received
	}
}
```
