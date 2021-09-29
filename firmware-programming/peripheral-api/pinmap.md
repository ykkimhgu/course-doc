# PinMap

## Pinmap 

Instead of separating GPIOx and Pin number, we can add these two information as one value using the defined Pinmap

 \`\`\`

```cpp
// A function that requires Port GPIOx and Pin number pin
// e.g.  foo(GPIOA, 5);
void foo(GPIO_TypeDef *GPIOx, int pin);


// is changed to 
// e.g. foo(PA_5);
void foo(PinName_t pin_num);


```

