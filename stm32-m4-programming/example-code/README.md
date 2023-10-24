---
description: EC vs Arduino vs mbed
---

# Sample code

## GPIO

### Blinking LED

{% tabs %}
{% tab title="EC" %}
```c
#include "ecSTM32F411.h"

#define LED_PIN 5
#define BUTTON_PIN 13

// Initialiization 
void setup(void) {
	RCC_PLL_init();
	SysTick_init();
	GPIO_init(GPIOA, LED_PIN, OUTPUT);    
}
	
int main(void) { 
	setup();
	
	while(1){
		delay_ms(500);  
		GPIO_write(GPIOA, LED_PIN, LOW);
		delay_ms(500);  
		GPIO_write(GPIOA, LED_PIN, HIGH);
	}
}
```
{% endtab %}

{% tab title="Arduino" %}
```cpp
// constants won't change. Used here to set a pin number:
const int ledPin =  LED_BUILTIN;// the number of the LED pin

// Variables will change:
int ledState = LOW;             // ledState used to set the LED

// Generally, you should use "unsigned long" for variables that hold time
// The value will quickly become too large for an int to store
unsigned long previousMillis = 0;        // will store last time LED was updated

// constants won't change:
const long interval = 1000;           // interval at which to blink (milliseconds)

void setup() {
  // set the digital pin as output:
  pinMode(ledPin, OUTPUT);
}

void loop() {
  // here is where you'd put code that needs to be running all the time.

  // check to see if it's time to blink the LED; that is, if the difference
  // between the current time and last time you blinked the LED is bigger than
  // the interval at which you want to blink the LED.
  unsigned long currentMillis = millis();

  if (currentMillis - previousMillis >= interval) {
    // save the last time you blinked the LED
    previousMillis = currentMillis;

    // if the LED is off turn it on and vice-versa:
    if (ledState == LOW) {
      ledState = HIGH;
    } else {
      ledState = LOW;
    }

    // set the LED with the ledState of the variable:
    digitalWrite(ledPin, ledState);
  }
}
```
{% endtab %}

{% tab title="mbed" %}
```cpp
#include "mbed.h"

DigitalOut led(LED1);

int main() {
    while(1) {
        led = 1;
        wait(0.5)
        led=0;
        wait(0.5);
    }
}
```
{% endtab %}
{% endtabs %}

###

### LED with button

{% tabs %}
{% tab title="EC" %}
```cpp
#include "ecSTM32F411.h"

#define LED_PIN 	5
#define BUTTON_PIN 13

// Initialiization 
void setup(void) {
	RCC_HSI_init();
	// initialize the pushbutton pin as an input:
	GPIO_init(GPIOC, BUTTON_PIN, INPUT);  
	// initialize the LED pin as an output:
	GPIO_init(GPIOA, LED_PIN, OUTPUT);    
}
	
int main(void) { 
 	setup();
	int buttonState=0;
	
	while(1){
		// check if the pushbutton is pressed. Turn LED on/off accordingly:
		buttonState = GPIO_read(GPIOC, BUTTON_PIN);
		if(buttonState)	GPIO_write(GPIOA, LED_PIN, LOW);
		else 		GPIO_write(GPIOA, LED_PIN, HIGH);
	}
}
```
{% endtab %}

{% tab title="Arduino" %}
```cpp
// constants won't change. They're used here to set pin numbers:
const int buttonPin = 2;     // the number of the pushbutton pin
const int ledPin =  13;      // the number of the LED pin

// variables will change:
int buttonState = 0;         // variable for reading the pushbutton status

void setup() {
  // initialize the LED pin as an output:
  pinMode(ledPin, OUTPUT);
  // initialize the pushbutton pin as an input:
  pinMode(buttonPin, INPUT);
}

void loop() {
  // read the state of the pushbutton value:
  buttonState = digitalRead(buttonPin);

  // check if the pushbutton is pressed. If it is, the buttonState is HIGH:
  if (buttonState == HIGH) {
    // turn LED on:
    digitalWrite(ledPin, HIGH);
  } else {
    // turn LED off:
    digitalWrite(ledPin, LOW);
  }
}
```
{% endtab %}

