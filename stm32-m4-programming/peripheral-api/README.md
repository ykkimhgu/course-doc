# Firmware Library

Github for Embedded Controller Students. You can download some sample library files.

{% embed url="https://github.com/ykkimhgu/EC-student/" %}

## List of EC\_HAL library

These are the list of functions you will create and use in our lecture.&#x20;

### version 2

> EC 2024\~

```cpp
///////////////////////////////////////////////////////
// EC HAL driver list

// PinName
void ecPinmap(PinName_t pinName, GPIO_TypeDef **GPIOx, unsigned int *pin);

// GPIO
void GPIO_init2(PinName_t pinName, uint32_t mode);
void GPIO_mode2(PinName_t pinName, uint32_t mode);

// PWM
void PWM_init(PinName_t pinName);
void PWM_pinmap(PinName_t pinName, TIM_TypeDef **TIMx, int *chN);
void PWM_period(PinName_t pinName,  uint32_t msec);	
void PWM_period_ms(PinName_t pinName,  uint32_t msec);
void PWM_period_us(PinName_t pinName, uint32_t usec);
void PWM_pulsewidth(PinName_t pinName, uint32_t pulse_width_ms);
void PWM_pulsewidth_ms(PinName_t pinName, uint32_t pulse_width_ms);
void PWM_duty(PinName_t pinName, float duty);

// Input Capture
void ICAP_pinmap(PinName_t pinName, TIM_TypeDef **TIMx, int *chN);
void ICAP_init(PinName_t pinName);
void ICAP_setup(PinName_t pinName, int ICn, int edge_type);
void ICAP_counter_us(PinName_t pinName, int usec);
uint32_t ICAP_capture(TIM_TypeDef* TIMx, uint32_t ICn);

// USART
void UART1_init(void);
void UART2_init(void);	
void UART1_baud(uint32_t baud);
void UART2_baud(uint32_t baud);
void USART1_write(uint8_t* buffer, uint32_t nBytes);
void USART2_write(uint8_t* buffer, uint32_t nBytes);
uint8_t USART1_read(void);										
uint8_t USART2_read(void);	
uint32_t is_USART1_RXNE(void);
uint32_t is_USART2_RXNE(void);

void USART_write(USART_TypeDef* USARTx, uint8_t* buffer, uint32_t nBytes);
void USART_init(USART_TypeDef* USARTx, uint32_t baud);  		
void UART_baud(USART_TypeDef* USARTx, uint32_t baud);											
uint32_t is_USART_RXNE(USART_TypeDef * USARTx);
uint8_t USART_read(USART_TypeDef * USARTx);										
void USART_setting(USART_TypeDef* USARTx, GPIO_TypeDef* GPIO_TX, int pinTX, GPIO_TypeDef* GPIO_RX, int pinRX, uint32_t baud); 
void USART_delay(uint32_t us);  

// ADC
void ADC_init(PinName_t pinName);
void ADC_sequence(PinName_t *seqCHn, int seqCHnums); 
void ADC_start(void);
uint32_t is_ADC_EOC(void);
uint32_t is_ADC_OVR(void);
void clear_ADC_OVR(void);
uint32_t ADC_read(void);

// Injected ADC
void JADC_init(PinName_t pinName);
void JADC_sequence(PinName_t *seqCHn, int seqCHnums); 
void JADC_start(void);
uint32_t is_ADC_JEOC(void);
void clear_ADC_JEOC(void);
uint32_t JADC_read(int JDRn);

void ADC_conversion(int convMode);
void ADC_trigger(TIM_TypeDef* TIMx, int msec, int edge);
void JADC_trigger(TIM_TypeDef* TIMx, int msec, int edge);
void ADC_pinmap(PinName_t pinName, uint32_t *chN);

```

### version 1

> EC 2019\~2023

