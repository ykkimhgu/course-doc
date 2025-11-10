# PROJECT: Line Tracing Car

**Date:** 2025-11-04

**Author/Partner:**

**Github:** repository link

**Demo Video:** Youtube link

**PDF version:**

## Introduction

Design an embedded system to control an RC car to drive on the racing track. The car can be controlled either (1) manually with your Laptop keyboard by Bluetooth communication or (2) automatically to drive around the track.

From the start to the finish line, it needs to complete the given missions.



<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

### Preparation Mission

* Move the car to the start line by manual control. (Manual Mode)
* The car needs to STOP automatically when it finds an obstacle in front

### Start Line Mission

* The car should be in AUTO Mode
* It should start when the obstacle is removed
* The track timer is started

### Intermission Mission

* Stop the car when it sees an obstacle
* To raise the obstacle from the ground, the car's arm should ring the bell (touch sensor)
* When the obstacle is raised, the car should continue driving

### Finish Line Mission

* Stop the car when it sees an obstacle
* It needs to ring the bell (touch sensor) to finish
* The track timer will be stopped and recorded



## Requirement

### Hardware

* MCU
  * NUCLEO-F411RE
* Actuator/Sensor/Others: Minimum
  * Bluetooth Module(HC-06)
  * DC motor x2, DC motor driver(L9110s or L298N)
  * IR Reflective Sensor (TCRT 5000) x2
  * HC-SR04 x2
  * RC Servo Motor (SG90)
  * Additional sensor/actuators are acceptable

### Software

* VS code, CMSIS, EC\_HAL library

## Preparation

Complete the following tutorials:

* [TU: Custom initialization](https://ykkim.gitbook.io/ec/ec-course/tutorial/tutorial-custom-initialization)

Use `ecSTM32F411v2.h` and `void MCU_init(void)` in your project code.



## Problem Definition

Design your RC car that has the following functions:

1. Line tracing on the given racing track
2. 2 control modes: Manual Mode and AUTO Mode
3. Completes the given missions:
   * Preparation Mission
   * Start Mission
   * Intermission Mission
   * Finish Mission
4. The track time will be recorded

On the PC, connected to MCU via bluetooth:

* Print the car status every 1 sec such as “( MOD: A DIR: F STR: 00 VEL: 00 )”

### Manual Mode

* Mode Change (MOD):
  * When 'M' or 'm' is pressed, it should enter Manual Mode
  * LD2 should be ON in Manual Mode
* Speed (VEL):
  * Increase or decrease speed each time you push the arrow key “UP” or “DOWN”, respectively.
  * You can choose the speed keys
  * Choose the speed level: V0 \~ V3
* Steer (STR):
  * Steering control with keyboard keys
  * Increase or decrease the steering angles each time you press the arrow key “RIGHT” or “LEFT”, respectively.
  * Steer angles with 3 levels for both sides
    * Example: Level -3, -2, -1, 0, 1, 2, 3 // '-' angle is turning to left
* Driving Direction (DIR)
  * Driving direction is forward or backward by pressing the key “F” or “B”, respectively.
  * You can choose the control keys
* Emergency Stop
  * RC car must stop running when key “S” is pressed.
  * This must be the highest priority

### Automatic Mode

* Mode Change:
  * When 'A' or 'a' is pressed, it should enter AUTO Mode
* LD2 should blink at 1 second rate in AUTO Mode
* It should drive on the racing track continuously
* Stops temporarily when it detects an object nearby on the driving path.
* If there is an obstacle in the front and on the side, use the servo motor to touch the bell (pressure sensor)
* When the pressure sensor responds, the obstacle is removed.
* If the obstacle is removed, it should drive continuously

## Procedure

{% stepper %}
{% step %}
### Design discussion

Discuss with teammates how to design an algorithm for this problem.
{% endstep %}

{% step %}
### Report contents

In the report, explain concisely how your system works with state tables/diagram or flow-chart.

* List all necessary states (states, input, output etc) to implement this design problem.
* List all necessary conditional FLAGS for programming.
* Show the logic flow from initialization.
* Any additional relevant diagrams or explanations.
{% endstep %}

{% step %}
### Configuration selection

Select appropriate configurations for the design problem. Fill in the table (examples provided below).
{% endstep %}

{% step %}
### Project setup

Create a new project under the directory `\repos\EC\PROJECT\PROJECT_RCcar`

* The project name is “PROJECT\_RCcar”
* You can share the same code with your teammate but need to write the report individually
{% endstep %}
{% endstepper %}

### Configuration Table (fill for your project)

| Functions                 | Register    | PORT\_PIN          | Configuration                                         |
| ------------------------- | ----------- | ------------------ | ----------------------------------------------------- |
| System Clock              | RCC         |                    | PLL 84MHz                                             |
| delay\_ms                 | SysTick     |                    |                                                       |
| Motor DIR                 | Digital Out |                    |                                                       |
| ...                       | ….          |                    |                                                       |
| TIMER                     | TIMER1      |                    |                                                       |
|                           | TIMER2      |                    |                                                       |
| Timer Interrupt           | ...         |                    | 10msec                                                |
| ADC                       | ADC         |                    |                                                       |
|                           | ….          |                    |                                                       |
| DC Motor Speed            | PWM2        |                    |                                                       |
| RC Motor                  | PWM1        |                    |                                                       |
| ADC sampling trigger      | PWM3        |                    |                                                       |
| RS-232 USB cable(ST-LINK) | USART2      |                    | No Parity, 8-bit Data, 1-bit Stop bit 38400 baud-rate |
| Bluetooth                 | USART1      | TXD: PA9 RXD: PA10 | No Parity, 8-bit Data, 1-bit Stop bit 9600 baud-rate  |

## Circuit Diagram

> You need to include the circuit diagram

![Circuit Diagram](<../../../.gitbook/assets/192134563 72f68b29 4127 42ac b064 2eda95a9a52a.png>)

## Code

Your code goes here: [ADD Code LINK such as github](https://github.com/ykkimhgu/EC-student/)

```c
// YOUR MAIN CODE ONLY
// YOUR CODE with Comments
```

## Results

Experiment images and results

> Show experiment images /results

Add [demo video link](../../lab/link/)

## Reference

Complete list of all references used (github, blog, paper, etc)

```
```

## What to submit <a href="#what-you-need-to-submit-2" id="what-you-need-to-submit-2"></a>

**`EC_LineTracingCar_ID1_ID2.zip`**

1. **Report: `\report\`**

* \\`img` folder
* EC\_LineTracingCar\_ID1\_ID2\_Report.md
* EC\_LineTracingCar\_ID1\_ID2\_Report.pdf

2. **Source code: `\src\`**

* `\include`**folder**
* EC\_LineTracingCar\_ID1\_ID2\_main.c

**Example:**

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>



## Troubleshooting

<details>

<summary>1. Print a string for BT (USART1)</summary>

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

</details>

<details>

<summary>3. Motor does not run under duty 0.5</summary>

SOL) Configure motor PWM period as 1kHz

</details>

<details>

<summary>4. Check and give different Interrupt Priority</summary>

Check if you have different NVIC priority number for each IRQs

</details>

<details>

<summary>5. Ultrasonic sensor does not measure properly when MCU is connected with motor driver</summary>

SOL) Give independent voltage source to motor driver. Giving DC power from MCU to motor driver is not recommended

</details>
