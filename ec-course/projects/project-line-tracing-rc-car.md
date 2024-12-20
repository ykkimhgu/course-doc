# Project - Line Tracing Car

**Date:** 2024-09-26

**Author/Partner:**

**Github:** repository link

**Demo Video:** Youtube link

**PDF version:**

###

***



### What you need to submit

under the folder of   `\EC\LAB\`

1. **Report**

* &#x20;\\`img`    folder
* &#x20;LAB\_LineTracingCar\_ID.md
* &#x20;LAB\_LineTracingCar\_ID.pdf

2. **Source code**

* &#x20;`\include`**folder**
* LAB\_LineTracingCar\_ID.c

<figure><img src="../../.gitbook/assets/image (135).png" alt=""><figcaption></figcaption></figure>

***



## Introduction

Design a simple line tracing RC car that tracks the racing lines that meets the following conditions:

* The car should automatically start at the given signal
* It must trace on the track line
* When it sees an obstacle on the driving path, it should temporarily stop until the obstacle is out of the path
* It has AUTO and MANUAL control mode

> There can be more missions to complete.

![image](https://github.com/user-attachments/assets/63db2b7e-288e-46f1-b1f0-048281494b4d)

***

## Requirement

**Hardware**

* MCU
  * NUCLEO-F411RE
* Actuator/Sensor/Others: Minimum
  * Bluetooth Module(HC-06)
  * DC motor x2, DC motor driver(L9110s)
  * IR Reflective Sensor (TCRT 5000) x2
  * HC-SR04
  * additional sensor/actuators are acceptable

**Software**

* Keil uVision, CMSIS, EC\_HAL library

## Preparation

#### Tutorials:

Complete the following tutorials: [TU: Custom initialization](https://ykkim.gitbook.io/ec/ec-course/tutorial/tutorial-custom-initialization)

Use `ecSTM32F411v2.h` and `void MCU_init(void)` in your project code.

#### LABS:

You should review previous labs for help

1. LAB: ADC IR Sensor
2. LAB: USART Bluetooth
3. LAB: Timer & PWM

## Problem Definition

Design your RC car that has the following functions:

1. Line tracing on the given racing track
2. has 2 control modes: **Manual Mode** to **AUTO Mode**
3. stops temporally when it detects an object nearby on the driving path

On the PC, connected to MCU via bluetooth

* Print the car status every 1 sec such as “ ( “ MOD: A DIR: F STR: 00 VEL: 00 ”)

#### Manual Mode

* Mode Change( MOD):
  * When 'M' or 'm' is pressed, it should enter **Manual Mode**
  * LD2 should be ON in the **Manual Mode**
* Speed (VEL):
  * Increase or decrease speed each time you push the arrow key “UP” or “DOWN”, respectively.
  * You can choose the speed keys
  * Choose the speed level: V0 \~ V3
* Steer (STR):
  * Steering control with keyboard keys
  * Increase or decrease the steering angles each time you press the arrow key “RIGHT” or “LEFT”, respectively.
  * Steer angles with 3 levels for both sides
    * example: Lelvel -3, -2, -1, 0, 1, 2, 3 // '-' angle is turning to left
* Driving Direction (DIR)
  * Forward or backward by pressing the key “F” or “B”, respectively.
  * You can choose other DIR keyboard keys
* Emergency Stop
  * RC car must stop running when key “S” is pressed.
  * This must be the highest priority

#### Automatic Mode

* Mode Change:
  * When 'A' or 'a' is pressed, it should enter **AUTO Mode**
* LD2 should blink at 2 second rate in AUTO Mode
* It should drive on the racing track continuously

#### Automatic Mode (Temporarily Stop)

* Stops temporally when it detects an object nearby on the driving path
* LD2 should blink at 0.5 second rate at this situation
* If the obstacle is removed, it should drive continuously

## Procedure

1. Discuss with the teammate how to design an algorithm for this problem
2.  In the report, you need to explain concisely how your system works with state tables/diagram or flow-chart.

    ● Listing all necessary states (states, input, output etc) to implement this design problem.

    ● Listing all necessary conditional FLAGS for programming.

    ● Showing the logic flow from the initialization

    and more
3. Select appropriate configurations for the design problem. Fill in the table.

| **Functions**             | **Register** | **PORT\_PIN**      | **Configuration**                                     |
| ------------------------- | ------------ | ------------------ | ----------------------------------------------------- |
| System Clock              | RCC          |                    | PLL 84MHz                                             |
| delay\_ms                 | SysTick      |                    |                                                       |
| Motor DIR                 | Digital Out  |                    |                                                       |
|                           | ….           |                    |                                                       |
| TIMER                     | TIMER1       |                    |                                                       |
|                           | TIMER2       |                    |                                                       |
| Timer Interrupt           | ...          |                    | 10msec                                                |
| ADC                       | ADC          |                    |                                                       |
|                           | ….           |                    |                                                       |
| DC Motor Speed            | PWM2         |                    |                                                       |
| ADC sampling trigger      | PWM3         |                    |                                                       |
| RS-232 USB cable(ST-LINK) | USART2       |                    | No Parity, 8-bit Data, 1-bit Stop bit 38400 baud-rate |
| Bluetooth                 | USART1       | TXD: PA9 RXD: PA10 | No Parity, 8-bit Data, 1-bit Stop bit 9600 baud-rate  |

4. Create a new project under the directory `\repos\EC\PROJECT\PROJECT_RCcar`

* The project name is “**PROJECT\_RCcar”**
* You can share the same code with your teammate. But need to write the report individually

#### Circuit Diagram

> You need to include the circuit diagram

![image](https://user-images.githubusercontent.com/38373000/192134563-72f68b29-4127-42ac-b064-2eda95a9a52a.png)

#### Code

Your code goes here: [ADD Code LINK such as github](https://github.com/ykkimhgu/EC-student/)

```
// YOUR MAIN CODE ONLY
// YOUR CODE with Comments
```

## Results

Experiment images and results

> Show experiment images /results

Add demo video link

## Reference

Complete list of all references used (github, blog, paper, etc)

```
```

## Troubleshooting

(Option) You can write Troubleshooting section
