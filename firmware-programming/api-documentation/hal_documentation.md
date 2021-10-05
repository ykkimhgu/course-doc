# HAL\_Documentation



```text
description: EC_HAL API
```

## Embedded Controller HAL

Written by: Your Name

Program: C/C++

IDE/Compiler: Keil uVision 5

OS: WIn10

MCU: STM32F411RE, Nucleo-64

#### Header File

 `#include "ecGPIO.h"`

### GPIO Digital In/Out

```text
#include "stm32f411xe.h"
​
#ifndef __EC_GPIO_H
#define __EC_GPIO_H
​
#define INPUT  0x00
#define OUTPUT 0x01
#define AF     0x02
#define ANALOG 0x03
​
#define HIGH 1
#define LOW  0
​
#define LED_PIN     5
#define BUTTON_PIN 13
​
​
void GPIO_init(GPIO_TypeDef *Port, int pin, int mode);
void GPIO_write(GPIO_TypeDef *Port, int pin, int Output);
int  GPIO_read(GPIO_TypeDef *Port, int pin);
void GPIO_mode(GPIO_TypeDef* Port, int pin, int mode);
void GPIO_ospeed(GPIO_TypeDef* Port, int pin, int speed);
void GPIO_otype(GPIO_TypeDef* Port, int pin, int type);
void GPIO_pupdr(GPIO_TypeDef* Port, int pin, int pudr);
​
#endif
​
```

#### GPIO\_init\(\)

Initializes GPIO pins: In/Out/AF/Analog

```text
void GPIO_init(GPIO_TypeDef *Port, int pin, int mode);
```

**Parameters**

* **Port:** Port Number, GPIOA~GPIOH
* **pin**: pin number \(int\) 0~15
* **mode**: INPUT \(0\), OUTPUT \(1\), AF\(02\), ANALOG \(03\)

**Example code**

```text
GPIO_init(GPIOA, 5, OUTPUT);
GPIO_init(GPIOC, 13, INPUT); //GPIO_init(GPIOC, 13, 0);
```

#### GPIO\_mode\(\)

Configures GPIO pin modes: In/Out/AF/Analog

```text
void GPIO_init(GPIO_TypeDef *Port, int pin, int mode);
```

**Parameters**

* **Port:** Port Number, GPIOA~GPIOH
* **pin**: pin number \(int\) 0~15
* **mode**: INPUT \(0\), OUTPUT \(1\), AF\(02\), ANALOG \(03\)

**Example code**

```text
GPIO_mode(GPIOA, 5, OUTPUT);
```

### Class or Header name

#### Function Name

```text
​
```

**Parameters**

* p1
* p2

**Example code**

```text
​
```

