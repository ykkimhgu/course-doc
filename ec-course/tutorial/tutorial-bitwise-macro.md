# Tutorial: Bitwise Macro

# Bitwise Operation Macro
Instead of using bitwise operations in C programming, which can be confusing to some students, we can use simple Macros for bitwise operations.

## Example: 

```cpp

// GPIO Output Set
GPIOA->ODR |= (1UL << LED_PIN);	 		
BIT_SET(GPIOA->ODR, LED_PIN);

// GPIO Output Clear
GPIOA->ODR &= ~(1UL << LED_PIN);	 	
BIT_CLEAR(GPIOA->ODR, LED_PIN);


// GPIO Mode Register 
GPIOA->MODER &= ~(3UL<<(2*LED_PIN)); 	// Clear as `b11=0x3` starting from  2*LED_PIN
BITS_CLEAR(GPIOA->MODER, 2 * LED_PIN, 3); // Clear as `b11=0x3` starting from  2*LED_PIN

GPIOA->MODER |=   3UL<<(2*LED_PIN);   // Set '11' for Pin 5
BITS_SET(GPIOA->MODER, 2 * LED_PIN, 3);


```


## Defining Bitwise Macro

```cpp

// Bitwise Macro Definition
#define BIT_SET(REG, BIT)      	((REG) |= 1<< (BIT))
#define BIT_CLEAR(REG, BIT)     ((REG) &= ~1<<(BIT))
#define BIT_READ(REG, BIT)      ((REG)>>BIT & (1))
#define BITS_SET(REG, BIT,NUM)     ((REG) |= NUM<< (BIT))
#define BITS_CLEAR(REG, BIT,NUM)   ((REG) &= ~(NUM<< (BIT))
//#define BITS_CLEAR(REG, BIT,NUM)   ((REG) &= ~((0x1<< NUM)-1)<<(BIT))
```

## Exercise
Modify `ecPinNames.h` to include the Bitwise Macro.

```c++
#ifndef EC_PINNAMES_H
#define EC_PINNAMES_H

#include "stm32f411xe.h"

#ifdef __cplusplus
extern "C" {
#endif


// Bitwise Macro Definition
#define BIT_SET(REG, BIT)      	((REG) |= 1<< (BIT))
#define BIT_CLEAR(REG, BIT)     ((REG) &= ~1<<(BIT))
#define BIT_READ(REG, BIT)      ((REG)>>BIT & (1))
#define BITS_SET(REG, BIT,NUM)     ((REG) |= NUM<< (BIT))
#define BITS_CLEAR(REG, BIT,NUM)   ((REG) &= ~(NUM<< (BIT))
//#define BITS_CLEAR(REG, BIT,NUM)   ((REG) &= ~((0x1<< NUM)-1)<<(BIT))

// ...


```

