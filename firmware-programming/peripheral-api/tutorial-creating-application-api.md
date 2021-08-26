# Tutorial: Creating Application API

## Overview

In this tutorial, you will learn concept of API, HAL and how to create application API.

* Understand the structure of mbed-os.
* Understand about the usage HAL and API and their applications.

![Structure of mbed-os](../../.gitbook/assets/image%20%2857%29.png)

## mbed GPIO\_DI

### mbed Application API - DigitalIn.h // HAL - gpio\_api.h

{% tabs %}
{% tab title="API - DigitalIn.h" %}
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
#ifdef FEATURE_EXPERIMENTAL_API
    final : public interface::DigitalIn
#endif
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

{% tab title="HAL - gpio\_api.h" %}
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

The above code is a header file about Digital in included in mbed-os. Basically, Application API is designed in a class structure so that it is easy to call and apply from the main code.

DigitalIn API calling the functions written in HAL, it automatically finds the GPIO applied to the pin number according to the pinname and calls function by callback.

Here, we can take a look at the features of the API applied to mbed-os.

* It has a complex structure, but it is sophisticated and systematic.
* Designed considering the differences in the hardware level.

## User GPIO\_DI

### User Application API - EC\_GPIO.h // HAL - ecGPIO.h

{% tabs %}
{% tab title="API - EC\_GPIO.h" %}
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

{% tab title="HAL - ecGPIO.h" %}
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

To create User's defined Application API, user defined HAL which is a sub-level function is required. 

After including the HAL header, functions defined in HAL are imported and configured in a form similar to that of mbed-os API. 

However, due to hardware function issues, we have to input the pin number and GPIO. 

## main function comparison

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



