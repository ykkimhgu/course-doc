# mbed : mini Fan

### Problem

An automatic mini-fan that runs only when the face is near the fan 

1. As the button B1 is pressed, change the DC motor velocity 

* The mode is OFF\(0%\), MID\(50%\), HIGH\(100%\) 
* As the B1 is pressed, it should toggle from OFF mode to HIGH mode and so on 

  2. Automatically ReStart and Stop the DC motor when the mode is 

* CONT: The distance is within about 50mm 
* PAUSE: The distance is beyond about 50mm

### State Diagram

![](../../.gitbook/assets/image%20%28104%29.png)

### Method 1 \(Recommended\)

Using structure variable for state definition

```cpp
// created by Y.K Kim


#include "mbed.h"

// State Definition
#define OFF 0
#define MID 1
#define HIGH 2

// State Input Definition
#define IN_CONT 1
#define IN_PAUSE 0

typedef struct {
    uint32_t out;
    uint32_t next[3];
} State_t;

State_t FSM[4] = {
 {0, {OFF,MID}},
 {5, {MID,HIGH}},
 {10, {HIGH,OFF}}
};



int state = OFF;
int stateInput = IN_CONT;
int stTime = 0;
int endTime = 0;
int bNextState = 0;
int bPressed=0;


InterruptIn button(USER_BUTTON);
DigitalOut  led(LED1);
PwmOut      pwm1(D11);
Serial      pc(USBTX, USBRX, 9600);
PwmOut      trig(D10); // Trigger 핀
InterruptIn echo(D7);  // Echo 핀
Timer       tim;




void nextState(int _state, int input) {
    double pwm;
    state=_state;
    
    if (bPressed)    
        state = FSM[_state].next[stateInput];    // Next State
    bPressed=0;
    
    if (stateInput == IN_CONT)
        pwm = FSM[state].out;
    else
        pwm = 0;
        
    pwm1.pulsewidth_ms(pwm);

    if (state == OFF)
        led = 0;
    else
        led = 1;

    
}

void pressed()
{   
    bPressed=1;
    nextState(state, stateInput);
    
}

void setup(void)
{
    pwm1.period_ms(10);
    trig.period_ms(60);     // period      = 60ms
    trig.pulsewidth_us(10); // pulse-width = 10us
}



void rising() {
    stTime = tim.read_us();
}

void falling() {
    endTime = tim.read_us();
}



int main()
{
    int thresh = 5;
    float distance = 0;

    setup();
    // BT interrupt
    button.fall(&pressed);
    // InputCapture interrupt
    echo.rise(&rising);
    echo.fall(&falling);
    // Start time measurement
    tim.start();

    while (1) {
        // Check distance
        distance = abs((float)(endTime - stTime)) / 58; // [cm]

        if (distance > thresh) {
            if (stateInput != IN_PAUSE) bNextState = 1;
            stateInput = IN_PAUSE;
        }
        else {
            if (stateInput != IN_CONT) bNextState = 1;
            stateInput = IN_CONT;
        }

        // change state only if there is a change in the input
        //if (bNextState)  
        nextState(state,stateInput);
        bNextState = 0;
        printf("input = %d, state = %d, distance = %.2f\r\n",stateInput, state, distance);
        wait(0.1);
    }
}


```





### Method 2

Using case switch for next  state definition



```cpp
// Method 1
// by Y.K Kim


#include "mbed.h"

// State Definition
#define OFF 0
#define MID 1
#define HIGH 2

// State Input Definition
#define IN_CONT 1
#define IN_PAUSE 0

int state=0;
int stateInput = 0;
int stTime = 0;
int endTime = 0;
int bNextState = 0;
int bPressed=0;

InterruptIn button(USER_BUTTON);
DigitalOut  led(LED1);
PwmOut      pwm1(D11);
Serial      pc(USBTX, USBRX, 9600);
PwmOut      trig(D10); // Trigger 핀
InterruptIn echo(D7);  // Echo 핀
Timer       tim;


// Mealy FSM
void  outputState(int _state, int input) {
    if (_state == OFF)
        led = 0;
    else
        led = 1;

    if (!input)
        pwm1.pulsewidth_ms(0);
    else
        pwm1.pulsewidth_ms(_state * 5);    
}

void nextState(int _state, int input) {
    int nextstate = 0;
    state=_state;
    
    if (bPressed){        
        switch (_state) {
            case 0:
            case 1:
                if (input)
                    nextstate = _state + 1;
                else
                    nextstate=_state;
                break;
            case 2:
                if (input)
                    nextstate = 0;
                else
                    nextstate = _state;
                break;
            }
        state = nextstate;
    }

    outputState(state, input);
    bPressed=0;
}


void pressed()
{   
   bPressed=1;
   nextState(state, stateInput);
}

void setup(void)
{
    pwm1.period_ms(10);

    trig.period_ms(60);     // period      = 60ms
    trig.pulsewidth_us(10); // pulse-width = 10us
}


void rising() {
    stTime = tim.read_us();
}

void falling() {
    endTime = tim.read_us();
}


int main()
{
    int thresh = 5;
    float distance = 0;

    setup();
    // BT interrupt
    button.fall(&pressed);  
    // InputCapture interrupt
    echo.rise(&rising);
    echo.fall(&falling);
    // Start time measurement
    tim.start();

    while (1) {     
        // Check distance
        distance = (float)(endTime - stTime) / 58; // [cm]

        if (distance > thresh){ 
            if (stateInput != IN_PAUSE) bNextState = 1;
            stateInput = IN_PAUSE;
        }
        else {          
            if (stateInput != IN_CONT) bNextState = 1;
            stateInput = IN_CONT;
        }
        
        // change state only if there is a change in the input
        if(bNextState)  nextState(state, stateInput);
        bNextState = 0;
        wait(0.1);
    }
}


```





