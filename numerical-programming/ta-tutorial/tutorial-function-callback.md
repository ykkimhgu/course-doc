# Tutorial: Passing a Function, Function callback

## Tutorial: Passing a Function, Function callback

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

### Calling a function within another function

There are several methods of calling a function within another function.

#### Method 1

**Calling the function that is defined globally or in the same scope**

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

// in Main() function
void main()
{
	double  xin=2.5;
	func_call(xin);
    ...
}
```

#### Method 2

**Pass a function as an input argument to another function.**

```cpp
///////////////////////////////
//*         Defined in myNM.h, myNM.c        *//


// Pass function as input argument
void func_call(double func(const double x), double xin)
{
	double yout = func(xin);
	printf("Y_out by my_func1= %f\n", yout);
}

///////////////////////////////
//*         MAIN.CPP        *//
// Defined in Main.cpp
double myFunc(const double x)
{
	double y = x*x;
}

// in Main()

// in Main() function
void main()
{
	double xin = 2;
	double yout = 0;
	func_call(myFunc, xin);
    ...
}
```

***

####
