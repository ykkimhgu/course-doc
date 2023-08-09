# LAB: Stepper Motor

**Date:** 2022-09-26

**Author/Partner:**

**Github:** repository link

**Demo Video:** Youtube link

**PDF version:**

## Introduction

In this lab, we will learn how to drive a stepper motor with digital output of GPIOs of MCU. You will use a FSM to design the algorithm for stepper motor control.

You must submit

* LAB Report (\*.md & \*.pdf)
* Zip source files(main\*.c, ecRCC.h, ecGPIO.h, ecSysTick.c etc...).
  * Only the source files. Do not submit project files

### Requirement

#### Hardware

* MCU
  * NUCLEO-F411RE
* Actuator/Sensor/Others:
  * 3Stepper Motor 28BYJ-48
  * Motor Driver ULN2003
  * breadboard

#### Software

* Keil uVision, CMSIS, EC\_HAL library

## Problem 1: Stepper Motor

### Hardware Connection

Read specification sheet of the motor and the motor driver for wiring and min/max input voltage/current.

![](https://user-images.githubusercontent.com/91526930/197428440-9f4a9c8c-2d81-4d0e-a4e2-b4a4b9def44d.png)

![](https://user-images.githubusercontent.com/91526930/197428469-a0d7a8fa-ba4c-482f-8688-ea87cfd9f4e0.png)

### Stepper Motor Sequence

We will use unipolar stepper motor for this lab

Fill in the blanks of each output data depending on the below sequence.

**Full-stepping sequence**

![](https://user-images.githubusercontent.com/91526930/197428513-f9a23147-3448-4bed-bda2-c90325b8c143.png)

![Full-stepping Sequence](https://user-images.githubusercontent.com/91526930/197428973-13acab66-049e-4f1c-be5c-176f9f15288b.png)

**Half-stepping sequence**

![](https://user-images.githubusercontent.com/91526930/197429006-d552ab16-0bbf-4c52-bdce-a0f2bfe5f0d8.png)

![Half-stepping Sequence](https://user-images.githubusercontent.com/91526930/197429050-173ac610-fa59-427d-b0c0-1e85ac20fbb2.png)

### Finite State Machine

Draw a State Table for Full-Step Sequence. Use Moore FSM for this case. See _‘Programming FSM’_ for hints.

* Full-Stepping Sequence

![](https://user-images.githubusercontent.com/91526930/197429145-243b63ac-86c4-4641-a7e0-1eb2277c00f4.png)

* Half-Stepping Sequence

![](https://user-images.githubusercontent.com/91526930/197429166-01b4e4e1-1579-4124-acb8-551176b030ea.png)

## Problem 2: Firmware Programming

### Create HAL library

Declare and Define the following functions in your library. You must

update your header files located in the directory `EC \lib\`.

**ecStepper.h**

```
// Initialize with 4 pins
void Stepper_init(GPIO_TypeDef* port1, int pin1, GPIO_TypeDef* port2, int pin2, GPIO_TypeDef* port3, int pin3, GPIO_TypeDef* port4, int pin4);

// whatSpeed [rev/min]
void Stepper_setSpeed(long whatSpeed);

// Run for n Steps
void Stepper_step(int steps, int direction, int mode); 

// Immediate Stop.
void Stepper_stop(void);
```

### Procedure

1. Create a new project under the directory `\repos\EC\LAB\LAB_Stepper_Motor`
   * The project name is “**LAB\_Stepper\_Motor”.**
   *   Create a new source file named as “**LAB\_Stepper\_Motor.c”**

       > You MUST write your name on the source file inside the comment section.
2. Include your updated library in `\repos\EC\lib\` to your project.
   * **ecGPIO.h, ecGPIO.c**
   * **ecRCC.h, ecRCC.c**
   * **ecEXTI.h, ecEXTI.c**
   * **ecSysTick.h**, **ecSysTick.c**
   * **ecStepper.h** **ecStepper.h**
3. Connect the MCU to the motor driver and the stepper motor.
4. Find out the number of steps required to rotate 1 revolution using Full-steppping.
5. Then, rotate the stepper motor 10 revolutions with 2 rpm. Measure if the motor rotates one revolution per second.
6. Repeat the above process with the opposite direction.
7. Increase and decrease the speed of the motor as fast as it can rotate to find the max speed of the motor.
8. Apply the half-stepping and repeat the above.

### Configuration

| Digital Out                                                             | SysTick |
| ----------------------------------------------------------------------- | ------- |
| <p>PB10, PB4, PB5, PB3<br>NO Pull-up Pull-down<br>Push-Pull<br>Fast</p> | delay() |

### Requirement

You have to program the stepping sequence using the state table. You can define the states using structures. Refer to _‘Programming FSM’_ for hints.

![image](https://user-images.githubusercontent.com/91526930/197430711-7610eb31-56c3-4cdd-88c7-6be689e1d3c7.png)

### Discussion

1.  Find out the trapezoid-shape velocity profile for stepper motor. When is this profile necessary?

    > Answer discussion questions
2.  How would you change the code more efficiently for micro-stepping control? You don’t have to code this but need to explain your strategy.

    > Answer discussion questions

### Code

Your code goes here: [ADD Code LINK such as github](https://github.com/ykkimhgu/EC-student/)

Explain your source code with necessary comments.

```
// YOUR MAIN CODE ONLY
// YOUR CODE
```

**Sample Code : Stepper Motor**

![image](https://user-images.githubusercontent.com/91526930/197431877-bffe4801-453f-42d8-b6ff-e8b9525e4f95.png)

### Results

Experiment images and results

> Show experiment images /results

Add [demo video link](link/)

## Reference

Complete list of all references used (github, blog, paper, etc)

## Troubleshooting

(Option) You can write Troubleshooting section
