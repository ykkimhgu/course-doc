# Tutorial: Passing a Function, Function callback

## Introduction

In this tutorial, we will learn how to pass a mathematical function f(x) as an input argument to another function.

There are several methods of calling a function within another function.

1. Calling the function that is defined globally or in the same scope

* What if you want to define the equation “myFun” in another file or in main.c (not in your NM library)?

```cpp
// Use global function or defined in the same header file

double myFunc(const double x){
double y = x*x;
return y;
}

void func_call(double xin)
{
double y = myFunc(xin);
printf ("Y_out = %d\n", y);
}

. . .
// in Main() function
double  xin=2.5;
func_call(xin);

```

2\. Pass a function as an input argument to another function.

```cpp
// Defined in myNM.h, myNM.c

// Pass function as input argument
void func_call(double func(const double x), double xin)
{
	double yout = func(xin);
	printf("Y_out by my_func1= %f\n", yout);
}

….



// Defined in Main.cpp
double myFunc(const double x)
{
double y = x*x;
}

// in Main()
double xin = 2;
double yout = 0;
func_call(myFunc, xin);

```



### **Example: Estimating Derivative from given mathematic equation**

How can you use `myFun()` within the function of `gradientFunc()`?

```cpp
void gradientFunc(double func(const double x), double x[ ], double dydx[ ], int m);
```

* First, generate dataset Y from  given function F(X) and input data X.
* Then, use `gradient1D( ) `  to estimate dy/dx from  generated dataset Y and input data X.

See [Tutorial: Header File](tutorial-header-file.md#introduction) for   `gradient1D( ) `  

### ****

### **Procedure**

Define a function that defines the target equation (e.g. y=x^2 )

`double myFunc(const double x)`



Create gradientFunc( ) to generate  **y** data from the equation function (myFunc).

`void gradientFunc(double func(const double x), double x[ ], double dydx[ ], int m)`



Validate the result with a simple test equation

** y=x^3  at x=0:0.2:4  // > dy/dx=3x^2**



### Example Code

Paste the following code or[ download from here](https://github.com/ykkimhgu/NumericalProg-student/blob/main/tutorial/TU_Differentiation_Part2\_Student_main.cpp)

For include headers, use your  **'myNM.cpp**' and '**myNM.h**'. [ You can download these example files here](https://github.com/ykkimhgu/NumericalProg-student/tree/main/tutorial/Include)

```cpp
/*-------------------------------------------------------------------------------\
@ Numerical Methods by Young-Keun Kim - Handong Global University

Author           : SSS LAB
Created          : 10-05-2021
Modified         : 8-05-2022
Language/ver     : C++ in MSVS2019

Description      : [Tutorial]Differentiation.cpp
-------------------------------------------------------------------------------*/


#include "stdio.h"
#include "stdlib.h"
#include "../Include/myNM.h"



double myFunc(const double x);

int main(int argc, char* argv[])
{

	// PART 1
	printf("\n**************************************************");
	printf("\n|                     PART 1.                    |");
	printf("\n**************************************************\n");
	
	int m = 21;
	double t[21] = { 0 };
	for (int i = 0; i < m; i++) t[i] = 0.2 * i;

	double x[] = { -5.87, -4.23, -2.55, -0.89, 0.67, 2.09, 3.31, 4.31, 5.06, 5.55, 5.78, 5.77, 5.52, 5.08, 4.46, 3.72, 2.88, 2.00, 1.10, 0.23, -0.59 };
	double  dxdt[21] = { 0 };
	
	// Estimate differentation from discrete dataset points
	gradient1D(t, x, dxdt, m);
	printVec(dxdt, m);

	
	// PART 2
	printf("\n**************************************************");
	printf("\n|                     PART 2.                    |");
	printf("\n**************************************************\n");
	
	double xin = 2.5;
	double y=myFunc(xin);
	printf("\n y=myFun(xin) = %f \n\n", y);	
	func_call(myFunc, xin);
	printf("\n**************************************************\n\n");
	
	
	// Estimate differentation from the user defined function 
	double dydx[21];
	gradientFunc(myFunc, t, dydx, m);
	printVec(dydx, m);
			

	system("pause");
	return 0;
}


// User defined function:  example  y=x*x
double myFunc(const double x) {
	return  x * x;
}
```

### ****
