# LAB: USART - LED, Bluetooth



**Date:** 2023-09-26

**Author/Partner:**

**Github:** repository link

**Demo Video:** Youtube link

**PDF version:**&#x20;



## Introduction

In this lab, we will learn how to configure and use ‘USART(Universal synchronous asynchronous receiver transmitter)’ of MCU. Then, we will learn how to communicate between your PC and MCU and MCU to another MCU with wired serial communication. 

- **Mission 1**: Control LED(LD2) of each other MCU.

- **Mission 2**:  Run DC motors with Bluetooth

![](https://user-images.githubusercontent.com/91526930/199908079-a7a06848-2246-43af-a973-03b50a77d8ea.png)

You must submit

* LAB Report (\*.md & \*.pdf)
* Zip source files(main\*.c, ecRCC.h, ecGPIO.h, ecSysTick.c   etc...).
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



## Problem 0: STM-Arduino



### Procedure

1) Create a new project under the directory `\EC\LAB\LAB_USART_LED`



2)  Follow the tutorial: **U(S)ART (Universal Asynchronous/synchronous Receiver and Transmitter)**

https://ykkim.gitbook.io/ec/ec-course/tutorial/tutorial-arduino-stm32/tutorial-arduino-stm32-part-2#u-s-art-universal-asynchronous-synchronous-receiver-and-transmitter





## Problem 1: EC  HAL library

### Using  HAL library

