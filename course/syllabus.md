# Syllabus



### Week 1

#### 

T1: Lecture: Grade Guideline

**T1: Lecture: Course Introduction** 

**T2:  Lecture: Review of Digital Logic**



F1: TU: C Review 

* C basics, Data type, 

**F1: Exercise: C Structure  \(homework, before semester\)** 

F2: TU: C Review \(Bitwise\)

**F2:  Exercise: Bit-wise Operation for Register  \(homework\)**

### 

### Week 2

**T12:  Lecture: Review of Digital Logic**

**T2: Tutorial: mbed OS - online** 

\*\*\*\*

**F1:TU:  mbed Part 1**

**F2: Tu: mbed Part 2**

\*\*\*\*

### Week 3

**T1:  Lecture: MCU Memory/Register Basic**

T2:   Lecture: CMSIS, uVision, mbed

F1,2: LAB: mbed  

Assignment: [Tutorial-Install uVision ](../uvision/installation.md)\(by W4\)



### Week 4

**T1,2: Holiday**

**T1: Lecture: GPIO** 

**T2: Lecture: GPIO Digital In/Digital Out Register**

### 

### Week 5

T1: TU: GPIO Digital Out

T2: TU: GPIO Digital In

F1,2: LAB: GPIO DigitalInOut \(LED\), 1 week

Assignment: github, md download

### 

### Week 6

T1: ****Lecture: System Clock Register \(RCC\)

T2: LAB: GPIO DigitalInOut \(7-segment\)



F1: TU: Creating Application API for GPIO

F1: TU: Github, docs\(md\)

F1: TU: keil uVision 5 Debugging

F2: LAB: GPIO DigitalInOut  \(7-segment\)

### 

### Week 7

**T1: Lecture: Interrupt** 

**T2: Lecture: Interrupt Register \(EXT\)**

**T2: Lecture:  SysTick Register**

F1~2:  LAB:  Ext Interrupt  & SysTick Interrupt 



\*\*\*\*

### Week 8

**T1: Lecture: Timer Output Compare / PWM**

T2: TU: PWM\_ServoMotor

F1,2: LAB: PWM\_DC Motor 

* Timer loop for input and output
* Calculate vel from encoder?



### Week 9

T1,2 **:** TU:  Stepper with FSM

F1,2: LAB: Stepper 

### 

### Week 9

T1**:** Lecture: Timer Input Capture

T2: TU: Timer Input Capture\(Ultrasonic\)

F1,2: LAB: Ultrasonic & Motor Control

### 

### Week 10

T: LAB: Stepper

F:  TEST:   DC motor with Sensor Input



### Week 10

T1**:** Lecture: Analog Input/Output \(DAC/ADC\)

T2: TU: ADC 

F1,2: LAB:ADC



### Week \_11

T1**:** Lecture: Serial Comm

T2: TU: UART 

F1,2: LAB: I2C / Bluetooth

### Week \_12

T1**:** Lecture: Wifi / wireless comm

T2: TU: WIFI  / Server

F1,2: LAB:  IoT  Sensor

### 

### Week \_13

T1,2**:**  Arduino ****

F1,2: Final LAB: 



### Week \_14

T1,2**:**  Test  ****

F1,2:  Final LAB: 



### Week \_15

T: Lecture: Machine Code

**T1:** Lecture:Register/Memory-Advanced



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