{% tab title="mbed" %}
```cpp
#include "mbed.h"

DigitalIn  button(USER_BUTTON);
DigitalOut led(LED1);

int main() {
    while(1) {
        if(!button) led = 1;
        else led = 0;
    }
}
```
{% endtab %}
{% endtabs %}

### Seven Segment

{% tabs %}
{% tab title="EC" %}
```cpp
#include "ecSTM32F411.h"

#define BUTTON_PIN 13

// Initialiization 
void setup(void)
{
	RCC_PLL_init();
	SysTick_init();
	GPIO_init(GPIOC, BUTTON_PIN, INPUT);  
	sevensegment_init();
}
	
int main(void) { 
	setup();
	unsigned int cnt = 0;
		
	while(1){
		// display 7-segment 0 to 9
		sevensegment_decode(cnt % 10);
		// increase number with button push
		if(GPIO_read(GPIOC, BUTTON_PIN) == 0) {
			cnt++; 
			delay_ms(500);
		}
		if (cnt > 9) cnt = 0;
	}
}
```
{% endtab %}
{% tab title="EC_2" %}
```cpp
#include "ecSTM32F411.h"
#include "ecGPIO2.h"
#include "ecPinNames.h"

#define BUTTON_PIN 13

// Initialiization 
void setup(void)
{
	RCC_PLL_init();
	SysTick_init();
	GPIO_init(GPIOC, BUTTON_PIN, INPUT);  
	sevensegment_init2();
}
	
int main(void) { 
	setup();
	unsigned int cnt = 0;
		
	while(1){
		// display 7-segment 0 to 9
		sevensegment_decode2(cnt % 10);
		// increase number with button push
		if(GPIO_read(GPIOC, BUTTON_PIN) == 0) {
			cnt++; 
			delay_ms(500);
		}
		if (cnt > 9) cnt = 0;
	}
}

/////////////////////////////////////////////////////
// Seven Segment Decoder 

void sevensegment_decode2(unsigned int num){
	// 7-segment Decoder 
	int number[11][8] = {
					{1,1,1,0,1,1,1,0},          //zero
					{0,0,1,0,0,1,0,0},          //one
					{1,0,1,1,1,0,1,0},          //two
					{1,0,1,1,0,1,1,0},          //three
					{0,1,1,1,0,1,0,0},          //four
					{1,1,0,1,0,1,1,0},          //five
					{1,1,0,1,1,1,1,0},          //six
					{1,0,1,0,0,1,0,0},          //seven
					{1,1,1,1,1,1,1,0},          //eight
					{1,1,1,1,0,1,1,0},          //nine
					{0,0,0,0,0,0,0,1}          //dot
	};
	
    	PinName_t sevenPins[] = { PA_5, PA_6, PA_7, PB_6, PC_7,  PA_9, PA_8, PB_10}
	// GPIO Write 
	GPIO_TypeDef* Port;
	unsigned int pin;
	unsigned int ledOut;

	for (int i = 0; i < 8; i++) { 
		ledOut = number[num][i];
		ecPinmap(sevenPins[i], Port, &pin);
		GPIO_write(Port, pin, ledOut)
	}

}



/////////////////////////////////////////////////////
// Seven Segment Initialization 

void sevensegment_init2(){
	// GPIO 7-segment pins 
    	PinName_t sevenPins[] = { PA_5, PA_6, PA_7, PB_6, PC_7,  PA_9, PA_8, PB_10};

	// GPIO Initialization
	GPIO_TypeDef* Port;
	unsigned int pin;

	for (int i = 0; i < 8; i++)
	{
		ecPinmap(sevenPins[i], Port, &pin);
        GPIO_init(Port, pin, OUTPUT);
		GPIO_mode(Port, pin, OUTPUT);
		GPIO_pupd(Port, pin, EC_PU);
	}
}


```
{% endtab %}
{% tab title="Arduino" %}
```cpp
// https://www.circuitbasics.com/arduino-7-segment-display-tutorial/

#include "SevSeg.h"
SevSeg sevseg; 

void setup(){
    byte numDigits = 1;
    byte digitPins[] = {};
    byte segmentPins[] = {6, 5, 2, 3, 4, 7, 8, 9};
    bool resistorsOnSegments = true;

    byte hardwareConfig = COMMON_CATHODE; 
    sevseg.begin(hardwareConfig, numDigits, digitPins, segmentPins, resistorsOnSegments);
    sevseg.setBrightness(90);
}

void loop(){
        sevseg.setNumber(4);
        sevseg.refreshDisplay();        
}
```
{% endtab %}