Download [ecUART_student.c ecUART_student.h](https://github.com/ykkimhgu/EC-student/blob/main/include/lib-student)

Change the file names as 

* ecUART.c, ecUART.h

  

Fill in empty spaces in the code. 

You must update your header files located in the directory `EC \lib\`.  



**ecUSART.h**

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


```



### Example Code



**Example 1**

```cpp
#include "stm32f4xx.h"
#include "ecGPIO.h"
#include "ecRCC.h"
#include "ecUART.h"
#include "ecSysTick.h"


static volatile uint8_t PC_Data = 0;
static volatile uint8_t BT_Data = 0;
uint8_t PC_string[]="Loop:\r\n";

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
	}
}


void USART1_IRQHandler(){          		// USART2 RX Interrupt : Recommended
	if(is_USART1_RXNE()){
		BT_Data = USART1_read();		// RX from UART1 (BT)		
		printf("RX: %c \r\n",BT_Data); // TX to USART2(PC)
	}
}
```

**Example 2**

```cpp
#include "stm32f4xx.h"
#include "ecGPIO.h"
#include "ecRCC.h"
#include "ecUART.h"
#include "ecSysTick.h"

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



**General USART Setup**

```cpp
// General Setting
void setup(){
  RCC_PLL_init();
  
  // BT serial : specific RX/TX pins 
  USART_setting(USART1, GPIOA,9,GPIOA,10, BAUD_9600); 	// PA9 - RXD , PA10 - TXD
}

```









## Problem 2: Communicate  MCU1-MCU2  using  RS-232

### Procedure

1\. Create a new project under the directory `\repos\EC\LAB\LAB_USART_LED`

* The project name is “**LAB_USART_LED”.**

* Create a new source files named as “**LAB_USART_LED.c”** 

> You MUST write your name on the source file inside the comment section. 



2\. Include your updated library in `\repos\EC\lib\`  to your project.

*  **ecGPIO.h, ecGPIO.c**
*  **ecRCC.h, ecRCC.c**
*  **ecUART.h, ecUART.c** 
*  and other necessary header files



3\. Connect each MCUs to each PC with **USART 2** via USB cable (ST-Link)

* MCU1-PC1,    MCU2-PC2



4\. Connect MCU1 to  MCU2 with **USART 1**

- connect RX/TX pins externally as
  - MCU1_TX  to MCU2_RXD
  - MCU1_RX - MCU2_TX

  > Connect the same GND for  MCU1-MCU2 



5. Send a message from PC_1 by typing keys on Teraterm.  It should send that message from MCU_1 to MCU_2.

   > Note that you have to press "Enter" to end the message.

6. The received message by MCU_2 should be displayed on PC_2. 

   

7\. Turn other MCU's LED(LD2) On/OFF by sending text: 

*   "**L**" for Turn OFF
*  "**H**" for Turn ON



### Configuration

| Type                         | Port - Pin              | Configuration                            |
| ---------------------------- | ----------------------- | ---------------------------------------- |
| System Clock                 |                         | PLL 84MHz                                |
| USART2 : USB cable (ST-Link) |                         | No Parity, 8-bit Data,<br />1-bit Stop bit,<br />38400 baud-rate |
| USART1 : MCU1 - MCU2         | TXD: PA9<br />RXD: PA10 | No Parity, 8-bit Data,<br />1-bit Stop bit,<br />38400 baud-rate |
| Digital Out: LD2<br /><br /> | PA5<br /><br />         |                                          |





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

Add [demo video link]()





## Problem 3: Control DC Motor via Bluetooth

### Bluetooth

![image](https://user-images.githubusercontent.com/38373000/200286160-bc9cd583-8f7e-4dc0-8acd-40135287287d.png)



Search for the bluetooth module specification sheet (HC-06) and study the pin configurations. The default PIN number is 1234.

Example of connecting to USART1

![image](https://user-images.githubusercontent.com/38373000/200287085-970d9741-d80b-4af1-b3ac-e7b0b0b58913.png)



### DC Motor Driver



Connect DC motor driver(L9110s) module pins to MCU as shown below. 

DO NOT use MCU’s VCC to motor driver. You should use external voltage source.

* A- IA: PWM pin (0~100% duty) for Motor A

* A- IB: Direction Pin (Digital Out H or L) for Motor B

![image](https://user-images.githubusercontent.com/38373000/200288512-5a3d57a2-6d98-410b-bf02-57646cde6fd4.png)



### Procedure

1\. Create a new project under the directory `\repos\EC\LAB\LAB_USART_Bluetooth

* The project name is “**LAB_USART_Bluetooth”.**

* Create a new source files named as “**LAB_USART_Bluetooth.c”**

> You MUST write your name on the source file inside the comment section. 



2\. Include your updated library in `\repos\EC\lib\`  to your project.

*  **ecGPIO.h, ecGPIO.c**
*  **ecRCC.h, ecRCC.c**
*  **ecUART.h, ecUART.c** 
*  **ecTIM.h, ecTIM.c** 



3\. Connect the MCU to  PC via Bluetooth.   Use USART 1

- connect RX/TX pins as

  - MCU TXD - BLUE  RXD

  - MCU RXD - BLUE  TXD


4. Check the Bluetooth connection by turning  MCU's LED(LD2) On/OFF by sending text of "**L0**" or "**L1**" from PC.



5. Run 2 DC motors(Left-wheel, Right-wheel) to steer. 

   *  Turn Left:  MotorA  /  MotorB  =  (50 / 80%) duty

   *  Turn Right:  MotorA /  MotorB  =  (80 / 50%) duty

   *  Go straight:  MotorA /  MotorB  =  (80 / 80 %) duty
   *  STOP:  MotorA /  MotorB  =  (0 / 0 %) duty


    You may use  the key inputs as your preference for Left, Right, Straight. 

   \-    Ex)  ‘L’, ‘R’, 'U' 'S'  



### Configuration

| Type                         | Port - Pin              | Configuration                            |
| ---------------------------- | ----------------------- | ---------------------------------------- |
| System Clock                 |                         | PLL 84MHz                                |
|                              |                         |                                          |
| USART1 : MCU -Bluetooth      | TXD: PA9<br />RXD: PA10 | No Parity, 8-bit Data,<br />1-bit Stop bit,<br />9600 baud-rate |
| Digital Out: LD2<br /><br /> | PA5<br /><br />         |                                          |
| PWM (Motor A)                | TIM2-Ch1                | PWM period (2kHz~10kHz)                  |
| PWM (Motor B)                | TIM2-Ch2                |                                          |







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

Add [demo video link]()







## Reference

Complete list of all references used (github, blog, paper, etc)

```

```



## Troubleshooting

### 1. Cannot find my bluetooth module in my PC
Change your BT searching setting in your Window

![image](https://github.com/ykkimhgu/course-doc/assets/38373000/0dd5911d-d88c-41fa-aac3-568d5c4d8f42)



