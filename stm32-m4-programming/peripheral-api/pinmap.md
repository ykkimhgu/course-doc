# PinName Configuration

## Pinmap

Instead of separating GPIOx and Pin number, we can add these two information as one value using the defined Pinmap

```cpp
// A function that requires Port GPIOx and Pin number pin
// e.g.  foo(GPIOA, 5);
void foo(GPIO_TypeDef *GPIOx, int pin);


// is changed to 
// e.g. foo(PA_5);
void foo(PinName_t pin_num);

```

Example Code

`ecPinNames.h`

```cpp

#ifndef EC_PINNAMES_H
#define EC_PINNAMES_H

#include "stm32f411xe.h"

#ifdef __cplusplus
extern "C" {
#endif


typedef enum {
    PortA = 0,
    PortB = 1,
    PortC = 2,
    PortD = 3,
    PortE = 4,
    PortF = 5,
    PortG = 6,
    PortH = 7,
    PortI = 8,
    PortJ = 9,
    PortK = 10
} PortName_t;



typedef enum {
    PA_0  = 0x00,
    PA_1  = 0x01,    
    PA_2  = 0x02,
    PA_3  = 0x03,
    PA_4  = 0x04,    
    PA_5  = 0x05,
    PA_6  = 0x06,
    PA_7  = 0x07,
    PA_8  = 0x08,
    PA_9  = 0x09,
    PA_10 = 0x0A,
    PA_11 = 0x0B,
    PA_12 = 0x0C,
    PA_13 = 0x0D,
    PA_14 = 0x0E,
    PA_15 = 0x0F,
    
    PB_0  = 0x10,
    PB_1  = 0x11,
    PB_2  = 0x12,
    PB_3  = 0x13,
    PB_4  = 0x14,
    PB_5  = 0x15,
    PB_6  = 0x16,
    PB_7  = 0x17,
    PB_8  = 0x18,
    PB_9  = 0x19,
    PB_10 = 0x1A,
    PB_12 = 0x1C,
    PB_13 = 0x1D,
    PB_14 = 0x1E,
    PB_15 = 0x1F,

    PC_0  = 0x20,
    PC_1  = 0x21,
    PC_2  = 0x22,
    PC_3  = 0x23,
    PC_4  = 0x24,
    PC_5  = 0x25,
    PC_6  = 0x26,
    PC_7  = 0x27,
    PC_8  = 0x28,
    PC_9  = 0x29,
    PC_10 = 0x2A,
    PC_11 = 0x2B,
    PC_12 = 0x2C,
    PC_13 = 0x2D,
    PC_14 = 0x2E,
    PC_15 = 0x2F,

    PD_2  = 0x32,

    PH_0  = 0x70,
    PH_1  = 0x71,


    // Arduino connector namings
    A0          = PA_0,
    A1          = PA_1,
    A2          = PA_4,
    A3          = PB_0,
    A4          = PC_1,
    A5          = PC_0,
    D0          = PA_3,
    D1          = PA_2,
    D2          = PA_10,
    D3          = PB_3,
    D4          = PB_5,
    D5          = PB_4,
    D6          = PB_10,
    D7          = PA_8,
    D8          = PA_9,
    D9          = PC_7,
    D10         = PB_6,
    D11         = PA_7,
    D12         = PA_6,
    D13         = PA_5,
    D14         = PB_9,
    D15         = PB_8,


    // Not connected
    NC = (int)0xFFFFFFFF
} PinName_t;



void ec_pinmap(PinName_t pinName, GPIO_TypeDef *GPIOx, unsigned int *pin)
{
	
	unsigned int pinNum= pinName & (0x000F);
	*pin=pinNum;

	unsigned int portNum=(pinName>>4);
	if (portNum==0)
		GPIOx=GPIOA;
	else if (portNum==1)
		GPIOx=GPIOB;
	else if (portNum==2)
		GPIOx=GPIOC;
	else if (portNum==3)
		GPIOx=GPIOD;
	else if (portNum==7)
		GPIOx=GPIOH;
	else 
		GPIOx=GPIOA;
}




#ifdef __cplusplus
}
#endif

#endif
```

Example Application

```cpp
// GPIO Mode          : Input(00), Output(01), AlterFunc(10), Analog(11, reset)
void GPIO_mode(GPIO_TypeDef *Port, int pin, int mode){
   Port->MODER &= ~(3UL<<(2*pin));     
   Port->MODER |= mode<<(2*pin);    
}
//  In MAIN()
GPIO_mode(GPIOA, 5, OUTPUT);
GPIO_mode(GPIOC, 15, INPUT);

////////////////////////////////////////////////////////////

// GPIO Mode          : Input(00), Output(01), AlterFunc(10), Analog(11, reset)
void GPIO_mode2(PinNames_t Px_pin, int mode){
	GPIO_TypeDef *Port;
	unsigned int pin;
	ecPinmap(pinName, Port, &pin);
	

   Port->MODER &= ~(3UL<<(2*pin));     
   Port->MODER |= mode<<(2*pin);    
}

//  In MAIN()
GPIO_mode2(PA_5, OUTPUT);
GPIO_mode2(PC_15, INPUT);

// Other Options. All works fine
#define LED_PIN2 PA_5
PinName_t ledPin=PA_5;
unsigned int ledPin2=PA_5;	

GPIO_mode2(LED_PIN2, OUTPUT);  
GPIO_mode2(ledPin, OUTPUT);
GPIO_mode2(ledPin2, OUTPUT);
	

```