{% tab title="mbed" %}
```cpp
#include "mbed.h"
 
    //pins are sorted from upper left corner of the display to the lower right corner
    //the display has a common cathode
    //the display actally has 8 led's, the last one is a dot 
DigitalOut led[8]={p18, p19, p17, p20, p16, p14, p15, p13};
 
 
    //each led that has to light up gets a 1, every other led gets a 0
    //its in order of the DigitalOut Pins above
int number[11][8]={
                    {1,1,1,0,1,1,1,0},          //zero
                    {0,0,1,0,0,1,0,0},          //one
                    {1,0,1,1,1,0,1,0},          //two
                    {1,0,1,1,0,1,1,0},          //three
                    {0,1,1,1,0,1,0,0},          //four
                    {1,1,0,1,0,1,1,0},          //five
                    {1,1,0,1,1,1,1,0},          //six
                    {1,0,1,0,0,1,0,0},          //seven
                    {1,1,1,1,1,1,1,0},          //eight
                    {1,1,1,1,0,1,1,0},          //nine
                    {0,0,0,0,0,0,0,1}          //dot
                  };
 
 
int main() {
    while (1) {
            //all led's off
        for(int i = 0; i<8;i++){led[i] = 0;}
        
            //display shows the number in this case 6
        for (int i=0; i<8; i++){led[i] = number[6][i];}         //the digit after "number" is displayed
 
            //before it gets tired
        wait(0.5);
    
    }
}
```
{% endtab %}
{% endtabs %}

## Interrupt

### Button External Interrupt

{% tabs %}
{% tab title="EC" %}
```cpp
#include "ecSTM32F411.h"

#define LED_PIN	5
#define BUTTON_PIN 13

// Initialiization 
void setup(void)
{
	RCC_PLL_init();
	SysTick_init();
	GPIO_init(GPIOA, LED_PIN, OUTPUT);
	GPIO_init(GPIOC, BUTTON_PIN, INPUT);
	GPIO_pupd(GPIOC, BUTTON_PIN, EC_PD);
	// Priority Highest(0) External Interrupt 
	EXTI_init(GPIOC, BUTTON_PIN, FALL, 0);
}

int main(void) {
	setup();
	while (1) {}
}

//EXTI for Pin 13
void EXTI15_10_IRQHandler(void) {
	if (is_pending_EXTI(BUTTON_PIN)) {
		LED_toggle();
		clear_pending_EXTI(BUTTON_PIN); 
	}
}
```
{% endtab %}

{% tab title="Arduino" %}
```cpp
const byte ledPin = 13;
const byte interruptPin = 2;
volatile byte state = LOW;

void setup() {
	pinMode(ledPin, OUTPUT);
	pinMode(interruptPin, INPUT_PULLUP);
	attachInterrupt(digitalPinToInterrupt(interruptPin), blink, CHANGE);
}

void loop() {
	digitalWrite(ledPin, state);
}

void blink() {
	state = !state;
}
```
{% endtab %}

{% tab title="mbed" %}
```cpp
#include "mbed.h"

InterruptIn button(USER_BUTTON); 
DigitalOut  led(LED1);

void pressed()
{
    led = 1; 
}

void released(){
    led = 0;
}

int main()
{
    button.fall(&pressed);
    button.rise(&released);
    while (1);
}
```
{% endtab %}
{% endtabs %}

### SysTick Interrupt

{% tabs %}
{% tab title="EC" %}
```cpp
/**
******************************************************************************
* @author  SSSLAB
* @Mod		 2021-8-30 by YKKIM  	
* @brief   Embedded Controller:  LAB Systick&EXTI with API
*					 - 7 segment
* 
******************************************************************************
*/

#include "stm32f411xe.h"
#include "ecGPIO.h"
#include "ecRCC.h"
#include "ecSysTick.h"

int count = 0;
// Initialiization 
void setup(void)
{
	RCC_PLL_init();
	SysTick_init();
	sevensegment_init();
}

int main(void) { 
	// Initialiization --------------------------------------------------------
		setup();
	
	// Inifinite Loop ----------------------------------------------------------
	while(1){
		sevensegment_decode(count);
		delay_ms(1000);
		count++;
		if (count >10) count =0;
		SysTick_reset();
	}
}
```
{% endtab %}

