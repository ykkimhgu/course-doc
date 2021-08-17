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

// SysTick
void SysTick_init(uint32_t msec);
void SysTick_Handler(void);  // in main()
void SysTick_delay(uint32_t msec);
uint32_t SysTick_val(void);


```



