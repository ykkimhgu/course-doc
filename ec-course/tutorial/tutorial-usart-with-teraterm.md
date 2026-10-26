# Tutorial: USART with TeraTerm

### **U(S)ART (Universal Asynchronous/synchronous Receiver and Transmitter)**

We are going to create a simple program that links MCU-PC via UART communication. MCU can receive and transmit 8-bit character data through UART communication.

NUCLEO-F401RE board offers UART2 channel with USB connector.

## Installing Serial Monitor

[Download 'TeraTerm'](https://github.com/TeraTermProject/teraterm/releases)

Select the lastest release version (\*.exe) file and install

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

After installation, open ‘Tera Term’ and make **New Connection.**

Choose ‘**Serial**’ tab -> Select ‘**COMx: STMicroelectronics STLink**’ port

> COMx, x is the port number and it can be different for each connection.

![teraterm](https://user-images.githubusercontent.com/79825525/129156752-893e425d-1653-496f-a4fa-13cbebe2a271.png)

**Setup(설정) > SerialPort**

* Check the baud rate is set as **9600** \[bps] and data is **8-bit**
* Use the default configuration for the other options.

![teraterm2](https://user-images.githubusercontent.com/79825525/129156774-2bfe2509-d5e2-4ba1-b3bc-b06d53dacd52.png)

## Tutorial Code

### Arduino

The following test code echos the pressed input key (from PC) back to the PC (TeraTerm display).

If you press any key on the Tera Term window, the MCU will receive it and then transmit the key back to the PC immediately. Thus, you can see what key you are pressing on the Tera Term window display.

```c

char keyIn;

void setup() {
  Serial.begin(9600);
  Serial.print("Hello Nucleo\r\n");
}

void loop() {
  if (Serial.available() > 0){
    keyIn = Serial.read();
    if (keyIn == '\n')
      Serial.print("\r\n");
    else if (keyIn)
      Serial.print(keyIn);
  }
}
```

### EC STM Library- USART

First, download the provided USART libray header files

* [ecUART2\_student.h](https://github.com/ykkimhgu/EC-student/blob/main/include/lib-student/)
* [ecUART2\_student.c](https://github.com/ykkimhgu/EC-student/blob/main/include/lib-student/)

> You may have to include the following macro in GPIO2.h
>
> {% code expandable="true" %}
> ```
> ```
> {% endcode %}

> ```c
> #define EC_AF  		2
>
> #define EC_NONE 0
> #define EC_PU 	1
> #define EC_PD 	2
>
> #define EC_PUSH_PULL 	0
>
> #define EC_LOW 		0
> #define EC_HIGH 	3
> ```

#### Example Code 1

A simple code that prints a message from MCU to PC.

* Method 1: use `printf()`
* Method 2: use `USART2_write()`

After you flash the code, press the MCU RESET button(black button).

> If nothing shows on serial monitor, disconnect the USB cable then reconnect.

Open the terminal in TeraTerm and check if you can see the text on the screen.

> If you do not see any text, disconnect and reconnect the USB cable to the MCU.

```c
/*----------------------------------------------------------------\
@ Embedded Controller by Young-Keun Kim - Handong Global University
Author           : [ YOUR NAME GOES HERE !!!!!]
Created          : 10-08-2026
Modified         : 00-00-2026 [WRITE THE DATE!!!!]
Language/ver     : C in VS Code

Description      : Tutorial - USART2 with the STUDENT library (ecUART2_student)
/----------------------------------------------------------------*/
#include <stdio.h>
#include <string.h>

#include "stm32f4xx.h"
#include "ecGPIO2.h"
#include "ecRCC2.h"
#include "ecSysTick2.h"
#include "ecUART2_student.h"

uint8_t PC_string[] = "Hello Handong\r\n";
uint8_t BT_string[20]={0};

uint32_t loop=0;

// Initialiization 
void setup(void)
{
	RCC_PLL_init();
	SysTick_init();
	USART2_init();
}

int main(void) {
	// Initialiization --------------------------------------------------------
	setup();

	// Method 1 : printf() 
	printf("MCU Initialized\r\n");

	// Method 2 : USART_write() 
	USART2_write(PC_string, sizeof(PC_string));

	// Inifinite Loop ----------------------------------------------------------
	while (1){
		// Method 3 :  Combination of string and number
		int n = sprintf((char*)BT_string, "Loop: %lu >> \r\n", loop);  
		USART2_write(BT_string, n);		
		loop++;
		delay_ms(2000);  
	}
}

```

#### Example Code 2

The following test code echos the pressed input key (from PC) back to the PC (TeraTerm display).

If you press any key on the Tera Term window, the MCU will receive it and then transmit the key back to the PC immediately. Thus, you can see what key you are pressing on the Tera Term window display.

Here, it uses USART2 Receive Interrupt handler.

```c
/*----------------------------------------------------------------\
@ Embedded Controller by Young-Keun Kim - Handong Global University
Author           : [ YOUR NAME GOES HERE !!!!!]
Created          : 10-08-2026
Modified         : 00-00-2026 [WRITE THE DATE!!!!]
Language/ver     : C in VS Code

Description      : Tutorial - USART2 with the STUDENT library (ecUART2_student)
/----------------------------------------------------------------*/
#include <stdio.h>
#include "stm32f4xx.h"
#include "ecRCC2.h"
#include "ecSysTick2.h"
#include "ecGPIO2.h"
#include "ecUART2_student.h"


uint8_t PC_data = 0;

// Initialiization 
void setup(void){	
	RCC_PLL_init();
	SysTick_init();
	// USART2: USB serial init
	USART2_init();
}

int main(void) {
	// Initialiization --------------------------------------------------------
	setup();
	printf("MCU Initialized\r\n");
	// Inifinite Loop ----------------------------------------------------------
	while(1){		
		delay_ms(2000);        
	}
}


// ECHO keyboard inputs
void USART2_IRQHandler(){        // USART2 RX Interrupt : Recommended
	if(is_USART2_RXNE()){
		PC_data = USART2_read();		// RX from UART2 (PC)
		USART2_write(&PC_data,1);		// TX to USART2	 (PC)	 Echo of keyboard typing		
	}
}
```

###
