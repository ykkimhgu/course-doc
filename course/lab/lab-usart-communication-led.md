# LAB: **USART Communication - LED Control**



**Date:** 2022-09-26

**Author/Partner:**

**Github:** repository link

**Demo Video:** Youtube link

**PDF version:**&#x20;



## Introduction

In this lab, we will learn how to configure and use ‘USART(Universal synchronous asynchronous receiver transmitter)’ of MCU. Then, we will learn how to communicate between your PC and MCU and MCU to another MCU with wired serial communication. 

- **Mission 1**: Control LED(LD2) of each other MCU.

- **Mission 2**: Control Lighting 3 LEDs of MCU2 from MCU1.

![](https://user-images.githubusercontent.com/91526930/199908079-a7a06848-2246-43af-a973-03b50a77d8ea.png)

You must submit

* LAB Report (\*.md & \*.pdf)
* Zip source files(main\*.c, ecRCC.h, ecGPIO.h, ecSysTick.c   etc...).
  * Only the source files. Do not submit project files



### Requirement

#### Hardware

* MCU
  * NUCLEO-F411RE
* Actuator/Sensor/Others:

#### Software

* Keil uVision, CMSIS, EC\_HAL library





## Problem 1: Create HAL library

### Create HAL library

Declare and Define  the following functions in your library. You must update your header files located in the directory `EC \lib\`.  

**ecUSART.h**

```c++
/* Given to Students */ 
void UART2_init();
void USART_write(USART_TypeDef* USARTx, uint8_t* buffer, uint32_t nBytes);
void USART_delay(uint32_t us);  

/* Exercise*/
void USART_begin(USART_TypeDef* USARTx, GPIO_TypeDef* GPIO_TX, int pinTX, GPIO_TypeDef* GPIO_RX, int pinRX, int baud);
void USART_init(USART_TypeDef* USARTx, int baud);  															// default pins. 
uint8_t USART_getc(USART_TypeDef * USARTx);										
uint32_t is_USART_RXNE(USART_TypeDef * USARTx);
```





## Problem 2: MCU1 - MCU2 via UART RS-232

### Procedure

1\. Create a new project under the directory `\repos\EC\LAB\LAB_USART_LED`

* The project name is “**LAB_USART_LED”.**

* Create 2 new source files named as “**LAB_USART_LED.c”** and “**LAB_USART_Multi_LEDs.c”**

> You MUST write your name on the source file inside the comment section. 



2\. Include your updated library in `\repos\EC\lib\`  to your project.

*  **ecGPIO.h, ecGPIO.c**
*  **ecRCC.h, ecRCC.c**
*  **ecUART.h, ecUART.c** 



3\. Connect each MCU to each PC with USART 2 via USB cable (ST-Link)

4\. Connect MCU1 and MCU2 with USART 1

- connect RX/TX pins as
  - MCU1 TXD - MCU2 RXD
  - MCU1 RXD - MCU2 TXD

5\. Check that sent message from a PC(Teraterm) is displayed on the other PC(Teraterm). Note that you have to press "Enter" to end the message.



### Configuration

| Type                                                         | Port - Pin                   | Configuration                                                |
| ------------------------------------------------------------ | ---------------------------- | ------------------------------------------------------------ |
| System Clock                                                 |                              | PLL 84MHz                                                    |
| USART2 : USB cable (ST-Link)                                 |                              | No Parity, 8-bit Data,<br />1-bit Stop bit,<br />38400 baud-rate |
| USART1 : MCU1 - MCU2                                         | TXD: PA9<br />RXD: PA10      | No Parity, 8-bit Data,<br />1-bit Stop bit,<br />38400 baud-rate |
| Digital Out: LD2<br /><br />Digital Out: LED 0 ~ 2 <br />(for MCU2) | PA5<br /><br />*Your choice* |                                                              |



### Mission 1

Turn other MCU's LED(LD2) On/OFF by sending text of "**L0**" or "**L1**".



#### Code

Your code goes here: [ADD Code LINK such as github](https://github.com/ykkimhgu/EC-student/)

Explain your source code with necessary comments.

```c++
// YOUR MAIN CODE ONLY
// YOUR CODE
```



#### Result

Experiment images and results

> Show experiment images /results

Add [demo video link]()



### Mission 2

1\. Connect 3 LEDs on MCU2. You can select which pins to connect.

2\. From sending text from MCU1 to MCU2, control the 3 LEDs to display binary data of the input number.

> LED = {LED2, LED1, LED0}
>
> "L0": LED={0, 0, 0} "L1": LED={0, 0, 1} ... "L7": LED={1, 1, 1}

3\. If the given number is higher than 9, it should display the warning message `"TYPE from L0 to L7 ONLY!!"`





#### Code

Your code goes here: [ADD Code LINK such as github](https://github.com/ykkimhgu/EC-student/)

Explain your source code with necessary comments.

```c++
// YOUR MAIN CODE ONLY
// YOUR CODE
```



#### Result

Experiment images and results

> Show experiment images /results

Add [demo video link]()





## Reference

Complete list of all references used (github, blog, paper, etc)

```

```



## Troubleshooting

(Option) You can write Troubleshooting section



