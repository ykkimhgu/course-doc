# Tutorial - Sine Taylor

### Preparation

1. Create a new visual studio project named as “ TU\_TaylorSeries”&#x20;
2.  [Download source codes](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/sineTaylor) : &#x20;

    `C_taylorSeries_exercise.c, myNM_tutorial.h,  myNM _tutorial.c`
3. Include `myNM_tutorial.h` & `myNM _tutorial.c` in your main source



## Exercise



**Q1 a)** Create  `sinTaylor(x)` that returns the output of sine x, where x is in  \[rad].

&#x20;    b) Create `sindTaylor(x)` that returns the output of sine x, where x in in \[deg]. &#x20;

****

**Q2.** Create `sinTaylor2(x)` with your own definition of **power() and factorial()** functions



#### TIP

![](<../../.gitbook/assets/image (88).png>)

![](<../../.gitbook/assets/image (92).png>)

## Example source code

#### main() code

```cpp
/*----------------------------------------------------------------\
@ Numerical Programming by Young-Keun Kim - Handong Global University

Author           : SSS LAB
Created          : 05-03-2021
Modified         : 08-30-2021
Language/ver     : C in MSVS2019

Description      : myNM_main.c
----------------------------------------------------------------*/

#include "myNM.h"

int main(void)
{
	double d = 45;
	double x = d * PI / 180;

	double S_N;

	/*===== Select the function to call =====*/
	S_N = sinTaylor(x);
	//S_N = sindTaylor(d);
	//S_N = sinTaylor2(x);
	//S_N = sinTaylor3(x);


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

## Solution

{% tabs %}
{% tab title="myNM.h" %}
```cpp
/*----------------------------------------------------------------\
@ Numerical Methods by Young-Keun Kim - Handong Global University

Author           : SSS Lab
Created          : 05-03-2021
Modified         : 05-03-2021
Language/ver     : C in MSVS2019

Description      : myNM.h
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

{% tab title="myNM.c" %}
```cpp
/*----------------------------------------------------------------\
@ Numerical Methods by Young-Keun Kim - Handong Global University

Author           : SSS Lab
Created          : 05-03-2021
Modified         : 05-03-2021
Language/ver     : C in MSVS2019

Description      : myNM.c
/----------------------------------------------------------------*/

#include "myNM.h"

// factorial function
double factorial(double _x)
{
	if (_x <= 1)
		return 1;
	else
		return _x * factorial(_x - 1);
}

//  Taylor series approximation for sin(x) using pre-defined functions (input unit: [rad])
double sinTaylor(double _x) 
{
	int N_max = 20;
	double epsilon = 1e-5;

	double S_N = 0, S_N_prev = 0, rel_chg = 0;
	int N = 0;

	do {
		N++;
		S_N_prev = S_N;
		S_N = 0;
		for (int k = 0; k < N; k++)
			S_N += pow(-1, k) * pow(_x, 2 * k + 1) / factorial(2 * k + 1);

		rel_chg = fabs((S_N - S_N_prev) / S_N_prev);

	} while (N < N_max && rel_chg >= epsilon);

	return S_N;
}

// Taylor series approximation for sin(x) using pre-defined functions (input unit: [deg])
double sindTaylor(double _x) 
{
	return sinTaylor(_x * PI / 180);
}

// Taylor series approximation for sin(x) without using pre-defined functions (input unit: [rad])
double sinTaylor2(double _x) 
{
	int N_max = 20;
	double epsilon = 1e-5;

	double S_N = 0, S_N_prev = 0, rel_chg = 0;
	int N = 0;

	do {
		N++;
		S_N_prev = S_N;
		S_N = 0;

		for (int k = 0; k < N; k++)
		{
			//  (-1)^n			
			int sign_part = 1;
			for (int i = 1; i <= k; i++)   //sign_part *= -1;
				sign_part *= -1;

			//  (x)^n			
			double pow_part = 1;
			for (int i = 1; i <= 2 * k + 1; i++)  //pow_part *= _x * _x;
				pow_part *= _x;

			// Factorial
			double fac_part = 1;
			for (int i = 1; i <= 2 * k + 1; i++)
				fac_part *= i;

			S_N += sign_part * pow_part / fac_part;
		}

		rel_chg = fabs((S_N - S_N_prev) / S_N_prev);

	} while (N < N_max && rel_chg >= epsilon);

	return S_N;
}

// Function that reduced the computation cost of sinTaylor2 (input unit: [rad])
double sinTaylor3(double _x) 
{
	int N_max = 20;
	double epsilon = 1e-5;

	double S_N = 0, S_N_prev = 0, rel_chg = 0;
	int N = 0;
		   
	int sign_part = -1;
	
	if (_x==0)
		return 0;
	else
	{	
		double pow_part = 0;
		if (_x=!0)		pow_part = 1 / _x;

		double fac_part = 1;
		
		do {
			N++;
			S_N_prev = S_N;

			sign_part *= -1;
			pow_part *= _x * _x;
			fac_part = max(fac_part * (2 * N - 2) * (2 * N - 1), 1);
			S_N += (sign_part * pow_part / fac_part);
			
			rel_chg = fabs((S_N - S_N_prev) / S_N_prev);
			
		} while (N < N_max && rel_chg >= epsilon);
	return S_N;
	}	
}
```
{% endtab %}
{% endtabs %}
