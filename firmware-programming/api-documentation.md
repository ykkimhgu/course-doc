# API documentation



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



