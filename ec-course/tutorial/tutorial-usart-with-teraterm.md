# Tutorial: USART with TeraTerm

### **U(S)ART (Universal Asynchronous/synchronous Receiver and Transmitter)**

We are going to create a simple program that links MCU-PC via UART communication. MCU can receive and transmit 8-bit character data through UART communication.

NUCLEO-F401RE board offers UART2 channel with USB connector.



## Installing Serial Monitor

[Download 'TeraTerm'](https://github.com/TeraTermProject/teraterm/releases)\


Select the lastest version (5.x ) .  Download the release version (\*.exe) file and install

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

After installation, open ‘Tera Term’  and make **New Connection.**



Choose ‘**Serial**’ tab -> Select ‘**COMx: STMicroelectronics STLink**’ port

> COMx, x is the port number and it can be different for each connection.

![teraterm](https://user-images.githubusercontent.com/79825525/129156752-893e425d-1653-496f-a4fa-13cbebe2a271.png)

**Setup(설정) > SerialPort**

* check the baud rate is set as **9600** \[bps] and data is **8-bit**
* use the default configuration for the other options.

![teraterm2](https://user-images.githubusercontent.com/79825525/129156774-2bfe2509-d5e2-4ba1-b3bc-b06d53dacd52.png)



## Test Code

### HAL Library- USART

First, download the provided USART libray header files

* [ecUART2\_simple\_student.h](https://github.com/ykkimhgu/EC-student/blob/main/include/lib-student/ecUART2\_simple\_student.h)
* [ecUART2\_simple\_student.c](https://github.com/ykkimhgu/EC-student/blob/main/include/lib-student/ecUART2\_simple\_student.c)



#### Code 1

A simple code that prints a message from MCU to PC.

After you flash the code, press the MCU RESET button(black button).

```c
#include "stm32f4xx.h"
#include <stdio.h>
#include "ecGPIO2.h"
#include "ecRCC2.h"
#include "ecUART2_simple.h"


void setup(void);

int main(void) {
	// Initialiization --------------------------------------------------------
	setup();
	printf("Hello Nucleo\r\n");
	// Inifinite Loop ----------------------------------------------------------
	while (1);
}

// Initialiization 
void setup(void)
{
	RCC_PLL_init();
	USART_init(USART2, 9600);
}
```



#### Code 2

The following test code echos the pressed input key (from PC) back to the PC (TeraTerm  display).

If you press any key on the Tera Term window, the MCU will receive it and then transmit the key back to the PC immediately. Thus, you can see what key you are pressing on the Tera Term window display.

Here, it uses USART Receive Interrupt handler.

```c
#include "stm32f4xx.h"
#include <stdio.h>
#include "ecGPIO2.h"
#include "ecRCC2.h"
#include "ecUART2_simple.h"

#define END_CHAR 13
uint8_t pcData = 0;

void setup(void);

int main(void) {
	// Initialiization --------------------------------------------------------
	setup();
	printf("Hello Nucleo\r\n");
	// Inifinite Loop ----------------------------------------------------------
	while (1);
}

// Initialiization 
void setup(void)
{
	RCC_PLL_init();
	USART_init(USART2, 9600);
}

void USART2_IRQHandler(){         //USART2 INT 
	if(is_USART_RXNE(USART2)){
		pcData = fgetc(USART2);
		USART_write(USART1, &pcData, 1);
		printf("%c", pcData);	
		if (pcData == END_CHAR)
			printf("\r\n");
	}
}
```



### Arduino

The following test code echos the pressed input key (from PC) back to the PC (TeraTerm  display).

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

