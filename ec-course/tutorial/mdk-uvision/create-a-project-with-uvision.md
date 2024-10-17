# Tutorial: Create a Project with uVision

## 1. **Open Keil 𝝁Vision IDE** and Create a new Project

**Project > New 𝝁Vision Project**

Create this tutorial project under the folder `..\..\repos\EC\tutorial\`

Name the project as **TU\_CreateProject**.

* Check you have a new folder named as `..\..\repos\EC\tutorial\TU_CreateProject\`

> Do not use 한글경로 (띄어쓰기) for Project Directory path

![](<../../../.gitbook/assets/image (10) (1).png>)

## 2. Project Setting

### **Select Device for Target**

**Device >** search for _STM32F411RETx_

> If you use other board, STM32F411 etc, choose the appropriate device.

![](<../../../.gitbook/assets/image (42).png>)

### **Manage Run-Time Environment**

Select CMSIS>CORE , Device>Setup

> This will use necessary library to start the MCU and GPIO drivers.

Check if the following startup codes are included under **Device** folder

‘startup\_stm32f411xe.s’, ‘system\_stm32f411xx.c’

![](<../../../.gitbook/assets/image (4) (1) (1).png>)

### Setup in Options for Target

\*\*Project> Options for Target (Alt+F7) \*\*

1. **Output Tab >** check on **Create HEX File** :

> This will create HEX file that contains the machine instruction codes

![](<../../../.gitbook/assets/image (9) (1).png>)

2. **C/C++ Tab>** Version of C and C++ should be \<default>

![image](https://github.com/user-attachments/assets/1f949f97-f3d0-4495-9c7f-664c40caa9be)

3. **Linker Tab**> Use Memory Layout from Target Dialog checked

> This will use the memory(register) layout of the specific target board

![](<../../../.gitbook/assets/image (36).png>)

4. **Debug tab>** Use: **ST-Link Debugger > Settings**

> You must connect MCU (internal ST-Link) to PC for this setting.

* Use: **ST-LInk Debugger**
* Debug Adapter> Unit: **ST-LINK/V2-1**
* Debug> Connect: **under Reset**

> This will configure USB link to MCU hardware. It will use ST-Link debugger embedded on the target board to debug the program. You will need to connect the target board to your PC for debugging

![](<../../../.gitbook/assets/image (38).png>)

## 3. Create main program

**Project Tab> Target1> Source Group1** (Right Click)  : **Add New item to Group**

Name the source file as `TU_CreateProject_Example_main.c`

> 경로, 폴더명에 한글 경로 사용하면 안됨!!
>
> 특히, Window User 이름이 한글이면 문제가 발생

![image](https://github.com/user-attachments/assets/ea1658f3-a8a0-43cf-a03e-8615570ad4ca)

Use sample source codes for test.

> Use the same file of  `TU_CreateProject_Example_main.c.`&#x20;
>
> Just change the source codes

{% tabs %}
{% tab title="TU_CreateProject_Example1" %}
```cpp
int main(void){
	int num1 = 1;
	int num2 = 2;
	int numout=num1+num2;		
	printf("hello handong");
	return 0;
}
```
{% endtab %}

{% tab title="TU_CreateProject_Example2" %}
```cpp
#include "stm32f4xx.h"

#define LED_PIN    5
#define BUTTON_PIN 13

// LED Blink by Button Pin (Button #2)

int main(void) {
		/* Part 1. RCC Register Setting */
		// RCC Control Register (HSI)
		RCC->CR |= ((uint32_t)RCC_CR_HSION); 
		// wait until HSI is ready
		while ( (RCC->CR & (uint32_t) RCC_CR_HSIRDY) == 0 ) {;} 
		// Select HSI as system clock source 
		// RCC Configuration Register 
		RCC->CFGR &= (uint32_t)((uint32_t) ~(RCC_CFGR_SW)); 
		RCC->CFGR |= (uint32_t)RCC_CFGR_SW_HSI;  
		// Wait till HSI is used as system clock source 
		while ((RCC->CFGR & (uint32_t)RCC_CFGR_SWS) != 0 ) {;} 
		// HSI is used as system clock         
		// RCC Peripheral Clock Enable Register 
		RCC->AHB1ENR |= RCC_AHB1ENR_GPIOAEN;
		
		/* Part 2. GPIO Register Setting */			
		// GPIO Mode Register
		GPIOA->MODER &= ~(3UL<<(2*LED_PIN)); 
		GPIOA->MODER |=   1UL<<(2*LED_PIN);  
		// GPIO Output Speed Register 
		GPIOA->OSPEEDR &= ~(3UL<<(2*LED_PIN));
		GPIOA->OSPEEDR |=   2UL<<(2*LED_PIN);  
		// GPIO Output Type Register  
		GPIOA->OTYPER &= ~(1UL<<LED_PIN);      
		// GPIO Pull-Up/Pull-Down Register 
		GPIOA->PUPDR  &= ~(3UL<<(2*LED_PIN));
		
			// Dead loop & program hangs here
     while(1){
		// Turn ON LED2
		GPIOA->ODR |= (1UL << LED_PIN);

		// Turn OFF LED2
		//GPIOA->ODR &= ~(1UL << LED_PIN);
    }
}
```
{% endtab %}
{% endtabs %}

## 4. Build and Run

### Build Target (F7)

Build the program of **Example 2**

Press F7 and build the target and check if there is any error message

### Download Target (F8)

If the MCU is connected to PC, flash the output program file

**Flash**>Download (F8)

For Example 2:

* Check if the LED\_2 of MCU (Nucleo-F411RE) board is turned on when the blue button (B2) is pressed

### Tips

Instead of starting from a blank project, use example codes and tutorial codes provided by ARM and STM webpage.
