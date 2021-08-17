# draft



------

### Need to consider

How to create program structure.



-----

### Week 1

#### T

Tutorial: mbed OS - online \(under\)

Lab: mbed \(under\)

#### F

Tutorial: Install uVision \(done\)

Tutorial: Create a new project with uVision \(done\)

\*\*\*\*

**C Programming**

Exercise: C Structure.  \(access, create, pointer\)- \(new, before semester\) 

Exercise: Bit-wise Operation for Register  \(new\)

* Shift
* Set
* Clear
* Read
* Use MACRO
* **define CLEAR\_BIT\(REG, BIT\)   \(\(REG\) &= ~\(BIT\)\)**

  **define READ\_BIT\(REG, BIT\)    \(\(REG\) & \(BIT\)\)**

  **define CLEAR\_REG\(REG\)        \(\(REG\) = \(0x0\)\)**

  **define WRITE\_REG\(REG, VAL\)   \(\(REG\) = \(VAL\)\)**

  **define READ\_REG\(REG\)         \(\(REG\)\)**

\*\*\*\*

**LAB: Using only CMSIS**

Lecture:  CMSIS vs mbed vs API

Lecture: MCU Architecture-Basic\(new, 10 slides\)

Lecture: MCUMemory/Register-Basic\(new, 10 slides\)



Lecture: GPIO \(done\)

TuLecture: GPIO Digital In/Digital Out Register\(완료\)

TuLecture: Define Parameter

* Exercise: Define Parameter 
* GPIO_TypeDef; is defined in "_stm32f4111xe.h"

TU: GPIO Digital Out \(LED ON\)

> 기본 Clock setting 코드는 함수로 제공 
>
> How to make a function from CMSIS lines

TU: GPIO Digital InOut \(LED ON when Pressing\)



LAB: GPIO DI/DO \(Toggle LED\)

* external LED 추가 하는 내용 \(under\)





Lecture: MCU Architecture-Advanced

Lecture: Register/Memory-Advanced

Tutorial: keil uVision 5 Debugging



Tutorial: Template code for MCU



**Creating API**

TU: Create GPIO DI/DO API in mbed style \(new\)

TU: Github, docs\(md\)

> mbed docs 참고

