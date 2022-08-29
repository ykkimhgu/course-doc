# LAB: Smart mini-fan with STM32-duino



## I. Introduction

In this lab, you are required to create a simple program that uses arduino IDE for implementing a simple embedded digital application. Refer to online arduino references for the full list of APIs.

### Hardware

NUCLEO -F401RE or NUCLEO -F411RE

Ultrasonic distance sensor(HC-SR04), DC motor (RK-280RA)

### Software

Arduino IDE



## II. Procedure

The program needs to runs the DC motor only when the distance of an object is within a certain value.  

Example: An automatic mini-fan that runs only when the face is near the fan 



1. As the button B1 is pressed, change the DC motor velocity

* The mode is OFF(0%), MID(50%), HIGH(70%), V.HIGH(100%)

* As the B1 is pressed, it should toggle from OFF mode to V.HIGH mode and so on

  

2. Automatically Restart and Stop the DC motor when the mode is

* CONT: The distance is within about 50mm

* PAUSE: The distance is beyond about 50mm

  

3. Print the distance and PWM duty ratio in Tera-Term console (every 1 sec).

   

4. When the DC is turned OFF temporarily then turned on again depending on the distance as in Condition (2), it should be turning at the previous speed. 

   

5. When the mode is OFF, turn off the LED(LED1). Otherwise Blink the LED by 1 sec.

   

6. Use FSM(finite-state-machine) to control the mini fan as described above. See below for example code.



### III. Configuration

#### Ultrasonic distance sensor

Trigger:

* Generate a trigger pulse as PWM to the sensor 

* Pin: **D10** (TIM4 CH1)

* PWM out: 50ms period, 10us pulse-width 

 

Echo:

* Receive echo pulses from the ultrasonic sensor 

* Pin: **D7** (Timer1 CH1)

* Input Capture: Input mode

* Measure the distance by calculating pulse-width of the echo pulse. 

 

#### USART

* Display measured distance in [cm] on serial monitor of Tera-Term.

* Baudrate 9600

 

#### DC Motor

* PWM: PWM1, set 10ms of period by default

* Pin: **D11** (Timer1 CH1N)



 

## IV. Report

You are required to write a concise lab report

 

**Lab Report:** 

* Write Lab Title, Date, Your name, Introduction

* Draw a flow-chart of your algorithm

* Need to show the source code in the report

* Also, need to include the external circuit diagram if necessary.

* Submit in both PDF and original file (*.docx etc)

* No need to print out. Only the On-Line submission.

 



##  V. FSM Example Code

### State Diagram Example

![](<../../.gitbook/assets/image (107).png>)



### Example 1 (Moore FSM)

Using structure variable for state definition

