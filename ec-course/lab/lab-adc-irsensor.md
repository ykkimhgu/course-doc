# LAB: ADC - Analog Sensor 



**Date:** 2023-11-13

**Author/Partner:**

**Github:** repository link

**Demo Video:** Youtube link

**PDF version:**&#x20;



## Introduction

IR Reflective Sensor

Create a simple program that uses ADCs to use analog sensors. For this lab, we will use two IR reflective sensors. 

The ADCs are triggered by a timer at the given sampling rate.  

This lab will be extended for the LAB: Line Tracing Car



You must submit

* LAB Report (\*.md & \*.pdf)
* Zip source files(main\*.c, ecRCC.h, ecGPIO.h, ecSysTick.c   etc...).
  * Only the source files. Do not submit project files



### Requirement

#### Hardware

* MCU
  * NUCLEO-F411RE
* Actuator/Sensor/Others:
  * IR Reflective Sensor (TCRT 5000) x2



#### Software

* Keil uVision, CMSIS, EC\_HAL library





## Problem 1: EC  library

### Using HAL library



Download [ecADC_student.c ecADC_student.h](https://github.com/ykkimhgu/EC-student/blob/main/include/lib-student)

Change the file names as 

* ecADC.c, ecADC.h

  

Fill-in the missing code lines to define the following functions.

You must update your header files located in the directory `EC \lib\`.  



**ecADC.h**

```c++
// ADC setting
void ADC_init(GPIO_TypeDef *port, int pin, int trigmode);	// trigmode : SW , TRGO
void ADC_continue(int contmode); 							// contmode : CONT, SINGLE / Operate both ADC,JADC
void ADC_TRGO(TIM_TypeDef* TIMx, int msec, int edge);		// set the TIMx TRGO (TIM2, TIM3 only)
void ADC_sequence(int length, int *seq); 					// configure the order of ADC channels	

void ADC_start(void);
uint32_t ADC_read(void);

uint32_t is_ADC_EOC(void);
uint32_t is_ADC_OVR(void);
void clear_ADC_OVR(void);

uint32_t ADC_pinmap(GPIO_TypeDef *port, int pin);
```





### Example Code



Check your library files by running the following examples. 



**Example 1:  Using one Analog sensor**

* Single-Channel, Continuous Scan, 1msec ADC triggering with TIM3

```cpp
#include "stm32f4xx.h"
#include "ecGPIO.h"
#include "ecRCC.h"
#include "ecUART.h"
#include "ecSysTick.h"





```





**Example 2:  Using 2 Analog sensors**

* Multi-Channel, Continuous Scan, 1msec ADC triggering with TIM3
* Channel Sequence:  

```cpp
#include "stm32f4xx.h"
#include "ecGPIO.h"
#include "ecRCC.h"
#include "ecUART.h"
#include "ecSysTick.h"





```







## Problem 2: Measurement from multiple analog sensors 
### IR Reflective Sensors (TCRT 5000)

First, download and read IR Reflective Sensor(TCRT 5000): [Spec Sheet](https://www.devicemart.co.kr/goods/download?id=1327416&rank=1)

TCRT5000 and TCRT5000L are reflective sensors that include an infrared emitter and phototransistor in a leaded package which blocks visible light.

![image](https://user-images.githubusercontent.com/91526930/200573269-26a4ffa1-c789-4545-9dc5-079294265d4d.png)

**The HC-SR04 Ultrasonic Range Sensor Features:**

- Input Voltage : 5V
- Detector type: phototransistor 
- Operating range within > 20 % relative collector current: 0.2 mm to 15 mm 
- Emitter wavelength: 950 nm

**APPLICATIONS**

- Position sensor for shaft encoder

- Detection of reflective material such as paper, IBM cards, magnetic tapes etc. 

- Limit switch for mechanical motions in VCR 

  





### Procedure

1. Create a new project under the directory `\repos\EC\LAB\LAB_ADC_IR`

* The project name is “**LAB_ADC_IR”**

* Create a new source file named as “**LAB_ADC_IR.c”**

> You MUST write your name on the source file inside the comment section. 





2\.  Update and use **ecSTM32F411.h**. This header should be defined as explained in 

[Tutorial: managing lib headers]: https://ykkim.gitbook.io/ec/ec-course/tutorial/tutorial-library-header-files#ec-header-files





### Configuration



| Type             | Port - Pin                                                   | Configuration                                                |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **System Clock** |                                                              | PLL 84MHz                                                    |
| **GPIO**         | PB_0<br />PB_1                                               | Analog Mode<br />No Pull-up Pull-down                        |
| **ADC**          | PB_0:  ADC1_CH8 (1st channel)<br />PB_1:  ADC1_CH9 (2nd channel) | ADC Clock Prescaler /8<br />12-bit resolution, right alignment<br />Continuous  Conversion mode<br />Scan mode: Two channels in regular group<br />External Trigger (Timer3 TRGO) @ 1kHz<br />Trigger Detection on Rising Edge |
| **TIM3**         |                                                              | Up-Counter, Counter CLK 1kHz<br />OC1M(Output Compare 1 Mode) :  PWM mode 1<br />Master Mode Selection: (TRGO) OC1REF |



### Line Tracing

![image](https://github.com/ykkimhgu/course-doc/assets/38373000/7dc22942-cc7a-48a1-b794-cabbbd264f18)



- Create a logic to trace a dark line on white background surface for your RC car. 

- Use 2 IR reflective sensors to detect if the black line is in between the sensors. It should display whether the system needs to move **Left or Right** to keep the line between sensors. 

- Set the ADC sampling rate trigger to be 1KHz

- Determine the threshold value to differentiate dark and white surface of the object.

- Display  (1) and (2) on  serial monitor of Tera-Term. Print the values every second.

  ​	(1) reflection value of IR1 and IR2 

  ​	(2) print ‘GO LEFT’ or ‘GO ‘RIGHT’ 
  
  

**Display Example**

![image](https://user-images.githubusercontent.com/91526930/200577686-909a4cb7-1af9-459a-8afa-aaaee64aa448.png)



### Circuit Diagram

> You need to include  the circuit diagram

![image](https://user-images.githubusercontent.com/38373000/192134563-72f68b29-4127-42ac-b064-2eda95a9a52a.png)



### Discussion

1.  How would you change the code if you need to use 3 Analog sensors?

   >  Answer discussion questions



2. Which registers should be modified if you need to use Injection Groups instead of regular groups for 2 analog sensors? 

   >  Answer discussion questions





### Code

Your code goes here: [ADD Code LINK such as github](https://github.com/ykkimhgu/EC-student/)

Explain your source code with necessary comments.

```
// YOUR MAIN CODE ONLY
// YOUR CODE
```





### Results

Experiment images and results

> Show experiment images /results

Add [demo video link](link/)







## Reference

Complete list of all references used (github, blog, paper, etc)

```

```



## Troubleshooting

(Option) You can write Troubleshooting section
