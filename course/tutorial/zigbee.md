# Tutorial: Zigbee with Nucleo board

### Installation

\[Download XCTU] https://hub.digi.com/support/products/xctu/?path=/support/asset/xctu-installer/

### Zigbee Configuration

#### Preperation

This section explains how to setup the zigbee network configuration for communication. To set network channel, ID of zigbee, you need to download XCTU software and prepare hardwares.

1. Arduino Uno (R3)
2. Xbee ZigBee TH(S2C)
3. XBee Shield 3.0

Then, connect hardwares like fig below and set toggle switch as USB

![](<../../.gitbook/assets/image (116).png>)

#### XCTU software setup

After assemble Xbee module and Xbee shield to Arduino Uno board, connect Arduino board to pc via usb and turn on XCTU software.

![](<../../.gitbook/assets/image (119).png>)

1. Find Xbee module
2. Check port, board name and select port
3. After choose port, push next
4. Check port number and add select device to XCTU

![](<../../.gitbook/assets/image (117).png>)

1. Select Xbee module
2. Select Update
3. Choose firmware which selected.
4. Select Update

#### Trouble shooting

![](<../../.gitbook/assets/image (115).png>)

While XCTU software setup process if a reset error occurs, please follow the steps below.

1. Remove the shield and Zigbee from the Arduino board, load the Arduino empty code and check the port.
2. Choose compile and upload.

#### Xbee Network channel, ID configuration

After setting the ZigBee firmware, select the ZigBee of the changed firmware as shown below and set the network channel and ID. (Initial value - CH : C / ID : 3332)

Network Channel range (0x0B - 0x1A)

Network ID range (0x0 - 0xFFFF)

![](<../../.gitbook/assets/image (120).png>)

### Xbee Serial Communication

#### Communication through XCTU

Repeat software setup and network channel, ID configuration process for another Xbee. Set the same network channel and ID as Zigbee to communicate. After registering 2 Zigbee modules at the same time, follow the procedure below to communicate.

![](<../../.gitbook/assets/image (121).png>)

1. Discover Xbee in the same network.
2. Open console window.
3. Connect communication.
4. Select another xbee module.
5. Connect communication.
6. Send text to other ZigBee modules and check if communication is possible.

#### Serial communication using STM32 board

![](<../../.gitbook/assets/image (118).png>)

In order to communicate with Xbee through stm32 board, **VCC, Ground, TX, RX** must be connected from the xbee shield. In the case of STM32 f411RE board, **USART2 is fixed to usb communication**, so s**elect between USART1 and USART6** and connect it to **RX of the shield - RX of the board / TX of the shield - TX of the board.**
