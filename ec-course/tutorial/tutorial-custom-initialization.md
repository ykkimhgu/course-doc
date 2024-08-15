# Tutorial: Custom initialization



## Custom MCU initialization



Instead of writing initial setting functions for each registers, you can call `MCU_init()` to call the commonly used default initialization.



Define this in `ecSTM32F411.h`.



```cpp
void MCU_init(void){
    // CLOCK PLL 84MHz
    RCC_PLL_init();
    
    // SysTick 1msec
    SysTick_init();    
    
    // Button PC13
    GPIO_init(GPIOC, 13, INPUT);
    GPIO_pupd(GPIOC, 13, EC_PD);    
    
    // LED PA5
    GPIO_init(GPIOA, 5, OUTPUT);    

    // TIMx Default Initialization
    // ...
    // PWM Default Initialization
    // ...        
    // USART Default Initialization
    // ...
    // Others
}
```

In  main source code,&#x20;

```cpp
#include “ecSTM32F411.h”	// includes all necessary library

// Initialization
void setup(){
	MCU_init();
	// Modify or Add more initializations
	// Modify or Add more initializations
}

// Main 
void main(){
	setup( );
	while(1){
	// polling logic goes here
	};
}
```



Examples of default initialization

### Clock

* PLL 84MHz

### SysTick

* tick period of 1msec&#x20;

### GPIO

* Evaluation board Button :  PC13
* Evaluation board LED :  PA5

### TIMx

* Timer2 Period:  1msec&#x20;
* Timer2 Interrupt: 1mec

