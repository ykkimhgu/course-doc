# Pointer

## Introduction

What are Pointers?

A pointer is a variable whose value is the address of another variable, i.e., direct address of the memory location.

\(a\) Define a pointer variable:     int \*ptr;

\(b\) Assign the address of a variable to a pointer:     ptr = &var;  

\(c\) Access the value at the address available in the pointer variable:    int value = \*ptr

![](../../.gitbook/assets/image%20%2874%29.png)

### For 32-Bit MCU

32-CPU: 1 word = 4 bytes \(32bit\) 

* Memory: byte units 
* Int : 4 byte 
* Pointer variable: 4 byte  \(default\)

![TCP school.com](../../.gitbook/assets/image%20%2875%29.png)

![](../../.gitbook/assets/image%20%2872%29.png)

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

