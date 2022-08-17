# Bitwise Operation

## Bitwise Operation in C

자료형과 메모리 주소를 바이트 단위로 구분하여 사용하였습니다. 비트 연산자는 **바이트** 단위보다 더 작은 **비트** 단위로 연산하는 연산자입니다.

## Reading Assignment: 코딩도장 레슨

아래 레슨 꼭 읽어보세요

* [23.0 비트 연산자 사용하기](https://dojang.io/mod/page/view.php?id=172)
* [23.1 비트 AND, OR, XOR 연산자 사용하기](https://dojang.io/mod/page/view.php?id=173)
* [23.3 시프트 연산자 사용하기](https://dojang.io/mod/page/view.php?id=174)
* [24.4 비트 연산자로 플래그 처리하기](https://dojang.io/mod/page/view.php?id=184)

## Summary

![](<../../../.gitbook/assets/image (7).png>)

![](<../../../.gitbook/assets/image (15).png>)

#### Example:

```cpp
	PA=PA | (1<<5);  		// set PA5 (as High) and mask others
	PA=PA & ~(1<<5);  	// reset PA5 (as LOW) and mask others
	bit = PA & (1<<5);  // check the bit5. bit=1 if  PA5 is 1
```

####

### Set flag: (**플래그 |= 마스크)**

> a |= (1 << k)

Example:

![](https://dojang.io/pluginfile.php/247/mod\_page/content/32/unit24-9.png)

Example:

![](<../../../.gitbook/assets/image (23).png>)

### Clear flag (**플래그 &= \~마스크)**

Example:

```cpp
unsigned char flag = 7;    // 7: 0000 0111
flag &= ~2;    

0000 0010
_________ ~
1111 1101
```

![](https://dojang.io/pluginfile.php/247/mod\_page/content/32/unit24-10.png)

Example:

![](<../../../.gitbook/assets/image (5).png>)

### Toggle flag **(플래그 ^= 마스크)**

> a ^= 1<\<k

Example:

![](https://dojang.io/pluginfile.php/247/mod\_page/content/32/unit24-11.png)

Example:

![](<../../../.gitbook/assets/image (26).png>)

### Read bits (**플래그 &= 마스크)**

> bit = a & (1<\<k) // Shift ‘bit 1’ left by k starting from LSB

Example:

![](<../../../.gitbook/assets/image (28).png>)

## Tip

```cpp
// Macro defined in stm32fxx.h

#define SET_BIT(REG, BIT)     ((REG) |= (BIT))
#define CLEAR_BIT(REG, BIT)   ((REG) &= ~(BIT))
#define READ_BIT(REG, BIT)    ((REG) & (BIT))
```
