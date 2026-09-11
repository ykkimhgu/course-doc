---
hidden: true
---

# Tutorial: Arduino-like API for STM

## Introduction

In this tutorial, you will learn how to create user application API similar to Arduino for a more user-friendly programming of MCU.



We will create our own API (**EC\_API**), in similar format as Arduino API, using the libraries created in the course.&#x20;



# Part 1.  EC STMArduino API  

https://docs.arduino.cc/learn/programming/reference/#digital-io



### Examples of Arduino API&#x20;

#### Digital I/O

```c
void pinMode(int pin, int mode);
int digitalRead(int pin);
void digitalWrite(int pin, int state);	

```

* mode= { INPUT, OUTPUT,INPUT_PULLUP, INPUT_PULLDOWN, OUTPUT_OPENDRAIN }


#### Time
```c
void delay(long milliseconds);
void delayMicroseconds(int microseconds);
```

#### Interrupts
```c
void attachInterrupt(int pin, void (*function)(void), int mode);
void detachInterrupt(int pin);

void interrupts();
void noInterrupts();
```





## Exercise  
### STMduino: GPIO I/O

Create the below API using your header files.  

To distinguish from arduino API, we are going to use capital letter for the first letter of the function name.

* pinMode () --> PinMode()

Download template code files: 



{% tabs %}
{% tab title="ecSTMduino2.h" %}

```cpp
#ifndef __EC_STMDUINO2_H
#define __EC_STMDUINO2_H

#include "stm32f411xe.h"
#include "ecPinNames.h"
#include "ecGPIO2.h"
#include "ecRCC2.h"
#include "ecSysTick2.h"
// Add More Lib
// #include "ecEXTI2.h"


/* ---------------------------------------------------------------
                    Init
---------------------------------------------------------------- */
// SYSCLK = PLL 84 MHz, SysTick = 1 ms tick, 
// Add more descriptions
void STMduinoInit(void);

/* ---------------------------------------------------------------
                    Digital I/O
---------------------------------------------------------------- */
#define INPUT_PULLUP      0x10
#define INPUT_PULLDOWN    0x11		
#define OUTPUT_OPENDRAIN  0x12		

void PinMode(PinName_t pin, uint32_t mode);
void DigitalWrite(PinName_t pin, uint32_t value);
int  DigitalRead(PinName_t pin);
```
{% endtab %}
{% tab title="ecSTMduino2.c" %}

```cpp
#include "ecSTMduino2.h"



/* ---------------------------------------------------------------
                    Init
---------------------------------------------------------------- */
void STMduinoInit(void){
	RCC_PLL_init();									// SYSCLK = 84 MHz, EC_SYSCLK updated
	SysTick_init();									// 1 ms tick, reload from EC_SYSCLK
	// Add More
}


/*----------------------------------------------------------------
                 GPIO  I/O  
---------------------------------------------------------------- */

// mode : INPUT, OUTPUT, INPUT_PULLUP, INPUT_PULLDOWN, OUTPUT_OPENDRAIN
void PinMode(PinName_t pin, int mode){
	switch (mode){
		case INPUT:				GPIO_init(pin, INPUT);							break;
		case INPUT_PULLUP:		GPIO_init(pin, INPUT);  GPIO_pupd(pin, EC_PU);	break;
		case INPUT_PULLDOWN:	GPIO_init(pin, INPUT);  GPIO_pupd(pin, EC_PD);	break;
		case OUTPUT:			GPIO_init(pin, OUTPUT);							break;	// push-pull by GPIO_init
		case OUTPUT_OPENDRAIN:	GPIO_init(pin, OUTPUT); GPIO_otype(pin, EC_OPEN_DRAIN); break;
		default:				break;
	}
}

void DigitalWrite(PinName_t pin, int value){
	//[TO-DO] YOUR CODE GOES HERE
}

int DigitalRead(PinName_t pin){
	//[TO-DO] YOUR CODE GOES HERE
	return 0; //[TO-DO] YOUR CODE GOES HERE
}

```

{% endtab %}
{% endtabs %}



### STMduino: Delay

{% tabs %}
{% tab title="ecSTMduino2.h" %}

```cpp
#ifndef __EC_STMDUINO2_H
#define __EC_STMDUINO2_H

#include "stm32f411xe.h"
#include "ecPinNames.h"
#include "ecGPIO2.h"
#include "ecRCC2.h"
#include "ecSysTick2.h"
// Add More Lib
// #include "ecEXTI2.h"



/* ---------------------------------------------------------------
                    Time (SysTick)
---------------------------------------------------------------- */
uint32_t Millis(void);
void     Delay(uint32_t ms);


```

{% endtab %}
{% tab title="ecSTMduino2.c" %}

```cpp
#include "ecSTMduino2.h"

/* ---------------------------------------------------------------
                    Time (SysTick)    
---------------------------------------------------------------- */

// msTicks (ecSysTick2.h) : 1 ms counter, ++ in SysTick_Handler()
uint32_t Millis(void){
	return msTicks;			
}

void Delay(uint32_t ms){
	//[TO-DO] YOUR CODE GOES HERE
}





```

{% endtab %}
{% endtabs %}





### EC_STMduino Example code

Compare this simple blink code with an Arduino Code

{% tabs %}
{% tab title="Sample: EC_STMduino" %}

