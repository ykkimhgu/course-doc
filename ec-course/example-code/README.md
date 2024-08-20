---
description: EC vs Arduino vs mbed
---

# Sample code

## GPIO

### Blinking LED

{% tabs %}
{% tab title="EC_2024" %}
```cpp
#include "ecSTM32F4v2.h"

#define LED_PIN PA_5
#define BUTTON_PIN PC_13

// Initialiization 
void setup(void) {
	RCC_PLL_init();
	SysTick_init();
	GPIO_init(LED_PIN, OUTPUT);    
}
	
int main(void) { 
	setup();
	
	while(1){
		delay_ms(500);  
		GPIO_write(LED_PIN, LOW);
		delay_ms(500);  
		GPIO_write(LED_PIN, HIGH);
	}
}
```
{% endtab %}

{% tab title="EC" %}
```cpp
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
{% tab title="EC_2024" %}
```cpp
#include "ecSTM32F4v2.h"

#define LED_PIN PA_5
#define BUTTON_PIN PC_13

// Initialiization 
void setup(void) {
	RCC_HSI_init();
	// initialize the pushbutton pin as an input:
	GPIO_init(BUTTON_PIN, INPUT);  
	// initialize the LED pin as an output:
	GPIO_init(LED_PIN, OUTPUT);    
}
	
int main(void) { 
 	setup();
	int buttonState=0;
	
	while(1){
		// check if the pushbutton is pressed. Turn LED on/off accordingly:
		buttonState = GPIO_read(BUTTON_PIN);
		if(buttonState)	GPIO_write(LED_PIN, LOW);
		else 		GPIO_write(LED_PIN, HIGH);
	}
}
```
{% endtab %}

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
{% tab title="EC_2024" %}
```cpp
#include "ecSTM32F4v2.h"

#define BUTTON_PIN PC_13

// Initialiization 
void setup(void)
{
	RCC_PLL_init();
	SysTick_init();
	GPIO_init(BUTTON_PIN, INPUT);  
	sevensegment_init();
}
	
int main(void) { 
	setup();
	unsigned int cnt = 0;
		
	while(1){
		// display 7-segment 0 to 9
		sevensegment_decode(cnt % 10);
		// increase number with button push
		if(GPIO_read(BUTTON_PIN) == 0) {
			cnt++; 
			delay_ms(500);
		}
		if (cnt > 9) cnt = 0;
	}
}
```
{% endtab %}

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
#include "ecGPIO.h"
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
{% tab title="EC_2024" %}
```cpp
#include "ecSTM32F4v2.h"

#define LED_PIN	PA_5
#define BUTTON_PIN PC_13

