# Tutorial: Header File

## Introduction 

You will learn how to create and maintain my own library/header of numerical programming

For all assignments on this lecture, 

* Declare all your functions in `myNM.h` 
* Define all your functions in `myNM.cpp` or `myNM.c`

## 

## Part 0. NP Lecture Assignment/Tutorial  Folder  

### Create local directory for Project

We will create the main directory for  NP \(**or** EC\) programs under 

**C:\Users\yourID\source\repos**

 You can search for 'repos' in window menu 

![](../../.gitbook/assets/image%20%2877%29.png)



This is where your assignment projects  should be located. 

For Numerical Programming

* Name the directory as "**NumerialProg"**  or ****"**NP**" or  **"NP\_2021"**.  
* > A name that clearly shows NP lecture name

For Embedded Controller

* Name the directory as "**EC"**. 



#### Example:

* **C:\Users\yourID\source\repos\NumerialProg**
* **C:\Users\yourID\source\repos\NumerialProg\Assignment**
* **C:\Users\yourID\source\repos\NumerialProg\Tutorial**
* **C:\Users\yourID\source\repos\NumerialProg\Include**



## Part 1. Create a tutorial C Project

Create a new folder under **\Tutorial** Directory and name it as **Differentiation**

* **C:\Users\yourID\source\repos\NumerialProg\Tutorial\Differentiation**



Create a new empty project in Visual Studio  Community.  Name the project as  **TU\_Differentiation**

\*\*\*\*

Create a new C/C++ source file for main\(\) and paste the following code

```cpp
/*-------------------------------------------------------------------------------\
@ Numerical Methods by Young-Keun Kim - Handong Global University

Author           : SSS LAB
Created          : 10-05-2021
Modified         : 8-05-2022
Language/ver     : C++ in MSVS2019

Description      : [Tutorial]Differentiation.cpp
-------------------------------------------------------------------------------*/

#include "SOL_myNM.h"
#include "stdio.h"
#include "stdlib.h"



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

