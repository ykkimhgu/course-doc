# Pointer

## Lesson

**코딩도장 핵심요약**

[ 포인터 사용하기 핵심요약](https://dojang.io/mod/page/view.php?id=605)



What are Pointers?

A pointer is a variable whose value is the address of another variable, i.e., direct address of the memory location.

\(a\) Define a pointer variable:     int \*ptr;

\(b\) Assign the address of a variable to a pointer:     ptr = &var;  

\(c\) Access the value at the address available in the pointer variable:    int value = \*ptr

![](../../.gitbook/assets/image%20%2875%29.png)

#### For 32-Bit Word system \(e.g. MCU\)

32-CPU: 1 word = 4 bytes \(32bit\) 

* Memory: byte units 
* Int : 4 byte 
* Pointer variable: 4 byte  \(default\)

![TCP school.com](../../.gitbook/assets/image%20%2876%29.png)

![](../../.gitbook/assets/image%20%2872%29.png)

![](../../.gitbook/assets/image%20%2874%29.png)



## Example Code

```cpp

#include <stdio.h>
#include <stdlib.h>

int main() {
int  var = 20;   
double a[4] = { 2, 2, 3, 4 };

// Pointer Declaration
int  *ptr;
double *ptr2 = NULL;// good practice for not Addr. assgined pointer
double *ptr3 = NULL;

// Pointer Assignment
ptr = &var;// store address of var in pointer variable
ptr2 = a;
ptr3 = &a[0];

printf("Address of var: %x\n", &var);
printf("Address of a  : %x\n", &a);
printf("Address of a[0]: %x\n", &a[0]);

// Using Pointer - Access the values pointed by Ptr
printf("\nAddress stored in \n  ptr: %x \n ptr2: %x  \n ptr3: %x\n", ptr, ptr2, ptr3);
printf("Value of \n  *ptr: %d \n *ptr2: %.1f  \n *ptr3: %.1f\n", *ptr, *ptr2, *ptr3);

system("pause");
return 0;
}

```

![](../../.gitbook/assets/image%20%2871%29.png)



## Exercise

* [Online C Compiler](https://www.onlinegdb.com/online_c_compiler)

* [Exercise Code](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/pointer-array)

* [Exercise-Solution Code](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/pointer-array/solution)

### Exercise 1

다음 소스 코드를 완성하여 10과 20이 각 줄에 출력되게 만드세요.

```c
#include <stdio.h>

int main()
{
    int *numPtr;
    int num1 = 10;
    int num2 = 20;

    ① ________________
    printf("%d\n", *numPtr);

    ②_________________
    printf("%d\n", *numPtr);

    return 0;
}
```

실행 결과

```
10
20
```

**Solution**

```
① numPtr = &num1;
② numPtr = &num2;
```



### Exercise 2

```c
int x =10;            
double y=2.5;
int *ptrX = &x;      
int *ptrY = &y;

/*
-Print the address of variable ‘x’
-Print the address of variable ‘y’
-Print the value of pointer ‘ptrX ‘
-Print the address of pointer ‘ptrX ‘
-Print the size of pointer ‘ptrX ‘
*/

/*
-Print the value of pointer ‘ptrY ‘
-Print the address of pointer ‘ptrY ‘
-Print the size of pointer ‘ptrY 
*/
```

**Solution**

Exercise Solution File