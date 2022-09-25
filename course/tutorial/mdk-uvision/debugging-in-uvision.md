# Tutorial: Keil uVision Debugging

## Overview

In this tutorial, we will learn how to use uVision for debugging firmware programming. 



### Preparation

- Open your previous tutorial project or can create a new project. 
- You can also copy the given example "TU_uVision_Debugging.c" [source code here](https://github.com/ykkimhgu/EC-student/tree/main/tutorial/tutorial-student)
- You will learn how to use debugging mode to check how the registers change

```c++
#include "stm32f4xx.h"
#define LED_PIN    5		//LD2
#define BUTTON_PIN 13


void RCC_HSI_init(void);   							//defined in ecRcc.h
void RCC_GPIOA_enable(void);
void RCC_GPIOC_enable(void);

int main(void) {	
	/* Part 1. RCC GPIOA Register Setting */
		RCC_GPIOA_enable();
		RCC_GPIOC_enable();
		
	/* Part 2. GPIO Register Setting for OUTPUT*/			
		// GPIO Mode Register
		GPIOA->MODER &= ~(3UL<<(2*LED_PIN)); // Clear '00' for Pin 5
		GPIOA->MODER |=   1UL<<(2*LED_PIN);  // Set '01' for Pin 5
		
		// GPIO Output Type Register  
		GPIOA->OTYPER &= ~(1UL<<LED_PIN);   	// 0:Push-Pull   
		
		// GPIO Pull-Up/Pull-Down Register 
		GPIOA->PUPDR  &= ~(3UL<<(2*LED_PIN)); // 00: none
		
		// GPIO Output Speed Register 
		GPIOA->OSPEEDR &= ~(3UL<<(2*LED_PIN));
		GPIOA->OSPEEDR |=   2UL<<(2*LED_PIN);  //10:Fast Speed
	
	
	/* Part 3. GPIO Register Setting for INPUT*/			
		// GPIO Mode Register
		GPIOC->MODER &= ~(3UL<<(2*BUTTON_PIN)); // 00: Input	 		
   
		// GPIO Pull-Up/Pull-Down Register 
		GPIOC->PUPDR &= ~(3UL<<(2*BUTTON_PIN)); 
		GPIOC->PUPDR  |= 2UL<<(2*BUTTON_PIN); 	// 10: Pull-down		    
	 
	 
	/* Part 4. Deal loop  */	
		while(1){
			unsigned int btVal=0;
			//Read bit value of Button
			btVal=(GPIOC->IDR) & (1UL << BUTTON_PIN);	
			if(btVal == 0)
				GPIOA->ODR |= (1UL << LED_PIN);	 		
			else
				GPIOA->ODR &= ~(1UL << LED_PIN); 
		}
}



void RCC_GPIOA_enable()
{
		// HSI is used as system clock         
		RCC_HSI_init();	
	
		// RCC Peripheral Clock for GPIO_A Enable 
		RCC->AHB1ENR |= RCC_AHB1ENR_GPIOAEN;
}

void RCC_GPIOC_enable()
{
		// HSI is used as system clock         
		RCC_HSI_init();	
	
		// RCC Peripheral Clock for GPIO_A Enable 
		RCC->AHB1ENR |= RCC_AHB1ENR_GPIOCEN;
}


void RCC_HSI_init() {
	// Enable High Speed Internal Clock (HSI = 16 MHz)
  RCC->CR |= ((uint32_t)RCC_CR_HSION);

  // wait until HSI is ready
  while ( (RCC->CR & (uint32_t) RCC_CR_HSIRDY) == 0 ) {;}
	
  // Select HSI as system clock source 
  RCC->CFGR &= (uint32_t)(~RCC_CFGR_SW); 									
  RCC->CFGR |= (uint32_t)RCC_CFGR_SW_HSI; 				
			
	// Wait till HSI is used as system clock source
  while ((RCC->CFGR & (uint32_t)RCC_CFGR_SWS) != 0 ); 
}
```



## Software vs Hardware Debug

- There are two methods to debug your program: software debug and hardware debug. 
  - Software Debug: you do not need to connect the MCU to PC to debug a software program. 
  - Hardware Debug: MCU board must be connected to the computer.
- For this tutorial, we will use Hardware Debugging.

![image](https://user-images.githubusercontent.com/91526930/192132991-0ab53fe9-3f23-4af1-85fb-b153fc5eb425.png)





## Debug Control

### Compile, Debug, and Run

- You can program the STM32 flash by clicking the LOAD button ![](https://user-images.githubusercontent.com/91526930/192132617-1cf8ef6c-f6cb-4be8-b35c-d5748253eb19.png). 
- Click the debug button ![image](https://user-images.githubusercontent.com/91526930/192132643-c8628ae6-4c0c-4e95-91b2-d881bbd85f3e.png) to start the debug and click it again to exit the debug. 
- You can use the breakpoint button ![](https://user-images.githubusercontent.com/91526930/192132654-5319d623-ed32-461e-b39c-b45709ce2033.png) to set a break point in either disassembly or source windows. 
- STM32 allows up to six breakpoints during hardware debugging. When a program stops at a breakpoint, the corresponding instruction has not been executed yet.
- You can choose either debugging *assembly code* or *C-code* line by choosing the disassembly or source window in focus.

![image](https://user-images.githubusercontent.com/91526930/192132687-f01c0e20-9e41-45ac-8f72-b9ddbd29dbc9.png)



- Control buttons of debugging

  ![image](https://user-images.githubusercontent.com/91526930/192132759-5ce23e5a-e89f-4864-ab21-64095b4608e0.png)

  - **Run (F5)**: Continues the execution from the current position until you click ***Stop*** or the program is paused by a breakpoint.
  - **Step In (F11)**: Execute one step and enter the function if the current step calls a function.
  - **Step Out (Ctrl + F11)**: Execute until the current function returns.
  - **Step Over (F10)**: Execute one step and run the function all at once if the current step calls a function.



### Pheripheral Registers

- Choose the menu: **Peripherals** **⟶** **System Viewer**
- View and update the control and data registers of all available peripherals
- Check Registers for GPIO Port A (PA5 for LED). 
- Check the value of Output Data Register (ODR) ODR5 and LED status
- Check and uncheck ODR5 with mouse click and see how LED turns on/off

![image](https://user-images.githubusercontent.com/91526930/192132973-52e62109-baf9-41e5-9bbb-ab235524a219.png)

- Check Registers for GPIO Port C (PC_13 for Button B1). 
- Check the value of Input Data Register (IDR) IDR13 
- Keep pressing ‘F10’ while B1 button is pressing and unpressing. Check the value of IDR13. 
- Now, check other peripheral registers such as RCC.



### Processor Registers

#### Core Registers

- Program counter (PC) r15 holds the memory address (location in memory) of the next  instruction to be fetched from the instruction memory.

- Stack point (SP) r13 holds a memory address that points to the top of the stack. SP is a shadow of either MSP or PSP. 
- xPSR  (Special-purpose program status registers) is a combination of the following three processor status registers:
  - Application PSR
  - Interrupt PSR
  - Execution PSR

| xPSR | Description                                            |
| ---- | ------------------------------------------------------ |
| N    | Negative or less than  flag (1 = result negative)      |
| Z    | Zero flag (1 = result 0)                               |
| C    | Carry or borrow flag (1 =  Carry true or borrow false) |
| V    | Overflow flag (1 =  overflow)                          |
| Q    | Q Sticky saturation flag                               |
| T    | Thumb state bit                                        |
| IT   | If-Then bits                                           |
| ISR  | ISR Number (6 bits)                                    |

![image](https://user-images.githubusercontent.com/91526930/192133259-eb544af9-3901-490c-bb51-7137134d97d7.png)