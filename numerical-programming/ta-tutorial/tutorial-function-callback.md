# Tutorial: Passing a Function, Function callback

## Introduction

In this tutorial, we will learn how to pass a mathematical function f(x) as an input argument to another function.

#### Find derivative of given function

How can you use `myfunc()` within the function of `gradientFunc()` that finds the derivative?

* myFun(x): returns F(x)= 2x^3
* gradientFunc(myfunc): returns 6x^2

Example:

```cpp
void gradientFunc(double myfunc(const double x), double x[ ], double dydx[ ], int m);
```

## Calling a function within another function

There are several methods of calling a function within another function.

### Method 1

**Calling the function that is defined globally or in the same scope**

[Download the demo source files](https://github.com/ykkimhgu/NumericalProg-student/blob/main/tutorial/TU\_FunctionCall/TU\_FunctionCall\_example1.zip)



{% tabs %}
{% tab title="TU_functionCalldemo.cpp" %}
```cpp
#include "stdio.h"
#include "stdlib.h"
#include "TU_functionCall_header.h"


// These are defined in TU_functionCall_header.h
// double myFunc(const double x);
// void func_call(double xin);

// How to change the function equation in myFunc()
// if myFunc() is defined in library header file?
//
// Q2) What if myFunc2() is needed to be used?
// What do you need to modify in TU_functionCall_header.h ?


void main()
{
	double  xin = 2.5;
	func_call(xin);
}
```


{% endtab %}

{% tab title="TU_functionCall_header.h" %}
```c
#pragma once

#include "stdio.h"
#include "stdlib.h"

double myFunc(const double x) {
	double y = x * x;
	return y;
}

double myFunc2(const double x) {
	double y = x * x * x;
	return y;
}

void func_call(double xin)
{
	double y = myFunc(xin);
	printf("Y_out = %f \n", y);
}
```
{% endtab %}
{% endtabs %}

### Method 2 (recommended)

**Pass a function as an input argument to another function.**

[Download source files](https://github.com/ykkimhgu/NumericalProg-student/blob/main/tutorial/TU\_FunctionCall/TU\_FunctionCall\_example2.zip)

{% tabs %}
{% tab title="TU_functionCalldemo2.cpp" %}
```c

#include "stdio.h"
#include "stdlib.h"
#include "TU_functionCall_header.h"


// These are defined in TU_functionCall_header.h
// double myFunc(const double x);
// void func_call(double xin);

// Q1) How to change the function equation in myFunc()
// if myFunc() is defined in library header file?
//
// Q2) What if myFunc2() is needed to be used?
 
// ANS: pass the function as an input argument for a higher flexibility

double myFunc(const double x) {
	double y = x * x;
	return y;
}

double myFunc2(const double x) {
	double y = x * x * x;
	return y;
}

void main()
{
	double  xin = 2.0;
	func_call(myFunc, xin);
	func_call(myFunc2, xin);
}
```
{% endtab %}

{% tab title="TU_functionCall_header.h" %}
```c
#pragma once

#include "stdio.h"
#include "stdlib.h"



void func_call(double func(const double x), double xin)
{
	double y = func(xin);
	printf("Y_out = %f \n", y);
}
```
{% endtab %}
{% endtabs %}



***

####
