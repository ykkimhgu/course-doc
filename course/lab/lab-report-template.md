---
description: Embedded Controller Lab Report
---

# LAB Report Template



## LAB: TITLE Goes Here

**Date:**

**Author/Partner:**

**Github:** repository link

**Demo Video:** Youtube link

## Introduction

A short introduction of this lab

## Requirement

Write a list of HW/SW requirement

### Hardware

* MCU
  * NUCLEO-F401RE
*   Sensor:

    * Ultrasonic distance sensor(HC-SR04) x1
    * and others

    ​
*   Actuator/Display

    * LED,
    * and others

    ​

### Software

*   Arduino IDE

    ​

***

## Problem

### Procedure

Explain the problem in detail



### Configuration

#### Ultrasonic distance sensor

Trigger:

* Generate a trigger pulse as PWM to the sensor
* Pin: **D10** (TIM4 CH1)
* PWM out: 50ms period, 10us pulse-width

### Circuit/Wiring Diagram

External circuit diagram that connects MCU pins to peripherals(sensor/actuator)

< Example Image>

&#x20;![image](https://www.researchgate.net/publication/345431623/figure/fig10/AS:955186134675470@1604745529957/Connection-with-Arduino-Buzzer.jpg)

***

## Algorithm

### Overview

Flowchart or FSM table/graph goes here

**Mealy FSM Table**

![image](https://user-images.githubusercontent.com/38373000/189826276-d306f435-fdf9-4612-aa98-026b383a896a.png)

### Description with Code

* Lab source code: [ADD Code LINK such as github](https://github.com/ykkimhgu/EC-student/)



Explain your source code with necessary comments

* Description 1

```
// CODE Snippet or Segments
// Don't show the whole code. Only necessary segments
```

* Description 2

```
// CODE Snippet or Segments
```

***

## Results and Analysis

### Results

Explain the final result with necessary images, graphs etc

### Demo Video

Add video link such as youtube etc

{% embed url="https://youtu.be/kXhjomwxeP4" %}
Example Demo Video
{% endembed %}

### Analysis

Briefly analyze results

***

## Reference

Complete list of all references used (github, blog, paper, etc)
