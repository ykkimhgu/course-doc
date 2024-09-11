# Tutorial: Differentiation

## Tutorial: Differentiation Solver- student

## Example

Estimate the velocity and acceleration from datasets of the position of an object.

```matlab
t = 0:0.2:4;
y = [-5.87 -4.23 -2.55 -0.89 0.67 2.09 3.31 4.31 5.06 5.55 5.78 5.77 5.52 5.08 4.46 3.72 2.88 2.00 1.10 0.23 -0.59];
```

### MATLAB Functions

```matlab
% asusume evenly spaced h
h=0.2;
vel = diff(y)./h;
tv=t(1:length(vel));
acc = diff(y,2)./h^2;
ta=t(1:length(acc));
```

##

## Exercise - MATLAB

Download the tutorial source file and fill in the blanks. Run the code and validate your answer

* MATLAB tutorial source file : [TU\_differentiation\_student.mlx](https://github.com/ykkimhgu/NumericalProg-student/blob/main/tutorial/TU\_differentiation\_student\_2023.mlx)

***

## Exercise - C Programming

Create a new folder under **\tutorial** Directory and name it as **differentiation**

* **e.g ) C:\Users\yourID\source\repos\NP\tutorial\TU\_differentiation**

Create a new empty project in Visual Studio Community. Name the project as **TU\_Differentiation**

Create a new C/C++ source file for main()

* Name the source file as `TU_Differentiation_main.cpp`
* Paste from the following code: [TU\_differentiation\_student.cpp](https://github.com/ykkimhgu/NumericalProg-student/blob/main/tutorial/TU\_differentiation\_student\_2022.cpp)



<details>

<summary>TU_differentiation_student.cpp</summary>

```c
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

//#include "myNM.h"
#include "../Include/myNM.h"


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
	

	system("pause");
	return 0;
}
```

</details>

### Problem 1 : Differentiation of Discrete Points

Create a function for numerical differentiation from a set of data. Read the instruction in the source code.

```c++
void gradient1D(double _x[], double _y[], double dydx[], int m);
```

Move your functions from main source file to your header files, located in `\include` folder

* function definitions: `myNP.h`
* function declaration: `myNP.cpp`

### Problem 2 : Differentiation from a function

Define a function that defines the target equation. Use y=x^2 , for x=0:0.2:4

```c++
double myFunc(const double x)
```

Create the following function that calls `myFunc()` to generate a discrete dataset and estimate differentiation.

```c++
void gradientFunc(double func(const double x), double x[ ], double dydx[ ], int m)`
```

Validate the result
