# mbed : mini Fan

### State Diagram



### Method 1

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
int begin = 0;
int end = 0;
int bNextState = 0;

InterruptIn button(USER_BUTTON);
DigitalOut  led(LED1);
PwmOut		pwm1(D11);
Serial      pc(USBTX, USBRX, 9600);
PwmOut      trig(D10); // Trigger 핀
InterruptIn echo(D7);  // Echo 핀
Timer       tim;

void pressed()
{	
	nextState(state);
}


void nextState(int state, int input) {
	int nextstate = 0;
	switch (state) {
		case 0,1:
			if (input)
				nextstate = state + 1;
			else
				nextstate=state
			outputState(state, input);
			break;
		case 2:
			if (input)
				nextstate = 0;
			else
				nextstate = state
			outputState(state, input);
			break;
	}
	state = nextstate;
}

// Mealy FSM
void  outputState(int state, int input) {
	if (state == OFF)
		led = 0;
	else
		led = 1;

	if (!input)
		pwm1.pulsewidth_ms(0);
	else
		pwm1.pulsewidth_ms(state * 0.5);	
}

void setup(void)
{
	pwm1.period_ms(10);

	trig.period_ms(60);     // period      = 60ms
	trig.pulsewidth_us(10); // pulse-width = 10us
}


void rising() {
	begin = tim.read_us();
}

void falling() {
	end = tim.read_us();
}


int main()
{
	int thresh = 10;
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
		distance = (float)(end - begin) / 58; // [cm]

		if (distance > thresh){ 
			if (stateInput != IN_PAUSE) bNextState = 1;
			stateInput = IN_PAUSE;
		}
		else { 			
			if (stateInput != IN_CONT) bNextState = 1;
			stateInput = IN_CONT;
		}
		
		// change state only if there is a change in the input
		if(bNextState)	nextState(state, stateInput);
		bNextState = 0;
		wait(0.5);
	}
}


```



### Method 2

Using structure variable for state definition





