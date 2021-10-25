# HUINS Embedded Kit



## Pin Map



|  Component |  Pin Mode |  PinName \(STM32f\) |  Arduino PinName |  Comments |
| :--- | :--- | :--- | :--- | :--- |
|  RGB LED |  PwmOut |  PA\_1 PC\_6 PB\_0 |  A1 PC\_6 A3 |  Period 1msec |
|  Ultrasonic echo |  InterruptIn |  PA\_8 |  D7 |  |
|  Ultrasonic trig |  PWMOut |  PB\_6 |  D10 |  |
|  |  |  |  |  |
|  DC motor: pwm |  PWMOut |  PA\_7 |  D11 |  TB6612FNG motor driver \(PWMA\) |
|  DC motor: dir |  DigitalOut |  PC\_8 |  |  1: CW 0: CCW AIN1, AIN2 are connected with NAND gate to be complement |
|  IR Proximal sensor |  DigitalIn |  PB\_5 |  D4 |  |
|  IR Motion Digital In |  DigitalIn |  PB\_4 |  D5 |  |
|  Buzzer |  DigitalOut |  PA\_13 |  |  |
|  Microphone |  AnalogIn |  PC\_0 |  A5 |  |
|  Bluetooth: TX Bluetooth: RX |  Serial |  PA\_11 PA\_12 |  |  BT-TX\(PA11\) connected to RXD of module |

