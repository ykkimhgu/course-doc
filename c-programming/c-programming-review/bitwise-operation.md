# Bitwise Operation

## Lecture PPT

{% file src="../../.gitbook/assets/Tutorial_C_Bitwise_2023.pdf" %}

## Online Lesson

**코딩도장 핵심요약**: [비트연산자 사용하기 핵심요약](https://dojang.io/mod/page/view.php?id=490)

### Bitwise Operation in C

자료형과 메모리 주소를 바이트 단위로 구분하여 사용하였습니다. 비트 연산자는 **바이트** 단위보다 더 작은 **비트** 단위로 연산하는 연산자입니다.

![](<../../.gitbook/assets/image (7).png>)

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Example

```cpp
	PA=PA | (1<<5);  	// set PA5 (as High) and mask others
	PA=PA & ~(1<<5);  	// reset PA5 (as LOW) and mask others
	bit = PA & (1<<5);  // check the bit5. bit=1 if  PA5 is 1
```

###

***

### Set flag: (**플래그 |= 마스크)**

> a |= (1 << k)

![](<../../.gitbook/assets/image (23).png>)

### Clear flag (**플래그 &= \~마스크)**

Example:

```cpp
unsigned char flag = 7;    // 7: 0000 0111
flag &= ~2;    

0000 0010
_________ ~
1111 1101
```

![](<../../.gitbook/assets/image (5) (1).png>)

###

### Toggle flag **(플래그 ^= 마스크)**

> a ^= 1<\<k

![](<../../.gitbook/assets/image (26).png>)

### Read a bit&#x20;

> (Method 1)  bit = a & (1<\<k) // Shift ‘bit 1’ left by k starting from LSB
>
> (Method 2)  bit = (a >>k) & (1) // Shift target ‘bit  right  by k&#x20;

Example:

<figure><img src="../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

### Read multiple bits

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>



### **Tip: Use Macro**

```cpp
#define BIT_SET(REG, BIT)			((REG) |= 1<< (BIT))
#define BIT_CLEAR(REG, BIT)			((REG) &= ~1<<(BIT))
#define BIT_READ(REG, BIT)			((REG)>>BIT & (1))
#define BITS_CLEAR(REG, BIT,NUM)			((REG) &= ~((0x1<< NUM)-1)<<(BIT))
#define BITS_SET(REG, BIT,NUM)			((REG) |= NUM<< (BIT))
```

##

***

## Exercise

### Exercise 1

What will be the output ?

* [C\_bitwise\_exercise\_01.c](https://github.com/ykkimhgu/Tutorial-C-Program/blob/main/bitwise/EC\_bitwise\_exercise\_01.c)

{% tabs %}
{% tab title="Exercise" %}
```cpp
#include <stdio.h>
 
int main()
{
    unsigned char num1 = 1;    // 0000 0001
    unsigned char num2 = 3;    // 0000 0011
    unsigned char num3 = 162;    // 162: 1010 0010
    unsigned char num4;
    num4 = ~num3;

 
    printf("%d\n", num1 & num2);    
    printf("%d\n", num1 | num2);    
    printf("%d\n", num1 ^ num2);    
    printf("%u\n", num4);    // 93: 0101 1101: num1의 비트 값을 뒤집음
 
    return 0;
}
```
{% endtab %}

{% tab title="Solution" %}
```cpp
#include <stdio.h>
 
int main()
{
    unsigned char num1 = 1;    // 0000 0001
    unsigned char num2 = 3;    // 0000 0011
    unsigned char num3 = 162;  // 162: 1010 0010
    unsigned char num4;
    num4 = ~num3;

    printf("%d\n", num1 & num2);    // 0000 0001: 01과 11을 비트 AND하면 01이 됨
    printf("%d\n", num1 | num2);    // 0000 0011: 01과 11을 비트 OR하면 11이 됨
    printf("%d\n", num1 ^ num2);    // 0000 0010: 01과 11을 비트 XOR하면 10이 됨
    printf("%u\n", num4);    // 93: 0101 1101: num1의 비트 값을 뒤집음
 
    return 0;
}
```
{% endtab %}
{% endtabs %}

###

### Exercise 2

What will be the output?

* [C\_bitwise\_exercise\_02.c](https://github.com/ykkimhgu/Tutorial-C-Program/blob/main/bitwise/EC\_bitwise\_exercise\_02.c)

{% tabs %}
{% tab title="Exercise" %}
```cpp
#include <stdio.h> 

int main()
{
    unsigned char num1 = 4;    // 4: 0000 0100
    unsigned char num2 = 4;    // 4: 0000 0100
    unsigned char num3 = 4;    // 4: 0000 0100
    unsigned char num4 = 4;    // 4: 0000 0100
    unsigned char num5 = 4;    // 4: 0000 0100
 
    num1 &= 5;     // 5(0000 0101) AND 연산 후 할당
    num2 |= 2;     // 2(0000 0010) OR 연산 후 할당
    num3 ^= 3;     // 3(0000 0011) XOR 연산 후 할당
    num4 <<= 2;    // 비트를 왼쪽으로 2번 이동한 후 할당
    num5 >>= 2;    // 비트를 오른쪽으로 2번 이동한 후 할당
 
    printf("%u\n", num1);    
    printf("%u\n", num2);    
    printf("%u\n", num3);    
    printf("%u\n", num4);    
    printf("%u\n", num5);    
 
    return 0;
}
```
{% endtab %}

{% tab title="Solution" %}
```cpp
#include <stdio.h> 

int main()
{
    unsigned char num1 = 4;    // 4: 0000 0100
    unsigned char num2 = 4;    // 4: 0000 0100
    unsigned char num3 = 4;    // 4: 0000 0100
    unsigned char num4 = 4;    // 4: 0000 0100
    unsigned char num5 = 4;    // 4: 0000 0100
 
    num1 &= 5;     // 5(0000 0101) AND 연산 후 할당
    num2 |= 2;     // 2(0000 0010) OR 연산 후 할당
    num3 ^= 3;     // 3(0000 0011) XOR 연산 후 할당
    num4 <<= 2;    // 비트를 왼쪽으로 2번 이동한 후 할당
    num5 >>= 2;    // 비트를 오른쪽으로 2번 이동한 후 할당
 
    printf("%u\n", num1);    //  4: 0000 0100: 100과 101을 비트 AND하면 100이 됨
    printf("%u\n", num2);    //  6: 0000 0110: 100과 010을 비트 OR하면 110이 됨
    printf("%u\n", num3);    //  7: 0000 0111: 100과 011을 비트 XOR하면 111이 됨
    printf("%u\n", num4);    // 16: 0001 0000: 100을 왼쪽으로 2번 이동하면 10000이 됨
    printf("%u\n", num5);    //  1: 0000 0001: 100을 오른쪽으로 2번 이동하면 1이 됨
 
    return 0;
}
```
{% endtab %}
{% endtabs %}

### Exercise 3

Fill in the blanks.

* [C\_bitwise\_exercise\_03.c](https://github.com/ykkimhgu/Tutorial-C-Program/blob/main/bitwise/EC\_bitwise\_exercise\_03.c)

{% tabs %}
{% tab title="Exercise" %}
```cpp
//Exercise_1: Turning ON LEDs of Port A(PA)
//Read PA6, 6th from LSB
PA = b00001111;                 // LED0 is LSB, Set to turn on LED
uint8_t bits = PA & ________;	// check bit of a6


// Exercise_2: Turning ON LEDs of Port A(PA)
// assume 8 LEDs are connected to Digital Out pins of PA
PA = b00001111;         // LED0 is LSB, Set to turn on LED
PA |= ____________; 	// turn ON LED4 
PA |= ____________; 	// turn ON LED4 and LED5


// Exercise_3: Turning off LEDs of Port A(PA)
PA = b00001111;         // LED0 is LSB, Set to turn on LED
PA &= ~(________);      // turn off LED2 
```
{% endtab %}

{% tab title="Solution" %}
```cpp
//Exercise_1: Turning ON LEDs of Port A(PA)
//Read PA6, 6th from LSB
PA = b00001111;                 // LED0 is LSB, Set to turn on LED
uint8_t bits = PA & (1 << 6);	// check bit of a6

// Exercise_2: Turning ON LEDs of Port A(PA)
// assume 8 LEDs are connected to Digital Out pins of PA
PA = b00001111;                 // LED0 is LSB, Set to turn on LED
PA |= (1 << 4); 	    // turn ON LED4 
PA |= (3 << 4); 	    // turn ON LED4 and LED5

// Exercise_3: Turning off LEDs of Port A(PA)
PA = b00001111;                 // LED0 is LSB, Set to turn on LED
PA &= ~(1 << 2);    	// turn off LED2 
```
{% endtab %}
{% endtabs %}

### Exercise 4

Download and Read instruction in the given sourcefile.

* [C\_bitwise\_exercise\_04.c](https://github.com/ykkimhgu/Tutorial-C-Program/blob/main/bitwise/EC\_bitwise\_exercise\_04.c)

Apply bitwise operations (Set HIGH,Toggle, Reset etc) as instructed.

```
#include <stdio.h>

void main() {

unsigned int a = 118;
printf("\n118 as binary = ");
dec2bin(a);

//read 8th bit of a :   a[7]
unsigned int bit_check = a & (1 << 7);
printf("\nresult1 = %d", bit_check);

// set 8th (a[7])  bit as HIGH
// YOUR CODE GOES HERE
printf("\nresult2 = ");
dec2bin(a);

}

/////////////////////////
void dec2bin(unsigned int n) {
	unsigned int a = 0x80;
	for (int i = 0; i < 8; i++) {
	if ((a & n) == a)
		printf("1");
	else
		printf("0");
	a = a>> 1;
	}
}
```
