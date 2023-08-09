# Tutorial - Sine Taylor

## Tutorial - Sine Taylor

## Part 1

**Q1 a)** Create `sinTaylor(x)` that returns the output of sine x, where x is in \[rad].

&#x20;     **b)** Create `sindTaylor(x)` that returns the output of sine x, where x in in \[deg].



**Q2.** Create `sinTaylor2(x)` with your own definition of \*\*power() \*\* functions

###

### Procedure

1. Create a new project “ **TU\_TaylorSeries**” with Visual Studio
2. Create the new source file and name it as “**C\_taylorSeries\_exercise.c”**
3. Copy the source code from this link: [C\_taylorSeries\_exercise.c](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/sineTaylor)
4. Fill in the definition of **sinTaylor(rad)** in the main source.
5. Compare your answer and calculate the absolute error

* sin(π/3)= 0.86602540378

1. Create **sindTaylor(deg)** for degree unit input and output.

* (use sinTaylor(rad) )

#### TIP

**Approximation of Sine with Taylor series**

![image](https://user-images.githubusercontent.com/38373000/188124702-a2729c59-db28-4369-92b8-d9c55f98a4f2.png)

**Pseudocode for Programming Sine with Taylor series**

![image](https://user-images.githubusercontent.com/84503980/188071951-00d2bb3d-735c-40c2-a0ba-85a5cc88bf9d.png)

**Pseudocode for Programming power()**

![image](https://user-images.githubusercontent.com/84503980/188072025-424bab29-036a-4b09-81d3-61f1c61916e5.png)

[See here for the TA Tutorial Video ](tutorial-sine-taylor.md#tutorial-video)

[See here for the Solution code](tutorial-sine-taylor.md#exercise-code-and-solution)



## Part 2

**Q. Define your sinTaylor(x) in a header file**

### Procedure

1. Create a new project “ **TU\_TaylorSeries\_Part2**” with Visual Studio
2. Create the new source file and name it as “**C\_taylorSeries\_exercise\_part2.c”**
3. Copy the source code from this link: [C\_taylorSeries\_exercise\_part2.c](https://github.com/ykkimhgu/Tutorial-C-Program/blob/main/sineTaylor/C\_taylorSeries\_exercise\_part2.c)
4. Create a new header file named as `myNP_tutorial.h` and `myNP_tutorial.c`

* These files can be [downloaded from the link](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/sineTaylor)
* These files should be saved in “ \include\” folder.

![image](https://user-images.githubusercontent.com/38373000/188126430-8af8fa78-70ea-44dd-97cd-5dbbdec34fe3.png)

**5.** Your **sinTaylor(rad)** of Exercise 1 should be declared and defined in the header file.

6\. Run and check the answer



[See here for the TA Tutorial Video ](tutorial-sine-taylor.md#tutorial-video)

[See here for the Solution code](tutorial-sine-taylor.md#exercise-code-and-solution)



## Tutorial-Video

#### PART 1:  TA Session

{% embed url="https://youtu.be/8AYZHpiocp4" %}

#### PART 2:  TA Session

{% embed url="https://youtu.be/W0bC-dC-e0M" %}



## Exercise code and solution

### For Exercise 1 and 2

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

{% tab title="solution" %}
```
/*----------------------------------------------------------------\
@ C-Tutorial by Young-Keun Kim - Handong Global University
Author           : YOUR NAME
Created          : 09-01-2022
Modified         : 09-01-2022
Language/ver     : C in MSVS2022
Description      : C_taylorSeries_exercise_solution.c
----------------------------------------------------------------*/

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
	S_N = sinTaylor2(x);
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
		S_N = S_N + pow(-1, k) * pow(_x, 2 * k + 1) / factorial(2 * k + 1);
		
	return S_N;
}
	
// Taylor series approximation for sin(x) using pre-defined functions (input unit: [deg])
double sindTaylor(double _x)
{
	return sinTaylor(_x * PI / 180);
}

// power fuction
double power(double _x, int N)
{
	double y = 1;

	for (int k = 1; k <= N; k++)
		y = y * _x;

	return y;
}


// Taylor series approximation for sin(x) without using pre-defined functions (input unit: [rad])
double sinTaylor2(double _x)
{
	int N_max = 10;
	double S_N = 0;

	for (int k = 0; k < N_max; k++)
		S_N = S_N + power(-1, k) * power(_x, 2 * k + 1) / factorial(2 * k + 1);

	return S_N;
}
```
{% endtab %}
{% endtabs %}



### For Exercise 3

{% tabs %}
{% tab title="C_taylorSeries_exercise_part2.c" %}
```
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
//#define		PI		3.14159265358979323846264338327950288419716939937510582

#include "../../../include/myNP_tutorial.h"

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

{% tab title="myNP_tutorial.c" %}
```cpp
/*----------------------------------------------------------------\
@ Numerical Methods by Young-Keun Kim - Handong Global University

Author           : SSS Lab
Created          : 05-03-2021
Modified         : 05-03-2021
Language/ver     : C in MSVS2019

Description      : myNP_tutorial.c
/----------------------------------------------------------------*/

#include "myNP_tutorial.h"

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
		S_N = S_N + pow(-1, k) * pow(_x, 2 * k + 1) / factorial(2 * k + 1);

	return S_N;
}

// Taylor series approximation for sin(x) using pre-defined functions (input unit: [deg])
double sindTaylor(double _x)
{
	return sinTaylor(_x * PI / 180);
}

// power fuction
double power(double _x, int N)
{
	double y = 1;

	for (int k = 1; k <= N; k++)
		y = y * _x;

	return y;
}

// Taylor series approximation for sin(x) without using pre-defined functions (input unit: [rad])
double sinTaylor2(double _x)
{
	int N_max = 10;
	double S_N = 0;

	for (int k = 0; k < N_max; k++)
		S_N = S_N + power(-1, k) * power(_x, 2 * k + 1) / factorial(2 * k + 1);

	return S_N;
}
```
{% endtab %}
{% endtabs %}
