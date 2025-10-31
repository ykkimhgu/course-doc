# LAB: USART - LED, Bluetooth

**Date:** 2025-09-26

**Author/Partner:**

**Github:** repository link

**Demo Video:** Youtube link

**PDF version:**

## Introduction

In this lab, we will learn how to configure and use ‘USART(Universal synchronous asynchronous receiver transmitter)’ of MCU. Then, we will learn how to communicate between your PC and MCU and MCU to another MCU with wired serial communication.

* **Mission 1**: Control LED(LD2) of each other MCU.
* **Mission 2**: Run DC motors with Bluetooth

![](https://user-images.githubusercontent.com/91526930/199908079-a7a06848-2246-43af-a973-03b50a77d8ea.png)

You must submit

* LAB Report (\*.md & \*.pdf)
* Zip source files(main\*.c, ecRCC.h, ecGPIO.h, ecSysTick.c etc...).
  * Only the source files. Do not submit project files

### Requirement

#### Hardware

* MCU
  * NUCLEO-F411RE
* Actuator/Sensor/Others:
  * DC motor, DC motor driver(L9110s),
  * Bluetooth Module(HC-06),

#### Software

* Keil uVision, CMSIS, EC\_HAL library

##

## Preparation

Install the serial monitor: TeraTerm

{% content-ref url="../tutorial/tutorial-usart-with-teraterm.md" %}
[tutorial-usart-with-teraterm.md](../tutorial/tutorial-usart-with-teraterm.md)
{% endcontent-ref %}

## Problem 1: EC HAL library

### Using library

#### Option 1:  Use the given library

Download the following files:&#x20;

* ecUART2.c   &#x20;
* ecUART2.h

#### Option 2:   Create your own library

Download the following files and fill in the empty spaces in the code.:&#x20;

* ecUART2\_exercise\_student.c   &#x20;
* ecUART2\_exercise\_student.h

Then, change their names as   &#x20;

* ecUART2.c   &#x20;
* ecUART2.h

You must update your header files located in the directory `EC\include\`.



**ecUSART2.h**

```cpp
// Configuration UART 1, 2 using default pins
void UART1_init(void);
void UART2_init(void);	
void UART1_baud(uint32_t baud);
void UART2_baud(uint32_t baud);

// USART write & read
void USART1_write(uint8_t* buffer, uint32_t nBytes);
void USART2_write(uint8_t* buffer, uint32_t nBytes);
uint8_t USART1_read(void);										
uint8_t USART2_read(void);	

// RX Interrupt Flag USART1,2
uint32_t is_USART1_RXNE(void);
uint32_t is_USART2_RXNE(void);

// Optional : Advanced Settings
// void USART_init(USART_TypeDef* USARTx, uint32_t baud);  		
// void UART_baud(USART_TypeDef* USARTx, uint32_t baud);	
// Example:
//     For BT serial : specific RX/TX pins 
//  USART_setting(USART1, PA_9,PA_10, BAUD_9600); 	// PA9 - RXD , PA10 - TXD

```

```cpp
```

## Problem 2: Communicate MCU1-MCU2 using RS-232

![](https://user-images.githubusercontent.com/91526930/199908079-a7a06848-2246-43af-a973-03b50a77d8ea.png)

### Procedure

1\. Create a new project under the directory `\repos\EC\LAB\LAB_USART_LED`

* The project name is “**LAB\_USART\_LED”.**
* Create a new source files named as “**LAB\_USART\_LED.c”**

> You MUST write your name on the source file inside the comment section.

2\. Include your updated library in `\repos\EC\include\`  or `\repos\EC\lib\`   to your project.

* **ecUART2.h, ecUART2.c**
* Update  **ecSTM32F4v2.h**

3\. Connect each MCUs to each PC with **USART 2** via USB cable (ST-Link)

* MCU1 to PC1
* MCU2 to PC2

4\. Connect MCU1 to MCU2 with **USART 1**

*   connect RX/TX pins externally with jumper wires as

    * MCU1\_TX to MCU2\_RXD
    * MCU1\_RX to  MCU2\_TX



5. Connect **GND pins** between MCU1-MCU2

{% hint style="info" %}
MUST connect the same GND pin for MCU1 and MCU2
{% endhint %}



6. Send a message from PC\_1 by typing keys on Teraterm. It should send that message from MCU\_1 to MCU\_2.

> Note that you have to press "Enter" to end the message.

7. The received message by MCU\_2 should be displayed on PC\_2.



8. Turn other MCU's LED(LD2) On/OFF by sending text:

* Press  key "**L**" for Turn OFF LED
* Press key "**H**" for Turn ON LED

### Configuration

| Type                            | Port - Pin                   | Configuration                                                       |
| ------------------------------- | ---------------------------- | ------------------------------------------------------------------- |
| System Clock                    |                              | PLL 84MHz                                                           |
| USART2 : USB cable (ST-Link)    |                              | <p>No Parity, 8-bit Data,<br>1-bit Stop bit,<br>38400 baud-rate</p> |
| USART1 : MCU1 - MCU2            | <p>TXD: PA9<br>RXD: PA10</p> | <p>No Parity, 8-bit Data,<br>1-bit Stop bit,<br>38400 baud-rate</p> |
| <p>Digital Out: LD2<br><br></p> | <p>PA5<br><br></p>           |                                                                     |

### Example Code

Example codes for communicating between two USARTs&#x20;

**Example 1 :**   RX/TX **one character** at a time

* Receive  (Interrupt),  Send  (Polling)&#x20;
* Display my key input on PC monitor (myKeyboard-->USART2 --> Serial monitor)
* Display the received message on PC monitor(USART1--> USART2 --> Serial monitor)

**Example 2 :**   RX/TX  **a string**&#x20;

{% tabs %}
{% tab title="Example 1 :  Character comm" %}
```c
#include "ecSTM32F4v2.h"// this should include #include "ecUART2.h" 
//#include "ecUART2.h" 

static volatile uint8_t PC_Data = 0;
static volatile uint8_t BT_Data = 0;
uint8_t PC_string[]="Loop:\r\n";

void setup(void){
	RCC_PLL_init();
	SysTick_init();
	
	// USART2: USB serial init
	UART2_init();
	UART2_baud(BAUD_9600);

	// USART1: BT/MCU2 serial init 
	UART1_init();
	UART1_baud(BAUD_9600);
}

int main(void){	
	setup();
	printf("MCU Initialized\r\n");	
	while(1){
		// USART Receive: Use Interrupt only
		// USART Transmit:  Interrupt or Polling
		USART2_write(PC_string, 7);
		delay_ms(2000);        
	}
}

void USART2_IRQHandler(){          		// USART2 RX Interrupt : Recommended
	if(is_USART2_RXNE()){
		PC_Data = USART2_read();		// RX from UART2 (PC)
		USART2_write(&PC_Data,1);		// TX to USART2	 (PC)	 Echo of keyboard typing
		USART1_write(&PC_Data,1);	
	}
}


void USART1_IRQHandler(){          		// USART2 RX Interrupt : Recommended
	if(is_USART1_RXNE()){
		BT_Data = USART1_read();		// RX from UART1 (BT)		
		printf("RX: %c \r\n",BT_Data); // TX to USART2(PC)
	}
}
```
{% endtab %}

{% tab title="Example 2: String comm" %}
```cpp
#include "ecSTM32F4v2.h"  // this should include #include "ecUART2.h" 

#define MAX_BUF 	10
#define END_CHAR 	13

static volatile uint8_t buffer[MAX_BUF]={0, };
static volatile uint8_t PC_string[MAX_BUF]={0, };
static volatile uint8_t PC_data = 0;

static volatile int idx = 0;
static volatile int bReceive =0;

void setup(void){
	RCC_PLL_init();
	SysTick_init();
	
	// USART2: USB serial init
	UART2_init();
	UART2_baud(BAUD_9600);

	// USART1: BT serial init 
	UART1_init();
	UART1_baud(BAUD_9600);
}

int main(void){	
	setup();
	printf("MCU Initialized\r\n");	
	
	while(1){
		if (bReceive == 1){
			printf("PC_string: %s\r\n", PC_string);				
			bReceive = 0;
		}
	}
}

void USART2_IRQHandler(){          		// USART2 RX Interrupt : Recommended
	if(is_USART2_RXNE()){
		PC_data = USART2_read();		// RX from UART2 (PC)
		USART2_write(&PC_data,1);		// TX to USART2	 (PC)	 Echo of keyboard typing
		
		// Creates a String from serial character receive				
		if(PC_data != END_CHAR && (idx < MAX_BUF)){	
			buffer[idx] = PC_data;
			idx++;
		}
		else if (PC_data== END_CHAR) {						
			bReceive = 1;
			// reset PC_string;
			memset(PC_string, 0, sizeof(char) * MAX_BUF);
			// copy to PC_string;
			memcpy(PC_string, buffer, sizeof(char) * idx);
			// reset buffer
			memset(buffer, 0, sizeof(char) * MAX_BUF);	
			idx = 0;
		}
		else{				//  if(idx >= MAX_BUF)			
			idx = 0;							
			// reset PC_string;
			memset(PC_string, 0, sizeof(char) * MAX_BUF);
			// reset buffer
			memset(buffer, 0, sizeof(char) * MAX_BUF);	// reset buffer
			printf("ERROR : Too long string\r\n");
		}
	}
}
```


{% endtab %}
{% endtabs %}



#### Code

Your code goes here: [ADD Code LINK such as github](https://github.com/ykkimhgu/EC-student/)

Explain your source code with necessary comments.

```c++
// YOUR MAIN CODE ONLY
// YOUR CODE
```

#### Result

Experiment images and results

> Show experiment images /results

Add [demo video link](lab-usart-led-bluetooth.md)

##

***



## Problem 3: Control DC Motor via Bluetooth

![image](https://user-images.githubusercontent.com/38373000/200286160-bc9cd583-8f7e-4dc0-8acd-40135287287d.png)

### Tutorial: Bluetooth

Follow the tutorial for Bluetooth setting.&#x20;

{% content-ref url="../tutorial/tutorial-bluetooth.md" %}
[tutorial-bluetooth.md](../tutorial/tutorial-bluetooth.md)
{% endcontent-ref %}

Search for the bluetooth module specification sheet (HC-06) and study the pin configurations.&#x20;

* The default PIN number is 1234.



Example of connecting to USART1

![image](https://user-images.githubusercontent.com/38373000/200287085-970d9741-d80b-4af1-b3ac-e7b0b0b58913.png)



### Procedure

1\. Create a new project under the directory \`\repos\EC\LAB\LAB\_USART\_Bluetooth

* The project name is “**LAB\_USART\_Bluetooth”.**
* Create a new source files named as “**LAB\_USART\_Bluetooth.c”**

> You MUST write your name on the source file inside the comment section.

2\. Include your updated library in `\repos\EC\include\`  or `\repos\EC\lib\`   to your project.

* **ecUART2.h, ecUART2.c**
* Update  **ecSTM32F4v2.h**



3\. Connect the MCU - BT module. Use USART 1

* Connect RX/TX pins as
  * MCU TX - BT RX
  * MCU RX - BT TX

4\. Connect the BT module (HC-06) in PC's bluetooth setting.



5. Check the Bluetooth connection by turning MCU's LED(LD2) ON/OFF by sending text of "**0**" or "**1**" from PC.



6. Connect 2 x DC motors to MCU/Motor Driver



7. Control motor by the Keyboard inputs

* Press 'J' :  To turn Left
  * MotorA / MotorB = (50 / 80%) duty
* Press 'L' : Turn Right
  * MotorA / MotorB = (80 / 50%) duty
* Press 'I' : Go straight
  * MotorA / MotorB = (80 / 80 %) duty
* Press 'K' : STOP
  * MotorA / MotorB = (0 / 0 %) duty

You may use other the key inputs as your preference.



### Configuration

| Type                                             | Port - Pin                   | Configuration                                                      |
| ------------------------------------------------ | ---------------------------- | ------------------------------------------------------------------ |
| **System Clock**                                 |                              | PLL 84MHz                                                          |
| **USART1 : MCU -Bluetooth**                      | <p>TXD: PA9<br>RXD: PA10</p> | <p>No Parity, 8-bit Data,<br>1-bit Stop bit,<br>9600 baud-rate</p> |
| <p><strong>Digital Out: LD2</strong><br><br></p> | <p>PA5<br><br></p>           |                                                                    |
| **PWM (Motor A)**                                | TIM2-Ch1                     | PWM period (2kHz\~10kHz)                                           |
| **PWM (Motor B)**                                | TIM2-Ch2                     |                                                                    |

#### Code

Your code goes here: [ADD Code LINK such as github](https://github.com/ykkimhgu/EC-student/)

Explain your source code with necessary comments.

```c++
// YOUR MAIN CODE ONLY
// YOUR CODE
```

#### Result

Experiment images and results

> Show experiment images /results

Add [demo video link](lab-usart-led-bluetooth.md)

## Reference

Complete list of all references used (github, blog, paper, etc)

```
```

## Troubleshooting

### 1. Cannot find my bluetooth module in my PC

Change your BT searching setting in your Window

![image](https://github.com/ykkimhgu/course-doc/assets/38373000/0dd5911d-d88c-41fa-aac3-568d5c4d8f42)
