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
	GPIO_otype(GPIOA, LED_PIN, EC_PUSH_PULL);
	GPIO_ospeed(GPIOA, LED_PIN, EC_HIGH);
	GPIO_write(GPIOA, LED_PIN, LOW);
    
	// External Interrupt Configuration
	GPIO_init(GPIOC, BUTTON_PIN, INPUT);
	GPIO_pupd(GPIOC, BUTTON_PIN, EC_PD);
	EXTI_init(GPIOC, BUTTON_PIN, FALL, 0);
    
	// Timer Configuration
	TIM_init(TIM2, 1);
    
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

void TIM4_IRQHanlder(void){   // TIM1~TIM8
	if(is_UIF(TIM4)){
		// Periodic tasks. Such as
		// Sensor read
		// motor controller out, etc 
		// make it as short as possible
        
		// clear UIF
		clear_UIF(TIM4);
	}
    
	if(is_CCIF(TIM4, 1)){
		// Capture value
        
		// clear CCIF
		clear_CCIF(TIM4, 1);
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
		// Periodic JADC acquisition. 
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

## Timer Input Capture: Ultrasonic Distance Sensor

```c
#include "ecSTM32F4v2.h"
#include "math.h"

uint32_t ovf_cnt = 0;//count over count
float distance = 0;
float timeInterval = 0;
float time1 = 0;//start time
float time2 = 0;//end time

#define TRIG PA_6 //pwm
#define ECHO PB_6 //echo


void setup(void);

int main(void){
	int count_test=0;
	setup();
	printf("Start LAB_TIMER_ICAP\r\n");
	while(1){
		distance = (float) timeInterval * 340.0 / 2.0 /10.0; 	// [mm] -> [cm]
		printf("%.2f cm\r\n", distance);//display distance
		delay_ms(1000);//0.5sec delay
	}
}

void TIM4_IRQHandler(void){
	if(is_UIF(TIM4)){                     // Update interrupt
		ovf_cnt++;													// overflow count
		clear_UIF(TIM4);  							    // clear update interrupt flag
	}
	if(is_CCIF(TIM4, 1)){ 								// TIM4_Ch1 (IC1) Capture Flag. Rising Edge Detect
		time1 = ICAP_capture(TIM4,1);									// Capture TimeStart
		clear_CCIF(TIM4, 1);                // clear capture/compare interrupt flag 
	}								                      
	else if(TIM4,2){ 									// TIM4_Ch2 (IC2) Capture Flag. Falling Edge Detect
		time2 = ICAP_capture(TIM4,2);									// Capture TimeEnd
		timeInterval = ((time2-time1)+ovf_cnt*((TIM4->ARR)+1))/100.0; 	// (10us * counter pulse -> [msec] unit) Total time of echo pulse
		ovf_cnt = 0;                        // overflow reset
		clear_CCIF(TIM4,2);								  // clear capture/compare interrupt flag 
	}
}

void setup(){

	RCC_PLL_init(); 
	SysTick_init();//1msec
	UART2_init1();
	GPIO_otype(TRIG, 0);//push pull
	GPIO_pupd(TRIG,0);//NO pull-up pull-down
	GPIO_ospeed(TRIG,EC_FAST);//FAST SPEED
	
	
  
// PWM configuration ---------------------------------------------------------------------	
	PWM_init(TRIG);			// PA_6: Ultrasonic trig pulse
	PWM_period_us(TRIG, 50000);    // PWM of 50ms period. Use period_us()
	PWM_pulsewidth_us(TRIG, 10);   // PWM pulse width of 10us
	
	
// Input Capture configuration -----------------------------------------------------------------------	
	ICAP_init(ECHO);    	// PB_6 as input caputre
	GPIO_pupd(ECHO,0);//NO pull-up pull-down
 	ICAP_counter_us(ECHO, 10);   	// ICAP counter step time as 10us
	ICAP_setup(ECHO, 1, IC_RISE);  // TIM4_CH1 as IC1 , rising edge detect
	ICAP_setup(ECHO, 2, IC_FALL);  // TIM4_CH2 as IC2 , falling edge detect

}

```
