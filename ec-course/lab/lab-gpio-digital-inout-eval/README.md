# LAB: GPIO Digital InOut(eval board)

## LAB: GPIO Digital InOut

**Date:** 2026-09-02&#x20;

**Author/Partner:** YOUR NAME GOES HERE

**Demo Video:** Youtube link

**PDF version:**&#x20;

## Introduction

In this lab, you are required to create a simple program that toggles multiple LEDs using a push-button input.&#x20;

You must create your own drivers for GPIO digital input and output control.



You must submit

* LAB Report (\*.pdf)
* Zip source files(\*main\*.c, ecRCC.h, ecGPIO.h etc...).
  * Only the source files.&#x20;
  * Do not submit project files

### Requirement

#### Hardware

* MCU
  * NUCLEO-F411RE
* Eval Board
* Actuator/Sensor/Others:

#### Software

* PlatformIO, CMSIS, EC\_HAL library

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

#### **Prepare Library  Files**

Manage your library in the directory of  `\repos\EC\include\`.

> DO NOT make duplicates of library files under each project folders

* **For VS.Code User:** Save your header library files in this directory. [See here for detail.](../../tutorial/tutorial-platformio-in-vscode.md)
* **For uVision User:** Save your header library files in this directory. [See here for detail.](https://ykkim.gitbook.io/ec/uvision/adding-my-api-header-in-uvision)



&#x20;[Download library files. ](https://github.com/ykkimhgu/EC-student/tree/main/include/lib-student)

* `ecRCC2.h, ecRCC2.c`
* `ecPinNames.h, ecPinNames.c`
* `ecGPIO2_student.h, ecGPIO2_student.c`



Create your own library for GPIO by renaming the downloaded files (`ecGPIO2_student.*)` as

* &#x20;`ecGPIO2.c`, `ecGPIO2.h`
* Open `ecGPIO2.c`  and change from  `#include "ecGPIO2_student.h"`  to `#include "ecGPIO2.h"`
* Write your name and modified date in the comment section

> You MUST write your name  inside the comment section.

#### Function Definitions

Fill-in the blanks in the header files to complete the definitions for the provided functions&#x20;



**ecGPIO2.h**

```cpp
void GPIO_init(PinName_t pinName, int mode);
void GPIO_write(PinName_t pinName, int Output);
int  GPIO_read(PinName_t pinName);
void GPIO_mode(PinName_t pinName, int mode);
void GPIO_ospeed(PinName_t pinName, int speed);
void GPIO_otype(PinName_t pinName, int type);
void GPIO_pupd(PinName_t pinName, int pupd);
```

* Example code in **ecGPIO2.c**

```cpp
// Input(00), Output(01), AlterFunc(10), Analog(11, reset)
void GPIO_mode(PinName_t pinName, uint32_t mode){
 	GPIO_TypeDef *port;
	unsigned int pin;
	ecPinmap(pinName, &port, &pin);
    
	port->MODER &= ~(3UL<<(2*pin));     
	port->MODER |= mode<<(2*pin);    
}
```

***

## Problem 2: Toggle a single LED with Digital Sensor(Photodetector)

### Problem&#x20;

&#x20;Toggle the LED by covering the photodetector sensor.

* Dark (LED ON), Bright (LED OFF) and repeat

### Procedure

1. Connect the evaluation board [(JKIT-NUCLEO) ](https://ykkim.gitbook.io/ec/ec-course/hardware/stm32f-evaluation-board#reference-manual) to the MCU.
2. Make sure that your library **ecGPIO2.h, ecGPIO2.c** are in `EC\include\`.
3. Create a new project under the directory `EC\lab\`

* Environment:  “**LAB\_GPIO\_DIO\_LED\_Photosensor”.**
* Source File: “**LAB\_GPIO\_DIO\_LED\_Photosensor.c”**

4. You must modify the **`platformio.ini` ,** to add new environment.&#x20;





### Configuration

Configure the MCU GPIO

| Function                  | Port - Pin | Configuration                                                |
| ------------------------- | ---------- | ------------------------------------------------------------ |
| **Photodetector on JKIT** | PA\_0      | Digital IN, Pull-Up                                          |
| **LED0**                  | PB\_12     | <p>Digital OUT, Pull-UP, </p><p>Open-Drain, Medium Speed</p> |



### Code

Your code goes here:

Explain your source code with necessary comments.

**Sample Code:** [example code ](https://github.com/ykkimhgu/EC-student/blob/main/lab/lab-student/LAB_GPIO_DIO_LED_student.c)

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





## Problem 3: Toggle multiple LEDs with a button

### Problem&#x20;

Use the four LEDs (LED0 to LED3) on the JKIT evaluation board.&#x20;

Each time Button B1 is pressed, turn on one LED at a time in a sequential loop

* e.g., LED0 -> LED1 -> LED2 -> LED3 -> LED0)

### Procedure

1. Connect the evaluation board [(JKIT-NUCLEO) ](https://ykkim.gitbook.io/ec/ec-course/hardware/stm32f-evaluation-board#reference-manual) to the MCU.
2. Make sure that your library **ecGPIO2.h, ecGPIO2.c** are in `EC\include\`.
3. Create a new project under the directory `EC\lab\`

* Environment:  “**LAB\_GPIO\_DIO\_multiLED”**
* Source File: ““**LAB\_GPIO\_DIO\_multiLED.c”**

4. You must modify the **`platformio.ini` ,** to add new environment.&#x20;

> You MUST write your name in the top of the source file, inside the comment section.

### Configuration

| Function           | Port - Pin                      | Configuration                                                |
| ------------------ | ------------------------------- | ------------------------------------------------------------ |
| **SW2  on JKIT**   | PA\_4                           | Digital IN, Pull-Up                                          |
| **LED\_0\~LED\_3** | PB\_12, PB\_13, PB\_14, PB\_152 | <p>Digital OUT, Pull-UP, </p><p>Open-Drain, Medium Speed</p> |



### Circuit Diagram

Draw Circuit diagram (if necessary). No need to draw circuit diagram when using JKIT.

> You need to modify the circuit diagram. Example Image

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

Add [demo video link](https://github.com/ykkimhgu/course-doc/blob/master/course/lab/link/README.md)

### Discussion

1. Find out a typical solution for software debouncing and hardware debouncing.
2. What method of debouncing did this NUCLEO board use for the push-button(B1)?

> Answer discussion questions

## Reference

Complete list of all references used (github, blog, paper, etc)

```
```

## Troubleshooting

(Option) You can write a Troubleshooting section
