# Untitled



```cpp
#ifndef _EC_HAL_H // or use (#pragma once)
#define _EC_HAL_H

#include <stdio.h>

#ifdef __cplusplus
 extern "C" {
#endif /* __cplusplus */

extern void addVector(float _src1[], float _src2[], float _dst[], int _vecLength);
// Your function declarations go here
// Your function declarations go here
// Your function declarations go here


#ifdef __cplusplus
}
#endif /* __cplusplus */

#endif

```

```cpp
#include "mbed.h"

PwmOut pwm1(D11);
DigitalIn  button(USER_BUTTON);
PwmOut pwm2(PC_8);
int pwm = 0;
int dir = 0;
int main() {
    pwm1.period_ms(10);
    pwm2.period_ms(10);
    while(1){
        if(!button) {
            pwm = pwm + 2.5;
            if(pwm>10) {
                if(dir ==1) dir = 0;
                else dir =1;
                pwm = 0;
                }
            }
    if (dir==1) {
        pwm1.pulsewidth_ms(pwm);
        pwm2.pulsewidth_ms(0);
        }
    else {
        pwm1.pulsewidth_ms(0);
        pwm2.pulsewidth_ms(pwm);
        }
    wait(0.1);
    }     
}

```

