# Tutorial: Header File

## Introduction&#x20;

You will learn how to create and maintain my own library/header of numerical programming

For all assignments on this lecture,&#x20;

* Declare all your functions in `myNP.h`&#x20;
* Define all your functions in `myNP.cpp` or `myNP.c`



## Step 1. Workspace  Folder &#x20;

### Create local directory for programming

We will create the main directory for  NP (**or** EC) programs under&#x20;

**C:\Users\yourID\source\repos**

> e.g.   **C:\Users\ykkim\source\repos**

&#x20;You can search for 'repos' in window menu&#x20;

![](<../../.gitbook/assets/image (77).png>)



This is where your assignment projects  should be located.&#x20;

For Numerical Programming

* Name the directory as  "**NP**" 

  > A name that clearly shows the course name

For Embedded Controller

* Name the directory as "**EC"**.&#x20;



#### Example:

* **C:\Users\yourID\source\repos\NP**
* **C:\Users\yourID\source\repos\NP\assignment**
* **C:\Users\yourID\source\repos\NP\tutorial**
* **C:\Users\yourID\source\repos\NP\include**



## Step 2. Create a tutorial C Project

Create a new folder under **\tutorial** Directory and name it as **differentiation**

* **C:\Users\yourID\source\repos\NP\tutorial\differentiation**



Create a new empty project in Visual Studio  Community.  Name the project as  **TU\_Differentiation**

Create a new C/C++ source file for main()&#x20;

* Name the source file as  `TU_Differentiation_main.cpp`

Paste the following code or[ download from here](https://github.com/ykkimhgu/NumericalProg-student/blob/main/tutorial/TU\_Differentiation\_Part1\_Student\_main.cpp)

```cpp
/*-------------------------------------------------------------------------------\
@ Numerical Methods by Young-Keun Kim - Handong Global University

Author           : SSS LAB
Created          : 10-05-2021
Modified         : 8-05-2022
Language/ver     : C++ in MSVS2019

Description      : [Tutorial]Differentiation.cpp
-------------------------------------------------------------------------------*/

#include "myNM.h"
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



## Part 2. Create your Header files

Under the directory of  **\include,** create **'myNM.cpp**' and '**myNM.h**'.&#x20;

[ You can download these example files here](https://github.com/ykkimhgu/NumericalProg-student/tree/main/tutorial/Include)

* **C:\Users\yourID\source\repos\NP\include**

{% hint style="info" %}
Do not make duplicate copies of these files  in your local drive. Update these files as you do assignments.
{% endhint %}

{% tabs %}
{% tab title="myNM.h" %}
```cpp
/*----------------------------------------------------------------\
@ Numerical Methods by Young-Keun Kim - Handong Global University

Author           : [YOUR NAME]
Created          : 26-03-2018
Modified         : 18-03-2021
Language/ver     : C++ in MSVS2019

Description      : myNM.h
----------------------------------------------------------------*/

#ifndef		_MY_NM_H		// use either (#pragma once) or  (#ifndef ...#endif)
#define		_MY_NM_H

#include <iostream>
#include <string>
#include <fstream>

/*----------------------------------------------------------------\
	Vector   NP Library
----------------------------------------------------------------*/

void printVec(double* _vec, int _row);


/*----------------------------------------------------------------\
	Differentiation  NP Library
----------------------------------------------------------------*/
// Return the dy/dx results for the input data. (truncation error: O(h^2))
void gradient1D(double x[], double y[], double dydx[], int m);

// Return the dy/dx results for the target equation. (truncation error: O(h^2))
void gradientFunc(double func(const double x), double x[], double dydx[], int m);

// Tutorial example for callback function 
void func_call(double func(const double x), double xin);



#endif
```
{% endtab %}

{% tab title="myNM.cpp" %}
```cpp
/*----------------------------------------------------------------\
@ Numerical Methods by Young-Keun Kim - Handong Global University

Author           : [YOUR NAME]
Created          : 26-03-2018
Modified         : 18-03-2021
Language/ver     : C++ in MSVS2019

Description      : myNM.cpp
----------------------------------------------------------------*/

#include "myNM.h"


/*----------------------------------------------------------------\
	Vector   NP Library
----------------------------------------------------------------*/

void printVec(double* vec, int row)
{
	for (int i = 0; i < row; i++)
		printf("Vector[%d] = %.1f \n", i, vec[i]);
	printf("\n");
}



/*----------------------------------------------------------------\
	Differentiation  NP Library
----------------------------------------------------------------*/

// Return the dy/dx results for the input data. (truncation error: O(h^2))
void gradient1D(double x[], double y[], double dydx[], int m) {
	double h = x[1] - x[0];

	if (sizeof(x) != sizeof(y)) {
		printf("ERROR: length of x and y must be equal\n");
		return;
	}
		
	// Two-Point FWD  O(h)
	// Modify to Three-Point FWD  O(h^2)
	dydx[0] = (y[1] - y[0]) / h;

	// Two-Point Central  O(h^2)
	for (int i = 1; i < m - 1; i++) {
		dydx[i] = (y[i + 1] - y[i - 1]) / (2 * h);
	}

	// Two-Point BWD  O(h)
	// Modify to Three-Point BWD  O(h^2)
	dydx[m - 1] = (y[m-1]-y[m - 2]) / (h);
}


// Return the dy/dx results for the target equation. (truncation error: O(h^2))
void gradientFunc(double func(const double x), double x[], double dydx[], int m) {
	double* y;

	y = (double*)malloc(sizeof(double) * m);
	for (int i = 0; i < m; i++) {
		y[i] = func(x[i]);
	}

	//printVec(y, m);
	gradient1D(x, y, dydx, m);

	free(y);
}

// Tutorial example for callback function 
void func_call(double func(const double x), double xin) {
	double yout = func(xin);
	printf("Y_out by my_func=%f\n", yout);
}

/*----------------------------------------------------------------\
	Integration  NP Library
----------------------------------------------------------------*/

```


{% endtab %}
{% endtabs %}





## Part 3.  Include your Header files

In the above main() program,  include your header library by finding the path.&#x20;

```cpp
#include "../../../include/myNM.h"   // Find the location of header files

// or

#include "myNM.h"   // if the PATH is already Included in Project
```

