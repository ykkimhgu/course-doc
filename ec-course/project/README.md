# LAB: EC Design Problem 

 

**Date:** 2023-09-26

**Author/Partner:**

**Github:** repository link

**Demo Video:** Youtube link







# I. Introduction

##  Overview

Design problem for an application of 32-bit MCUs ( STM32F411).  You can freely choose the design problem related to smart factory, smart home system or any other related to our mechanical and control engineering.



*You need explain the overview of the design problem with a diagram and brief abstract*

[Please refer to past design problem](https://github.com/ykkimhgu/course-doc/blob/master/ec-course/project/LAB_DesignProblem_Smart%20Home_2021%20(1).pdf) 

* [Watch past final project videos](https://ykkim.gitbook.io/ec/ec-course/project/past-projects)

![image](https://user-images.githubusercontent.com/38373000/192134563-72f68b29-4127-42ac-b064-2eda95a9a52a.png)





### Score: Total 100pt

This project will be scored based on (1) Complexity (2) Completeness (3) Creativity

* Demonstration: 70pt (including demo video)
* Report: 30pt (1 report per team)
* Peer Evaluation: +10 to -10





## Requirements

You must use at least 3 sensors: including 1 analog sensor and 1 digital sensor

*  You can choose sensors that were not used in the course LABs.
*  You will get higher scores if you use more sensors
*   You must use at least 2 actuators
*  You must use at least 1 wireless communication. 
*  You must use at least 1 timer interrupt IRQ 
*  You will get a higher score if you use more numbers of MCUs, sensors, and actuators 

 

### Hardware

*Write down all the necessary hardware.*

| **Item**        | **Model/Description**                   | **Qty** |
| --------------- | --------------------------------------- | ------- |
| MCU             | NUCLEO  -F411RE                         | 2       |
| Analog  Sensor  | IR  reflective optical sensor(TCRT5000) | 1       |
| Digital  Sensor | -                                       | -       |
| Actuator        | -                                       | -       |
| Display         | -                                       | -       |
| Communication   | -                                       | -       |
| Others          | -                                       | -       |

###  


### Software

Keil uVision IDE, CMSIS, EC_HAL

 

---





#  II. Problem 



## Problem Description

*Detail description of the problem goes here.*

Your problem must have at least 3 modes(states). Explain necessary states(modes) and description

 

Example diagram: 

![img](file:///C:/Users/ykkim/AppData/Local/Temp/msohtmlclip1/01/clip_image002.gif)



Description of system modes, how your MCU should function and more



*For example:* 

**1.**   **System Mode:** 

| **MODE**                     | **Description**                                              |
| ---------------------------- | ------------------------------------------------------------ |
| Normal Mode  (NORM_MODE)     | Pressing   MODE_button (B1 of MCU_1) toggles from   NORM_mode 🡨🡪SECUR _mode |
| Security Mode  (SECUR_MODE): | Pressing   STOP_button (B1 of MCU_2) resets from SIREN_mode to SECUR_mode. |

**2.**   **Front Door:**

| **MODE**                     | **Description**                                              |
| ---------------------------- | ------------------------------------------------------------ |
| Normal Mode  (NORM_MODE)     | When a person is detected within the given  distance from the door(outside): Keep  turning on the door light as long as the person is present.   When a person is detected inside the house  nearby the door: Turn on the door light for given period of time and does not  generates the SIREN _TRG trigger. |
| Security Mode  (SECUR_MODE): | When a person is detected within the given  distance from the door(outside): Keep  turning on the door light as long as the person is present and sends the  VISITOR_LOG message to the server.    When  a person is detected inside the house nearby the door: Keep turning on the door light and  generates the SIREN _TRG trigger. |



  **3. Sensor Unit: MCU_1**

 

| **Function**                  | **Sensor**                  | **Configuration**                                            | **Comments**                                                 |
| ----------------------------- | --------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Door  Person detect  (front)  | Ultrasonic  distance sensor | Sampling 0.1 sec  Check object presence within 30cm   Generates PERSON_OUTSIDE flag | Need to check for outlier measurements  (at least 7/10 Postive)  VISITOR_LOG under SECUR_MODE |
| Door  Person detect  (inside) | PIR  motion sensor          | Edge trigger  Generates PERSON_INSIDE flag                   |                                                              |

 



 

## MCU Configuration 

You are free to select appropriate configurations for the design problem. Create a configuration table to list all the necessary setup. 

Example: 

| **Functions**   | **Register** | **PORT_PIN**        | **Configuration**                                      |
| --------------- | ------------ | ------------------- | ------------------------------------------------------ |
| System Clock    | RCC          | -                   | PLL 84MHz                                              |
| delay_ms        | SysTick      | -                   |                                                        |
| Bluetooth       | USART1       | TXD: PA9  RXD: PA10 | No Parity, 8-bit Data, 1-bit Stop  bit  9600 baud-rate |
| Timer Interrupt | TIMER1       | ...                 | ...                                                    |

 



## Circuit Diagram

 

**MUST condition:** Show the wiring of each sensor/actuator component to MCUs. 

*DO NOT draw by hand.!!!*

![image](https://user-images.githubusercontent.com/38373000/192134563-72f68b29-4127-42ac-b064-2eda95a9a52a.png)

##  

 



 

# III. Algorithm 

## Logic Design

 In the report, you need to explain concisely how you designed the system  with state tables/diagram or flow-chart.

* Listing all necessary states (states, input, output etc) to implement this design problem.

* Listing all necessary conditional FLAGS for programming.

* Showing the logic flow from the initialization

* and more

  

## Code

Your code (private repositor) goes here: [ADD Code LINK such as github](https://github.com/ykkimhgu/EC-student/)

```c++
// YOUR MAIN CODE ONLY

// YOUR CODE with Comments
```



---



# IV. Results and Demo

Experiment images and results

> Show experiment images /results
>
> You are required to show In-class demonstration. 

 

Add [demo video link]

 

---



# V. Reference

Complete list of all references used (github, blog, paper, etc)





# Appendix 

## Troubleshooting

## Other Appendix

 
