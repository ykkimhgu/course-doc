# LAB: ADC - IR reflective sensor

**Date:** 2026-11-06

**Author/Partner:**

**Github:** repository link

**Demo Video:** Youtube link

**PDF version:**

## Introduction

Create a simple program that uses ADCs to use analog sensors. For this lab, we will use two IR reflective sensors.

The ADCs are triggered by a timer at the given sampling rate.

This lab will be extended to Project: Line Tracing Car

You must submit

* LAB Report (\*.md & \*.pdf)
* Zip source files(main\*.c, ecRCC.h, ecGPIO.h, ecSysTick.c etc...).
  * Only the source files. Do not submit project files

### Requirement

#### Hardware

* MCU
  * NUCLEO-F411RE
* Actuator/Sensor/Others:
  * IR Reflective Sensor (TCRT 5000) x2

#### Software

* Keil uVision, CMSIS, EC\_HAL library

## Problem 0: STM-Arduino

<img src="https://ykkim.gitbook.io/ec/~gitbook/image?url=https%3A%2F%2Fuser-images.githubusercontent.com%2F79825525%2F129156915-b3ca7031-c459-428e-be9b-dbe6acce91b2.png&#x26;width=768&#x26;dpr=3&#x26;quality=100&#x26;sign=e8fa8454e22f73747417f7e05b6e6f58&#x26;sv=3" alt="" height="276" width="1017">

* Photo-resistor module outputs a low voltage under a bright condition, and vice versa.
* Shine your phone’s flashlight on the photoresistor
  * The sensor’s output voltage should decreases
* Try different voltage thresholds to adjust the detection sensitivity.
* Create a new program named as ‘**TU\_arduino\_AnalogIn**.**ino**
* Click on **Upload** button.
* Open ‘Tera Term’ and make New Connection.

```c
const int ledPin = 13;
const int cdsPin = 0;
float measure;

int ledState = LOW;

void setup() {
  pinMode(ledPin, OUTPUT);
 
  Serial.begin(9600);
}

void loop() {
  
  measure = analogRead(cdsPin);     // mapping(0 ~ 3.3V -> 0 ~ 1023)
  measure = measure * 3300 / 1024;  // [mV] (0 ~ 1023 -> 0 ~ 3300[mV])
  Serial.print("measure = ");
  Serial.print(measure);
  Serial.println(" [mV]");

  if (measure > 2500) ledState = HIGH;
  else                ledState = LOW;

  digitalWrite(ledPin, ledState);
  delay(200);
}
```

#### &#x20;<a href="#exercise-1-photodetection" id="exercise-1-photodetection"></a>

## ADC Library (ecADC.h)

Download the exercise files.

