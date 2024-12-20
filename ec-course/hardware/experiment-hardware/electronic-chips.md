# Electronic Chips

## 7-Segment and Decoder

### Overview

Display decimal number (0\~9) on a 7-segment display

* Inputs: 4-bit numbers (D C B A) // (00001111)
* Output:
  * 7-segment decoder: 7-bit numbers ( a to g)
  * 7-segment display: decimal number 0\~9

Specification

* 7-segment display: common anode
* 7-segment decoder: [74LS47N (datasheet download)](https://pdf1.alldatasheet.com/datasheet-pdf/download/5724/MOTOROLA/SN74LS47N.html)

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### BCD 7-segment decoder

Model: [74LS47N (datasheet download)](https://pdf1.alldatasheet.com/datasheet-pdf/download/5724/MOTOROLA/SN74LS47N.html)

* All output pins are active low

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

### 7-segment Display

* Common anode: (common pin is connected to VCC)
* Giving ‘LOW’ to the pin -> LED ON
* Needs a load resistor for each pin

<figure><img src="../../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Connecting to MCU

Display ‘0’ to ‘9’ on 7-segment display by giving

* Input (D,C,B,A) in range of (0,0,0,0)\~(1,0,0,1)
* Use Arduino Uno

<figure><img src="../../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>