{% tab title="EC_API" %}
```cpp
/**
******************************************************************************
* @author  SSSLAB
* @Mod		 2021-8-30 by YKKIM  	
* @brief   Embedded Controller:  LAB Systick&EXTI with API
*					 
* 
******************************************************************************
*/

#include "EC_API.h"

EC_Ticker tick(1);
int count = 0;

// Initialiization 
void setup(void)
{
	RCC_PLL_init();
	sevensegment_init();
}

int main(void) { 
	// Initialiization --------------------------------------------------------
		setup();
	
	// Inifinite Loop ----------------------------------------------------------
	while(1){
		sevensegment_decode(count);
		tick.Delay_ms(1000);
		count++;
		if (count ==10) count =0;
		tick.reset();
	}
}
```
{% endtab %}

{% tab title="mbed" %}
```cpp
#include "mbed.h"

```
{% endtab %}
{% endtabs %}

##


## PWM 

### PWM out on LED
{% tabs %}
{% tab title="EC" %}
```cpp

#include "ecSTM32F411.h"
#define BUTTON_PIN 13

PWM_t pwm;
float duty = 0;
int count = 0;

// Initialiization 
void setup(void) {
	RCC_PLL_init();
	SysTick_init();
	GPIO_init(GPIOC, BUTTON_PIN, INPUT);
	GPIO_pupd(GPIOC, BUTTON_PIN, EC_PD);
	EXTI_init(GPIOC, BUTTON_PIN, FALL, 0);

	// PWM of 20msec:  TIM2_CH2 (PA_1)
	PWM_init(&pwm, GPIOA, 1);
	PWM_period_ms(&pwm, 20);
}


int main(void) { 
	setup();

	while(1){
		for (count= 0; count < 8; count++) {
			duty = 0.1 + 0.1 * count;	
			PWM_duty(&pwm, duty);
			delay_ms(500);
		}
	}
}

void EXTI15_10_IRQHandler(void) {  
	if (is_pending_EXTI(BUTTON_PIN)) {
		count = 0;
		clear_pending_EXTI(BUTTON_PIN); 
	}
}


```
{% endtab %}
{% tab title="Arduino" %}
```cpp
const int pwmPin = 11;   // PWM pin
const int buttonPin = 3;  // button pin

int buttonState = HIGH;

void setup() {
  pinMode(pwmPin, OUTPUT);
 
 // initialize the pushbutton pin as an input:
  pinMode(buttonPin, INPUT_PULLUP);
  attachInterrupt(digitalPinToInterrupt(buttonPin), motorOperation, CHANGE);
}

void loop() {

  if (buttonState == LOW){
    for (int i = 0; i < 10; i++){
      analogWrite(pwmPin, 40 + 10*i);
      delay(100);
    }
  
    for (int i = 10; i > 0; i--){
      analogWrite(pwmPin, 40 + 10*i);
      delay(100);
    }
  }
  else{
    analogWrite(pwmPin, 0);
  }
}

void motorOperation(){
  buttonState = digitalRead(buttonPin);
}


```
{% endtab %}
{% endtabs %}

## PWM \_ DC Motor

{% tabs %}
{% tab title="mbed" %}
```cpp
#include "mbed.h"
#include "motordriver.h"

Motor A(D11, PC_8); // pwm, dir
Motor B(D12, PD_2); // pwm, dir

int main() {
    while (1) {
        // For speed test.
        for (float s= 0; s < 1.0f ; s += 0.1f) {
			A.forward(s); 
            wait(1);
		}

		A.stop();
        wait(3);

        for (float s= 0; s < 1.0f ; s += 0.1f) {
			A.backward(s);
            wait(1);
       }
    }
}



```
{% endtab %}
{% endtabs %}

