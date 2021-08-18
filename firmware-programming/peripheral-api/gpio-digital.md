# GPIO Digital





```cpp
////    -2021-      //////
class DigitalIn
{
public:
    DigitalIn(PinName pin) : gpio()
    {
        gpio_init_in(&gpio, pin);
        //gpio() 교체하기!
    }

    DigitalIn(PinName pin, PinMode mode) : gpio()
    {
        gpio_init_in_ex(&gpio, pin, mode);
    }

    ~DigitalIn()
    {
        gpio_free(&gpio);
    }

    int read()
    {
        return gpio_read(&gpio);
        //GPIO_Read(GPIOX, Pin, IDR)
    }

    void mode(PinMode pull);
    int is_connected()
    {
        return gpio_is_connected(&gpio);
    }

    operator int()
    {
        return read();
    }

protected:
#if !defined(DOXYGEN_ONLY)
    gpio_t gpio;
#endif //!defined(DOXYGEN_ONLY)
};

```



```cpp
///////////////////////////////////////////////////////
// ec C functions for LAB

void GPIO_init(GPIO_TypeDef* Port, int Pin, int mode);
void GPIO_write(GPIO_TypeDef* Port, int Pin, int Output);
int  GPIO_read(GPIO_TypeDef* Port, int Pin);

//
void GPIO_Initialize(GPIOX, Pin, I / OMode)
void GPIO_OutMode(GPIOX, Pin, Outmode)
void GPIO_OutPUDR(GPIOX, Pin, PUDR)
void GPIO_InMode(GPIOX, Pin, InMode)
void GPIO_InPUDR(GPIOX, Pin, PUDR)
void GPIO_Read(GPIOX, Pin, IDR)
void GPIO_Write(GPIOX, Pin, ODR)


////
void SysTick_init(void);
void SysTick_Handler(void);
void SysTick_counter();
void delay (uint32_t T);

```



```cpp
/**
******************************************************************************
* @author  SSSLAB
* @Mod		 2021-8-12 by YKKIM  	
* @brief   Embedded Controller:  Tutorial Digital In
*					 - Turn on LED LD2 while Button B1 is pressing
* 
******************************************************************************
*/


// GPIO Mode			 : Input(00), Output(01), AlterFunc(10), Analog(11, reset)
// GPIO Speed			 : Low speed (00), Medium speed (01), Fast speed (10), High speed (11)
// GPIO Output Type: Output push-pull (0, reset), Output open drain (1)
// GPIO Push-Pull	 : No pull-up, pull-down (00), Pull-up (01), Pull-down (10), Reserved (11)


#include "stm32f4xx.h"
#define LED_PIN    5		//LD2
#define BUTTON_PIN 13


void RCC_HSI_init(void);   							//defined in ecRcc.h
void RCC_GPIOA_enable(void);
void RCC_GPIOC_enable(void);

int main(void) {	
	/* Part 1. RCC GPIOA Register Setting */
		RCC_GPIOA_enable();
		RCC_GPIOC_enable();
		
	/* Part 2. GPIO Register Setting for OUTPUT*/			
		// GPIO Mode Register
		GPIOA->MODER &= ~(3UL<<(2*LED_PIN)); // Clear '00' for Pin 5
		GPIOA->MODER |=   1UL<<(2*LED_PIN);  // Set '01' for Pin 5
		
		// GPIO Output Type Register  
		GPIOA->OTYPER &= ~(1UL<<LED_PIN);   	// 0:Push-Pull   
		
		// GPIO Pull-Up/Pull-Down Register 
		GPIOA->PUPDR  &= ~(3UL<<(2*LED_PIN)); // 00: none
		
		// GPIO Output Speed Register 
		GPIOA->OSPEEDR &= ~(3UL<<(2*LED_PIN));
		GPIOA->OSPEEDR |=   2UL<<(2*LED_PIN);  //10:Fast Speed
	
	
	/* Part 3. GPIO Register Setting for INPUT*/			
		// GPIO Mode Register
		GPIOC->MODER &= ~(3UL<<(2*BUTTON_PIN)); // 00: Input	 		
   
		// GPIO Pull-Up/Pull-Down Register 
		GPIOC->PUPDR &= ~(3UL<<(2*BUTTON_PIN)); 
		GPIOC->PUPDR  |= 2UL<<(2*BUTTON_PIN); 	// 10: Pull-down		    
	 
	 
	/* Part 4. Deal loop  */	
		while(1){
			unsigned int btVal=0;
			//Read bit value of Button
			btVal=(GPIOC->IDR) & (1UL << BUTTON_PIN);	
			if(btVal == 0)
				GPIOA->ODR |= (1UL << LED_PIN);	 		
			else
				GPIOA->ODR &= ~(1UL << LED_PIN); 
		}
}



void RCC_GPIOA_enable()
{
		// HSI is used as system clock         
		RCC_HSI_init();	
	
		// RCC Peripheral Clock for GPIO_A Enable 
		RCC->AHB1ENR |= RCC_AHB1ENR_GPIOAEN;
}

void RCC_GPIOC_enable()
{
		// HSI is used as system clock         
		RCC_HSI_init();	
	
		// RCC Peripheral Clock for GPIO_A Enable 
		RCC->AHB1ENR |= RCC_AHB1ENR_GPIOCEN;
}


void RCC_HSI_init() {
	// Enable High Speed Internal Clock (HSI = 16 MHz)
  RCC->CR |= ((uint32_t)RCC_CR_HSION);

  // wait until HSI is ready
  while ( (RCC->CR & (uint32_t) RCC_CR_HSIRDY) == 0 ) {;}
	
  // Select HSI as system clock source 
  RCC->CFGR &= (uint32_t)(~RCC_CFGR_SW); 									
  RCC->CFGR |= (uint32_t)RCC_CFGR_SW_HSI; 				
			
	// Wait till HSI is used as system clock source
  while ((RCC->CFGR & (uint32_t)RCC_CFGR_SWS) != 0 ); 
}





```



```cpp
/**
******************************************************************************
* @author  SSSLAB
* @Mod		 2021-8-12 by YKKIM  	
* @brief   Embedded Controller:  LAB Digital In/Out
*					 - Toggle LED LD2 by Button B1  pressing
* 
******************************************************************************
*/

#include "stm32f4xx.h"
#include "myGPIO.h"
#include "myRCC.h"

#define LED_PIN 	5
#define BUTTON_PIN 13

void setup(void);
	
int main(void) { 
	// Initialiization --------------------------------------------------------
	setup();
	
	// Inifinite Loop ----------------------------------------------------------
	while(1){
		if(GPIO_read(GPIOC, BUTTON_PIN) == 0)	GPIO_write(GPIOA, LED_PIN, HIGH);
		else 																	GPIO_write(GPIOA, LED_PIN, LOW);
	}
}


// Initialiization 
void setup(void)
{
	RCC_HSI_init();	
	GPIO_init(GPIOC, BUTTON_PIN, INPUT);  // calls RCC_GPIOC_enable()
	GPIO_init(GPIOA, LED_PIN, OUTPUT);    // calls RCC_GPIOA_enable()
}
```



```cpp
int main(void)
{
//
GPIO_mode(GPIOA, LED_PIN,0);
//
}

// GPIO Mode          : Input(00), Output(01), AlterFunc(10), Analog(11, reset)
void GPIO_mode(GPIO_TypeDef *Port, int Pin, int mode){
   Port->MODER &= ~(3UL<<(2*Pin));     
   Port->MODER |= mode<<(2*Pin);    
}
```

