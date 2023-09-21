# LAB: Timer & PWM

**Date:** 2023-09-26

**Author/Partner:**

**Github:** repository link

**Demo Video:** Youtube link

**PDF version:**

## Introduction

In this lab, you are required to create a simple program that control a sevo motor with PWM output.

You must submit

* LAB Report (*.pdf)
* Zip source files(main\*.c, ecRCC.h, ecGPIO.h, ecSysTick.c etc...).
  * Only the source files. Do not submit project files

### Requirement

#### Hardware

* MCU
  * NUCLEO-F411RE
* Actuator/Sensor/Others:
  * 3 LEDs and load resistance
  * RC Servo Motor (SG90)
  * DC motor
  * breadboard

#### Software

* Keil uVision, CMSIS, EC\_HAL library

## Problem 1: Create HAL library

### Create HAL library

Declare and Define the following functions in your library. You must

update your header files located in the directory `EC \lib\`.

**ecTIM.h**

```
// Timer Period setup
void TIM_init(TIM_TypeDef *TIMx, uint32_t msec);
void TIM_period_us(TIM_TypeDef* TIMx, uint32_t usec);
void TIM_period_ms(TIM_TypeDef* TIMx, uint32_t msec);

// Timer Interrupt setup
void TIM_INT_init(TIM_TypeDef* TIMx, uint32_t msec);
void TIM_INT_enable(TIM_TypeDef* TIMx);
void TIM_INT_disable(TIM_TypeDef* TIMx);

// Timer Interrupt Flag 
uint32_t is_UIF(TIM_TypeDef *TIMx);
void clear_UIF(TIM_TypeDef *TIMx);
```

**ecPWM.h**

```
/* PWM STRUCTURE */
typedef struct {
	GPIO_TypeDef *port;
	int pin;
	TIM_TypeDef *timer;
	int ch;
} PWM_t;

/* PWM initialization */
// Default: 84MHz PLL, 1MHz CK_CNT, 50% duty ratio, 1msec period
void PWM_init(PWM_t *pwm, GPIO_TypeDef *port, int pin);

/* PWM PERIOD SETUP */
// allowable range for msec:  1~2,000
void PWM_period_ms(PWM_t *pwm,  uint32_t msec);	
// allowable range for usec:  1~1,000
void PWM_period_us(PWM_t *pwm, uint32_t usec);

/* DUTY RATIO SETUP */
// High Pulse width in msec
void PWM_pulsewidth_ms(PWM_t *pwm, float pulse_width_ms);
// Duty ratio 0~1.0
void PWM_duty(PWM_t *pwm, float duty);
```

## Problem 2: RC Servo motor

An RC servo motor is a tiny and light weight motor with high output power. It is used to control rotation angles, approximately 180 degrees (90 degrees in each direction) and commonly applied in RC car, and Small-scaled robots.

The angle of the motor can be controlled by the pulse width (duty ratio) of PWM signal. The PWM period should be set at **20ms or 50Hz**. Refer to the data sheet of the RC servo motor for detailed specifications.

![image](https://user-images.githubusercontent.com/38373000/195773601-f0f19e35-0a6f-49af-aa87-574c86bfec62.png)

Make a simple program that changes the angle of the RC servo motor that rotates with a given period of time and reset by pressing the push button (PC13).

* The button input has to be External Interrupt
* Use Port A Pin 1 as PWM output pin, for TIM2\_Ch2.
* Use Timer interrupt of period 500msec.
* The angle of RC servo motor should rotate from 0° to 180° and back 0° at a step of 10° at the rate of 500msec.

You need to observe how the PWM signal output is generated as input button is pushed, using an oscilloscope. You need to capture the Oscilloscope output in the report.

### Procedure

1. Create a new project under the directory `\repos\EC\LAB\LAB_PWM_RCmotor`

* The project name is “**LAB\_PWM\_RCmotor”.**
* Create a new source file named as “**LAB\_PWM\_RCmotor.c”**

> You MUST write your name on the source file inside the comment section.

2\. Include your updated library in `\repos\EC\lib\` to your project.

* **ecGPIO.h, ecGPIO.c**
* **ecRCC.h, ecRCC.c**
* **ecEXTI.h, ecEXTI.c**
* **ecTIM.h**, **ecTIM.c**
* **ecPWM.h** **ecPWM.h**

1. Connect the RC servo motor to MCU pin (PA1) , VCC and GND
2. Increase the angle of RC servo motor from 0° to 180° with a step of 10° every 500msec. After reaching 180°, decrease the angle back to 0°. Use timer interrupt IRQ.
3. When the button is pressed, it should reset to the angle 0° and start over. Use EXT interrupt.

### Configuration

| Button            | PWM Pin       | Timer                      | PWM                      |
| ----------------- | ------------- | -------------------------- | ------------------------ |
| Digital In (PC13) | AF (PA1)      | TIM3                       | TIM2\_CH2 (PA1)          |
| Pull-Up           | Push-Pull     | Timer Period: 100usec      | PWM period: 20msec       |
|                   | Pull-up, Fast | Timer Interrupt of 500msec | duty ratio: 0.5\~2.5msec |

### Circuit Diagram

> You need to include the circuit diagram

![image](https://user-images.githubusercontent.com/38373000/192134563-72f68b29-4127-42ac-b064-2eda95a9a52a.png)

### Discussion

1. Derive a simple logic to calculate for CRR and ARR values to generate xHz and y% duty ratio of PWM. How can you read the values of input clock frequency and PSC?

> Answer discussion questions

1.  What is the smallest and highest PWM frequency that can be generated for Q1?

    > Answer discussion questions

### Code

Your code goes here: [ADD Code LINK such as github](https://github.com/ykkimhgu/EC-student/)

Explain your source code with necessary comments.

```
// YOUR MAIN CODE ONLY
// YOUR CODE
```

**Sample Code : Timer Interrupt IRQ**

![image](https://user-images.githubusercontent.com/38373000/195773862-3b856e3e-4df9-4f30-b060-329ecafef888.png)

**Sample Code : PWM output**

![image](https://user-images.githubusercontent.com/38373000/195773773-544fdeb1-1050-4063-b974-cdb617521359.png)

### Results

Experiment images and results

> Show experiment images /results

Add [demo video link](../../course/lab/link/)

## Reference

Complete list of all references used (github, blog, paper, etc)

```

```

## Troubleshooting

(Option) You can write Troubleshooting section
