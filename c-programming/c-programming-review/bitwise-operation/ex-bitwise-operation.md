# Exercise: Bitwise Operation

## Bitwise Exercise

### Exercise: Bitwise operation 1

What will be the output ?

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

### Exercise: Bitwise operation 2

What will be the output?   

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



### Exercise\_3: Bit manipulation

Fill in the blanks

```cpp
//Exercise_1: Turning ON LEDs of Port A(PA)
//Read PA6, 6th from LSB
PA =b00001111;   	      //LED0 is LSB, Set to turn on LED
uint8t bits=PA&________; // check bit of a6

// Exercise_2: Turning ON LEDs of Port A(PA)
// assume 8 LEDs are connected to Digital Out pins of PA

PA =b00001111;   	      //LED0 is LSB, Set to turn on LED
PA |=____________; 	    // turn ON LED4 
PA |=____________; 	    // turn ON LED4 and LED5

// Exercise_3: Turning off LEDs of Port A(PA)
PA=b00001111;           //LED0 is LSB, Set to turn on LED
PA &=________;           // turn off LED2 

```