### motordriver.h, motordriver.cpp // by Huins

{% tabs %}
{% tab title="mbed" %}
```
// motordriver.h

#ifndef MBED_MOTOR_H
#define MBED_MOTOR_H
 
#include "mbed.h"
 
class Motor {
public:
	Motor(PinName pwm, PinName dir);
	void forward(float speed);
	void backward(float speed);
	void stop(void);
        
protected:
	PwmOut _pwm;
	DigitalOut _dir;
	int sign; //모터의 현재상태. 이를 이용하여 순방향에서 역방향으로 바로 방향을 바꾸는 것을 방지한다.
 
};

#endif
```

```
// motordriver.cpp

#include "motordriver.h"
#include "mbed.h"
 
Motor::Motor(PinName pwm, PinName dir):
        _pwm(pwm), _dir(dir) {
 
    // Set initial condition of PWM
    _pwm.period(0.001);
    _pwm = 0;
 
    // Initial condition of output enables
    _dir = 0; // 회전 방향 지시: 0 = forward, 1 = backward
    sign = 0; // 회전 상태 표시: 1 = forward, -1 = backward 
}
 
void Motor::forward(float speed) {
	float temp = 0;

    // backward로 회전 중이면, break mode에서 200 ms 동안 대기하는 코드 작성
	
    // forward 방향으로 입력된 speed로 회전하는 코드 작성
	
	
    sign = 1;
}

void Motor::backward (float speed) {
	float temp = 0;

    // forward로 회전 중이면, break mode에서 200 ms 동안 대기하는 코드 작성
	
    // forward 방향으로 입력된 speed로 회전하는 코드 작성
	
    
	sign = -1;
}

 
void Motor::stop(void) {
	_pwm = 0;
}
 
```
{% endtab %}
{% endtabs %}

## Stepper Motor

{% tabs %}
{% tab title="mbed" %}
```cpp

```
{% endtab %}

{% tab title="EC" %}
```cpp
/**
******************************************************************************
* @author  SSSLAB
* @Mod		 2021-8-12 by YKKIM  	
* @brief   Embedded Controller:  Tutorial ___
*					 - _________________________________
* 
******************************************************************************
*/
#include "stm32f411xe.h"
#include "ecGPIO.h"
#include "ecRCC.h"
#include "ecTIM.h"
#include "ecEXTI.h"
#include "ecSysTick.h"
#include "ecStepper.h"

void setup(void);
	
int main(void) { 
	// Initialiization --------------------------------------------------------
	setup();
	
	Stepper_step(10000, 0, FULL);  // (Step : 1024, Direction : 0 or 1, Mode : FULL or HALF)
	
	// Inifinite Loop ----------------------------------------------------------
	while(1){;}
}

// Initialiization 
void setup(void)
{	
	
	RCC_PLL_init();                                 // System Clock = 84MHz
	SysTick_init();                                 // Systick init
	
	EXTI_init(GPIOC,BUTTON_PIN,FALL,0);             // External Interrupt Setting
	GPIO_init(GPIOC, BUTTON_PIN, EC_DIN);           // GPIOC pin13 initialization

	Stepper_init(GPIOB,10,GPIOB,4,GPIOB,5,GPIOB,3); // Stepper GPIO pin initialization
	Stepper_setSpeed(5);                          //  set stepper motor speed
}

void EXTI15_10_IRQHandler(void) {  
	if (is_pending_EXTI(BUTTON_PIN)) {
		Stepper_stop();
		clear_pending_EXTI(BUTTON_PIN); // cleared by writing '1'
	}
}
```
{% endtab %}
{% endtabs %}

###

##

## Ultrasonic Sensor : PWM & Input Capture

{% tabs %}
{% tab title="mbed" %}
```cpp
#include "mbed.h"

Serial      pc(USBTX, USBRX, 9600);
PwmOut      trig(D10); // Trigger 핀
InterruptIn echo(D7);  // Echo 핀
Timer       tim;

int begin = 0;
int end = 0;

void rising(){
    begin = tim.read_us();
}

void falling(){
    end = tim.read_us();
}

int main(void){
    float distance = 0;
    
    trig.period_ms(60);     // period      = 60ms
    trig.pulsewidth_us(10); // pulse-width = 10us
    
    echo.rise(&rising);
    echo.fall(&falling);
    
    tim.start();
    
    while(1){
        distance =  (float)(end - begin) / 58; // [cm]
        pc.printf("Distance = %.2f[cm]\r\n", distance);
        wait(0.5);
    }
    
} 

```
{% endtab %}
{% endtabs %}

