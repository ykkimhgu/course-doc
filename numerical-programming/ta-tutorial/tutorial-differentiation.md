# Tutorial: Differentiation



# Ploblem 

Estimate the velocity and acceleration from datasets of the position of an object.

```matlab
t = 0:0.2:4;
y = [-5.87 -4.23 -2.55 -0.89 0.67 2.09 3.31 4.31 5.06 5.55 5.78 5.77 5.52 5.08 4.46 3.72 2.88 2.00 1.10 0.23 -0.59];
```


# Tutorial: MATLAB

## Example code
```matlab
% asusume evenly spaced h
h=0.2;
vel = diff(y)./h;
tv=t(1:length(vel));
acc = diff(y,2)./h^2;
ta=t(1:length(acc));
```


## Exercise (25 min)

Download the tutorial source file and fill in the blanks. 

Run the code and validate your answer

* MATLAB tutorial source file : [TU\_differentiation\_student.mlx](https://github.com/ykkimhgu/NumericalProg-student/blob/main/tutorial/TU\_Differentiation/TU\_differentiation\_student\_2024.mlx)

***


# Tutorial: C-Programming


Create a new folder under **\tutorial** Directory and name it as **differentiation**

* **e.g ) C:\Users\yourID\source\repos\NP\tutorial\TU\_differentiation**

Create a new empty project in Visual Studio Community. Name the project as **TU\_Differentiation**

Create a new C/C++ source file for main()

* Name the source file as `TU_Differentiation_main.cpp`

Paste from the following code

<details>

<summary>TU_differentiation_student.cpp</summary>

```c
/*------------------------------------------------------------------------------\
@ Numerical Methods by Young-Keun Kim - Handong Global University

Author          : Young-Keun Kim
Created         : 01-04-2019
Modified        : 11-27-2023
Language/ver	: C in MSVS2017
Course		: Numerical Programming

Description      : [Tutorial]Differentiation.cpp
-------------------------------------------------------------------------------*/

#include "stdio.h"
#include "stdlib.h"

#include "../../include/myNP.h"
//#include "../../include/myNP_myID.h"

double myFunc(const double x);


int main(int argc, char* argv[])
{

	/*==========================================================================*/
	/*   Part 1 -     Differentiation from discrete dataset points              */
	/*==========================================================================*/

	printf("\n**************************************************");
	printf("\n|                     PART 1.                    |");
	printf("\n**************************************************\n");

	/************      Variables declaration & initialization      ************/
	int m = 21;
	double t[21] = { 0 };
	for (int i = 0; i < m; i++) t[i] = 0.2 * i;

	double x[] = { -5.87, -4.23, -2.55, -0.89, 0.67, 2.09, 3.31, 4.31, 5.06, 5.55, 5.78, 5.77, 5.52, 5.08, 4.46, 3.72, 2.88, 2.00, 1.10, 0.23, -0.59 };
	double  dxdt[21] = { 0 };

	/************      Solve  &	Show Output	   ************/
	// Differentiation from discrete dataset points
	
	// [YOUR CODE GOES HERE]
	// gradient1D(t, x, dxdt, m);
	// printVec(dxdt, m);

	

	/*==========================================================================*/
	/*   Part 2 -     Differentiation from a function                           */
	/*==========================================================================*/

	
	printf("\n**************************************************");
	printf("\n|                     PART 2.                    |");
	printf("\n**************************************************\n");

	/************      Variables declaration & initialization      ************/
	double xin = 2.5;
	double dydx[21] = { 0 };  // m=21 points

	// User defined function F(x)
	double y = myFunc(xin);
	printf("\n y=myFun(xin) = %f \n\n", y);


	/************      Solve  &	Show Output	   ************/
	// Estimate differentiation from the user defined function 
	
	// [YOUR CODE GOES HERE]
	// gradientFunc(myFunc, t, dydx, m);
	// printVec(dydx, m);


	system("pause");
	return 0;
}


// User defined function:  example  y=x*x
// Modify to User Function
double myFunc(const double x) {
	return  x * x;
}
```

</details>



## Exercise 1 : Differentiation from Discrete Points (25 min)

Create a C-program function for numerical differentiation from a set of discrete data. Read the instruction in the source code.

For m dataset, x\[0] to x\[m-1]

* 2-Point forward difference: for the first point x\[0] to \[m-2]
* 2-Point backward difference: for the last point x\[m-1]

```c
void gradient1D(double _x[], double _y[], double dydx[], int m);
```

The gradient1D function should be defined in your library header files, located in `\include` folder

* function definitions: `myNP_yourID.h`
* function declaration: `myNP_yourID.cpp`

###

{% hint style="info" %}
Review how to use 1D arrays in C-Programming.  [See tutorial here](../../c-programming/c-programming-review/array.md)
{% endhint %}

```c
// Example Code using 1D Array

void printVec(double _vec[], int _row);

int main()
{
	// Static 1-D array with fixed array size and initialized
	double a[4] = { 1, 2, 3, 4 };
	// Print 1-D array element
	printVec(a, 4);
	return 0;
}

void printVec(double _vec[], int _row)
{
	for (int i = 0; i<_row; i++)
	  printf("Vector[%d] = %f \n", i, _vec[i]);
}
```

###

## Exercise  2 : Differentiation from a function (25 min)

Define a function named as `myFunc()` that defines the user's equation `F(x)`.

For this tutorial, use `y=x^3` , for x=0:0.2:4

```c
double myFunc(const double _x);
```

Create a function named as `gradientFunc()` that returns the gradient of the user's equation.

* It should receive `myFunc()` as the input argument.
* It generates a discrete dataset of the user's `F(x)`
* This function should return a 1D array of dydx\[] .

```c
void gradientFunc(double func(const double x), double _x[ ], double dydx[ ], int m);
```

This function should be defined in your library header files, located in `\include` folder

* function definitions: `myNP_yourID.h`
* function declaration: `myNP_yourID.cpp`

Validate the result
