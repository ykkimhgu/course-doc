# LAB: Timer & PWM



**Date:** 2026-09-02

**Author/Partner:** YOUR NAME GOES HERE

**Demo Video:** Youtube link



## Introduction

Create a simple program that control a sevo motor and a DC motor with PWM output.

You must submit

* LAB Report (\*.md & \*.pdf)
* Zip source files(main\*.c, ecRCC2.h, ecGPIO2.h, ecSysTick2.c etc...).
  * Only the source files. Do not submit project files

### Requirement

**Hardware**

> ⚠️ **Warning**
>
> In this lab, **do not use the JKIT Eval Board**.\
> Please disconnect it before proceeding with the following lab steps.

* MCU
  * NUCLEO-F411RE
* Actuator/Sensor/Others:
  * RC Servo Motor (SG90)
  * DC motor (5V)
  * DC motor driver(LS9110s or L298N)

**Software**

* PlatformIO, CMSIS, EC\_HAL library

## Tutorial: DC Motor Driver

Learn how to connect a DC motor to the LS9110s motor driver

{% embed url="https://ykkim.gitbook.io/ec/ec-course/tutorial/tutorial-dcmotor-motor-driver-connection" %}

##

***

## Problem 0: STM-Arduino

We will create a simple program to control a DC motor using a PWM input signal.

* Connect the DC motor to the DC motor driver
* Pressing the user button will turn on the DC motor

### Procedure

