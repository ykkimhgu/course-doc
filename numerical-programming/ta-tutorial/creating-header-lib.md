# Tutorial: NP Library Header Files

## Introduction

You will learn how to create and maintain NP library header files

For this tutorial, we will create ;

* Declare all your functions in `myNM_tutorial.h`
* Define all your functions in `myNM_tutorial.c`
*   Include your library in main source`C_createHeader_example.cpp`

    > Don't worry about the file extension of \*.cpp or \*.c
    >
    > You can use either extension with Visual Studio for Numerical Programming course

***

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

* **...\\...\NP\\**
* **...\\...\NP\tutorial**
* **...\\...\NP\include**

## Step 2. Create a tutorial C Project

Under **\tutorial** Directory, create a new folder named as **`TU_createheader`**

* i.e.:   `C:\Users\yourID\source\repos\NP\tutorial\TU_createheader`

Create a new empty project in Visual Studio Community

* Name the project as **`TU_createheader`**

Create a new C/C++ source file for main()

* Name the source file as **`TU_createHeader_example.cpp`**

Paste the following code or[ download src file from here](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/createHeader)

```cpp
// TU_createHeader_example.cpp

#include "stdio.h"
#include "stdlib.h"

// Include path will be included at the end of tutorial
// #include "myNP_tutorial.h"


void printVec(double* vec, int row);

int main(int argc, char* argv[])
{
	double x[3] = { 1, 2, 3 };
	int x_size = sizeof(x)/sizeof(double);
	printVec(x, x_size);
}


void printVec(double* vec, int size)
{
	for (int i = 0; i < size; i++)
		printf("Vector[%d] = %.1f \n", i, vec[i]);
	printf("\n");
}
```

##

## Step 3. Create your Header files

Under the directory of **\include,** create **'myNP\_tutorial.cpp**' and '**myNP\_tutorial.h**'.

* **C:\Users\yourID\source\repos\NP\include**
* [You can download source files here](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/createHeader)

{% hint style="info" %}
Do not make duplicate copies of these files in your local drive. Update these files as you do assignments.
{% endhint %}

{% tabs %}
{% tab title="myNP_tutorial.h" %}
```cpp
#ifndef		_MY_NP_H		// use either (#pragma once) or  (#ifndef ...#endif)
#define		_MY_NP_H

#include <stdio.h>
#include <stdlib.h>
#include <math.h>

extern void printVec(double* vec, int row);

#endif
```
{% endtab %}

{% tab title="myNP_tutorial.cpp" %}
```cpp
// Change the directory path
#include "../include/myNP_tutorial.h"

void printVec(double* vec, int size)
{
	for (int i = 0; i < size; i++)
		printf("Vector[%d] = %.1f \n", i, vec[i]);
	printf("\n");
}
```
{% endtab %}
{% endtabs %}

## Step 4. Include your Header files

In the above main() program, include your header library by finding the path.

Now, you need to **delete** the function definition of `printVec()` in main(), for we have included the function from the header library file.

The main source file should be modified as

```cpp
/*  C_createHeader_example.cpp  */

#include "stdio.h"
#include "stdlib.h"

// Change the Include path 
#include "../../../include/myNP_tutorial.h"   // Find the location of header files
// #include "myNP_tutorial.h"   // if the PATH is already Included in Project


void printVec(double* vec, int row);

int main(int argc, char* argv[])
{

	double x[3] = { 1, 2, 3 };
	int x_size = sizeof(x)/sizeof(double);

	printVec(x, x_size);

}

// void printVec() definition is deleted in this file
```

Compile and run the program.
