# Tutorial: Bluetooth

## Bluetooth (HC-06)

![HC-06](https://user-images.githubusercontent.com/91526930/199959099-ce223951-62f6-4177-8f3f-69bff56295e9.png)

## Procedure

1\. Connect the bluetooth module(HC-06) and the MCU. For using USART1, select PA9(TX) and PA10(RX).

![Connection](https://user-images.githubusercontent.com/91526930/199960746-056ea90d-091d-411d-8b5f-0b9321dfbcdc.png)



2\. Search and add the bluetooth named "HC-06(n)". (n = 01, 02, ... )

![image](https://user-images.githubusercontent.com/91526930/199961779-e1b33008-2f27-4e84-8332-af3ea6f9cb55.png)





3\. Enter the password "1234".

![password](https://user-images.githubusercontent.com/91526930/199961977-bea93fb8-d841-4403-963b-2afb834e59e7.png)





4\. If it's connected normally, you can connect the serial port with Teraterm.

![Serial Port](https://user-images.githubusercontent.com/91526930/199962246-89a14a38-9a3f-4962-9f6d-a62260e11cab.png)



5\. Run the example code "TU_USART_Bluetooth_student.c" [code link](https://github.com/ykkimhgu/EC-student/tree/main/tutorial/tutorial-student)

```c++
#include "stm32f4xx.h"
#include "ecGPIO.h"
#include "ecRCC.h"
#include "ecUART.h"

#define END_CHAR 13
#define MAX_BUF 100

uint8_t pcData = 0;
uint8_t btData = 0;
uint8_t buffer[MAX_BUF] = {0, };
int bReceive = 0;
int idx = 0;

void setup(void);

int main(void) {
	// Initialiization --------------------------------------------------------
	setup();
	printf("Hello Nucleo\r\n");
	USART_write(USART1,(uint8_t*) "Hello bluetooth\r\n",17);
	// Inifinite Loop ----------------------------------------------------------
	while (1){
				
	}
			
}

// Initialiization 
void setup(void)
{
	RCC_PLL_init();
	USART_init(USART2, 9600);
	USART_begin(USART1, GPIOA, 9, GPIOA, 10, 9600); 	// PA9: TXD , PA10: RXD
}

void USART1_IRQHandler(){         //USART1 INT 
	if(is_USART_RXNE(USART1)){
		btData = USART_getc(USART1);
		USART_write(USART1,(uint8_t*) "BT sent : ", 10);
		USART_write(USART1, &btData, 1);
		USART_write(USART1, "\r\n", 2);
		printf("NUCLEO got : %c (from BT)\r\n",btData);
	}
}


void USART2_IRQHandler(){         //USART2 INT 
	if(is_USART_RXNE(USART2)){
		pcData = USART_getc(USART2);
		USART_write(USART1, &pcData, 1);
		//printf("Nucleo got : %c \r\rn", pcData);
		
		printf("%c", pcData);
		
		if (pcData == END_CHAR)
			printf("\r\n");
		
	}
}
```

