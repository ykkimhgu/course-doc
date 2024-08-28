# Tutorial - Sine Taylor

## Tutorial - Sine Taylor

## Tutorial - Programming Sin(x) with Taylor Series

[Download Supplementary PPT](https://github.com/ykkimhgu/Tutorial-C-Program/blob/main/sineTaylor/\(C-program\)%20Sine%20function%20with%20Taylor%20series\_2023.pdf)

### Part 1

**a)** Create `sinTaylor(x)` that returns the output of sine x, where x is in \[**rad**].

**b)** Create `sindTaylor(x)` that returns the output of sine x, where x in in \[**deg**].

You must use your own function of power() and factorial() from [Assignment 0](../assignment/assignment-factorial-and-power.md)

####

#### Procedure

1. Create a new project “ **TU\_TaylorSeries**” with Visual Studio
2. Create the new source file and name it as “**TU\_taylorSeries\_exercise.cpp”**
3. Copy the source code : [C\_taylorSeries\_exercise.c](tutorial-sine-taylor.md#c\_taylorseries\_exercise.c)
4. Fill in the definition of **sinTaylor(rad)** in the main source.
5. Compare your answer and calculate the absolute error

* sin(π/3)= 0.86602540378

6. Create **sindTaylor(deg)** for degree unit input and output.

* Hint: re-use sinTaylor(rad) definition

**TIP**

**Approximation of Sine with Taylor series**

![image](https://user-images.githubusercontent.com/38373000/188124702-a2729c59-db28-4369-92b8-d9c55f98a4f2.png)

**Pseudocode for Programming Sine with Taylor series**

![image](https://user-images.githubusercontent.com/84503980/188071951-00d2bb3d-735c-40c2-a0ba-85a5cc88bf9d.png)

**Pseudocode for Programming power()**

![image](https://user-images.githubusercontent.com/84503980/188072025-424bab29-036a-4b09-81d3-61f1c61916e5.png)

[See here for the TA Tutorial Video](tutorial-sine-taylor.md#tutorial-video)

***

### Part 2

**Q. Define your sinTaylor(x) in the NP library header file**

#### Procedure

1. Create a new project “ **TU\_TaylorSeries\_Part2**” with Visual Studio
2. Create the new source file and name it as “**TU\_taylorSeries\_exercise\_part2.cpp”**
3. Copy the source code from this link: [TU\_taylorSeries\_exercise\_part2.c](tutorial-sine-taylor.md#c\_taylorseries\_exercise\_part2.c)
4.  Update the existing library header files named as `myNP_tutorial.h` and `myNP_tutorial.c`

    > These files should be saved in “ \include\” folder.

![image](https://user-images.githubusercontent.com/38373000/188126430-8af8fa78-70ea-44dd-97cd-5dbbdec34fe3.png)

**5.** Your **sinTaylor(rad)** of Exercise 1 should be declared and defined in the header file.

6\. Run and check the answer

[See here for the TA Tutorial Video](ta-session.md#ta-session-taylor-series-programming)

***

### Extra Work

1. Create `double cosTaylor(double rad)`
2. Create `double expTaylor(double x)`

***

### Exercise Code

#### Part 1

{% tabs %}
{% tab title="C_taylorSeries_exercise.c" %}
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#define		PI		3.14159265358979323846264338327950288419716939937510582


double factorial(int _x);
double sinTaylor(double _x);
double sindTaylor(double _x);

double power(double _x, int N);
double sinTaylor2(double _x);

int main(int argc, char* argv[])
{

	double x = PI / 3;
	//double x = 60;

	double S_N = 0;

	/*===== Select the function to call =====*/
	S_N = sinTaylor(x);
	//S_N = sindTaylor(x);
	
	printf("\n\n");
	printf("=======================================\n");
	printf("    sin( %f[rad] ) Calculation   \n", x);
	printf("=======================================\n");	
	printf("   -  My     result = %3.12f    \n", S_N);
	printf("   -  Math.h result = %3.12f    \n", sin(x));
	printf("   -  absolute err. = %3.12f    \n", S_N - sin(x));
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


//  Taylor series approximation for sin(x) using pre-defined functions (input unit: [rad])
double sinTaylor(double _x)
{	
	int N_max = 10;
	double S_N = 0;			

	for (int k = 0; k < N_max; k++)
		// [TODO] add your algorithm here
	
	return S_N;
}


// Taylor series approximation for sin(x) using pre-defined functions (input unit: [deg])
double sindTaylor(double _x)
{
	// [TODO] add your algorithm here
}

// power fuction
double power(double _x, int N)
{
	// [TODO] add your algorithm here
}

//  Taylor series approximation for sin(x) using pre-defined functions (input unit: [rad])
double sinTaylor2(double _x)
{
	// [TODO] add your algorithm here
}
```
{% endtab %}
{% endtabs %}

***

#### Part 2

{% tabs %}
{% tab title="C_taylorSeries_exercise_part2.c" %}
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
//#define		PI		3.14159265358979323846264338327950288419716939937510582

#include "../../include/myNP_tutorial.h"

int main(int argc, char* argv[])
{

	double x = PI / 3;
	//double x = 60;

	double S_N = 0;

	/*===== Select the function to call =====*/
	S_N = sinTaylor(x);
	//S_N = sindTaylor(x);

	printf("\n\n");
	printf("=======================================\n");
	printf("    sin( %f[rad] ) Calculation   \n", x);
	printf("=======================================\n");
	printf("   -  My     result = %3.12f    \n", S_N);
	printf("   -  Math.h result = %3.12f    \n", sin(x));
	printf("   -  absolute err. = %3.12f    \n", S_N - sin(x));
	printf("=======================================\n");

	system("pause");
	return 0;
}
```
{% endtab %}

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

// Taylor series approximation for sin(x) using pre-defined functions (input unit: [rad])
extern double sinTaylor(double _x);

// Taylor series approximation for sin(x) using pre-defined functions (input unit: [deg])
extern double sindTaylor(double _x);

// Taylor series approximation for sin(x) without using pre-defined functions (input unit: [rad])
extern double sinTaylor2(double _x);

// Function that reduced the computation cost of sinTaylor2 (input unit: [rad])
extern double sinTaylor3(double _x);

#endif
```
{% endtab %}
{% endtabs %}







After you have completed all the exercises,  you can check sample solutions here

[Exercise solutions](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/sineTaylor)

