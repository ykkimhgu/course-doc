# GPIO Digital

## CMSIS

### Tutorial Code: LED Toggle

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
  
  
  void bit_toggle(unsigned int pinNum){
   GPIOA->ODR ^= 1<<pinNum;
   }
}
}




```

##

## HAL

### ecGPIO.h (partial code)

```cpp
#include "stm32f411xe.h"

#ifndef __EC_GPIO_H
#define __EC_GPIO_H

#define INPUT  0x00
#define OUTPUT 0x01
#define AF     0x02
#define ANALOG 0x03

#define HIGH 1
#define LOW  0

#define LED_PIN 	5
#define BUTTON_PIN 13

void GPIO_init(GPIO_TypeDef *Port, int pin, int mode);
void GPIO_write(GPIO_TypeDef *Port, int pin, int Output);
int  GPIO_read(GPIO_TypeDef *Port, int pin);
void GPIO_mode(GPIO_TypeDef* Port, int pin, int mode);
void GPIO_ospeed(GPIO_TypeDef* Port, int pin, int speed);
void GPIO_otype(GPIO_TypeDef* Port, int pin, int type);
void GPIO_pudr(GPIO_TypeDef* Port, int pin, int pudr);

#endif
```

### ecGPIO.cpp (a partial code)

```cpp
#include "stm32f4xx.h"
#include "stm32f411xe.h"
#include "ecGPIO.h"


void GPIO_init(GPIO_TypeDef *Port, int pin, int mode){     
   
	if (Port == GPIOA)
		RCC_GPIOA_enable();
	if (Port == GPIOC)
		RCC_GPIOC_enable();
	// Make it for GPIOB, GPIOD..GPIOH

	// You can make a more general function of
	// void RCC_GPIO_enable(GPIO_TypeDef *Port); 

	GPIO_mode(Port, pin, mode);
	// The rest are default values
}

//int main(void)
//{
//
//   GPIO_mode(GPIOA, LED_PIN,0);
//
//}

// GPIO Mode          : Input(00), Output(01), AlterFunc(10), Analog(11, reset)
void GPIO_mode(GPIO_TypeDef *Port, int pin, int mode){
   Port->MODER &= ~(3UL<<(2*pin));     
   Port->MODER |= mode<<(2*pin);    
}

```

### Example code for LAB: LED toggle

Tutorial\_DigitalInOut\_LED\_Button\_HAL.c

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

## Application API

### EC\_GPIO.h

```cpp
#include "stm32f411xe.h"
#include "ecGPIO.h"
#include "ecRCC.h"


#ifndef __EC_GPIO_API_H
#define __EC_GPIO_API_H


#define EC_DOUT  	1
#define EC_DIN 		0

#define EC_PU 1
#define EC_PD 0
#define EC_NONE 0

#define EC_LOW 		0
#define EC_MEDIUM 1
#define EC_FAST 	2
#define EC_HIGH 	3

#define LED_PIN 	5
#define BUTTON_PIN 13


/* System CLOCK is HSI by default */

class EC_DigitalIn
{
public:
    EC_DigitalIn(GPIO_TypeDef *Port, int pin) 
    {
			uint8_t mode=EC_DIN; // mode=0
			GPIO_init(Port, pin, mode);
			Port_t=Port;
			pin_t=pin;
			mode_t=mode;	
    }

    ~EC_DigitalIn()
    {
			 delete[] Port_t;
    }

    int read()
    {
				val_t=GPIO_read(Port_t, pin_t);
				return val_t;
    }
		
		void pupdr(int _pupd){
			GPIO_pudr(Port_t, pin_t, _pupd);
		}
    
    operator int()
    {
        return read();
    }

	private:
			GPIO_TypeDef *Port_t;
			int	pin_t;
			int mode_t;	
			int val_t;	
};



class EC_DigitalOut
{
public:
		EC_DigitalOut(GPIO_TypeDef *Port, int pin);
		// Exercise. Define the function in EC_GPIO.cpp

    ~EC_DigitalOut()
    {
			 delete[] Port_t;
    }

    void write(int _outVal);
		// Exercise. Define the function in EC_GPIO.cpp

  	void pupdr(int _pupd);
		// Exercise. Define the function in EC_GPIO.cpp
		
		void otype(int _type);
		// Exercise. Define the function in EC_GPIO.cpp
		
		void ospeed(int _speed);
	// Exercise. Define the function in EC_GPIO.cpp	
	
		EC_DigitalOut &operator= (int value)
    {
				 write(value);
				 return *this;
    }
		int read()
    {
				return GPIO_read(Port_t, pin_t);
    }
		operator int()
		{
		// Underlying call is thread safe
			return read();
		}

	private:
			GPIO_TypeDef *Port_t;
			int	pin_t;
			int mode_t;	

};

#endif
```

### EC\_GPIO.cpp

```cpp
#include "EC_GPIO.h"

/* System CLOCK is HSI by default */


EC_DigitalOut::EC_DigitalOut(GPIO_TypeDef *Port, int pin) 
{
			uint8_t mode=EC_DOUT;  // mode=1;			
			GPIO_init(Port, pin, mode);
			this->Port_t=Port;
			this->pin_t=pin;
			this->mode_t=mode;	
}
	
		
void EC_DigitalOut::write(int _outVal)
{
		GPIO_write(Port_t, pin_t, _outVal);
}

void EC_DigitalOut::pupdr(int _pupd){
	GPIO_pudr(Port_t, pin_t, _pupd);
}

void EC_DigitalOut::otype(int _type){
	GPIO_otype(Port_t, pin_t, _type);
}

void EC_DigitalOut::ospeed(int _speed){
	GPIO_ospeed(Port_t, pin_t, _speed);
}




```

### Example code for LAB: LED toggle

Tutorial\_DigitalInOut\_LED\_Button\_API.

```cpp
/**
******************************************************************************
* @author  SSSLAB
* @Mod		 2021-8-12 by YKKIM  	
* @brief   Embedded Controller:  LAB Digital In/Out with API
*					 - Toggle LED LD2 by Button B1  pressing
* 
******************************************************************************
*/

#include "EC_GPIO.h"

#define LED_PIN 	5
#define BUTTON_PIN 13

EC_DigitalIn button(GPIOC,BUTTON_PIN);
EC_DigitalOut led(GPIOA,LED_PIN);

	
int main(void) { 
	// Initialiization --------------------------------------------------------

	// Inifinite Loop ----------------------------------------------------------
	while(1){
		//if(button.read() == 0)	led.write(HIGH);
		//else 										led.write(LOW);
		if(!button)	led=1;
		else 				led=0;

	}
}
```

### mbed

{% tabs %}
{% tab title="mbed" %}
```cpp
#include "mbed.h"

DigitalIn  button(USER_BUTTON);
DigitalOut led(LED1);

int main() {
    while(1) {
        if(!button) led = 1;
        else led = 0;
    }
}
```
{% endtab %}
{% endtabs %}
