# LAB: Timer & PWM
> Servo motor and DC motor

**Date:** 2023-10-19

**Author/Partner:**

**Github:** repository link

**Demo Video:** Youtube link

## Introduction

Create a simple program that control a sevo motor and a DC motor with PWM output.

You must submit

* LAB Report (\*.md & \*.pdf)
* Zip source files(main\*.c, ecRCC.h, ecGPIO.h, ecSysTick.c etc...).
  * Only the source files. Do not submit project files

### Requirement

**Hardware**

* MCU
  * NUCLEO-F411RE
* Actuator/Sensor/Others:
  * 3 LEDs and load resistance
  * RC Servo Motor (SG90)
  * DC motor (5V)
  * DC motor driver(LS9110s)
  * breadboard

**Software**

* Keil uVision, CMSIS, EC\_HAL library


## Tutorial: STM-Arduino
{% embed url="https://ykkim.gitbook.io/ec/ec-course/tutorial/tutorial-arduino-stm32/tutorial-arduino-stm32-part-2#pwm-pulse-width-modulation-dc-motor" %}

We are going to create a simple program that run DC - Motor by giving pwm signal as input.

Press the reset button(black) and verify the operation. If you press the user button, DC-Motor will turn on.


### Procedure
1. Create a new project under the directory `\EC\LAB\LAB_EXTI`
2. Open _Arduino IDE_ and Create a new program named as ‘**TU_arduino_PWM.ino**’.
3. Write the following code.
4. upload and run. 

```c
const int pwmPin = 11;   // PWM pin
const int buttonPin = 3;  // button pin

int buttonState = HIGH;

void setup() {
  pinMode(pwmPin, OUTPUT);
 
 // initialize the pushbutton pin as an input:
  pinMode(buttonPin, INPUT_PULLUP);
  attachInterrupt(digitalPinToInterrupt(buttonPin), motorOperation, CHANGE);
}

void loop() {

  if (buttonState == LOW){
    for (int i = 0; i < 10; i++){
      analogWrite(pwmPin, 40 + 10*i);
      delay(100);
    }
  
    for (int i = 10; i > 0; i--){
      analogWrite(pwmPin, 40 + 10*i);
      delay(100);
    }
  }
  else{
    analogWrite(pwmPin, 0);
  }
}

void motorOperation(){
  buttonState = digitalRead(buttonPin);
}
```


## Tutorial: STM32F4xx
### 1. Tutorial: DC motor driver connection 
Understand how to connect a DC motor to the motor driver (LS9110s).

{% embed url="https://ykkim.gitbook.io/ec/ec-course/tutorial/tutorial-dcmotor-motor-driver-connection" %}


---
## Problem 1: RC servo motor

An RC servo motor is a tiny and light weight motor with high output power. It is used to control rotation angles, approximately 180 degrees (90 degrees in each direction) and commonly applied in RC car, and Small-scaled robots. The angle of the motor can be controlled by the pulse width (duty ratio) of PWM signal. The PWM period should be set at **20ms or 50Hz**. Refer to the datasheet of the RC servo motor for detailed specifications.

