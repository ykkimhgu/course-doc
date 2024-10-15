# Tutorial: Finite State Machine programming

## FSM Examples

### Example 1

**INPUT:**

* X: Button Pressed {0, 1}

**OUTPUT:**

* LED {ON, OFF}

**STATE:**

* S0: FAN OFF State
* S1: FAN ON State

#### Moore FSM Table

<figure><img src="../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>


#### Example Code: Moore FSM

{% tabs %}
{% tab title="(C-prog) Moore Simple Example Code" %}
```cpp

#include <stdio.h>

// State definition
#define S0  0
#define S1  1

#define LOW  0
#define HIGH  1

unsigned char state = S0;
unsigned char nextstate = S0;
unsigned char input = LOW;
unsigned char ledOut = LOW;


typedef struct {
	unsigned int next[2];   // nextstate = FSM[state].next[input]
	unsigned int out;    // output = FSM[state].out
} State_t;

State_t FSM[2] = {
  {{S0, S1},LOW},
  {{S1, S0},HIGH}
};

int main()
{
    printf("Start\n\r");
    
    input=LOW;    
    nextstate = FSM[state].next[input];
    state=nextstate;
    ledOut = FSM[state].out;
    printf("state=%d,  ledOut=%d \n\r",state,ledOut);
    
    input=HIGH;
    nextstate = FSM[state].next[input];
    state=nextstate;
    ledOut = FSM[state].out;
    printf("state=%d,  ledOut=%d \n\r",state,ledOut);

    input=LOW;    
    nextstate = FSM[state].next[input];
    state=nextstate;
    ledOut = FSM[state].out;
    printf("state=%d,  ledOut=%d \n\r",state,ledOut);

    return 0;
}

```
{% endtab %}

{% tab title="(STMduino) Moore Example Code" %}
```cpp
// State definition
#define S0  0
#define S1  1

// Address number of output in array
#define LED 1

typedef struct {
	uint32_t next[2];   // nextstate = FSM[state].next[input]
	uint32_t out;    // output = FSM[state].out
} State_t;

State_t FSM[2] = {
  {{S0, S1},LOW},
  {{S1, S0},HIGH}
};

const int ledPin = 13;
const int btnPin = 3;

unsigned char state = S0;
unsigned char input = 0;
unsigned char ledOut = LOW;

void setup() {
	// initialize the LED pin as an output:
	pinMode(ledPin, OUTPUT);
		
	// initialize the pushbutton pin as an interrupt input:
	pinMode(btnPin, INPUT_PULLUP);
	attachInterrupt(digitalPinToInterrupt(btnPin), pressed, FALLING);

	Serial.begin(9600);
}

void loop() {
	// First, Update next state. Then, Output.  Repeat
	// 1. Update State <-- Next State
	nextState();

	// 2. Output of states - Logic
	stateOutput();

	digitalWrite(ledPin, ledOut);

	delay(1000);
}

void pressed() {
	input = 1;
}

void nextState() {
	state = FSM[state].next[input];
	// Intialize Button Pressed 
	input = 0;
}

void stateOutput() {
	ledOut = FSM[state].out;
}
```
{% endtab %}
{% endtabs %}


#### Mealy FSM Table

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>


#### Example Code: Mealy FSM

{% tabs %}
{% tab title="(C-prog) Mealy Simple Example Code" %}
```cpp
#include <stdio.h>

// State definition
#define S0  0
#define S1  1

#define LOW  0
#define HIGH  1

unsigned char state = S0;
unsigned char nextstate = S0;
unsigned char input = LOW;
unsigned char ledOut = LOW;


// State table definition
typedef struct {
	unsigned int next[2];       // nextstate = FSM[state].next[input]
	unsigned int out[2];        // output = FSM[state].out[input]
} State_t;

State_t FSM[2] = {
  { {S0, S1},{LOW,HIGH} },
  { {S1, S0},{HIGH,LOW} }
};

int main()
{
    printf("Start\n\r");
    
    input=LOW;
    ledOut = FSM[state].out[input];
    nextstate = FSM[state].next[input];
    state=nextstate;
    printf("state=%d,  ledOut=%d \n\r",state,ledOut);
    
    input=HIGH;
    ledOut = FSM[state].out[input];
    nextstate = FSM[state].next[input];
    state=nextstate;
    printf("state=%d,  ledOut=%d \n\r",state,ledOut);
    
    input=LOW;
    ledOut = FSM[state].out[input];
    nextstate = FSM[state].next[input];
    state=nextstate;
    printf("state=%d,  ledOut=%d \n\r",state,ledOut);
    
    return 0;
}

```
{% endtab %}

{% tab title="(STMduino) Mealy Simple Example Code" %}
```cpp


// State definition
#define S0  0
#define S1  1

// Address number of output in array
#define PWM 0
#define LED 1

const int ledPin = 13;
const int pwmPin = 11;
const int btnPin = 3;

unsigned char state = S0;
unsigned char nextstate = S0;
unsigned char input = 0;
unsigned char ledOut = LOW;
unsigned char pwmOut = 0;

// State table definition
typedef struct {
	unsigned int next[2];       // nextstate = FSM[state].next[input]
	unsigned int out[2];        // output = FSM[state].out[input]
} State_t;

State_t FSM[2] = {
  { {S0, S1},{LOW,HIGH} },
  { {S1, S0},{HIGH,LOW} }
};

void setup() {
	// initialize the LED pin as an output:
	pinMode(ledPin, OUTPUT);

	
	// initialize the pushbutton pin as an interrupt input:
	pinMode(btnPin, INPUT_PULLUP);
	attachInterrupt(digitalPinToInterrupt(btnPin), pressed, FALLING);
}

void loop() {
	// First, Output of current State. Then Update next state. Repeat

	// 1. Output State
	stateOutput();
	digitalWrite(ledPin, ledOut);

	// 2. Update State <-- Next State
	nextState();

	delay(1000);
}


void pressed() {
	input = 1;
}

void nextState() {
	nextstate = FSM[state].next[input];
	state = nextstate;

	// Intialize Button Pressed 
	input = 0;
}

void stateOutput() {	
	ledOut = FSM[state].out[input];
}
```
{% endtab %}
{% endtabs %}

