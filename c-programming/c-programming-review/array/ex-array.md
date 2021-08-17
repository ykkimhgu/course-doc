# Exercise: Array

## 1D Array exercise

### Exercise\_1: Passing 1-D Array to a Function

Declare and define the following functions in header files “EC\_Tutorial.h, EC\_Tutorial.c”

`void addVector(float _src1[], float _src2[], float _dst[], int _vecLength);` 

`void printVector (float _out[], int _vecLength);`

```cpp
#include “EC_Tutorial.h"

void main() 
{
// Exercise 1 ***********************************************
printf(“Exercise 1\n");

float x[4] = {1,2,3,4};
float y[4] = {5,6,7,8};
float out[4] = { 0 };
float out_dotProduct = 0;

int vecLength = 4;

addVector(x, y, out, vecLength);
printf("addVector result (x + y): ");
printVec(out, vecLength);

system("pause");
}

```



## 2D Array exercise

### 