* [ecADC2\_student.c](https://github.com/ykkimhgu/EC-student/blob/main/include/lib-student),  [ecADC2\_student.h](https://github.com/ykkimhgu/EC-student/blob/main/include/lib-student)

Change the file names as `ecADC2.c, h`

> You must update your header files in the directory `EC\include\`. and add in `ecSTM32F4v2.h`

**ecADC.h**

{% code expandable="true" %}
```cpp

////////////////////////////////////////////////////////////////
/* Basic Syntax                                                */
////////////////////////////////////////////////////////////////

// Analog pin, ADC clock, 12-bit, sampling time, one-channel sequence.
// software trigger, one conversion,
void ADC_init(PinName_t pinName);

// single conversion : start, wait for EOC, return the result.
uint32_t ADC_read_poll(void);

// Use inside ADC_IRQHandler() after is_ADC_EOC().
uint32_t ADC_read(void);

// Hardware trigger : the TIMx UPDATE event (TRGO) every msec. 
// Only TIM2 and TIM3 reach the ADC regular group on the F411
int ADC_trigger_init(TIM_TypeDef *TIMx, uint32_t msec, uint32_t edge);


////////////////////////////////////////////////////////////////
/* JADC : the INJECTED group,                                  */
////////////////////////////////////////////////////////////////

// Analog pin + one-channel injected sequence. 
// software trigger, no interrupt, 
void JADC_init(PinName_t pinName);

// SW triggered single injected conversion
uint32_t JADC_read_poll(void);

// Result of the n-th conversion of the injected sequence, JDRn = 1 ~ 4.
// Reading JDRx does NOT clear JEOC - call clear_ADC_JEOC() yourself.
uint32_t JADC_read(uint32_t JDRn);

// Hardware trigger : the TIMx UPDATE event (TRGO) every msec. 
// Also enables the JEOC interrupt, TIM1, TIM2, TIM4, TIM5
// Returns 0 on success, -1 if TIMx has no TRGO entry for the injected group.
int JADC_trigger_init(TIM_TypeDef *TIMx, uint32_t msec, uint32_t edge);

```
{% endcode %}


### Example Code:  Polling Analog Read 
* Two Sensors(Channels), Single Conversion
* Trigger: SW trigger, Polling
* Injected Group  
** JSQ1=ADC_CH_8(PB_0)
** JSQ2=ADC_CH_9(PB_1)
* Regular Group 
** SQ1=ADC_CH_8(PB_0)
** SQ2=ADC_CH_9(PB_1)

{% tabs %}
{% tab title="JADC-Polling" %}
```c
/*----------------------------------------------------------------\
@ Embedded Controller by Young-Keun Kim - Handong Global University
Author           : [ YOUR NAME GOES HERE !!!!!]
Created          : 10-23-2026
Modified         : 00-00-2026 [WRITE THE DATE!!!!]
Language/ver     : C in VS Code

Description      : Tutorial - ADC polling, injected group (ecADC2)

/----------------------------------------------------------------*/
#include "ecSTM32F4v2.h"
#include "ecADC2.h"

//Analog Sensor parameter//
uint32_t value1, value2;
PinName_t seqCHn[2] = {PB_0, PB_1};

void setup(void);

int main(void) {
	// Initialiization --------------------------------------------------------
	setup();

	// Inifinite Loop ----------------------------------------------------------
	while(1){
		value1 = JADC_read_poll();		// one start converts the sequence
		value2 = JADC_read(2);			// JDR2 already holds conversion 2

		printf("value1 = %lu \r\n",value1);
		printf("value2 = %lu \r\n",value2);
		printf("\r\n");

		delay_ms(1000);
	}
}

// Initialiization
void setup(void)
{
	RCC_PLL_init();         // System Clock = 84MHz
	UART2_init();			// UART2 Init
	SysTick_init();			// SysTick Init

	// JADC Init  Default: SW trigger, no interrupt
	JADC_init(PB_0);
	JADC_init(PB_1);

	// JADC channel sequence setting
	JADC_sequence(seqCHn, 2);
}


```  
{% endtab %}
{% tab title="ADC-Polling" %}
```c
/*----------------------------------------------------------------\
@ Embedded Controller by Young-Keun Kim - Handong Global University
Author           : [ YOUR NAME GOES HERE !!!!!]
Created          : 10-23-2026
Modified         : 00-00-2026 [WRITE THE DATE!!!!]
Language/ver     : C in VS Code

Description      : Tutorial - ADC polling, regular group (ecADC2)

/----------------------------------------------------------------*/
#include "ecSTM32F4v2.h"
#include "ecADC2.h"

//Analog Sensor parameter//
uint32_t value1, value2;
PinName_t seqCHn[2] = {PB_0, PB_1};

void setup(void);

int main(void) {
	// Initialiization --------------------------------------------------------
	setup();

	// Inifinite Loop ----------------------------------------------------------
	while(1){
		value1 = ADC_read_poll();		// SQ1 : PB_0
		value2 = ADC_read_poll();		// SQ2 : PB_1

		printf("value1 = %lu \r\n",value1);
		printf("value2 = %lu \r\n",value2);
		printf("\r\n");

		delay_ms(1000);
	}
}

// Initialiization
void setup(void)
{
	RCC_PLL_init();         // System Clock = 84MHz
	UART2_init();			// UART2 Init
	SysTick_init();			// SysTick Init

	// ADC Init  Default: SW trigger, single conversion, no interrupt
	ADC_init(PB_0);
	ADC_init(PB_1);

	// ADC channel sequence setting
	ADC_sequence(seqCHn, 2);
}


```    
{% endtab %}
{% endtabs %}

### Example Code:  Timer-Triggered, EOC Interrupt  
* Two Sensors(Channels), Single Conversion
* Trigger: Timer trigger
** Injected Group: TIM1,2,4,5 only
** Regular Group: TIM 2,3 only  
* Injected Group : TIM_5  
** JSQ1=ADC_CH_8(PB_0)
** JSQ2=ADC_CH_9(PB_1)
* Regular Group : TIM_3
** SQ1=ADC_CH_8(PB_0)
** SQ2=ADC_CH_9(PB_1)

  
{% tabs %}
{% tab title="JADC-Interrupt" %}
```c
/*----------------------------------------------------------------\
@ Embedded Controller by Young-Keun Kim - Handong Global University
Author           : [ YOUR NAME GOES HERE !!!!!]
Created          : 10-23-2026
Modified         : 00-00-2026 [WRITE THE DATE!!!!]
Language/ver     : C in VS Code

Description      : Tutorial - ADC timer-triggered + interrupt, injected group (ecADC2)

/----------------------------------------------------------------*/
#include "ecSTM32F4v2.h"
#include "ecADC2.h"

//Analog Sensor parameter//
volatile uint32_t value1, value2;
PinName_t seqCHn[2] = {PB_0, PB_1};

void setup(void);

int main(void) {
	// Initialiization --------------------------------------------------------
	setup();

	// Inifinite Loop ----------------------------------------------------------
	while(1){
		printf("value1 = %lu \r\n",value1);
		printf("value2 = %lu \r\n",value2);
		printf("\r\n");

		delay_ms(1000);
	}
}

// Initialiization
void setup(void)
{
	RCC_PLL_init();         // System Clock = 84MHz
	UART2_init();			// UART2 Init
	SysTick_init();			// SysTick Init

	// JADC Init  Default: SW trigger, no interrupt
	JADC_init(PB_0);
	JADC_init(PB_1);

	// JADC channel sequence setting
	JADC_sequence(seqCHn, 2);

	// HW trigger : TIM5 TRGO @ 1msec - enables JEOC interrupt
	JADC_trigger_init(TIM5, 1, ADC_RISE);
}


void ADC_IRQHandler(void){
	if(is_ADC_JEOC()){		// one JEOC after the whole sequence
		value1 = JADC_read(1);
		value2 = JADC_read(2);
		clear_ADC_JEOC();
	}
}


```    
{% endtab %}
{% tab title="ADC-Interrupt" %}
```c
/*----------------------------------------------------------------\
@ Embedded Controller by Young-Keun Kim - Handong Global University
Author           : [ YOUR NAME GOES HERE !!!!!]
Created          : 10-23-2026
Modified         : 00-00-2026 [WRITE THE DATE!!!!]
Language/ver     : C in VS Code

Description      : Tutorial - ADC timer-triggered + interrupt, regular group (ecADC2)

/----------------------------------------------------------------*/
#include "ecSTM32F4v2.h"
#include "ecADC2.h"

//Analog Sensor parameter//
volatile uint32_t value1, value2;
volatile uint32_t seqIdx = 0;			// EOC fires once per conversion
PinName_t seqCHn[2] = {PB_0, PB_1};

void setup(void);

int main(void) {
	// Initialiization --------------------------------------------------------
	setup();

	// Inifinite Loop ----------------------------------------------------------
	while(1){
		printf("value1 = %lu \r\n",value1);
		printf("value2 = %lu \r\n",value2);
		printf("\r\n");

		delay_ms(1000);
	}
}

// Initialiization
void setup(void)
{
	RCC_PLL_init();         // System Clock = 84MHz
	UART2_init();			// UART2 Init
	SysTick_init();			// SysTick Init

	// ADC Init  Default: SW trigger, single conversion, no interrupt
	ADC_init(PB_0);
	ADC_init(PB_1);

	// ADC channel sequence setting
	ADC_sequence(seqCHn, 2);

	// HW trigger : TIM3 TRGO @ 1msec, enables EOC interrupt
	ADC_trigger_init(TIM3, 1, ADC_RISE);
}


void ADC_IRQHandler(void){
	if(is_ADC_OVR()) clear_ADC_OVR();
	if(is_ADC_EOC()){		// EOC per conversion : SQ1, then SQ2
		if(seqIdx == 0)	value1 = ADC_read();
		else			value2 = ADC_read();
		seqIdx ^= 1;
	}
}


```  
{% endtab %}
{% endtabs %}




## Problem: Measurement from multiple analog sensors

### IR Reflective Sensors (TCRT 5000)

First, download and read IR Reflective Sensor(TCRT 5000): [Spec Sheet](https://www.devicemart.co.kr/goods/download?id=1327416\&rank=1)

TCRT5000 and TCRT5000L are reflective sensors that include an infrared emitter and phototransistor in a leaded package, which blocks visible light.

![image](https://user-images.githubusercontent.com/91526930/200573269-26a4ffa1-c789-4545-9dc5-079294265d4d.png)

**The HC-SR04 Ultrasonic Range Sensor Features:**

* Input Voltage : 5V
* Detector type: phototransistor
* Operating range within > 20 % relative collector current: 0.2 mm to 15 mm
* Emitter wavelength: 950 nm

**APPLICATIONS**

* Position sensor for shaft encoder
* Detection of reflective material such as paper, IBM cards, magnetic tapes etc.
* Limit switch for mechanical motions in VCR

### Procedure

1. Create a new project under the directory `\repos\EC\LAB\LAB_ADC`

* The Enviornment  name is “**LAB\_ADC\_IR”**
* Create a new source file named as “**LAB\_ADC\_IR.c”**

> You MUST write your name on the source file inside the comment section.

2\. Update and use **ecSTM32F411.h**. This header should be defined as explained in

### Configuration

| Type             | Port - Pin                                                          | Configuration                                                                                                                                                                                                                 |
| ---------------- | ------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **System Clock** |                                                                     | PLL 84MHz                                                                                                                                                                                                                     |
| **GPIO**         | <p>PB_0<br>PB_1</p>                                                 | <p>Analog Mode<br>No Pull-up Pull-down</p>                                                                                                                                                                                    |
| **ADC (JADC)**   | <p>PB_0: ADC1_CH8 (1st channel)<br>PB_1: ADC1_CH9 (2nd channel)</p> | <p>ADC Clock Prescaler /8<br>12-bit resolution, right alignment<br>Continuous Conversion mode<br>Scan mode: Two channels in regular group<br>External Trigger (Timer3 Trigger) @ 1kHz<br>Trigger Detection on Rising Edge</p> |
| **TIM3**         |                                                                     | <p>Up-Counter, Counter CLK 1kHz<br>OC1M(Output Compare 1 Mode) : PWM mode 1<br>Master Mode Selection: (Trigger) OC1REF</p>                                                                                                    |

### Line Tracing

![line](https://github.com/ykkimhgu/EC-student/assets/84508106/8092b591-4e70-49b3-8690-4e9e09642bd1)

* Create a logic to trace a dark line on white background surface for your RC car.
* Use 2 IR reflective sensors to detect if the black line is in between the sensors. It should display whether the system needs to move **Left or Right** to keep the line between sensors.
* Set the ADC sampling rate trigger to be 1KHz
* Determine the threshold value to differentiate dark and white surface of the object.
* Display (1) and (2) on serial monitor of Tera-Term.&#x20;
  * Print the values every second.
  * reflection value of IR1 and IR2
  * print ‘GO LEFT’ or ‘GO ‘RIGHT’

**Display Example**

![image](https://user-images.githubusercontent.com/91526930/200577686-909a4cb7-1af9-459a-8afa-aaaee64aa448.png)

### Circuit Diagram

> You need to include the circuit diagram

![image](https://user-images.githubusercontent.com/38373000/192134563-72f68b29-4127-42ac-b064-2eda95a9a52a.png)

### Discussion

1. How would you change the code if you need to use 3 Analog sensors?

> Answer discussion questions

2.  Which registers should be modified if you need to use Injection Groups instead of regular groups for 2 analog sensors?

    > Answer discussion questions

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

Add [demo video link](https://github.com/ykkimhgu/course-doc/blob/master/ec-course/lab/link/README.md)

## Reference

Complete list of all references used (github, blog, paper, etc)

```
```

## Troubleshooting

(Option) You can write Troubleshooting section
