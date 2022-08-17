# 2D Array

# 

## Lesson

**코딩도장 핵심요약**: [2D Array 핵심요약](https://dojang.io/mod/page/view.php?id=673)






![image](https://user-images.githubusercontent.com/38373000/185039344-617ca154-16c4-4c83-8240-593e2457744f.png)





## Example Code

### Example 1

[C_array2d_example.c](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/pointer-array)

```c++
#include <stdio.h>
#include <stdlib.h>

// Matrix addtion
void addMat(double _matA[][3], double _matB[][3], double _matOut[][3], int _dim);

// Matrix multiplication
void multMat(double _matA[][3], double _matB[][3], double _matOut[][3], int _dim);

// Transposed matrix
void transMat(double _matA[][3], double _matOut[][3], int _dim);

// Print matrix
void printMat(double _matA[][3], int _dim);


int main() {

	// 2D array declaration and definition
	double A[3][3] = { 10, 20, 30, 40, 50, 60, 70, 80, 90 };
	double B[3][3] = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
	double addMat_out[3][3] = { 0 };
	double multMat_out[3][3] = { 0 };
	double transMat_out[3][3] = { 0 };

	// Calling functions
	addMat(A, B, addMat_out, 3);
	multMat(A, B, multMat_out, 3);
	transMat(A, transMat_out, 3);

	// Print results
	printf("addMat out = \n");
	printMat(addMat_out, 3);
	printf("\nmultMat_out = \n");
	printMat(multMat_out, 3);
	printf("\ntransMat_out = \n");
	printMat(transMat_out, 3);

	system("pause");
	return 0;
}

// Matrix addtion
void addMat(double _matA[][3], double _matB[][3], double _matOut[][3], int _dim) {
	for (size_t i = 0; i < _dim; i++)
		for (size_t j = 0; j < _dim; j++)
			_matOut[i][j] = _matA[i][j] + *(*(_matB + i) + j);	// *(*(_matB + i) + j) == _matB[i][j]

}

// Matrix multiplication
void multMat(double _matA[][3], double _matB[][3], double _matOut[][3], int _dim) {
	for (size_t i = 0; i < _dim; i++)
		for (size_t j = 0; j < _dim; j++) {
			_matOut[i][j] = 0;
			for (size_t k = 0; k < _dim; k++)
				_matOut[i][j] += _matA[i][k] * _matB[k][j];
		}
}

// Transposed matrix
void transMat(double _matA[][3], double _matOut[][3], int _dim) {
	for (size_t i = 0; i < _dim; i++)
		for (size_t j = 0; j < _dim; j++)
			_matOut[i][j] = _matA[j][i];
}

// Print matrix
void printMat(double _matA[][3], int _dim) {
	for (int i = 0; i < _dim; i++) {
		for (int j = 0; j < _dim; j++)
			printf("%f\t", _matA[i][j]);
		printf("\n");
	}
	printf("\n");
}
```





### Example 2

[C_array2D_example.](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/pointer-array/solution)[cpp](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/pointer-array)

```cpp
#include <stdio.h>
#include <iostream>
#include <stdlib.h>
#include <math.h>

using namespace std;

// Commonly used
void printVec(double* _vec, int _row);
void printMat(double(_mat)[3][3], int _row, int _col);  // if array dimension(M,N) is known


// Other options
void printMat2(double(*_mat)[3], int _row, int _col);	 // if array dimension(N) is known
void printMat3(double(_mat)[][3], int _row, int _col);  // if array dimension(M,N) is known


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
	printMat2(A, _row, _col);	
	printMat3(A, _row, _col);


	// Simple method to access 2-D arrays
	printf("A[i][k] \t \n");
	for (i = 0; i < _row; i++)
	{
		for (k = 0; k < _col; k++)
			printf("A[%d][%d]=%.1f \t", i, k, A[i][k]);
		printf("\n");
	}
	printf("\n\n");


	// Different ways of using 2-D arrays, Note that  a[i]== *(a+i)
	printf("(*(A+i))[k] \t *(A[i])+k \t *(*(A + i))+k \n");
	for (i = 0; i < _row; i++)
	{
		for (k = 0; k < _col; k++)
		{			
			printf("A[%d][%d]=%.1f \t", i, k, (*(A + i))[k]);
			printf("A[%d][%d]=%.1f \t", i, k, *(A[i] + k));
			printf("A[%d][%d]=%.1f \n", i, k, *(*(A + i)) + k);
		}
	}
	printf("\n\n");
	//----------------------------------------------------------------------//

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

void printMat2(double(*_mat)[3], int _row, int _col)
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

void printMat3(double(_mat)[][3], int _row, int _col)
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



---



## Exercise

* [Online C Compiler](https://www.onlinegdb.com/online_c_compiler)

* [Exercise Code](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/pointer-array)

* [Exercise-Solution Code](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/pointer-array/solution)



### Exercise 1

[C_array2d_exercise.c](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/pointer-array)

Create functions for

* addMat(X, Y, Out, dim)
* subtractMat(X, Y, Out, dim)
* mulMat(X, Y, Out, dim)
* transposeMat(X, Y, Out, dim)



*Assume 3x3 matrix for this exercise*. •*Use* float or double as the variable type 

The size of matrices need to be fixed and declared priori. 