1. Create a new project under the directory **`lab\`**
2. Open _Arduino IDE_ and Create a new program named as ‘**TU\_arduino\_PWM.ino**’.
3. Write the following code.
4. Upload and Run



{% code expandable="true" %}
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
{% endcode %}



***

## PreLab: Timer Out Register

{% content-ref url="../prelab-timer-interrupt-and-pwm.md" %}
[prelab-timer-interrupt-and-pwm.md](../prelab-timer-interrupt-and-pwm.md)
{% endcontent-ref %}

Declare and define the following functions in your library. You must update your header files located in the directory `EC \include\`.

{% tabs %}
{% tab title="ecTIM2.h" %}
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


{% endtab %}

{% tab title="ecPWM2.h" %}
<pre class="language-cpp"><code class="lang-cpp"><strong>/* PWM Configuration using PinName_t Structure */
</strong>
/* PWM initialization */
// Default: 84MHz PLL, 1MHz CK_CNT, 50% duty ratio, 1msec period
void PWM_init(PinName_t pinName);
void PWM_pinmap(PinName_t pinName, TIM_TypeDef **TIMx, int *chN);


/* PWM PERIOD SETUP */
// allowable range for msec:  1~2,000
void PWM_period_ms(PinName_t pinName,  uint32_t msec);	// same as PWM_period()
// allowable range for usec:  1~1,000
void PWM_period_us(PinName_t pinName, uint32_t usec);


/* DUTY RATIO SETUP */
// High Pulse width in msec
void PWM_pulsewidth_ms(PinName_t pinName, uint32_t pulse_width_ms);  // same as void PWM_pulsewidth
// Duty ratio 0~1.0
void PWM_duty(PinName_t pinName, float duty);

</code></pre>
{% endtab %}
{% endtabs %}

#### Procedure <a href="#procedure-1" id="procedure-1"></a>

## Problem 1: RC servo motor

An RC servo motor is a lightweight motor with high output power. It is used to control rotation angles of approximately 180 degrees (90 degrees in each direction) and is commonly applied in RC cars and small-scale robots. The angle of the motor can be controlled by the pulse width (duty ratio) of a PWM signal. The PWM period should be set to 20ms (or 50Hz). Refer to the datasheet of the RC servo motor for detailed specifications

![image](https://user-images.githubusercontent.com/38373000/195773601-f0f19e35-0a6f-49af-aa87-574c86bfec62.png)

Make a simple program that changes the angle of the RC servo motor that rotates back and forth from 0 deg to 180 degree within a given period of time.

* Connect the RC servo motor to MCU pin (PA15) , VCC and GND
  * Use Port A Pin 15 as PWM output pin for TIM2\_CH1.
  * Use Timer interrupt of period 500msec.
* Increase the angle of RC servo motor from 0° to 180° with a step of 10° every 500msec.&#x20;
* After reaching 180°, decrease the angle back to 0°.&#x20;
  * Use timer interrupt IRQ.
* When the user button (PA\_4) is pressed, it should reset to the angle 0° and start over.&#x20;
  * Use EXT interrupt.



> You need to observe how the PWM signal output is generated as the input button is pushed, using an oscilloscope. You need to capture the Oscilloscope output in the report.

####

### Procedure

1. Create a new project under the directory `EC\lab\`

* Enviornment:  “**LAB\_PWM”.**
* Source File:  “**LAB\_PWM\_RCmotor.c”**

> You MUST write your name on the source file inside the comment section.

\
2\. You must modify the `platformio.ini` **,** to add new environment.



#### Configuration

| Type                | Port - Pin        | Configuration                                       |
| ------------------- | ----------------- | --------------------------------------------------- |
| **Button**          | Digital In (PC13) | Pull-Up                                             |
| **PWM Pin**         | AF (PA1)          | Push-Pull, Pull-Up, Fast                            |
| **PWM Timer**       | TIM2\_CH2 (PA1)   | TIM2 (PWM) period: 20msec, Duty ratio: 0.5\~2.5msec |
| **Timer Interrupt** | TIM3              | TIM3: Timer Interrupt of 500 msec                   |

####

#### Circuit Diagram

> You need to include the circuit diagram (if necessary)

![image](https://user-images.githubusercontent.com/38373000/192134563-72f68b29-4127-42ac-b064-2eda95a9a52a.png)

####

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

Add [demo video link](https://github.com/ykkimhgu/course-doc/blob/master/ec-course/lab/link/README.md)



#### Discussion

1. Derive a simple logic to calculate CRR and ARR values to generate x\[Hz] and y\[%] duty ratio of PWM. How can you read the values of input clock frequency and PSC?

> Answer discussion questions

2.  What is the smallest and highest PWM frequency that can be generated for Q1?

    > Answer discussion questions

***

## Problem 2: DC motor

### Problem

Make a simple program that rotates a DC motor  at a LOW Speed to HIGH Speed for every 2 seconds

* Control the duty ratio from 25% -->75%--> 25% --> and so on.
* The rotating speed level changes every 2 seconds.

Connect DC motor and DC motor driver.

* PA\_0 for the DC motor PWM
* PC\_2 for Direction Pin

User Button  (PA4):&#x20;

* When Button is pressed, it should toggle from PAUSE to CONTINUE motor run

> &#x20; **you MUST read** [Tutorial: DC motor driver connection](https://ykkim.gitbook.io/ec/ec-course/tutorial/tutorial-dcmotor-motor-driver-connection)

### Procedure

1. Create a new environment&#x20;

* Source FIle:  “**LAB\_PWM\_DCmotor”**
* Source FIle:  “**LAB\_PWM\_DCmotor.c”**

> You MUST write your name on the source file inside the comment section.

#### Configuration

####

| Function            | Port - Pin        | Configuration                       |
| ------------------- | ----------------- | ----------------------------------- |
| **Button**          | Digital In (PC13) | Pull-Up                             |
| **Direction Pin**   | Digital Out (PC2) | Push-Pull                           |
| **PWM Pin**         | AF (PA0)          | Push-Pull, Pull-Up, Fast            |
| **PWM Timer**       | TIM2\_CH1 (PA0)   | TIM2 (PWM) period: **1msec (1kHz)** |
| **Timer Interrupt** | TIM3              | TIM3: Timer Interrupt of 500 msec   |

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

Add [demo video link](https://github.com/ykkimhgu/course-doc/blob/master/ec-course/lab/link/README.md)

### Reference

Complete list of all references used (github, blog, paper, etc)

```
```

## Troubleshooting

#### 1. motor PWM duty ratio for different DIR

When, DIR=0 duty=0.8--> PWM 0.8 // 실제 모터에 전달되는 pwm

Whe, DIR=1 duty=0.8--> PWM 0.2 // 실제 모터에 전달되는 PWM

* Sample Solution

```c++
float targetPWM;  // pwm for motor input 
float duty=abs(DIR-targetPWM); // duty with consideration of DIR=1 or 0

PWM_duty(PWM_PIN, duty);
```

#### 2. Motor does not run under duty 0.5

SOL) Configure motor PWM period as 1kHz

#### 3. Check and give different Interrupt Priority

Check if you have different NVIC priority number for each IRQs

(Option) You can write Troubleshooting section

#### 4. Print a string for BT (USART1)

Use `sprintf()`

```c
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

Reference: [https://dojang.io/mod/page/view.php?id=352](https://dojang.io/mod/page/view.php?id=352)
