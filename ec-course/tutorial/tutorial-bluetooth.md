# Tutorial: Bluetooth

## Bluetooth (HC-06)

![HC-06](https://user-images.githubusercontent.com/91526930/199959099-ce223951-62f6-4177-8f3f-69bff56295e9.png)

## Procedure

1\. Connect the bluetooth module(HC-06) and the MCU.

&#x20;For using USART1, select PA9(TX) and PA10(RX).

* BT(TX) -  PA10(RX)
* BT(RX)-  PA9(TX)

![Connection](https://user-images.githubusercontent.com/91526930/199960746-056ea90d-091d-411d-8b5f-0b9321dfbcdc.png)

2\. Search and add the bluetooth named "HC-06(n)". (n = 01, 02, ... )

![image](https://user-images.githubusercontent.com/91526930/199961779-e1b33008-2f27-4e84-8332-af3ea6f9cb55.png)

3\. Enter the password "1234".

![password](https://user-images.githubusercontent.com/91526930/199961977-bea93fb8-d841-4403-963b-2afb834e59e7.png)

4\. If it's connected normally, you can connect the serial port with Teraterm.

![Serial Port](https://user-images.githubusercontent.com/91526930/199962246-89a14a38-9a3f-4962-9f6d-a62260e11cab.png)

**Sample Code**

* Assumes you have `ecUART2.h` &#x20;

```c
#include "ecSTM32F4v2.h"
// this should include #include "ecUART2.h" 
//#include "ecUART2.h" 

static volatile uint8_t PC_Data = 0;
static volatile uint8_t BT_Data = 0;
uint8_t PC_string[]="Hi_BT\r\n";

void setup(void){
	RCC_PLL_init();
	SysTick_init();
	
	// USART1: BT serial init 
	UART1_init();
	UART1_baud(BAUD_9600);
}

int main(void){	
	setup();
	//printf("MCU Initialized\r\n");	
	while(1){
		// USART Receive: Use Interrupt only
		// USART Transmit:  Interrupt or Polling
		USART1_write(PC_string, 7);
		delay_ms(2000);        
	}
}

```
