---
description: EC-STM32
---

# Example Code

## Example Code

Every example below is a real, build-verified file in this folder — `pio run -e <env>` builds it standalone against the current `include/` library. Topics are ordered the same way as the course site's [Sample code](https://ykkim.gitbook.io/ec/ec-course/example-code) page, so the two are easy to cross-reference. Unlike that page, this one shows only the current library's code, not a multi-year comparison.

For the full function-by-function API, see the matching page in docs/ (linked under each heading below). For the same topics written in plain Arduino/STM32duino style, for comparison, see examples-stm32duino/. For every peripheral's setup() calls in one copy-paste reference file, see `Example_Template_main.c` / docs/ecSTM32-template.md.

Written by: Young-Keun Kim Course: HGU Embedded Controller MCU: STM32F411RE (Nucleo-64)

***

### Table of Contents

* GPIO
  * Blinking LED
  * LED with Button
  * Seven Segment
* EXTI Interrupt
* SysTick Interrupt
* Timer Interrupt
* PWM
* Stepper Motor
* Timer Input Capture: Ultrasonic Distance Sensor
* ADC
  * Polling
  * Hardware-Triggered + Interrupt
* JADC — Injected Group, Multi-Channel
* UART
  * USART2 Only
  * USART1 + USART2 Simultaneously
