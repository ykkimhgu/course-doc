---
description: 'Tutorial: mbed - Exercise'
---

# Tutorial: mbed - Exercise

## Mbed Exercise

We are going to create a simple program that count 3 seconds by using **Timer, Tick, InterruptIn, Serial** classes. Result will be printed out  through UART communication.

You need to create codes to function as follows. 

* Set the baud-rate of uart communication as 9600\[bps\] 
* ‘count’ value should be increased by 1 for every 0.1 \[sec\]. Use **‘Tick’**. 
* If ‘count’ becomes 30, print out the time-duration\(unit: \[s\]\) of counting 30 to Tera-Term. Use **‘Timer’** and **‘Serial’**. 
* If push-button is pressed, ‘count’ and timer value should be reset. Use **‘InterruptIn’**.

Result

![](../../.gitbook/assets/image%20%2849%29.png)

