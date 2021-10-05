# Peripheral Firmware

Github for Embedded Controller Students

{% embed url="https://github.com/ykkimhgu/EC-student/" %}



## List of EC\_HAL 

```cpp
///////////////////////////////////////////////////////
// EC HAL driver list

// GPIO
void GPIO_init(GPIO_TypeDef *Port, int pin, int mode);
void GPIO_write(GPIO_TypeDef *Port, int pin, int Output);
int  GPIO_read(GPIO_TypeDef *Port, int pin);
void GPIO_mode(GPIO_TypeDef* Port, int pin, int mode);
void GPIO_ospeed(GPIO_TypeDef* Port, int pin, int speed);
void GPIO_otype(GPIO_TypeDef* Port, int pin, int type);
void GPIO_pudr(GPIO_TypeDef* Port, int pin, int pudr);


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
void SysTick_reset(void);


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

## List of mbed API

### Input/Output drivers

| IN.OUT.Driver | Time | USART |
| :--- | :--- | :--- |
| [AnalogIn](https://os.mbed.com/docs/mbed-os/v6.13/apis/i-o-apis.html) | [Ticker](https://os.mbed.com/docs/mbed-os/v6.13/apis/ticker.html) | Serial |
| [AnalogOut](https://os.mbed.com/docs/mbed-os/v6.13/apis/analogout.html) |  [Time](https://os.mbed.com/docs/mbed-os/v6.13/apis/time.html) | ✔ |
| [DigitalIn](https://os.mbed.com/docs/mbed-os/v6.13/apis/digitalin.html) |  [Timer](https://os.mbed.com/docs/mbed-os/v6.13/apis/timer.html) | ✔ |
| [DigitalOut](https://os.mbed.com/docs/mbed-os/v6.13/apis/digitalout.html) | ✔ | ✔ |
| [DigitalInOut](https://os.mbed.com/docs/mbed-os/v6.13/apis/digitalinout.html) | ✔ | ✔ |
| [InterruptIn](https://os.mbed.com/docs/mbed-os/v6.13/apis/interruptin.html) | ✔ | ✔ |
| [PwmOut](https://os.mbed.com/docs/mbed-os/v6.13/apis/pwmout.html) | ✔ | ✔ |

### Platform

Platform APIs provide general purpose MCU management infrastructure, common data structures and a consistent user experience on top of different standard libraries and toolchains.

#### Time

 [Ticker](https://os.mbed.com/docs/mbed-os/v6.13/apis/ticker.html)

 [Time](https://os.mbed.com/docs/mbed-os/v6.13/apis/time.html)

 [Timer](https://os.mbed.com/docs/mbed-os/v6.13/apis/timer.html)