## Timer

{% tabs %}
{% tab title="EC" %}
```

```
{% endtab %}

{% tab title="mbed" %}
```cpp
#include "mbed.h"

Timer       timer;
Serial      pc(USBTX, USBRX, 9600); // for using ‘printf()’

int begin, end;
int cnt = 0;

int main(void){

    timer.start();
    
    begin = timer.read_us();
    
    while(cnt < 100) cnt++;
    
    end = timer.read_us();
    
    pc.printf("Counting 100 takes %d [us]", end-begin);
}
```
{% endtab %}
{% endtabs %}

###

## Input Capture

{% tabs %}
{% tab title="mbed" %}
```cpp
#include "mbed.h"

Serial      pc(USBTX, USBRX, 9600);
PwmOut      trig(D10); // Trigger 핀
InterruptIn echo(D7);  // Echo 핀
Timer       tim;

int begin = 0;
int end = 0;

void rising(){
    begin = tim.read_us();
}

void falling(){
    end = tim.read_us();
}

int main(void){
    float distance = 0;
    
    trig.period_ms(60);     // period      = 60ms
    trig.pulsewidth_us(10); // pulse-width = 10us
    
    echo.rise(&rising);
    echo.fall(&falling);
    
    tim.start();
    
    while(1){
        distance =  (float)(end - begin) / 58; // [cm]
        pc.printf("Distance = %.2f[cm]\r\n", distance);
        wait(0.5);
    }
    
} 


```
{% endtab %}
{% endtabs %}

###

## Ticker

{% tabs %}
{% tab title="mbed" %}
```cpp
#include "mbed.h"

Ticker     tick;
DigitalOut led(LED1);

void INT(){
    led = !led;      
}
int main(void){
    tick.attach(&INT, 1); // 1초마다 LED blink
    
    while(1);
}
```
{% endtab %}
{% endtabs %}

###

## ADC

{% tabs %}
{% tab title="mbed" %}
```cpp
#include "mbed.h"
                                                
Serial      pc(USBTX, USBRX, 9600);                                                
AnalogIn    CDS(A0);
DigitalOut  led(LED1);

int main() {
    float measure;
        
    while(1) {
        measure = CDS.read(); // mapping(0~3.3V -> 0.0~1.0)
        measure = measure * 3300; // [mV] (0.0~1.0 -> 0~3300[mV])
        pc.printf("measure = %f mV\n\r", measure);
        
        if (measure < 200) led = 1;
        else               led = 0;
        
        wait(0.2); 
    }
}
```
{% endtab %}
{% endtabs %}

## UART

{% tabs %}
{% tab title="mbed" %}
```cpp
#include "mbed.h"
 
Serial  uart(USBTX, USBRX, 9600);
 
int main(){
    char RXD;    
    while(1)
    {        
        if(uart.readable()){
            RXD = uart.getc();
            uart.printf("%c", RXD);
        }
    }
}
```
{% endtab %}

{% tab title="EC" %}

```cpp
/**
******************************************************************************
* @author  SSSLAB
* @Mod		 2023-10-24 by YKKIM  	
* @brief   Embedded Controller:  Sample Code _ UART2
* 
******************************************************************************
*/
#include "stm32f4xx.h"
#include "ecGPIO.h"
#include "ecRCC.h"
#include "ecUART_simple_student.h"
#include "ecSysTick.h"

int cnt = 0;

void setup(void);

int main(void) {
	// Initialiization --------------------------------------------------------
	setup();
	printf("Hello Nucleo\r\n");
	
	while(1){
		
		printf("Time: %d s\r\n", cnt);
		delay_ms(1000);
		cnt++;
	}

}

// Initialiization 
void setup(void)
{
	RCC_PLL_init();
	SysTick_init();
	UART2_init();
}
```

{% endtab %}

{% endtabs %}

###

##

