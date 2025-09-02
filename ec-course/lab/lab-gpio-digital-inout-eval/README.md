# LAB: GPIO Digital InOut(eval board)

## LAB: GPIO Digital InOut

**Date:** 2025-09-02

**Author/Partner:**

**Github:** repository link

**Demo Video:** Youtube link

**PDF version:**

## Introduction

In this lab, you are required to create a simple program that toggle multiple LEDs with a push-button input. Create HAL drivers for GPIO digital in and out control and use your library.

You must submit

* LAB Report (\*.pdf)
* Zip source files(main\*.c, ecRCC.h, ecGPIO.h etc...).
  * Only the source files. Do not submit project files

### Requirement

#### Hardware

* MCU
  * NUCLEO-F411RE
* Actuator/Sensor/Others:
  * eval board

#### Software

* PlatfromIO, CMSIS, EC\_HAL library

***

## Problem 0: STM-Arduino

We are going to create a simple program that turns LED(LD2) on and off by pressing the user button(BT1), using Arduino Syntax

### GPIO Digital In/Out

Create a new project under the directory `\repos\EC\lab\`

* The project folder name is “**\LAB\_GPIO\_DIO\_LED”.**

Configures the specified pin to behave either as an input or an output.

```cpp
pinMode(pin, mode)
```

* pin: the pin number to set the model of.
* mode: INPUT, OUTPUT or INPUT\_PULLUP.

> Look up for pinMode() function in arduino reference for detail description.

### Example code

Open _Arduino IDE_ and Create a new program named as ‘**TU\_arduino\_GPIO\_LED\_button.ino**’.

Write the following source code: [source code](https://github.com/ykkimhgu/EC-student/tree/main/stm32duino-tutorial).

```cpp
const int btnPin = 3;
const int ledPin = 13;

int btnState = HIGH;

void setup() {
  pinMode(ledPin, OUTPUT);
  pinMode(btnPin, INPUT);
}

void loop() {
  btnState = digitalRead(btnPin);

  if (btnState == HIGH) 
    digitalWrite(ledPin, LOW);
  
  else 
    digitalWrite(ledPin, HIGH);
  
}
```

The user button pin is `PC13`, but this pin cannot be used in arduino. So, you should connect `PC13` to `pinName` `D3` by using wire.

![button pin connection](https://user-images.githubusercontent.com/91526930/186584565-3dc47e19-e5c5-43a2-b4c4-d4de8916a2c8.png)

Click on **upload** button. Push the reset button(black) and check the performance.

The LED(LD2) should be turned on when the button is pressed.

***

## Problem 1: Create EC\_HAL library

### Procedure

Create the library directory `\repos\EC\include\`.

Save your header library files in this directory. [See here for detail.](https://ykkim.gitbook.io/ec/uvision/adding-my-api-header-in-uvision)

> DO NOT make duplicates of library files under each project folders

Create your own library for Digital\_In and Out : `ecGPIO2.h, ecGPIO2.c`

* [Download library files from here](https://github.com/ykkimhgu/EC-student/tree/main/include/lib-student)
* Use the provided `ecRCC2.h` and `ecRCC2.c`
* Modify `ecGPIO2.c`, `ecGPIO2.h`

**ecRCC2.h** (provided)

```cpp
void RCC_HSI_init(void);  
void RCC_GPIOA_enable(void);   
void RCC_GPIOB_enable(void); 
void RCC_GPIOC_enable(void);
```

**ecGPIO2.h**

```cpp
void GPIO_init(PinName_t pinName, int mode);
void GPIO_write(PinName_t pinName, int Output);
int  GPIO_read(PinName_t pinName);
void GPIO_mode(PinName_t pinName, int mode);
void GPIO_ospeed(PinName_t pinName, int speed);
void GPIO_otype(PinName_t pinName, int type);
void GPIO_pupd(PinName_t pinName, int pupd);

/*
// Version 1
void GPIO_init(GPIO_TypeDef *Port, int pin,  int mode);  
void GPIO_write(GPIO_TypeDef *Port, int pin,  int output);  
int  GPIO_read(GPIO_TypeDef *Port, int pin);  
void GPIO_mode(GPIO_TypeDef* Port, int pin, int mode);  
void GPIO_ospeed(GPIO_TypeDef* Port, int pin,  int speed);  
void GPIO_otype(GPIO_TypeDef* Port, int pin,  int type);  
void GPIO_pupd(GPIO_TypeDef* Port, int pin,  int pupd);
*/
```

* Example code in **ecGPIO2.c**

```cpp
/* ecGPIO2.c  */

// Input(00), Output(01), AlterFunc(10), Analog(11, reset)
void GPIO_mode(PinName_t pinName, uint32_t mode){
 	GPIO_TypeDef *port;
	unsigned int pin;
	ecPinmap(pinName, &port, &pin);
    
	port->MODER &= ~(3UL<<(2*pin));     
	port->MODER |= mode<<(2*pin);    
}


