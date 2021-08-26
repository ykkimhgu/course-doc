# Tutorial: Creating Application API

## Overview

In this tutorial, you will learn concept of API, HAL and how to create application API.

* Understand the structure of mbed-os.
* Understand about the usage HAL and API and their applications.

![Structure of mbed-os](../../.gitbook/assets/image%20%2857%29.png)

## mbed GPIO\_DI

### mbed Application API - DigitalIn.h // hal/gpio\_api.h

{% tabs %}
{% tab title="DigitalIn.h" %}
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

{% tab title="hal/gpio\_api.h" %}
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

### 

