# Assignment Factorial and Power

## Assignment: Factorial and Power

This assignment is to help you review C-programming for Numerical Programming.

> Do NOT use Chat-GPT and online materials.&#x20;

You can do it all by yourself !!

***

## Problem

**Q1. Create power(x,N) function that returns**

$$
y=x^N
$$

```cpp
double power(double x, int N);
```

**Q2. Create factorial(x) function that returns**

$$
y=x! = x(x - 1)(x - 2)...1
$$

```cpp
double factorial(int x);
```

**Hint**

![image](https://github.com/ykkimhgu/Tutorial-C-Program/assets/38373000/925138f0-0c57-4012-a069-0e0c583052b8)

##

***

## Preparation

You must complete the following tutorials first.

* &#x20;[Prepare NP Workspace](https://ykkim.gitbook.io/ec/numerical-programming/preparation-for-np#prepare-np-workspace)
* &#x20;[Install VS Community](https://ykkim.gitbook.io/ec/c-programming/c-programming-review/installing-visual-studio)

##

## Procedure



1.  Create a new project “ **Assignment\_powerNfactorial**” with Visual Studio Community.

    It should be under the designated folder of `\NP\assignment\`

    ​

    `...\source\repos\NP\assignment\Assignment_powerNfactorial\`

    ​

    ​
2. Create a new source file

* name it as `Assignment_powerNfactorial_yourName.cpp`
* or you can copy the file from here:  [Assignment\_powerNfactorial\_student.cpp](https://github.com/ykkimhgu/Tutorial-C-Program/blob/main/powerNfactorial/Assignment_powerNfactorial_student.cpp)

​

3. Fill in the definitions of **power()** and **factorial()**

​

4. Run the code and validate the answers

​

5. Capture the output window and paste in the report

##

## Submit

Submit as “**Assignment\_powerNfactorial\_yourName.zip”** that includes



1\) Source code

* submit only “**Assignment\_powerNfactorial\_yourName.cpp”**



2\) Report: “**Assignment\_powerNfactorial\_yourName.pdf”**

* [Download the report template](https://github.com/ykkimhgu/NumericalProg-student/blob/main/docs/NP_Assignment_Factorial_YourName_2025.docx): NP\_Assignment\_Factorial\_YourName\_2025.docx

​&#x20;

<figure><img src="https://github.com/user-attachments/assets/a49263e0-6f85-41dc-849b-51c20075ae79" alt=""><figcaption></figcaption></figure>

***

## Code

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <math.h>


double factorial(int _x);
double power(double _x, int N);

int main(int argc, char* argv[])
{

	double x = 2.5;
	int N = 5;
	double y1 = 1;
	double y2 = 1;

	y1 = power(x, N);
	y2 = factorial(N);


	printf("\n\n");
	printf("=======================================\n");
	printf("    power(%f,%d ) Calculation   \n", x, N);
	printf("=======================================\n");
	printf("   -  My     result = %3.12f    \n", y1);
	printf("   -  Math.h result = %3.12f    \n", pow(x, N));
	printf("   -  absolute err. = %3.12f    \n", y1 - pow(x, N));
	printf("=======================================\n");

	printf("\n\n");
	printf("=======================================\n");
	printf("    factorial( %d) Calculation   \n", N);
	printf("=======================================\n");
	printf("   -  My     result = %.0f    \n", y2);
	printf("=======================================\n");


	system("pause");
	return 0;
}


// power fuction
double power(double x, int N)
{
	// [TODO] add your algorithm here
	// [TODO] add your algorithm here

	return y;
}


// factorial function
double factorial(int N)
{
	double y = 1;
	for (int k = 2; k <= N; k++)
		// [TODO] add your algorithm here
	
	return y;
}
```