// Initialiization 
void setup(void)
{
	RCC_PLL_init();
	SysTick_init();
	GPIO_init(LED_PIN, OUTPUT);
	GPIO_init(BUTTON_PIN, INPUT);
	GPIO_pupd(BUTTON_PIN, EC_PD);
	// Priority Highest(0) External Interrupt 
	EXTI_init(BUTTON_PIN, FALL, 0);
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
{% tab title="EC_2024" %}
```cpp
#include "stm32f411xe.h"
#include "ecGPIO2.h"
#include "ecRCC2.h"
#include "ecSysTick2.h"

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

{% tab title="EC" %}
```cpp
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

## Timer

{% tabs %}
{% tab title="EC_2024" %}
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

## Timer Interrupt IRQ

{% tabs %}
{% tab title="EC_2024" %}
```cpp
#include "stm32f411xe.h"
#include "ecGPIO2.h"
#include "ecRCC2.h"
#include "ecTIM2.h"


#define LED_PIN	PA_5
uint32_t _count = 0;
void setup(void);


int main(void) {
	// Initialization --------------------------------------------------
	setup();
	
	// Infinite Loop ---------------------------------------------------
	while(1){}
}


// Initialization
void setup(void){
	RCC_PLL_init();				// System Clock = 84MHz
	GPIO_init(GPIOA, LED_PIN, OUTPUT);	// calls RCC_GPIOA_enable()
	TIM_UI_init(TIM2, 1);			// TIM2 Update-Event Interrupt every 1 msec 
	TIM_UI_enable(TIM2);
}

void TIM2_IRQHandler(void){
	if(is_UIF(TIM2)){			// Check UIF(update interrupt flag)
		_count++;
		if (_count > 1000) {
			LED_toggle();		// LED toggle every 1 sec
			_count = 0;
		}
		clear_UIF(TIM2); 		// Clear UI flag by writing 0
	}
}
```
{% endtab %}

{% tab title="EC" %}
```cpp
#include "stm32f411xe.h"
#include "ecGPIO.h"
#include "ecRCC.h"
#include "ecTIM.h"


#define LED_PIN	5
uint32_t _count = 0;
void setup(void);


int main(void) {
	// Initialization --------------------------------------------------
	setup();
	
	// Infinite Loop ---------------------------------------------------
	while(1){}
}


// Initialization
void setup(void){
	RCC_PLL_init();				// System Clock = 84MHz
	GPIO_init(GPIOA, LED_PIN, OUTPUT);	// calls RCC_GPIOA_enable()
	TIM_UI_init(TIM2, 1);			// TIM2 Update-Event Interrupt every 1 msec 
	TIM_UI_enable(TIM2);
}

void TIM2_IRQHandler(void){
	if(is_UIF(TIM2)){			// Check UIF(update interrupt flag)
		_count++;
		if (_count > 1000) {
			LED_toggle();		// LED toggle every 1 sec
			_count = 0;
		}
		clear_UIF(TIM2); 		// Clear UI flag by writing 0
	}
}
```
{% endtab %}

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

## PWM Out

### PWM out on LED

{% tabs %}
{% tab title="EC_2024" %}
```cpp
#include "stm32f411xe.h"
#include "math.h"

// #include "ecSTM32F4v2.h"
#include "ecPinNames.h"
#include "ecGPIO2.h"
#include "ecSysTick2.h"
#include "ecRCC2.h"
#include "ecTIM2.h"
#include "ecPWM2.h"

// Definition Button Pin & PWM Port, Pin
#define BUTTON_PIN PC_13
#define PWM_PIN PA_5

void setup(void);

int main(void) {
	// Initialization --------------------------------------------------
	setup();	
	
	// Infinite Loop ---------------------------------------------------
	while(1){
		LED_toggle();		
		for (int i=0; i<5; i++) {						
			PWM_duty(PWM_PIN, (float)0.2*i);			
			delay_ms(1000);
		}		
	}
}

// Initialiization 
void setup(void) {	
	RCC_PLL_init();
	SysTick_init();
		
	// PWM of 20 msec:  TIM2_CH1 (PA_5 AFmode)
	GPIO_init(PWM_PIN, EC_AF);
	PWM_init(PWM_PIN);	
	PWM_period(PWM_PIN, 20);   // 20 msec PWM period
}
```
{% endtab %}

{% tab title="EC" %}
```cpp
#include "stm32f411xe.h"
#include "math.h"

// #include "ecSTM32F411.h"
#include "ecPinNames.h"
#include "ecGPIO.h"
#include "ecSysTick.h"
#include "ecRCC.h"
#include "ecTIM.h"
#include "ecPWM.h"

// Definition Button Pin & PWM Port, Pin
#define BUTTON_PIN 13
#define PWM_PIN PA_5

void setup(void);

int main(void) {
	// Initialization --------------------------------------------------
	setup();	
	
	// Infinite Loop ---------------------------------------------------
	while(1){
		LED_toggle();		
		for (int i=0; i<5; i++) {						
			PWM_duty(PWM_PIN, (float)0.2*i);			
			delay_ms(1000);
		}		
	}
}

// Initialiization 
void setup(void) {	
	RCC_PLL_init();
	SysTick_init();
		
	// PWM of 20 msec:  TIM2_CH1 (PA_5 AFmode)
	GPIO_init(GPIOA, 5, EC_AF);
	PWM_init(PWM_PIN);	
	PWM_period(PWM_PIN, 20);   // 20 msec PWM period
}
```
{% endtab %}

{% tab title="EC_2022" %}
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

### PWM \_ DC Motor

{% tabs %}
{% tab title="EC_2024" %}
```cpp
#include "stm32f4xx.h"
#include "ecGPIO2.h"
#include "ecRCC2.h"
#include "ecTIM2.h"
#include "ecPWM2.h"
#include "ecPinNames.h"
#include "ecEXTI2.h"
#include "ecUART2.h"

#define DIR_PIN PC_2
#define MOTOR PA_0
#define BUTTON_PIN PC_13

float duty = 0.5f;
uint8_t pause_flag = 1;

void setup(void);

int main(void) {
	// Initialiization --------------------------------------------------------
	setup();
	printf("Hello Nucleo\r\n");
	
	// Inifinite Loop ----------------------------------------------------------
	while (1){
		PWM_duty(PA_0, duty);
	}
}

// Initialiization 
void setup(void)
{
	RCC_PLL_init();

	//UART2 Configuration
	UART2_init();
	
	// External Interrupt Button input: Falling, Pull-Up
	GPIO_init(BUTTON_PIN, INPUT);
	GPIO_pupd(BUTTON_PIN, EC_PU);
	EXTI_init(BUTTON_PIN, FALL, 0);

	// Direction Output Configuration
	GPIO_init(DIR_PIN, OUTPUT);
	GPIO_write(DIR_PIN, 0);

	// PWM Configuration
	PWM_init(PA_0);
	PWM_period_ms(PA_0, 1);		// PWM period: 1msec
}

void EXTI15_10_IRQHandler(void)
{
	if(is_pending_EXTI(BUTTON_PIN)){
		//When Button is pressed, it should PAUSE or CONTINUE motor run (flag)
		pause_flag ^= 1;
		duty *= (float)pause_flag;

		// Clear EXTI Pending
		clear_pending_EXTI(BUTTON_PIN);
	}
}
```
{% endtab %}

{% tab title="EC" %}
```cpp
#include "stm32f4xx.h"
#include "ecGPIO.h"
#include "ecRCC.h"
#include "ecTIM.h"
#include "ecPWM.h"
#include "ecPinNames.h"
#include "ecEXTI.h"
#include "ecUART.h"

#define DIR_PIN 2
#define MOTOR PA_0

float duty = 0.5f;
uint8_t pause_flag = 1;

void setup(void);

int main(void) {
	// Initialiization --------------------------------------------------------
	setup();
	printf("Hello Nucleo\r\n");
	
	// Inifinite Loop ----------------------------------------------------------
	while (1){
		PWM_duty(PA_0, duty);
	}
}

// Initialiization 
void setup(void)
{
	RCC_PLL_init();

	//UART2 Configuration
	UART2_init();
	
	// External Interrupt Button input: Falling, Pull-Up
	GPIO_init(GPIOC, BUTTON_PIN, INPUT);
	GPIO_pupd(GPIOC, BUTTON_PIN, EC_PU);
	EXTI_init(GPIOC, BUTTON_PIN, FALL, 0);

	// Direction Output Configuration
	GPIO_init(GPIOC, DIR_PIN, OUTPUT);
	GPIO_write(GPIOC, DIR_PIN, 0);

	// PWM Configuration
	PWM_init(PA_0);
	PWM_period_ms(PA_0, 1);		// PWM period: 1msec
}

void EXTI15_10_IRQHandler(void)
{
	if(is_pending_EXTI(BUTTON_PIN)){
		//When Button is pressed, it should PAUSE or CONTINUE motor run (flag)
		pause_flag ^= 1;
		duty *= (float)pause_flag;

		// Clear EXTI Pending
		clear_pending_EXTI(BUTTON_PIN);
	}
}
```
{% endtab %}

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

## Stepper Motor

{% tabs %}
{% tab title="EC_2024" %}
```cpp
#include "stm32f411xe.h"
#include "ecGPIO2.h"
#include "ecRCC2.h"
#include "ecTIM2.h"
#include "ecEXTI2.h"
#include "ecSysTick2.h"
#include "ecStepper2.h"

void setup(void);
	
int main(void) { 
	// Initialiization --------------------------------------------------------
	setup();
	
	Stepper_step(2048, 1, FULL);  // (Step : 1024, Direction : 0 or 1, Mode : FULL or HALF)
	
	// Inifinite Loop ----------------------------------------------------------
	while(1){;}
}

// Initialiization 
void setup(void)
{	
	
	RCC_PLL_init();                                 // System Clock = 84MHz
	SysTick_init();                                 // Systick init
	
	EXTI_init(BUTTON_PIN,FALL,0);             // External Interrupt Setting
	GPIO_init(BUTTON_PIN, EC_DIN);           // GPIOC pin13 initialization

	Stepper_init(PB_10,PB_4,PB_5,PB_3); // Stepper GPIO pin initialization
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

{% tab title="EC" %}
```cpp
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
	
	Stepper_step(2048, 1, FULL);  // (Step : 1024, Direction : 0 or 1, Mode : FULL or HALF)
	
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

{% tab title="mbed" %}
```cpp
```
{% endtab %}
{% endtabs %}

## Timre Input Capture: Ultrasonic Distance Sensor&#x20;

{% tabs %}
{% tab title="EC_2024" %}
```cpp
#include "stm32f411xe.h"
#include "math.h"
#include "ecGPIO2.h"
#include "ecRCC2.h"
#include "ecTIM2.h"
#include "ecUART2_simple_student.h"
#include "ecSysTick2.h"

uint32_t ovf_cnt = 0;
uint32_t ccr1 = 0;
uint32_t ccr2 = 0;
float    period = 0;

void setup(void);

int main(void){
	
	setup();
	while(1){
		printf("period = %f[msec]\r\n", period);		// print out the period on TeraTerm
		delay_ms(100);
	}
}


void setup(void) {	
	// Configuration Clock PLL
	RCC_PLL_init();
	
	// UART2 Configuration to use printf()
	UART2_init();
	
	// SysTick Configuration to use delay_ms()
	SysTick_init();

	// Input Capture Configuration PA_0(TIM2, 1)
	ICAP_init(PA_0);
	
	// Priority Configuration
	NVIC_SetPriority(TIM2_IRQn, 2);						// Set the priority of TIM2 interrupt request
	NVIC_EnableIRQ(TIM2_IRQn);							// TIM2 interrupt request enable
}

// Timer2 IRQ Handler (timer & Input Capture)
void TIM2_IRQHandler(void){
	if(is_UIF(TIM2)){                  // If Update-event interrupt Occurs
		// Handle overflow
		ovf_cnt++;
		
		clear_UIF(TIM2);   					// clear update-event interrupt flag
	}
	if(is_CCIF(TIM2, IC_1)){				// if CC interrupt occurs	
		// Calculate the period of 1Hz pulse
		ccr2 = ICAP_capture(TIM2, IC_1);						// capture counter value
		period = ((ccr2 - ccr1) + ovf_cnt * (TIM2->ARR + 1)) / 1000; 		// calculate the period with ovf_cnt, ccr1, and ccr2
		
		ccr1 = ccr2;
		ovf_cnt = 0;
		
		clear_CCIF(TIM2, IC_1);	// clear capture/compare interrupt flag ( it is also cleared by reading TIM2_CCR1)
	}
}
```
{% endtab %}

{% tab title="EC" %}
```cpp
#include "stm32f411xe.h"
#include "math.h"
#include "ecGPIO.h"
#include "ecRCC.h"
#include "ecTIM.h"
#include "ecUART_simple_student.h"
#include "ecSysTick.h"

uint32_t ovf_cnt = 0;
uint32_t ccr1 = 0;
uint32_t ccr2 = 0;
float    period = 0;

void setup(void);

int main(void){
	
	setup();
	while(1){
		printf("period = %f[msec]\r\n", period);		// print out the period on TeraTerm
		delay_ms(100);
	}
}


void setup(void) {	
	// Configuration Clock PLL
	RCC_PLL_init();
	
	// UART2 Configuration to use printf()
	UART2_init();
	
	// SysTick Configuration to use delay_ms()
	SysTick_init();

	// Input Capture Configuration PA_0(TIM2, 1)
	ICAP_init(PA_0);
	
	// Priority Configuration
	NVIC_SetPriority(TIM2_IRQn, 2);						// Set the priority of TIM2 interrupt request
	NVIC_EnableIRQ(TIM2_IRQn);							// TIM2 interrupt request enable
}

// Timer2 IRQ Handler (timer & Input Capture)
void TIM2_IRQHandler(void){
	if(is_UIF(TIM2)){                  // If Update-event interrupt Occurs
		// Handle overflow
		ovf_cnt++;
		
		clear_UIF(TIM2);   					// clear update-event interrupt flag
	}
	if(is_CCIF(TIM2, IC_1)){				// if CC interrupt occurs	
		// Calculate the period of 1Hz pulse
		ccr2 = ICAP_capture(TIM2, IC_1);						// capture counter value
		period = ((ccr2 - ccr1) + ovf_cnt * (TIM2->ARR + 1)) / 1000; 		// calculate the period with ovf_cnt, ccr1, and ccr2
		
		ccr1 = ccr2;
		ovf_cnt = 0;
		
		clear_CCIF(TIM2, IC_1);	// clear capture/compare interrupt flag ( it is also cleared by reading TIM2_CCR1)
	}
}
```
{% endtab %}

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

## ADC

{% tabs %}
{% tab title="EC_2024_single" %}
```cpp
#include "stm32f411xe.h"
#include "ecGPIO2.h"
#include "ecRCC2.h"
#include "ecTIM2.h"
#include "ecSysTick2.h"
#include "ecUART2.h"
#include "ecADC2.h"
#include "ecPinNames.h"

//IR parameter//
uint32_t IR;

void setup(void);
	
int main(void) { 
	
	// Initialiization --------------------------------------------------------
	setup();
	
	// Inifinite Loop ----------------------------------------------------------
	while(1){
		printf("IR = %d \r\n",IR);
		printf("\r\n");
		delay_ms(1000);
	}
}

// Initialiization 
void setup(void)
{	
	RCC_PLL_init();                         // System Clock = 84MHz
	UART2_init();
	SysTick_init();
	
	// ADC setting
  ADC_init(PB_1);
	
}

void ADC_IRQHandler(void){
	if((is_ADC_OVR())){
		clear_ADC_OVR();
	}
	
	if(is_ADC_EOC()){       //after finishing sequence
			IR = ADC_read();
	}
}

```
{% endtab %}

{% tab title="EC_2024_multi" %}
```cpp
#include "stm32f411xe.h"
#include "ecGPIO2.h"
#include "ecRCC2.h"
#include "ecTIM2.h"
#include "ecSysTick2.h"
#include "ecUART2.h"
#include "ecADC2.h"
#include "ecPinNames.h"

//IR parameter//
uint32_t IR1_val, IR2_val;
int flag = 0;
PinName_t seqCHn[2] = {PB_0, PB_1};

void setup(void);
	
int main(void) { 
	

	// Initialiization --------------------------------------------------------
	setup();
	
	// Inifinite Loop ----------------------------------------------------------
	while(1){
		printf("IR1 = %d \r\n",IR1_val);
		printf("IR2 = %d \r\n",IR2_val);
		printf("\r\n");
		
		delay_ms(1000);
	}
}

// Initialiization 
void setup(void)
{	
	RCC_PLL_init();                         // System Clock = 84MHz
	UART2_init();							// UART2 Init
	SysTick_init();							// SysTick Init
	
	// ADC Init
	ADC_init(PB_0);
	ADC_init(PB_1);

	// ADC channel sequence setting
	ADC_sequence(seqCHn, 2);
	
}


void ADC_IRQHandler(void){
	if((is_ADC_OVR())){
		clear_ADC_OVR();
	}
	
	if(is_ADC_EOC()){       //after finishing sequence
		if (flag==0)
			IR1_val = ADC_read();  
		else if (flag==1)
			IR2_val = ADC_read();
			
		flag = !flag;
	}
}
```
{% endtab %}

{% tab title="EC_single" %}
```cpp
#include "stm32f411xe.h"
#include "ecGPIO.h"
#include "ecRCC.h"
#include "ecTIM.h"
#include "ecSysTick.h"
#include "ecUART.h"
#include "ecADC.h"
#include "ecPinNames.h"

//IR parameter//
uint32_t IR;

void setup(void);
	
int main(void) { 
	
	// Initialiization --------------------------------------------------------
	setup();
	
	// Inifinite Loop ----------------------------------------------------------
	while(1){
		printf("IR = %d \r\n",IR);
		printf("\r\n");
		delay_ms(1000);
	}
}

// Initialiization 
void setup(void)
{	
	RCC_PLL_init();                         // System Clock = 84MHz
	UART2_init();
	SysTick_init();
	
	// ADC setting
  ADC_init(PB_1);
	
}

void ADC_IRQHandler(void){
	if((is_ADC_OVR())){
		clear_ADC_OVR();
	}
	
	if(is_ADC_EOC()){       //after finishing sequence
			IR = ADC_read();
	}
}

```
{% endtab %}

{% tab title="EC_multi" %}
```cpp
#include "stm32f411xe.h"
#include "ecGPIO.h"
#include "ecRCC.h"
#include "ecTIM.h"
#include "ecSysTick.h"
#include "ecUART.h"
#include "ecADC.h"
#include "ecPinNames.h"

//IR parameter//
uint32_t IR1_val, IR2_val;
int flag = 0;
PinName_t seqCHn[2] = {PB_0, PB_1};

void setup(void);
	
int main(void) { 
	

	// Initialiization --------------------------------------------------------
	setup();
	
	// Inifinite Loop ----------------------------------------------------------
	while(1){
		printf("IR1 = %d \r\n",IR1_val);
		printf("IR2 = %d \r\n",IR2_val);
		printf("\r\n");
		
		delay_ms(1000);
	}
}

// Initialiization 
void setup(void)
{	
	RCC_PLL_init();                         // System Clock = 84MHz
	UART2_init();							// UART2 Init
	SysTick_init();							// SysTick Init
	
	// ADC Init
	ADC_init(PB_0);
	ADC_init(PB_1);

	// ADC channel sequence setting
	ADC_sequence(seqCHn, 2);
	
}


void ADC_IRQHandler(void){
	if((is_ADC_OVR())){
		clear_ADC_OVR();
	}
	
	if(is_ADC_EOC()){       //after finishing sequence
		if (flag==0)
			IR1_val = ADC_read();  
		else if (flag==1)
			IR2_val = ADC_read();
			
		flag = !flag;
	}
}
```
{% endtab %}

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

## JADC

{% tabs %}
{% tab title="EC_2024" %}
```cpp
#include "stm32f411xe.h"
#include "ecGPIO2.h"
#include "ecRCC2.h"
#include "ecTIM2.h"
#include "ecSysTick2.h"
#include "ecUART2.h"
#include "ecADC2.h"
#include "ecPinNames.h"

//IR parameter//
uint32_t IR1_val, IR2_val;
PinName_t seqCHn[2] = {PB_0, PB_1};

void setup(void);

int main(void) { 
	// Initialiization --------------------------------------------------------
	setup();
	
	// Inifinite Loop ----------------------------------------------------------
	while(1){
		printf("IR1 = %d \r\n",IR1_val);
		printf("IR2 = %d \r\n",IR2_val);
		printf("\r\n");
		
		delay_ms(1000);
	}
}

// Initialiization 
void setup(void)
{	
	RCC_PLL_init();                         // System Clock = 84MHz
	UART2_init();							// UART2 Init
	SysTick_init();							// SysTick Init
	
	// JADC Init
	JADC_init(PB_0);
	JADC_init(PB_1);

	// JADC channel sequence setting
	JADC_sequence(seqCHn, 2);
}


void ADC_IRQHandler(void){
	if(is_ADC_OVR())
		clear_ADC_OVR();
	
	if(is_ADC_JEOC()){		// after finishing sequence
		IR1_val = JADC_read(1);
		IR2_val = JADC_read(2);

		clear_ADC_JEOC();
	}
}
```
{% endtab %}

{% tab title="EC" %}
```cpp
#include "stm32f411xe.h"
#include "ecGPIO.h"
#include "ecRCC.h"
#include "ecTIM.h"
#include "ecSysTick.h"
#include "ecUART.h"
#include "ecADC.h"
#include "ecPinNames.h"

//IR parameter//
uint32_t IR1_val, IR2_val;
PinName_t seqCHn[2] = {PB_0, PB_1};

void setup(void);

int main(void) { 
	// Initialiization --------------------------------------------------------
	setup();
	
	// Inifinite Loop ----------------------------------------------------------
	while(1){
		printf("IR1 = %d \r\n",IR1_val);
		printf("IR2 = %d \r\n",IR2_val);
		printf("\r\n");
		
		delay_ms(1000);
	}
}

// Initialiization 
void setup(void)
{	
	RCC_PLL_init();                         // System Clock = 84MHz
	UART2_init();							// UART2 Init
	SysTick_init();							// SysTick Init
	
	// JADC Init
	JADC_init(PB_0);
	JADC_init(PB_1);

	// JADC channel sequence setting
	JADC_sequence(seqCHn, 2);
}


void ADC_IRQHandler(void){
	if(is_ADC_OVR())
		clear_ADC_OVR();
	
	if(is_ADC_JEOC()){		// after finishing sequence
		IR1_val = JADC_read(1);
		IR2_val = JADC_read(2);

		clear_ADC_JEOC();
	}
}
```
{% endtab %}
{% endtabs %}

## UART

{% tabs %}
{% tab title="EC_2024_1" %}
```cpp
#include "stm32f4xx.h"
#include "ecGPIO2.h"
#include "ecRCC2.h"
#include "ecUART2.h"
#include "ecSysTick2.h"


static volatile uint8_t PC_Data = 0;
static volatile uint8_t BT_Data = 0;
uint8_t PC_string[]="Loop:\r\n";

void setup(void){
	RCC_PLL_init();
	SysTick_init();
	
	// USART2: USB serial init
	UART2_init();
	UART2_baud(BAUD_9600);

	// USART1: BT serial init 
	UART1_init();
	UART1_baud(BAUD_9600);
}

int main(void){	
	setup();
	printf("MCU Initialized\r\n");	
	while(1){
		// USART Receive: Use Interrupt only
		// USART Transmit:  Interrupt or Polling
		USART2_write(PC_string, 7);
		delay_ms(2000);        
	}
}

void USART2_IRQHandler(){          		// USART2 RX Interrupt : Recommended
	if(is_USART2_RXNE()){
		PC_Data = USART2_read();		// RX from UART2 (PC)
		USART2_write(&PC_Data,1);		// TX to USART2	 (PC)	 Echo of keyboard typing		
	}
}


void USART1_IRQHandler(){          		// USART2 RX Interrupt : Recommended
	if(is_USART1_RXNE()){
		BT_Data = USART1_read();		// RX from UART1 (BT)		
		printf("RX: %c \r\n",BT_Data); // TX to USART2(PC)
	}
}
```
{% endtab %}

{% tab title="EC_2024_2" %}
```cpp
#include "stm32f4xx.h"
#include "ecGPIO2.h"
#include "ecRCC2.h"
#include "ecUART2.h"
#include "ecSysTick2.h"

#define MAX_BUF 	10
#define END_CHAR 	13

static volatile uint8_t buffer[MAX_BUF]={0, };
static volatile uint8_t PC_string[MAX_BUF]={0, };
static volatile uint8_t PC_data = 0;

static volatile int idx = 0;
static volatile int bReceive =0;

void setup(void){
	RCC_PLL_init();
	SysTick_init();
	
	// USART2: USB serial init
	UART2_init();
	UART2_baud(BAUD_9600);

	// USART1: BT serial init 
	UART1_init();
	UART1_baud(BAUD_9600);
}

int main(void){	
	setup();
	printf("MCU Initialized\r\n");	
	
	while(1){
		if (bReceive == 1){
			printf("PC_string: %s\r\n", PC_string);				
			bReceive = 0;
		}
	}
}

void USART2_IRQHandler(){          		// USART2 RX Interrupt : Recommended
	if(is_USART2_RXNE()){
		PC_data = USART2_read();		// RX from UART2 (PC)
		USART2_write(&PC_data,1);		// TX to USART2	 (PC)	 Echo of keyboard typing
		
		// Creates a String from serial character receive				
		if(PC_data != END_CHAR && (idx < MAX_BUF)){	
			buffer[idx] = PC_data;
			idx++;
		}
		else if (PC_data== END_CHAR) {						
			bReceive = 1;
			// reset PC_string;
			memset(PC_string, 0, sizeof(char) * MAX_BUF);
			// copy to PC_string;
			memcpy(PC_string, buffer, sizeof(char) * idx);
			// reset buffer
			memset(buffer, 0, sizeof(char) * MAX_BUF);	
			idx = 0;
		}
		else{				//  if(idx >= MAX_BUF)			
			idx = 0;							
			// reset PC_string;
			memset(PC_string, 0, sizeof(char) * MAX_BUF);
			// reset buffer
			memset(buffer, 0, sizeof(char) * MAX_BUF);	// reset buffer
			printf("ERROR : Too long string\r\n");
		}
	}
}
```
{% endtab %}

{% tab title="EC_2023_1" %}
```cpp
#include "stm32f4xx.h"
#include "ecGPIO.h"
#include "ecRCC.h"
#include "ecUART.h"
#include "ecSysTick.h"


static volatile uint8_t PC_Data = 0;
static volatile uint8_t BT_Data = 0;
uint8_t PC_string[]="Loop:\r\n";

void setup(void){
	RCC_PLL_init();
	SysTick_init();
	
	// USART2: USB serial init
	UART2_init();
	UART2_baud(BAUD_9600);

	// USART1: BT serial init 
	UART1_init();
	UART1_baud(BAUD_9600);
}

int main(void){	
	setup();
	printf("MCU Initialized\r\n");	
	while(1){
		// USART Receive: Use Interrupt only
		// USART Transmit:  Interrupt or Polling
		USART2_write(PC_string, 7);
		delay_ms(2000);        
	}
}

void USART2_IRQHandler(){          		// USART2 RX Interrupt : Recommended
	if(is_USART2_RXNE()){
		PC_Data = USART2_read();		// RX from UART2 (PC)
		USART2_write(&PC_Data,1);		// TX to USART2	 (PC)	 Echo of keyboard typing		
	}
}


void USART1_IRQHandler(){          		// USART2 RX Interrupt : Recommended
	if(is_USART1_RXNE()){
		BT_Data = USART1_read();		// RX from UART1 (BT)		
		printf("RX: %c \r\n",BT_Data); // TX to USART2(PC)
	}
}
```
{% endtab %}

{% tab title="EC_2023_2" %}
```cpp
#include "stm32f4xx.h"
#include "ecGPIO.h"
#include "ecRCC.h"
#include "ecUART.h"
#include "ecSysTick.h"

#define MAX_BUF 	10
#define END_CHAR 	13

static volatile uint8_t buffer[MAX_BUF]={0, };
static volatile uint8_t PC_string[MAX_BUF]={0, };
static volatile uint8_t PC_data = 0;

static volatile int idx = 0;
static volatile int bReceive =0;

void setup(void){
	RCC_PLL_init();
	SysTick_init();
	
	// USART2: USB serial init
	UART2_init();
	UART2_baud(BAUD_9600);

	// USART1: BT serial init 
	UART1_init();
	UART1_baud(BAUD_9600);
}

int main(void){	
	setup();
	printf("MCU Initialized\r\n");	
	
	while(1){
		if (bReceive == 1){
			printf("PC_string: %s\r\n", PC_string);				
			bReceive = 0;
		}
	}
}

void USART2_IRQHandler(){          		// USART2 RX Interrupt : Recommended
	if(is_USART2_RXNE()){
		PC_data = USART2_read();		// RX from UART2 (PC)
		USART2_write(&PC_data,1);		// TX to USART2	 (PC)	 Echo of keyboard typing
		
		// Creates a String from serial character receive				
		if(PC_data != END_CHAR && (idx < MAX_BUF)){	
			buffer[idx] = PC_data;
			idx++;
		}
		else if (PC_data== END_CHAR) {						
			bReceive = 1;
			// reset PC_string;
			memset(PC_string, 0, sizeof(char) * MAX_BUF);
			// copy to PC_string;
			memcpy(PC_string, buffer, sizeof(char) * idx);
			// reset buffer
			memset(buffer, 0, sizeof(char) * MAX_BUF);	
			idx = 0;
		}
		else{				//  if(idx >= MAX_BUF)			
			idx = 0;							
			// reset PC_string;
			memset(PC_string, 0, sizeof(char) * MAX_BUF);
			// reset buffer
			memset(buffer, 0, sizeof(char) * MAX_BUF);	// reset buffer
			printf("ERROR : Too long string\r\n");
		}
	}
}
```
{% endtab %}

{% tab title="EC_2022" %}
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
{% endtabs %}
