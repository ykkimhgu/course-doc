# Tutorial - Sine Taylor

## Tutorial - Programming sin(x)  (In Class Activity)

## Preparation

#### PPT  Download:  [Download Supplementary PPT](https://github.com/ykkimhgu/Tutorial-C-Program/blob/main/sineTaylor/\(C-program\)%20Sine%20function%20with%20Taylor%20series_2023.pdf)

#### You must follow the Tutorial:  [creating-header-lib.md](creating-header-lib.md "mention")&#x20;

#### You must do:   [Assignment 0](../assignment/assignment-factorial-and-power.md)

####





## Problem 1

### Introduction

**a)** Create `sinTaylor(x)` that returns the output of sine x, where x is in \[**rad**].

**b)** Create `sindTaylor(x)` that returns the output of sine x, where x in in \[**deg**].

{% hint style="info" %}
You must use your own function of power() and factorial() from [Assignment 0](../assignment/assignment-factorial-and-power.md)
{% endhint %}

### **Procedure**

* Create a new empty project in Visual Studio Community
  * Name the project as:   **`TU_TaylorSeries`**&#x20;
*   It should be saved under `\tutorial` directory

    * i.e.: `C:\Users\yourID\source\repos\NP\tutorial\TU_TaylorSeries`


* Create a new C/C++ source file for main()
  * Name the source file as `TU_taylorSeries_exercise.cpp`
* Copy the source code from

<details>

<summary>C_taylorSeries_exercise.cpp</summary>

```c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#define		PI		3.14159265358979323846264338327950288419716939937510582


double factorial(int _x);
double power(double _x, int N);
double sinTaylor(double _x);
double sindTaylor(double _x);

int main(int argc, char* argv[])
{

	double xdeg = 60;	
	double x = PI / 3;
	double sinN = 0;
	double sinDeg = 0;
	
	/*===== Select the function to call =====*/
	sinN = sinTaylor(x);  // in [rad]
		
	printf("\n\n");
	printf("=======================================\n");
	printf("    sin( %f[rad] ) Calculation   \n", x);
	printf("=======================================\n");	
	printf("   -  My     result = %3.12f    \n", sinN);
	printf("   -  Math.h result = %3.12f    \n", sin(x));
	printf("   -  absolute err. = %3.12f    \n", sinN - sin(x));
	printf("=======================================\n");


	sinDeg = sindTaylor(xdeg); // in [deg]
	printf("\n\n");
	printf("=======================================\n");
	printf("    sin( %f[deg] ) Calculation   \n", xdeg);
	printf("=======================================\n");	
	printf("   -  My     result = %3.12f    \n", sinDeg);
	printf("=======================================\n");
	
	
	system("pause");
	return 0;
}


// factorial function
double factorial(int N)
{
	int y = 1;
	for (int k = 2; k <= N; k++)
		y = y * k;
	return y;
}


// power function
double power(double _x, int N)
{
	double y = 1;
	for (int k = 1; k <= N; k++)
		y = y * _x;
	return y;
}

//  Taylor series approximation for sin(x) (input unit: [rad])
double sinTaylor(double _x)
{	
	int N_max = 10;
	double sinN = 0;			

	for (int k = 0; k < N_max; k++)
		// [TODO] add your algorithm here
	
	return sinN;
}


// Taylor series approximation for sin(x) (input unit: [deg])
double sindTaylor(double _x)
{
	double sinDeg=0;
	// [TODO] add your algorithm here
	return sinDeg;
}


```

</details>

* Fill in the definition of **sinTaylor(rad)** in the main source.
*   Compare your answer and calculate the absolute error

    > sin(π/3)= 0.86602540378
* Create **sindTaylor(deg)** for degree unit input and output.

> Hint: re-use sinTaylor(rad) definition

#### **TIP**

**Approximation of Sine with Taylor series**

