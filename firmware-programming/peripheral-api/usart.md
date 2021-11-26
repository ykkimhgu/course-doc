# USART

## EC\_HAL  for USART

{% embed url="https://github.com/ykkimhgu/EC-student/tree/main/tutorial-student" %}

### Header File

`ecUSART.h`

```cpp
#ifndef __EC_USART_H
#define __EC_USART_H

#include "stm32f411xe.h"
#include <stdio.h>
#include "ecGPIO.h"
#include "ecRCC.h"

#define POL 0
#define INT 1

// You can modify this
#define BAUD_9600	0
#define BAUD_19200	1
#define BAUD_57600	2
#define BAUD_115200 	3
#define BAUD_921600 	4


// ********************** USART 2 (USB) ***********************
	// PA_2 = USART2_TX
	// PA_3 = USART2_RX
	// Alternate function(AF7), High Speed, Push pull, Pull up
	// APB1
// **********************************************************

// ********************** USART 1 ***************************
	// PB_6 = USART1_TX (default)	// PA_9  (option)
	// PB_3 = USART1_RX (default)	// PA_10 (option)
	// APB2
// **********************************************************

// ********************** USART 6 ***************************
	// PA_11 = USART6_TX (default)	// PC_6  (option)
	// PA_12 = USART6_RX (default)	// PC_7 (option)
	// APB2
// **********************************************************


/* Given to Students */ 
void UART2_init();
void USART_write(USART_TypeDef* USARTx, uint8_t* buffer, uint32_t nBytes);
void USART_delay(uint32_t us);  

/* Exercise*/
void USART_begin(USART_TypeDef* USARTx, GPIO_TypeDef* GPIO_TX, int pinTX, GPIO_TypeDef* GPIO_RX, int pinRX, int baud);
void USART_init(USART_TypeDef* USARTx, int baud);  															// default pins. 
uint8_t USART_getc(USART_TypeDef * USARTx);										
uint32_t is_USART_RXNE(USART_TypeDef * USARTx);
										

#endif
```

####

## Example Code&#x20;

### HAL: USART example&#x20;

```cpp
/**
******************************************************************************
* @author  SSSLAB
* @Mod		 2021-8-12 by YKKIM
* @brief   Embedded Controller:  TUTORIAL : Usart serial communication
*
******************************************************************************
*/

#include "stm32f4xx.h"
#include "ecGPIO.h"
#include "ecRCC.h"
#include "ecUART.h"

uint8_t TX_Data = 0;
uint8_t PC_Data = 0;

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
	USART_init(USART2, 38400);
	USART_begin(USART1, GPIOA,9,GPIOA,10, 9600); 	// PA9 - RXD , PA10 - TXD
}


void USART2_IRQHandler(){         //USART2 INT 
	if(USART2->SR & USART_SR_RXNE){
		PC_Data = USART_getc(USART2);
		USART_write(USART1,&PC_Data,1);
		printf("NUCLEO1 got : %c \r\n",PC_Data);
	}
}


```

#### Tutorial: SysTick Initiation
