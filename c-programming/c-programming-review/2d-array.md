# 2D Array

## Lesson

**코딩도장 핵심요약**: [2D Array 핵심요약](https://dojang.io/mod/page/view.php?id=673)

![image](https://user-images.githubusercontent.com/38373000/185039344-617ca154-16c4-4c83-8240-593e2457744f.png)



## Example Code

### Example

[C\_array2D\_example.cpp](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/pointer-array)

```cpp
#include <stdio.h>
#include <iostream>
#include <stdlib.h>
#include <math.h>

using namespace std;

// Commonly used
void printVec(double* _vec, int _row);
void printMat(double(_mat)[3][3], int _row, int _col);  // if array dimension(M,N) is known


int main()
{
	int val = 1;
	int i, j, k;
	
	/*  Static Vector Allocation:  */
	// 1-D array fixed array size and initial constant values 	
	double a[4] = { 1, 2, 3, 4 };
	// Print 1-D array element		
	printf("\n Printing Vector \n");
	printVec(a, 4);
	

	/* Static Matrix Allocation */
	// 2-D array of fixed array size and initial constant values
	// Declaring & Assigning Array
	int _row = 3, _col = 3;
	double *P[3];
	double A[3][3] = { { 1, 2, 3 }, { 4, 5, 6 }, { 10, 9, 8 } };
				
	// Print 2-D array element		
	printf("\n Printing Matrix \n");

	// Passing 2D to a function
	printMat(A, _row, _col);	

	// Simple method to access 2-D arrays
	printf("A[i][k] \t \n");
	for (i = 0; i < _row; i++)
	{
		for (k = 0; k < _col; k++)
			printf("A[%d][%d]=%.1f \t", i, k, A[i][k]);
		printf("\n");
	}
	printf("\n\n");


	system("pause");

}

void printVec(double *_vec, int _row)
{
	for (int i = 0; i<_row; i++)
		printf("Vector[%d] = %.1f \n", i, _vec[i]);
	printf("\n");
}

void printMat(double(_mat)[3][3], int _row, int _col)
{
	int i, j;
	for (i = 0; i < _row; i++)
	{
		for (j = 0; j < _col; j++)
			printf("%.1f \t", _mat[i][j]);
		printf("\n");
	}
	printf("\n");
}
```



## Exercise

* [Online C Compiler](https://www.onlinegdb.com/online\_c\_compiler)
* [Exercise Code](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/pointer-array)
* [Exercise-Solution Code](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/pointer-array/solution)

### Exercise 1

[C\_array2d\_exercise.c](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/pointer-array)

Create functions for

* addMat(X, Y, Out, dim)
* subtractMat(X, Y, Out, dim)
* mulMat(X, Y, Out, dim)
* transposeMat(X, Y, Out, dim)

_Assume 3x3 matrix for this exercise_. •_Use_ float or double as the variable type

The size of matrices need to be fixed and declared priori.