```cpp
// created by Y.K Kim

// State definition
#define OFF   0
#define MID   1
#define HIGH  2

// State Input definition
#define IN_CONT   1
#define IN_PAUSE  0

typedef struct {
  uint32_t out;
  uint32_t next[2];
} State_t;

State_t FSM[3] = {
  {0, {OFF, MID}},
  {80, {MID, HIGH}},
  {160, {HIGH, OFF}}
};

const int ledPin = 13;
const int pwmPin = 11;
const int btnPin = 3;
const int trigPin = 10;
const int echoPin = 7;

int state = OFF;
int stateInput = IN_CONT;
int bNextState = 0;
int bPressed = 0;
int ledState = LOW;

unsigned long duration;
float distance;
int thresh = 5;

void setup() {
  // initialize the LED pin as an output:
  pinMode(ledPin, OUTPUT);

  // Initialize pwm pin as an output:
  pinMode(pwmPin, OUTPUT);
  
  // initialize the pushbutton pin as an interrupt input:
  pinMode(btnPin, INPUT_PULLUP);
  attachInterrupt(digitalPinToInterrupt(btnPin), pressed, FALLING);

  // Initialize the trigger pin as an output
  pinMode(trigPin, OUTPUT);

  // Initialize the echo pin as an input
  pinMode(echoPin, INPUT);
  
  Serial.begin(9600);
}

void loop() {
    // Generate pwm singal on the trigger pin.
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  delayMicroseconds(10);

  // Distance is calculated using how much time it takes.
  duration = pulseIn(echoPin, HIGH);
  distance = (float)duration / 58.0;

  if (distance > thresh){
    if (stateInput != IN_PAUSE) bNextState = 1;
    stateInput = IN_PAUSE;
  }
  else {
    if (stateInput != IN_CONT) bNextState = 1;
    stateInput = IN_CONT;
  }

  // change state only if there is a change in the input
  if (bNextState)
    nextState();
  bNextState = 0;
  
  Serial.printf("input : %d, state : %d, distance = ", stateInput, state);
  Serial.println(distance);
  delay(100);
}

void pressed(){
  bPressed = 1;
  nextState();
}

void nextState(){
  int pwm;
  if (bPressed)
    state = FSM[state].next[1];
  bPressed = 0;

  if (stateInput == IN_CONT)
    pwm = FSM[state].out;
  else
    pwm = 0;

  analogWrite(pwmPin, pwm);

  if (state == OFF)
    ledState = LOW;
  else
    ledState = HIGH;

  digitalWrite(ledPin, ledState);
}
```



### Example 2 (Mealy FSM)

Using case switch for next state definition.

```cpp
// by Y.K Kim

// State definition
#define OFF   0
#define MID   1
#define HIGH  2

// State Input definition
#define IN_CONT   1
#define IN_PAUSE  0

const int ledPin = 13;
const int pwmPin = 11;
const int btnPin = 3;
const int trigPin = 10;
const int echoPin = 7;

int state = OFF;
int stateInput = IN_CONT;
int bNextState = 0;
int bPressed = 0;
int ledState = LOW;

unsigned long duration;
float distance;
int thresh = 5;

void setup() {
  // initialize the LED pin as an output:
  pinMode(ledPin, OUTPUT);

  // Initialize pwm pin as an output:
  pinMode(pwmPin, OUTPUT);
  
  // initialize the pushbutton pin as an interrupt input:
  pinMode(btnPin, INPUT_PULLUP);
  attachInterrupt(digitalPinToInterrupt(btnPin), pressed, FALLING);

  // Initialize the trigger pin as an output
  pinMode(trigPin, OUTPUT);

  // Initialize the echo pin as an input
  pinMode(echoPin, INPUT);
  
  Serial.begin(9600);
}

void loop() {
    // Generate pwm singal on the trigger pin.
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  delayMicroseconds(10);

  // Distance is calculated using how much time it takes.
  duration = pulseIn(echoPin, HIGH);
  distance = (float)duration / 58.0;

  if (distance > thresh){
    if (stateInput != IN_PAUSE) bNextState = 1;
    stateInput = IN_PAUSE;
  }
  else {
    if (stateInput != IN_CONT) bNextState = 1;
    stateInput = IN_CONT;
  }

  // change state only if there is a change in the input
  if (bNextState)
    nextState();
  bNextState = 0;
  
  Serial.printf("input : %d, state : %d, distance = ", stateInput, state);
  Serial.println(distance);
  delay(100);
}

void pressed(){
  bPressed = 1;
  nextState();
}

void nextState(){
  int nextstate = 0;
  if (bPressed){
    switch(state){
      case 0:
      case 1:
        if (stateInput)
          nextstate = state + 1;
        else
          nextstate = state;
        break;
      case 2:
      if (stateInput)
        nextstate = 0;
      else
        nextstate = state;
      break;
    }
    state = nextstate;
  }

  outputState();
  bPressed = 0;
}

void outputState(){
  if (stateInput == IN_CONT)
    analogWrite(pwmPin, state*80);
  else
    analogWrite(pwmPin, 0);

  if (state == OFF)
    digitalWrite(ledPin, LOW);
  else
    digitalWrite(ledPin, HIGH);
}
```