![image](https://user-images.githubusercontent.com/38373000/188124702-a2729c59-db28-4369-92b8-d9c55f98a4f2.png)

**Pseudocode for Programming Sine with Taylor series**

![image](https://user-images.githubusercontent.com/84503980/188071951-00d2bb3d-735c-40c2-a0ba-85a5cc88bf9d.png)

**Pseudocode for Programming power()**

![image](https://user-images.githubusercontent.com/84503980/188072025-424bab29-036a-4b09-81d3-61f1c61916e5.png)

### Video for Problem 1

[See here for the TA Tutorial Video](tutorial-sine-taylor.md#tutorial-video)

{% embed url="https://youtu.be/8AYZHpiocp4" %}

***

## Problem  2

### Introduction

Define your sinTaylor(x) in the NP library header file

### **Procedure**

* Create a new empty project in Visual Studio Community
  * Name the project as:   **`TU_TaylorSeries_Part2`**
*   It should be saved under `\tutorial` directory

    * i.e.: `C:\Users\yourID\source\repos\NP\tutorial\TU_TaylorSeries_Part2`


* Create a new C/C++ source file for main()
  * Name the source file as `TU_taylorSeries_exercise_part2.cpp`&#x20;
* Copy the source code

<details>

<summary>C_taylorSeries_exercise_part2.cpp</summary>

```c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
//#define		PI		3.14159265358979323846264338327950288419716939937510582

#include "../../include/myNP_tutorial.h"

int main(int argc, char* argv[])
{
	double xdeg = 60;	
	double x = PI / 3;
	double sinN = 0;
	double sinDeg = 0;
	
	/*===== Select the function to call =====*/
	sinN = sinTaylor(x);  // in [rad]
		
	printf("\n\n");
	printf("=======================================\n");
	printf("    sin( %f[rad] ) Calculation   \n", x);
	printf("=======================================\n");	
	printf("   -  My     result = %3.12f    \n", sinN);
	printf("   -  Math.h result = %3.12f    \n", sin(x));
	printf("   -  absolute err. = %3.12f    \n", sinN - sin(x));
	printf("=======================================\n");


	sinDeg = sindTaylor(xdeg); // in [deg]
	printf("\n\n");
	printf("=======================================\n");
	printf("    sin( %f[deg] ) Calculation   \n", xdeg);
	printf("=======================================\n");	
	printf("   -  My     result = %3.12f    \n", sinDeg);
	printf("=======================================\n");
	
	
	system("pause");
	return 0;
}


```

</details>

**(Library File Preparation)**&#x20;

* Under the directory of `\include`**,** prepare header files
  * Files:   `myNP_tutorial.cpp` and `myNP_tutorial.h`.
  * **`C:\Users\yourID\source\repos\NP\include`**
  * [ You can download source files here](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/createHeader)
  * This is the same header file as in   **Tutorial: NP Library Header Files** [#step-3.-create-library-header-files](creating-header-lib.md#step-3.-create-library-header-files "mention")

> These files should be saved in “ \include\” folder.

![image](https://user-images.githubusercontent.com/38373000/188126430-8af8fa78-70ea-44dd-97cd-5dbbdec34fe3.png)

**(Library File Update)**&#x20;

* Update the header files&#x20;
  * Your **sinTaylor(rad)** of Problem 1 should be declared and defined in the header file.
  * See below as example

{% tabs %}
{% tab title="myNP_tutorial.h" %}
```cpp
/*----------------------------------------------------------------\
@ Numerical Methods by Young-Keun Kim - Handong Global University

Author           : SSS Lab
Created          : 05-03-2021
Modified         : 05-03-2021
Language/ver     : C in MSVS2019

Description      : myNP_tutorial.h
/----------------------------------------------------------------*/

#ifndef		_MY_NM_H		// use either (#pragma once) or  (#ifndef ...#endif)
#define		_MY_NM_H
#define		PI		3.14159265358979323846264338327950288419716939937510582

#include <stdio.h>
#include <stdlib.h>
#include <math.h>

// Factorial function
extern double factorial(double _x);
extern double power(double _x, int N);

// Taylor series approximation for sin(x) using pre-defined functions (input unit: [rad])
extern double sinTaylor(double _x);

// Taylor series approximation for sin(x) using pre-defined functions (input unit: [deg])
extern double sindTaylor(double _x);



#endif
```
{% endtab %}

{% tab title="myNP_tutorial.c" %}
```c
/*----------------------------------------------------------------\
@ C-Tutorial by Young-Keun Kim - Handong Global University

Author           : SSS LAB
Created          : 05-03-2021
Modified         : 08-19-2022
Language/ver     : C++ in MSVS2022

Description      : myNP_tutorial.c
/----------------------------------------------------------------*/

#include "myNP_tutorial.h"



// factorial function
double factorial(int N)
{
	// [TODO] add your algorithm here
	return 0;
}

// power function
double power(double _x, int N)
{
	// [TODO] add your algorithm here
	return 0;
}



//  Taylor series approximation for sin(x) using pre-defined functions (input unit: [rad])
double sinTaylor(double _x)
{	
	int N_max = 10;
	double sinN = 0;			

	//for (int k = 0; k < N_max; k++)
		// [TODO] add your algorithm here
	
	return sinN;
}


// Taylor series approximation for sin(x) using pre-defined functions (input unit: [deg])
double sindTaylor(double _x)
{
	double sinDeg = 0;
	// [TODO] add your algorithm here
	return sinDeg;
}
```
{% endtab %}
{% endtabs %}

* Run and check the answer



### Video for Problem 2

[See here for the TA Tutorial Video](ta-session.md#ta-session-taylor-series-programming)

{% embed url="https://youtu.be/W0bC-dC-e0M?si=c3U2DwKB1_5lFZjL" %}

***

## Exercise

1. Create `double cosTaylor(double rad)`
2. Create `double expTaylor(double x)`

***

After you have completed all the exercises, you can check sample solutions here

[Exercise solutions](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/sineTaylor)

***

##

##
