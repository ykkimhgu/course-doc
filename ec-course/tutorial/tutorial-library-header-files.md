# Tutorial: Library header files

## Hierarchy of Library Header Files&#x20;

The relationship between library header files for MCU register configurations:

* STM32F411RE  Board

<figure><img src="../../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

**1) Given by ARM:** &#x20;

* stm32f411xe.h

**2) Custom Lib headers created in EC course**

* ecXXXXX.h



## EC  header files

Create library file named as `ecSTM32F411.h`

Save the file in your lib folder.  e.g)  `/EC/lib/`

We will update `ecSTM32F411.h` as we process each tutorial and lab.

```cpp
// ecSTM32F411.h

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

#endif
```



