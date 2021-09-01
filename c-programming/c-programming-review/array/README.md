# Array

## 

## Example Code

### Example 1: Static 1D array example

\[exercise\]1Darray.c

```cpp
#include <stdio.h>
#include <stdlib.h>

// Vector addition
void addVec(double _vecA[], double _vecB[], double _vecOut[], int _dim);

int main() {

	// 1D array declaration and definition
	double vecA[] = { 1, 2, 3 };
	double vecB[] = { 10, 20, 30 };
	double vecC[3] = { 0 };

	// Applying 'addVec' function
	addVec(vecA, vecB, vecC, 3);

	// Print result
	for (int i = 0; i < 3; i++)
		printf("%f\t", vecC[i]);
	printf("\n");

	system("pause");
	return 0;
}

// Vector addition
void addVec(double _vecA[], double _vecB[], double _vecOut[], int _dim) {
	for (int i = 0; i < _dim; i++)
		_vecOut[i] = _vecA[i] + *(_vecB + i);		// *(_vecB + i) == _vecB[i]
}

```

### Example 2: 2D array example

```cpp
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



### Example 2: Static 1D 2D array example

Example\_1D\_2D\_static.c

```cpp
/*------------------------------------------------------------------------------------------\
@ Numerical Methods by Young-Keun Kim - Handong Global University

Author           : Young-Keun Kim
Created          : 01-02-2015
Modified         : 24-02-2017
Language/ver     : C++ in MSVS2013


Description:	 : Static  1D/2D Array  Example for Tutorial 1
------------------------------------------------------------------------------------------*/




#include <stdio.h>
#include <iostream>
#include <stdlib.h>
#include <math.h>

using namespace std;

void printVec(double *_vec, int _row);
void printVec(int *_vec, int _row);

void printMat(double** _mat, int _row, int _col);		// only for double *P[3]
void printMat2(double(*_mat)[3], int _row, int _col);	 // if array dimension(N) is known
void printMat3(double(_mat)[][3], int _row, int _col);  // if array dimension(M,N) is known
void printMat4(double(_mat)[3][3], int _row, int _col);  // if array dimension(M,N) is known


int main()
{
	int val = 1;
	int i, j, k;
	
	// Static Vector Allocation:  1-D array fixed array size and initial constant values
	double a[4] = { 1, 2, 3, 4 };
	double b[] = { 2, 3, 4, 5 };
	double c[4] = { 0 };	
	// Print 1-D array element		
	printVec(a, 4);
	


	// Static Matrix Allocation: 2-D array,	fixed array size and initial constant values
	//Declaring & Assigning Array
	int _row = 3, _col = 3;
	double *P[3];
	double A[3][3] = { { 1, 2, 3 }, { 4, 5, 6 }, { 10, 9, 8 } };
	double AT[][3] = { { 0, 0, 0 }, { 0, 0, 0 }, { 0, 0, 0 } };
		
		
	// Print 2-D array element		
	printf("\n Printing Matrix \n");

	//Passign to a function
	//printMat(P, _row, _col);  // 사용안함
	printMat2(A, _row, _col);
	printMat3(A, _row, _col);	
	printMat4(A, _row, _col);


	// Different ways of using 2-D arrays, Note that  a[i]== *(a+i)
	printf("A[i][k] \t (*(A+i))[k] \t *(A[i])+k \t *(*(A + i))+k \n");
	for (i = 0; i < _row; i++)
	{
		for (k = 0; k < _col; k++)
		{
			printf("A[%d][%d]=%.1f \t", i, k, A[i][k]);
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

void printVec(int *_vec, int _row)
{
	for (int i = 0; i<_row; i++)
		printf("Vec[%d] = %d \n", i, _vec[i]);
	printf("\n");
}

void printMat(double** _mat, int _row, int _col)
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

void printMat4(double(_mat)[3][3], int _row, int _col)
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