![image](https://user-images.githubusercontent.com/38373000/195773601-f0f19e35-0a6f-49af-aa87-574c86bfec62.png)

#### 1-1. Create HAL library

Download files:

* [ecPinNames.h ecPinNames.c](https://github.com/ykkimhgu/EC-student/tree/main/include/lib-student)&#x20;
* [ecTIM\_student.h, ecTIM\_student.c](https://github.com/ykkimhgu/EC-student/tree/main/include/lib-student)&#x20;
* [ecPWM\_student.h, ecPWM\_student.c](https://github.com/ykkimhgu/EC-student/tree/main/include/lib-student)

Then, change the library files as

* ecTIM.h, ecTIM.c
* ecPWM.h, ecPWM.c

Declare and define the following functions in your library. You must update your header files located in the directory `EC \lib\`.

**ecTIM.h**

```cpp
// Timer Period setup
void TIM_init(TIM_TypeDef *TIMx, uint32_t msec);
void TIM_period(TIM_TypeDef* TIMx, uint32_t msec);
void TIM_period_ms(TIM_TypeDef* TIMx, uint32_t msec);
void TIM_period_us(TIM_TypeDef* TIMx, uint32_t usec);

// Timer Interrupt setup
void TIM_UI_init(TIM_TypeDef* TIMx, uint32_t msec);
void TIM_UI_enable(TIM_TypeDef* TIMx);
void TIM_UI_disable(TIM_TypeDef* TIMx);


// Timer Interrupt Flag 
uint32_t is_UIF(TIM_TypeDef *TIMx);
void clear_UIF(TIM_TypeDef *TIMx);
```

**ecPWM.h**

```cpp
/* PWM Configuration using PinName_t Structure */

/* PWM initialization */
// Default: 84MHz PLL, 1MHz CK_CNT, 50% duty ratio, 1msec period
void PWM_init(PinName_t pinName);
void PWM_pinmap(PinName_t pinName, TIM_TypeDef **TIMx, int *chN);


/* PWM PERIOD SETUP */
// allowable range for msec:  1~2,000
void PWM_period(PinName_t pinName,  uint32_t msec);	
void PWM_period_ms(PinName_t pinName,  uint32_t msec);	// same as PWM_period()
// allowable range for usec:  1~1,000
void PWM_period_us(PinName_t pinName, uint32_t usec);


/* DUTY RATIO SETUP */
// High Pulse width in msec
void PWM_pulsewidth(PinName_t pinName, uint32_t pulse_width_ms);
void PWM_pulsewidth_ms(PinName_t pinName, uint32_t pulse_width_ms);  // same as void PWM_pulsewidth
// Duty ratio 0~1.0
void PWM_duty(PinName_t pinName, float duty);

```

### Procedure

Make a simple program that changes the angle of the RC servo motor that rotates back and forth from 0 deg to 180 degree within a given period of time.

Reset to '0' degree by pressing the push button (PC13).

* Button input has to be an External Interrupt
* Use Port A Pin 1 as PWM output pin for TIM2\_CH2.
* Use Timer interrupt of period 500msec.
* Angle of RC servo motor should rotate from 0° to 180° and back 0° at a step of 10° at the rate of 500msec.

You need to observe how the PWM signal output is generated as the input button is pushed, using an oscilloscope. You need to capture the Oscilloscope output in the report.

####

1. Create a new project under the directory `\repos\EC\LAB\LAB_PWM`

* The project name is “**LAB\_PWM”.**
* Create a new source file named as “**LAB\_PWM\_RCmotor.c”**

> You MUST write your name on the source file inside the comment section.

2\. Include your updated library in `\repos\EC\lib\` to your project.

* **ecPinNames.h** **ecPinNames.c**
* **ecGPIO.h, ecGPIO.c**
* **ecRCC.h, ecRCC.c**
* **ecEXTI.h, ecEXTI.c**
* **ecTIM.h**, **ecTIM.c**
* **ecPWM.h** **ecPWM.h**

3. Connect the RC servo motor to MCU pin (PA1) , VCC and GND
4. Increase the angle of RC servo motor from 0° to 180° with a step of 10° every 500msec. After reaching 180°, decrease the angle back to 0°. Use timer interrupt IRQ.
5. When the button is pressed, it should reset to the angle 0° and start over. Use EXT interrupt.

#### Configuration

| Type                | Port - Pin        | Configuration                                       |
| ------------------- | ----------------- | --------------------------------------------------- |
| **Button**          | Digital In (PC13) | Pull-Up                                             |
| **PWM Pin**         | AF (PA1)          | Push-Pull, Pull-Up, Fast                            |
| **PWM Timer**       | TIM2\_CH2 (PA1)   | TIM2 (PWM) period: 20msec, Duty ratio: 0.5\~2.5msec |
| **Timer Interrupt** | TIM3              | TIM3: Timer Interrupt of 500 msec                   |
|                     |                   |                                                     |

####

#### Circuit Diagram

> You need to include the circuit diagram

![image](https://user-images.githubusercontent.com/38373000/192134563-72f68b29-4127-42ac-b064-2eda95a9a52a.png)

#### Discussion

1. Derive a simple logic to calculate CRR and ARR values to generate x\[Hz] and y\[%] duty ratio of PWM. How can you read the values of input clock frequency and PSC?

> Answer discussion questions

2.  What is the smallest and highest PWM frequency that can be generated for Q1?

    > Answer discussion questions

#### Code

Your code goes here: [ADD Code LINK such as github](https://github.com/ykkimhgu/EC-student/)

Explain your source code with necessary comments.

```
// YOUR MAIN CODE ONLY
// YOUR CODE
```

#### Example Code

**Sample Code : Timer Interrupt IRQ**

```cpp
#include "stm32f411xe.h"
#include "ecGPIO.h"
#include "ecRCC.h"
#include "ecTIM.h"


#define LED_PIN	5
uint32_t _count = 0;
void setup(void);


int main(void) {
	// Initialization --------------------------------------------------
	setup();
	
	// Infinite Loop ---------------------------------------------------
	while(1){}
}


// Initialization
void setup(void){
	RCC_PLL_init();				// System Clock = 84MHz
	GPIO_init(GPIOA, LED_PIN, OUTPUT);	// calls RCC_GPIOA_enable()
	TIM_UI_init(TIM2, 1);			// TIM2 Update-Event Interrupt every 1 msec 
	TIM_UI_enable(TIM2);
}

void TIM2_IRQHandler(void){
	if(is_UIF(TIM2)){			// Check UIF(update interrupt flag)
		_count++;
		if (_count > 1000) {
			LED_toggle();		// LED toggle every 1 sec
			_count = 0;
		}
		clear_UIF(TIM2); 		// Clear UI flag by writing 0
	}
}
```

**Sample Code : PWM output**

```cpp
#include "stm32f411xe.h"
#include "math.h"

// #include "ecSTM32F411.h"
#include "ecPinNames.h"
#include "ecGPIO.h"
#include "ecSysTick.h"
#include "ecRCC.h"
#include "ecTIM.h"
#include "ecPWM.h"   // ecPWM2.h


// Definition Button Pin & PWM Port, Pin
#define BUTTON_PIN 13
#define PWM_PIN PA_5
void setup(void);


int main(void) {
	// Initialization --------------------------------------------------
	setup();	
	
	// Infinite Loop ---------------------------------------------------
	while(1){
		LED_toggle();		
		for (int i=0; i<5; i++) {						
			PWM_duty(PWM_PIN, (float)0.2*i);			
			delay_ms(1000);
		}		
	}
}


// Initialiization 
void setup(void) {	
	RCC_PLL_init();
	SysTick_init();
		
	// PWM of 20 msec:  TIM2_CH1 (PA_5 AFmode)
	GPIO_init(GPIOA, 5, EC_AF);
	PWM_init(PWM_PIN);	
	PWM_period(PWM_PIN, 20);   // 20 msec PWM period
}
```

#### Results

Experiment images and results

> Show experiment images /results

Add [demo video link](link/)

***

## Problem 2: DC motor

### Procedure

Make a simple program that rotates a DC motor that changes the duty ratio from 25% -->75%--> 25% --> and so on.

The rotating speed level changes every 2 seconds.

By pressing the push button (PC13), toggle from Running and stopping the DC motor

**First, you MUST read** [Tutorial: DC motor driver connection](https://ykkim.gitbook.io/ec/ec-course/tutorial/tutorial-dcmotor-motor-driver-connection)

1. Use the same project.

* Create a new source file named “**LAB\_PWM\_DCmotor.c”**
* You need to eliminate the other source file that contains `main()` from the project
  * e.g. Eliminate "“**LAB\_PWM\_RCmotor.c”** from the project

> You MUST write your name on the source file inside the comment section.

2. Connect DC motor and DC motor driver.

* PA\_0 for the DC motor PWM
* PC\_2 for Direction Pin

3. Change DC motor from LOW Speed to HIGH Speed for every 2 seconds

* e.g. 25% -->75%--> 25% --> and so on.

4. When Button is pressed, it should PAUSE or CONTINUE motor run

#### Configuration

####

| Function            | Port - Pin        | Configuration                                   |
| ------------------- | ----------------- | ----------------------------------------------- |
| **Button**          | Digital In (PC13) | Pull-Up                                         |
| **Direction Pin**   | Digital Out (PC2) | Push-Pull                                       |
| **PWM Pin**         | AF (PA0)          | Push-Pull, Pull-Up, Fast                        |
| **PWM Timer**       | TIM2\_CH1 (PA0)   | TIM2 (PWM) period: **1msec (1kHz)**             |
| **Timer Interrupt** | TIM3              | TIM3: Timer Interrupt of 500 msec               |
|                     |                   |                                                 |

#### Circuit Diagram

> You need to include the circuit diagram

![image](https://user-images.githubusercontent.com/38373000/192134563-72f68b29-4127-42ac-b064-2eda95a9a52a.png)

#### Code

Your code goes here: [ADD Code LINK such as github](https://github.com/ykkimhgu/EC-student/)

Explain your source code with necessary comments.

```
// YOUR MAIN CODE ONLY
// YOUR CODE
```

#### Results

Experiment images and results

> Show experiment images /results

Add [demo video link](link/)

### Reference

Complete list of all references used (github, blog, paper, etc)

```

```

## Troubleshooting

### 1. motor PWM duty ratio for different DIR

When,  DIR=0
duty=0.8--> PWM 0.8 // 실제 모터에 전달되는 pwm

Whe, DIR=1
duty=0.8--> PWM 0.2 // 실제 모터에 전달되는 PWM

*** a solution ***
```c++
float targetPWM;  // pwm for motor input 
float duty=abs(DIR-targetPWM); // duty with consideration of DIR=1 or 0

PWM_duty(PWM_PIN, duty);
```


### 2. Motor does not run under duty 0.5
SOL) Configure motor PWM period as 1kHz

### 3. Check and give different Interrupt Priority
Check if you have different NVIC priority number for each IRQs


(Option) You can write Troubleshooting section
```


### 4. Print a string for BT (USART1)
Use `sprintf()`

```c++
#define _CRT_SECURE_NO_WARNINGS    // sprintf 보안 경고로 인한 컴파일 에러 방지
#include <stdio.h>     // sprintf 함수가 선언된 헤더 파일

char BT_string[20]=0;

int main()
{
	sprintf(BT_string, "DIR:%d PWM: %0.2f\n", dir, duty);    // 문자, 정수, 실수를 문자열로 만듦
	USART1_write(BT_string, 20);
	// ...
}
```
https://dojang.io/mod/page/view.php?id=352
**
