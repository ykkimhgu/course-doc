# Tutorial: Managing library header files

## Tutorial: Managing library header files

## EC-2024

### Hierarchy of Library Header Files

The relationship between library header files for MCU register configurations:

* STM32F411RE Board

![image](https://github.com/user-attachments/assets/78ec9b01-be5f-4483-a32d-df44f70246c3)

**1) Provided library by ARM:**

* stm32f411xe.h

**2) Custom Lib headers created in EC course**

* ecSTM32F4v2.h
* ecXXXXX.h

### EC header files

Create library file named as `ecSTM32F4v2.h`

Save the file in your include folder. e.g) `/EC/include/`

We will update `ecSTM32F4v2.h` as we process each tutorial and lab.

```cpp
//ecSTM32F4v2.h


/**
******************************************************************************
* @course   Embedded Controller- HGU
* @author	iiLAB
* @mod		2024-8-05 by YKKIM
* @brief	STM32F411 Library for EC
*
******************************************************************************
*/

#ifndef __EC_STM_H
#define __EC_STM_H

// STM built-In Library
#include "stm32f4xx.h"
#include "stm32f411xe.h"
#include "math.h"

// EC course Library
#include "ecPinNames.h"
#include "ecRCC2.h"
#include "ecGPIO2.h"
//#include "ecEXTI2.h"
//#include "ecSysTick2.h"
//#include "ecTIM2.h"
//#include "ecPWM2.h"
//#include "ecStepper2.h"
//#include "ecADC2.h"
//#include "ecUART2.h"

#endif



 
```

##



***

## EC 2023

### Hierarchy of Library Header Files

The relationship between library header files for MCU register configurations:

* STM32F411RE Board

<figure><img src="../../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

**1) Given by ARM:**

* stm32f411xe.h

**2) Custom Lib headers created in EC course**

* ecXXXXX.h

### EC header files

Create library file named as `ecSTM32F411.h`

Save the file in your include folder. e.g) `/EC/include/`

We will update `ecSTM32F411.h` as we process each tutorial and lab.

```cpp
//ecSTM32F411.h

#ifndef __EC_STM_H
#define __EC_STM_H

#include "stm32f4xx.h"
#include "stm32f411xe.h"
#include "math.h"

#include "ecPinNames.h"
#include "ecRCC.h"
#include "ecGPIO.h"
#include "ecEXTI.h"
#include "ecSysTick.h"
#include "ecTIM.h"
#include "ecPWM.h"
#include "ecStepper.h"
#include "ecADC.h"
#include "ecUART.h"


 
```