```cpp
#include "ecSTMduino2.h"

#define LED PA_5

void setup(void) {
	PinMode(LED, OUTPUT);	
}

void loop(void) {
	DigitalWrite(LED, HIGH);
	Delay(500);
	DigitalWrite(LED, LOW);
	Delay(500);
}

//////////////////////////////////////////////////
// This is hidden in Arduino ino
int main(void) {	
	STMduinoInit();
    setup();
	while (1) loop();
}

```

{% endtab %}
{% tab title="Sample: Arduino" %}

```cpp
// Blink.ino

#define LED 5

void setup() {
  pinMode(LED, OUTPUT);
}

void loop() {
  digitalWrite(LED, HIGH);
  delay(500);
  digitalWrite(LED, LOW);
  delay(500);
}
```

{% endtab %}
{% endtabs %}



### (Optional Exercise) STMduino: Interrupt

```cpp
#ifndef __EC_STMDUINO2_H
#define __EC_STMDUINO2_H

#include "stm32f411xe.h"
#include "ecPinNames.h"
#include "ecGPIO2.h"
#include "ecRCC2.h"
#include "ecSysTick2.h"
// Need to include More Lib
 #include "ecEXTI2.h"


/* ---------------------------------------------------------------
                    External Interrupts
---------------------------------------------------------------- */
int  AttachInterrupt(PinName_t pin, void (*isr)(void), int mode);					// default priority
int  AttachInterruptPrio(PinName_t pin, void (*isr)(void), int mode, int priority);	// explicit priority
void DetachInterrupt(PinName_t pin);
void Interrupts(void);
void NoInterrupts(void);	


```







# Part 2.  STM Arduino API C++ Style - ecSTMduinoCPP





## Tutorial: Create EC\_API - **for Digital In**

We are going to make a more user-friendly API, similar to  mbed API based on C++. 

To eliminate any redundancy defintion of variables, we will use prefix '**EC\_ '** for Classnames.



### ecSTMduinoCPP.h



The header file is defined in C++.

> The main source code also must be  \*.cpp,  not in *.c

### 

Here is a sample code for the library. You can add more class and functions. 

{% tabs %}
{% tab title="EC_API - EC_GPIO_API.h" %}

```cpp
#include "stm32f411xe.h"
#include "ecGPIO.h"
#include "ecRCC.h"
#include <stdint.h>

#ifndef __EC_GPIO_API_H
#define __EC_GPIO_API_H

#define EC_DIN 		0

#define EC_PU 1
#define EC_PD 0
#define EC_NONE 0

#define EC_LOW 		0
#define EC_MEDIUM 1
#define EC_FAST 	2
#define EC_HIGH 	3

/* System CLOCK is HSI by default */

class EC_DigitalIn // declare class set the Application API name and declare class
{
public:
    EC_DigitalIn(GPIO_TypeDef *Port, int pin)
    // API initial seting
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
				val_t = GPIO_read(Port_t, pin_t);
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
```
{% endtab %}

{% tab title="EC_HAL - ecGPIO.h" %}
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

void GPIO_init(GPIO_TypeDef *Port, int pin, int mode);
void GPIO_write(GPIO_TypeDef *Port, int pin, int Output);
int  GPIO_read(GPIO_TypeDef *Port, int pin);
void GPIO_mode(GPIO_TypeDef* Port, int pin, int mode);
void GPIO_ospeed(GPIO_TypeDef* Port, int pin, int speed);
void GPIO_otype(GPIO_TypeDef* Port, int pin, int type);
void GPIO_pudr(GPIO_TypeDef* Port, int pin, int pudr);

#endif
```
{% endtab %}
{% endtabs %}

### Use Application API

Lets compare the simple code based on 'EC\_HAL' vs EC API'.

{% tabs %}
{% tab title="EC-HAL" %}
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
#include "ecGPIO.h"
#include "ecRCC.h"

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
{% endtab %}

{% tab title="EC-API" %}
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
	
		if(!button)	led=1;
		else 				led=0;
	}
}
```
{% endtab %}
{% endtabs %}

## Exercise: Create EC\_API - **for Digital Out**

Lets borrow the Digital Out class from mbed API. To eliminate any redundancy defintion of variables, we will use prefix '**EC\_ '** for Class, Variable names.

### Download source file

Download the source files: [EC\_GPIO\_API\_student.h, EC\_GPIO\_API\_student.cpp](https://github.com/ykkimhgu/EC-student/tree/main/tutorial/tutorial-student)

Rename the files as 'EC\_GPIO\_API.cpp ' and 'EC\_GPIO\_API.h'

### Define Application API

Complete the definition of the class EC\_DigitiOut.

You need to define the function in **"EC\_GPIO\_API.cpp"**

Then, run the following code.

```cpp
#include "EC_GPIO_API.h"

#define LED_PIN 	5
#define BUTTON_PIN 13

EC_DigitalIn button(GPIOC,BUTTON_PIN);
EC_DigitalOut led(GPIOA,LED_PIN);

	
int main(void) { 
	// Initialiization --------------------------------------------------------

	// Inifinite Loop ----------------------------------------------------------
	while(1){
		if(button.read() == 0)	led.write(HIGH);
		else 										led.write(LOW);
		//if(!button)	led=1;
		//else 				led=0;
		

	}
}
```
