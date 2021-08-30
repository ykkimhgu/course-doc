# Tutorial: Creating Application API

## Introduction 

In this tutorial, you will learn how to create user application API for a simple, user-friendly programming of MCU. 

We will create our own application API \(**EC API**\) , in similar format as mbed API. 

EC API  is based on the EC\_HAL API that is based on CMSIS-CORE.

![Structure of mbed-os](../../.gitbook/assets/image%20%2857%29.png)

### Comparison mbed API vs EC API

**mbed API example code**

Example code for Digital In and Out using  mbed 

```cpp
#include "mbed.h"

DigitalIn  button(USER_BUTTON);
DigitalOut led(LED1);

int main() {
    while(1) {
        if(!button)  led = 1;  //if(!button.read())
        else         led = 0;
    }
}
```



**EC API example code**

We are going to create EC API is similar form, such as 

{% tabs %}
{% tab title="EC\_API" %}
```cpp
#include "EC_GPIO.h"

#define LED_PIN 	5
#define BUTTON_PIN 13

EC_DigitalIn button(GPIOC,BUTTON_PIN);
EC_DigitalOut led(GPIOA,LED_PIN);

	
int main(void) { 
	while(1){
		if(!button)	led=1;			//if(!button.read())
		else 				led=0;
	}
}
```
{% endtab %}
{% endtabs %}

## Case study: mbed API

Lets analyze how user API is structured in mbed. The application API is defined with C++ class and its methods.  Each methods are based on HAL API, which is defined based on CMSIS-CORE.

For example, GPIO Digital In.

### \* mbed API:  Class Digital In \(DigitalIn.h\) 

### \* mbed HAL API:   gpio\_api.h

{% tabs %}
{% tab title="mbed API - DigitalIn.h" %}
```cpp
* mbed Microcontroller Library
 * Copyright (c) 2006-2020 ARM Limited

#ifndef MBED_DIGITALIN_H
#define MBED_DIGITALIN_H

#include "platform/platform.h"
#include "interfaces/InterfaceDigitalIn.h"
#include "hal/gpio_api.h"

namespace mbed {

class DigitalIn
{

public:
    
    DigitalIn(PinName pin) : gpio()
    {
        gpio_init_in(&gpio, pin);
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


} 

#endif

```
{% endtab %}

{% tab title="mbed HAL - gpio\_api.h" %}
```cpp

/** \addtogroup hal */

#ifndef MBED_GPIO_API_H
#define MBED_GPIO_API_H

#include <stdint.h>
#include "device.h"
#include "pinmap.h"

#ifdef __cplusplus
extern "C" {
#endif

typedef struct {
    uint8_t pull_none : 1;
    uint8_t pull_down : 1;
    uint8_t pull_up : 1;
} gpio_capabilities_t;

uint32_t gpio_set(PinName pin);

int gpio_is_connected(const gpio_t *obj);

void gpio_init(gpio_t *obj, PinName pin);
void gpio_free(gpio_t *obj);
void gpio_mode(gpio_t *obj, PinMode mode);
void gpio_dir(gpio_t *obj, PinDirection direction);
void gpio_write(gpio_t *obj, int value);
int gpio_read(gpio_t *obj);
void gpio_init_in(gpio_t *gpio, PinName pin);
void gpio_init_in_ex(gpio_t *gpio, PinName pin, PinMode mode);
void gpio_init_out(gpio_t *gpio, PinName pin);
void gpio_init_out_ex(gpio_t *gpio, PinName pin, int value);
void gpio_init_inout(gpio_t *gpio, PinName pin, PinDirection direction, PinMode mode, int value);
void gpio_get_capabilities(gpio_t *gpio, gpio_capabilities_t *cap);
const PinMap *gpio_pinmap(void);

#ifdef __cplusplus
}
#endif

#endif

/** @}*/
```
{% endtab %}
{% endtabs %}

#### mbed API:  Class Digital In \(DigitalIn.h\) 

DigitalIn header defines the application API designed in C++ class structure. After class construction/initiation, the methods are easy to be used by the user.  Here, you don't need to specifically define and refer to the register pointer for specific digital in pins. 

In Each methods, it calls the functions defined in mbed HAL\_API.

#### mbed HAL API:   gpio\_api.h

Underneath the simple application API,  it calls more complex, more lower level HAL API. For example, in class construction \(initialization\),  it finds which GPIO to be applied from the Pinname, using the call back function. `gpio_init_in(&gpio, pin);`



## Tutorial:  Create  EC\_API -   **for Digital In** 

Lets borrow the DigitalIn class from mbed API. To eliminate any redundancy defintion of variables, we will use prefix '**EC\_ '** for Class, Variable names.

### 

### Create Application API source file

#### Application API:   EC\_GPIO\_API.h, EC\_GPIO\_API.cpp 

First, create header and source file as  EC\_ GPIO _\__API. h  and  EC\_ GPIO _\__API. cpp

> we will use \*.cpp, which is C++ source file



### Define Application API 

Use the following source code to start.  ecGPIO.h is the file you have created in **LAB:GPIO Dgital InOut.**

Unlike mbed API, we are going to input the GPIO and the pin number for initialization.  

> In " EC\_GPIO\_API.cpp ",  you can define each methods. For this tutorial, we will use only \*.h header file

{% tabs %}
{% tab title="EC\_API - EC\_GPIO\_API.h" %}
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

{% tab title="EC\_HAL - ecGPIO.h" %}
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



### Use  Application API 

Lets compare the simple code based on 'EC\_HAL' vs  EC API'

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



## Exercise:  Create  EC\_API -   **for Digital Out** 

Lets borrow the Digital Out class from mbed API. To eliminate any redundancy defintion of variables, we will use prefix '**EC\_ '** for Class, Variable names.

### Download source file

Download the source files:  [EC\_GPIO\_API\_student.h, EC\_GPIO\_API\_student.cpp ](https://github.com/ykkimhgu/EC-student/tree/main/tutorial-student)



### Define Application API 

Complete the definition of the class  EC\_DigitiOut.

You need to define the function in  "EC\_GPIO\_API\_student.cpp" 



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



