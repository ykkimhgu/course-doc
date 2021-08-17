# Create a Project with uVision

**Run Keil 𝝁Vision IDE**

 Project &gt;  New 𝝁Vision Project…

> Do not use 한글경로 \(띄어쓰기\) for Project Directory path

![](../.gitbook/assets/image%20%289%29.png)

### **Select Device for Target**

 **Device &gt;**  search for  _STM32F411RETx_

> If you use other board, STM32F401 etc, choose the appropriate device.

![](../.gitbook/assets/image%20%2835%29.png)

### **Manage Run-Time Environment**

Select  CMSIS&gt;CORE  , Device&gt;Setup

> This will use necessary library to start the MCU and GPIO drivers.

Check if the following startup codes are included under **Device** folder

 ‘startup\_stm32f411xe.s’, ‘system\_stm32f411xx.c’

![](../.gitbook/assets/image%20%283%29.png)

### **Project&gt; Options for Target: \(Alt+F7\)**

 **Output tab** &gt;  Create HEX File  checked

> This will create HEX file that  contains the machine instruction codes

![](../.gitbook/assets/image%20%288%29.png)

**Linker Tab**&gt; Use Memory Layout from Target Dialog  checked

> This will use the memory\(register\) layout of the specific target board

![](../.gitbook/assets/image%20%2829%29.png)

**Debug tab&gt;**  Use: ST-Link Debugger &gt; Settings   

* Must connect MCU\(internal ST-Link\) to PC
* Use: ST-LInk Debugger 
* Press Settings
* Debug Adapter&gt;  Unit: ST-LINK/V2-1
* Debug&gt;  Connect: under Reset

> This will configure USB link to MCU hardware. It will use ST-Link debugger embedded on the target board to debug the program. You will need to connect the target board to your PC for debugging

![](../.gitbook/assets/image%20%2831%29.png)

### Create Source file

Project Tab&gt; Target1&gt; Source Group1  Right Click &gt; Add New item to Group

Name the source file as " TU-CreateProject-Main.c"

> 한글 경로 사용하면 안됨!!



![](../.gitbook/assets/image%20%2810%29.png)

Use samle source codes for test

{% tabs %}
{% tab title="Example 1" %}
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

{% tab title="Example 2" %}
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
			 GPIOA->ODR = 1UL << LED_PIN;  
		}
}

```
{% endtab %}
{% endtabs %}

### Build Target \(F7\)

Press F7 and build the target. 

Check if there is any error message

### 

### Download Target\(F8\)

If the MCU is connected to PC, flash the output file by pressing

**Flash**&gt;Download \(F8\)

* For Example 2: Check if the LED\_2 of testboard is turned on when button B2 is pressed.  \(Nucleo-F411RE\)





### Tips

Instead of starting from a blank project, use example codes and tutorial codes provided by ARM and STM webpage.

