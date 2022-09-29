# Tutorial : 7-Segment Display



## Hardware

### 7-segment display (5101ASR)

![5101ASR](https://user-images.githubusercontent.com/91526930/192131094-79b776c1-f606-491f-873c-cb102b61d800.png)

![7-segment circuit](https://user-images.githubusercontent.com/91526930/192131110-2fc8e880-10ef-4034-a4a2-9de59ecd42c1.png)

![image](https://user-images.githubusercontent.com/91526930/192942501-63b87284-7c94-4863-8200-106baa02b907.png)

You should know common cathode and anode for circuit configuration.

> Detail information about 7 segment display - [click here](https://www.electronics-tutorials.ws/combination/comb_6.html)





### Array resistor (B331J)

![array resistor](https://user-images.githubusercontent.com/91526930/192131231-c6ae1c48-a236-43f8-9577-010ccd46eccc.png)



## Circuit Configuration
![segment circuit](https://user-images.githubusercontent.com/91526930/192195401-101a346e-44b4-45a5-8099-1b099d4e97c9.png)


![circuit on breadbord](https://user-images.githubusercontent.com/91526930/192194707-c62df336-9869-4de1-9d72-cb2355166989.png)


## Code

Tutorial code : "TU_GPIO_LED_7segment_student.c" [here](https://github.com/ykkimhgu/EC-student/tree/main/tutorial/tutorial-student)

```c++
#include "stm32f4xx.h"
#include "ecRCC.h"
#include "ecGPIO.h"

void setup(void);
	
int main(void) {	
	// Initialiization --------------------------------------------------------
	setup();
	
	// Inifinite Loop ----------------------------------------------------------
	while(1){
		GPIO_write(GPIOA, 5, LOW);
		GPIO_write(GPIOA, 6, LOW);
		GPIO_write(GPIOA, 7, HIGH);
		GPIO_write(GPIOB, 6, HIGH);
		GPIO_write(GPIOC, 7, HIGH);
		GPIO_write(GPIOA, 9, LOW);
		GPIO_write(GPIOA, 8, LOW);
		GPIO_write(GPIOB, 10, LOW);
	}
}

void setup(void){
	RCC_HSI_init();
	GPIO_init(GPIOA, 5, OUTPUT);
	GPIO_init(GPIOA, 6, OUTPUT);
	GPIO_init(GPIOA, 7, OUTPUT);
	GPIO_init(GPIOB, 6, OUTPUT);
	GPIO_init(GPIOC, 7, OUTPUT);
	GPIO_init(GPIOA, 9, OUTPUT);
	GPIO_init(GPIOA, 8, OUTPUT);
	GPIO_init(GPIOB, 10, OUTPUT);
}
```
