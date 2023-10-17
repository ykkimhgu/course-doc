# Tutorial: Bitwise Macro



```cpp

// GPIO Mode Register
	//GPIOA->MODER &= ~(3UL<<(2*LED_PIN)); // Clear '00' for Pin 5
	//GPIOA->MODER |=   1UL<<(2*LED_PIN);  // Set '01' for Pin 5
	BITS_CLEAR(GPIOA->MODER, 2 * LED_PIN, 2);
	BITS_SET(GPIOA->MODER, 2 * LED_PIN, 1);
	
	//GPIOA->ODR |= (1UL << LED_PIN);	 		// Change only LED_PIN = H  
	BIT_SET(GPIOA->ODR, LED_PIN);
```