```cpp
///////////////////////////////////////////////////////
// EC HAL function list

// Clock RCC
void RCC_HSI_init(void);
void RCC_PLL_init(void);
void RCC_GPIOA_enable(void);
void RCC_GPIOB_enable(void);
void RCC_GPIOC_enable(void);
void RCC_GPIOD_enable(void);

// GPIO
void GPIO_init(GPIO_TypeDef *Port, int pin, int mode);
void GPIO_write(GPIO_TypeDef *Port, int pin, int Output);
int  GPIO_read(GPIO_TypeDef *Port, int pin);
void GPIO_mode(GPIO_TypeDef* Port, int pin, int mode);
void GPIO_ospeed(GPIO_TypeDef* Port, int pin, int speed);
void GPIO_otype(GPIO_TypeDef* Port, int pin, int type);
void GPIO_pupd(GPIO_TypeDef* Port, int pin, int pupd);

// Seven segment
void sevensegment_init(void);
void sevensegment_decode(int num);
void sevensegment_display_init(void);
void sevensegment_display(uint8_t num);

// EXT Interrupt
void EXTI_init(GPIO_TypeDef *Port, int pin, int trig, int priority);
void EXTI_enable(uint32_t pin);
void EXTI_disable(uint32_t pin);
uint32_t is_pending_EXTI(uint32_t pin);
void clear_pending_EXTI(uint32_t pin);

// SysTick
void SysTick_init(void);
void SysTick_Handler(void); // in Main()
void SysTick_counter(void);
void delay_ms(uint32_t msec);
void SysTick_reset(void);
uint32_t SysTick_val(void);

// Timer
void TIM_init(TIM_TypeDef *TIMx, uint32_t msec);
void TIM_period(TIM_TypeDef* TIMx, uint32_t msec);		
void TIM_period_us(TIM_TypeDef* TIMx, uint32_t usec);  	
void TIM_period_ms(TIM_TypeDef* TIMx, uint32_t msec);

void TIM_UI_init(TIM_TypeDef* TIMx, uint32_t msec); 
void TIM_UI_enable(TIM_TypeDef* TIMx); 
void TIM_UI_disable(TIM_TypeDef* TIMx);

uint32_t is_UIF(TIM_TypeDef *TIMx);
void clear_UIF(TIM_TypeDef *TIMx);
uint32_t is_CCIF(TIM_TypeDef *TIMx, uint32_t CCnum);
void clear_CCIF(TIM_TypeDef *TIMx, uint32_t CCnum);

// Input Capture
typedef struct{
	GPIO_TypeDef *port;
	int pin;   
	TIM_TypeDef *timer;
	int ch; 
	int ICnum;
} IC_t;

void ICAP_pinmap(IC_t *timer_pin);
void ICAP_init(IC_t *ICx, GPIO_TypeDef *port, int pin);
void ICAP_setup(IC_t *ICx, int ICn_type, int edge_type);
void ICAP_counter_us(IC_t *ICx, int usec);
void TIM_TRGO_init(TIM_TypeDef* timx, uint32_t msec);

// PWM
typedef struct{
   GPIO_TypeDef *port;
   int pin;
   TIM_TypeDef *timer;
   int ch;
} PWM_t;

void PWM_init(PWM_t *pwm, GPIO_TypeDef *port, int pin);
void PWM_period_ms(PWM_t *pwm,  uint32_t msec);		
void PWM_period_us(PWM_t *pwm, uint32_t usec); 

void PWM_pulsewidth_ms(PWM_t *pwm, int pulse_width_ms);
void PWM_pulsewidth_us(PWM_t *pwm, int pulse_width_us);
void PWM_duty(PWM_t *pwm, float duty);
void PWM_pinmap(PWM_t *PWM_pin);

// USART
void UART2_init(void);
void USART_write(USART_TypeDef* USARTx, uint8_t* buffer, uint32_t nBytes);
void USART_delay(uint32_t us);  
void USART_begin(USART_TypeDef* USARTx, GPIO_TypeDef* GPIO_TX, int pinTX, GPIO_TypeDef* GPIO_RX, int pinRX, int baud);
void USART_init(USART_TypeDef* USARTx, int baud);  															
uint8_t USART_getc(USART_TypeDef * USARTx);										
uint32_t is_USART_RXNE(USART_TypeDef * USARTx);

// ADC
void ADC_init(GPIO_TypeDef *port, int pin, int trigmode);
void ADC_continue(int contmode); 													
void ADC_TRGO(TIM_TypeDef* TIMx, int msec, int edge);
void ADC_sequence(int length, int *seq); 
void ADC_start(void);
uint32_t is_ADC_EOC(void);
uint32_t is_ADC_OVR(void);
void clear_ADC_OVR(void);
uint32_t ADC_read(void);
uint32_t ADC_pinmap(GPIO_TypeDef *port, int pin);

// JADC
void JADC_init(GPIO_TypeDef *port, int pin, int mode);
void JADC_TRGO(TIM_TypeDef* TIMx, int msec, int edge);
void JADC_sequence(int length, int *seq); 
void JADC_start(void);
```

