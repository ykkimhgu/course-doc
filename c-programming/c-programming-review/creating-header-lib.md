# Creating Header Lib

## Introduction

You will learn how to create and maintain my own library/header file

For this tutorial, we will create ;

* Declare all your functions in `myNM_tutorial.h`
* Define all your functions in `myNM_tutorial.c`

## Step 1. Workspace Folder

### Create local directory for programming

We will create the main directory under

**C:\Users\yourID\source\repos**

> e.g. **C:\Users\ykkim\source\repos**

You can search for 'repos' in window menu

![image](https://user-images.githubusercontent.com/38373000/185348195-07f482ba-3aac-4fc8-8298-9928f06fc534.png)

This is where your assignment projects should be located.

For this tutorial, let us create the new workspace directory as

*   Name the directory as "**NP**"

    > A name that clearly shows the course name

Create more necessary sub directories

#### Example:

* **C:\Users\yourID\source\repos\NP**
* **C:\Users\yourID\source\repos\NP\tutorial**
* **C:\Users\yourID\source\repos\NP\include**



## Step 2. Create a tutorial C Project

Under **\tutorial** Directory,  create a new folder named as **TU\_createheader**

* **C:\Users\yourID\source\repos\NP\tutorial\TU\_createheader**



Create a new empty project in Visual Studio Community. Name the project as **TU\_createheader**



Create a new C/C++ source file for main()

* Name the source file as `TU_createHeader_main.cpp`

Paste the following code or[ download src file from here](https://github.com/ykkimhgu/NumericalProg-student/blob/main/tutorial/TU\_Differentiation\_Part1\_Student\_main.cpp)

```cpp
/*-------------------------------------------------------------------------------\
@ C-Tutorial  by Young-Keun Kim - Handong Global University

Author           : SSS LAB
Created          : 10-05-2021
Modified         : 8-05-2022
Language/ver     : C++ in MSVS2019

Description      : [Tutorial] Create Header Exercise
-------------------------------------------------------------------------------*/

#include "stdio.h"
#include "stdlib.h"

// Include path will be included at the end of tutorial
// #include "myNP_tutorial.h"


double myFunc(const double x);

int main(int argc, char* argv[])
{

	// PART 1
	printf("\n**************************************************");
	printf("\n|                     PART 1.                    |");
	printf("\n**************************************************\n");
	
	int m = 3;
	double x[3] = { 1, 2, 3};
	
	double  dxdt[21] = { 0 };
	
	// Estimate differentation from discrete dataset points
	gradient1D(t, x, dxdt, m);
	printVec(dxdt, m);

	

	
	
	// Estimate differentation from the user defined function 
	double dydx[21];
	gradientFunc(myFunc, t, dydx, m);
	printVec(dydx, m);
			

	system("pause");
	return 0;
}


// User defined function:  example  y=x*x
void printArray(x, m)
{

};


```



## Part 2. Create your Header files

Under the directory of **\include,** create **'myNP_tutorial.cpp**' and '**myNP_tutorial.h**'.

* **C:\Users\yourID\source\repos\NP\include**

* [You can download source files here](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/createHeader)

  

{% hint style="info" %}
Do not make duplicate copies of these files in your local drive. Update these files as you do assignments.
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



## Part 3. Include your Header files

In the above main() program, include your header library by finding the path.

Now, you need to delete the function definition of ` printArray()` in main(), for we have included the function from the header library file.



The main source file should be modified as

```c++
#include "stdio.h"
#include "stdlib.h"

// Change the Include path 
#include "../../../include/myNP_tutorial.h"   // Find the location of header files


// #include "myNP_tutorial.h"   // if the PATH is already Included in Project



int main(int argc, char* argv[])
{

	// PART 1
	printf("\n**************************************************");
	printf("\n|                     PART 1.                    |");
	printf("\n**************************************************\n");
	
	int m = 3;
	double x[3] = { 1, 2, 3};
	
	double  dxdt[21] = { 0 };
	
	// Estimate differentation from discrete dataset points
	gradient1D(t, x, dxdt, m);
	printVec(dxdt, m);

	// Estimate differentation from the user defined function 
	double dydx[21];
	gradientFunc(myFunc, t, dydx, m);
	printVec(dydx, m);


	system("pause");
	return 0;
}


```

