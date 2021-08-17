# Peripheral API



## List of EC\_HAL API 

```cpp
///////////////////////////////////////////////////////
// EC HAL driver list

// GPIO
void GPIO_init(GPIO_TypeDef* Port, int Pin, int mode);
void GPIO_write(GPIO_TypeDef* Port, int Pin, int Output);
int  GPIO_read(GPIO_TypeDef* Port, int Pin);

// old
void GPIO_Initialize(GPIOX, Pin, I / OMode)
void GPIO_OutMode(GPIOX, Pin, Outmode)
void GPIO_OutPUDR(GPIOX, Pin, PUDR)
void GPIO_InMode(GPIOX, Pin, InMode)
void GPIO_InPUDR(GPIOX, Pin, PUDR)
void GPIO_Read(GPIOX, Pin, IDR)
void GPIO_Write(GPIOX, Pin, ODR)


// EXT Interrupt
void EXTI_init(GPIO_TypeDef *Port, int Pin, int trig,int priority);
uint32_t EXTI_is_cleared(uint32_t EXTInum);
void EXTI_clearpending(uint32_t EXTInum);
void EXTI_enable(uint32_t EXTInum);
void EXTI_disable(uint32_t EXTInum);


// SysTick
void SysTick_init(uint32_t msec);
void SysTick_Handler(void);  // in main()
void SysTick_delay(uint32_t msec);
uint32_t SysTick_val(void);


// Clock RCC
void RCC_HSI_init();
void RCC_PLL_init();


// PWM
void PWM_init(TIM_t *PWM_pin, GPIO_TypeDef *port, int pin);
void PWM_period_ms(TIM_t *PWM_pin, float period_ms);
void PWM_width_ms(TIM_t *PWM_pin, float pulse_width_ms);


// Timer
typedef struct{
	GPIO_TypeDef *port;		
	int pin;						
	TIM_TypeDef *timer;	
	int ch;							
}TIM_t;

void TIM_period_us(uint32_t nTimer, float usec);
void CAP_init(TIM_t *cap_pin, GPIO_TypeDef *port, int pin);
void CAP_setup(TIM_t *cap_pin, int ICn_type, int edge_type);
void TIM_pinmap(TIM_t *timer_pin);
void TIM3_init(float msec);


// USART
typedef struct{
   USART_TypeDef *USARTx;
   GPIO_TypeDef *tx_port;
   GPIO_TypeDef *rx_port;   
   int tx_pin;               
   int rx_pin;
   int baud_rate;   
   int type;
   int mode;
} UART_t;

void UART1_GPIO_init(void);
void UART2_GPIO_init(void);

void BT_init(int baud_rate);
void BT_write(uint8_t *buffer, uint32_t nBytes);
uint8_t BT_read ();

void UART1_init(int baud_rate, int mode);
void UART2_init(int baud_rate, int mode);

void USART_init (USART_TypeDef * USARTx, int baud_rate, int mode);
void USART_write(USART_TypeDef * USARTx, uint8_t *buffer, uint32_t nBytes);
uint8_t USART_read (USART_TypeDef * USARTx);
void USART_delay(uint32_t us);

void USART_IRQHandler(USART_TypeDef * USARTx, uint8_t *buffer, uint32_t * pRx_counter);
```



