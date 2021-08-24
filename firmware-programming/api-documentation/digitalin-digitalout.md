# DigitalIn DigitalOut



## Digital In/Out Class Reference

\#include &lt;EC\_GPIO.h&gt;

```cpp

class EC_DigitalIn
{
public:
    EC_DigitalIn(GPIO_TypeDef *Port, int pin); 
    ~EC_DigitalIn();
    int read();
		void pupdr(int _pupd);
		
private:
			GPIO_TypeDef *Port_t;
			int	pin_t;
			int mode_t;	
			int val_t;	
};


class EC_DigitalOut
{
public:
		EC_DigitalOut(GPIO_TypeDef *Port, int pin);
    ~EC_DigitalOut()
    void write(int _outVal);
  	void pupdr(int _pupd);
		void otype(int _type);
		void ospeed(int _speed);
private:
		GPIO_TypeDef *Port_t;
		int	pin_t;
		int mode_t;	
};
```



### EC\_DigitalIn\(GPIO\_TypeDef \*Port, int pin\)

Create a DigitalOut connected to the specified pin. 

#### Parameter

* int pin:  DigitalOut pin to connect to.  0~31
* Port:  GPIOA~GPIOH

### int read \(\)

Return the output setting, represented as 0 or 1 \(int\)

### void write \( int \_outVal\)

Set the output, specified as 0 or 1 \(int\)

#### Parameters 

* int \_outVal:  An integer specifying the pin output value, 0 for logical 0, 1 for logical 1