/*
// Version 1
// Input(00), Output(01), AlterFunc(10), Analog(11)
void GPIO_mode(GPIO_TypeDef *Port, int pin, int mode){
   Port->MODER &= ~(3UL<<(2*pin));     
   Port->MODER |= mode<<(2*pin);    
}
*/
```

## Problem 2: Toggle a single LED with Digital Sensor(Photodetector)

### Procedure

1. Create a new project under the directory `\repos\EC\lab\`

* The project name is “**LAB\_GPIO\_DIO\_LED\_SENSOR”.**
* Name the source file as “**LAB\_GPIO\_DIO\_LED\_SENSOR.c”**
* Use the [example code provided here](https://github.com/ykkimhgu/EC-student/blob/main/lab/lab-student/LAB_GPIO_DIO_LED_student.c).

2\. Include your rary **ecGPIO2.h, ecGPIO2.c** in `\repos\EC\include\`.

> You MUST write your name in the top of the source file, inside the comment section.

3\. Toggle the LED by covering the photodetector sensor.

* Dark  (LED ON), Bright (LED OFF) and repeat

### Configuration

| Digital Sensor (Photodetector) | LED                               |
| ------------------------------ | --------------------------------- |
| Digital In                     | Digital Out                       |
| GPIOA, Pin 0                   | GPIOB, Pin 12                     |
| PULL-UP                        | Open-Drain, Pull-up, Medium Speed |

### Code

Your code goes here:

Explain your source code with necessary comments.

**Sample Code**

```cpp
#include "ecRCC2.h"
#include "ecGPIO2.h"

#define LED_PIN PB_12
#define BUTTON_PIN PA_0

// Initialiization 
void setup(void) {
	RCC_HSI_init();
	// initialize the pushbutton pin as an input:
	GPIO_init(BUTTON_PIN, INPUT);  
	// initialize the LED pin as an output:
	GPIO_init(LED_PIN, OUTPUT);    
}
	
int main(void) { 
 	setup();
	int buttonState=0;
	
	while(1){
		// check if the pushbutton is pressed. Turn LED on/off accordingly:
		buttonState = GPIO_read(BUTTON_PIN);
		if(buttonState)	GPIO_write(LED_PIN, LOW);
		else 		GPIO_write(LED_PIN, HIGH);
	}
}
```

### Discussion

1. Find out a typical solution for software debouncing and hardware debouncing.
2. What method of debouncing did this NUCLEO board use for the push-button(B1)?

##

## Problem 3: Toggle multiple LEDs with a button

### Procedure

1. Create a new project under the directory `\repos\EC\lab\`

* The project name is “**LAB\_GPIO\_DIO\_multiLED”.**
* Name the source file as “**LAB\_GPIO\_DIO\_multiLED.c”**

> You MUST write your name in the top of the source file, inside the comment section.

2. Include your rary **ecGPIO2.h, ecGPIO2.c** in `\repos\include\`.
3. Connect 4 LEDs externally with necessary load resistors.

* As Button B1 is Pressed, light one LED at a time, in sequence.
* Example: LED0--> LED1--> …LED3--> …LED0….

### Configuration

| Button        | LED                              |
| ------------- | -------------------------------- |
| Digital In    | Digital Out                      |
| GPIOA, Pin 4  | PB12,PB13,PB14,PB15              |
| PULL-UP       | Push-Pull, Pull-up, Medium Speed |

### Circuit Diagram

Circuit diagram

> You need to modify the circuit diagram

![image](https://user-images.githubusercontent.com/38373000/191176652-df38f9ad-5190-4c24-8dfc-fee6206555d9.png)

### Code

Your code goes here

Explain your source code with necessary comments.

```
// YOUR MAIN CODE ONLY
```

### Results

Experiment images and results

> Show experiment images /results

Add [demo video link](../../../course/lab/link/)

### Discussion

> Answer discussion questions

##

## Problem 4: Toggle a single LED with Button

### Procedure

1. Create a new project under the directory `\repos\EC\lab\`

* The project name is “**LAB\_GPIO\_DIO\_LED\_SINGLE”.**
* Name the source file as “**LAB\_GPIO\_DIO\_LED\_SINGLE.c”**
* Use the [example code provided here](https://github.com/ykkimhgu/EC-student/blob/main/lab/lab-student/LAB_GPIO_DIO_LED_student.c).

2\. Include your library **ecGPIO2.h, ecGPIO2.c** in `\repos\EC\include\`.

> You MUST write your name in the top of the source file, inside the comment section.

3\. Toggle the LED by pushing the button.

* Push button (LED ON), Push Button (LED OFF) and repeat

### Configuration

| Button (B1)   | LED                               |
| ------------- | --------------------------------- |
| Digital In    | Digital Out                       |
| GPIOA, Pin 4  | GPIOB, Pin 12                     |
| PULL-UP       | Open-Drain, Pull-up, Medium Speed |

### Code

Your code goes here:

Explain your source code with necessary comments.

**Sample Code**

```cpp
#include "ecRCC2.h"
#include "ecGPIO2.h"

#define LED_PIN PB_12
#define BUTTON_PIN PA_4

// Initialiization 
void setup(void) {
	RCC_HSI_init();
	// initialize the pushbutton pin as an input:
	GPIO_init(BUTTON_PIN, INPUT);  
	// initialize the LED pin as an output:
	GPIO_init(LED_PIN, OUTPUT);    
}
	
int main(void) { 
 	setup();
	int buttonState=0;
	
	while(1){
		// check if the pushbutton is pressed. Turn LED on/off accordingly:
		buttonState = GPIO_read(BUTTON_PIN);
		if(buttonState)	GPIO_write(LED_PIN, LOW);
		else 		GPIO_write(LED_PIN, HIGH);
	}
}
```

### Discussion

> Answer discussion questions

##

## Reference

Complete list of all references used (github, blog, paper, etc)

```
```

## Troubleshooting

(Option) You can write a Troubleshooting section
