# Troubleshooting

## Printf() troubleshooting

#### Try not to use Printf().

#### If needed, use Printf() in main-while() with sufficient time delay.

* printf() can cause a bottleneck for other processing

#### Do not use Printf() in a Timer Interrupt IRQ handler function.

* In some cases, using printf() in a Timer Interrupt for debugging your algorithm can freeze the processor

#### Do not use Printf() just before USARTwrite() for the same variable

*   In some cases, using printf() then USARTwrite() of a variable value can send "blank" message .

    * e.g.

    `printf("%c",value1);` `// USART2,`

    `USARTWrite(usart1, &value1) // other serial`

    * This can send '\_\_' blank value to USART1

## Zigbee communication troubleshooting

When Connecting Each Zigbee to each MCU on a different Laptop.

#### VCC/GND check

* VCC and GND pins of Zigbee modules (for different laptops) MUST NOT be connected with the same MCU
* Separate the two Zigbee-MCU vcc/gnd pins
* Try not to use breadboard between Zigbee and MCUs

#### Unstable power source from Laptop

* Try to use external power adaptor for MCU, instead of laptop
  * Laptop with battery may not give stable 5V that can cause communication error
* Check if Laptop is connected to the power adaptor, not on battery.

#### For Debugging,

* Use the same Laptop for checking the communication between two zigbee-MCU modules
