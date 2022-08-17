# Dynamic Alloc

## Lesson

**코딩도장 핵심요약**: [구조체 포인터 메모리 할당](https://dojang.io/mod/page/view.php?id=418)



We have used fixed sized 1-D or 2-D arrays 

`double a[4] = { 2, 2, 3, 4 };`

How can the user set the size of the array i.e., change the value of m by n) during the run-time? Is it possible to declare arrays without knowing its size?



**malloc()**

```c++
void * malloc(size_t size);	
```

Characteristics of “void \* ” type (void pointer type)

* void pointer type can be assigned arbitrary types
* void pointer type can perform pointer operations
* in order to use the void pointer type properly, casting is necessary
* returns a pointer to the allocated memory, or NULL if the request fails

Example:

```C++
//** 1D Array
(int *)malloc(sizeof(int) * (_row));

//** 2D Array
// 1. allocate row array first
matA = (int**)malloc(sizeof(int*) * (_row));
// 2. Then, allocate column 
for(int i = 0; i < _row; i++)	
    (matA)[i] = (int*)malloc(sizeof(int) * (_col));

```








## Example Code

### Example 1

**Dynamic Allocation- 1D using functions**

[C_malloc1d_example.c](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/structure)

```c++
val = 1;
int *vecC;	
// Memory allocation
vecC = (int*)malloc(sizeof(vecC) * (_row));	

// Initialization with a value
for (i = 0; i < _row; i++)
    (vecC)[i] = val;

// Free allocated memory when program ends
free(vecC);
```



### Example 2

**Dynamic Allocation- 1D using functions**

[C_malloc1d_example2.c](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/structure)

```c++
int *vecD;
// Memory allocation
createVec(&vecD, _row);
// Initilization with values
initVec(vecD, _row,1);	
// Free allocated memory
free(vecD);

////////////////////////////////////////////////////

void createVec(int** _vec, int _row){		
	*_vec = (int*)malloc(sizeof(int) * (_row));
}
 
void initVec(int* _vec, int _row, int _val)
{
	int i;
	for(i = 0; i < _row; i++)
			(_vec)[i] = _val;	
}

```



### Example 3

**Dynamic Allocation- 2D using functions**

[C_malloc2d_example2.c](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/structure)

```c++
int **matB;
createMat(&matB,_row, _col);
initMat(matB,_row, _col,3);
printMat(matB, _row, _col);	
system("pause" );
free(matA);
free(matB);


////////////////////////////////////////////////////

void createMat(int*** _mat, int _row, int _col)
{
	int i;	
	*_mat = (int**)malloc(sizeof(int) * (_row));
	for(i = 0; i < _row; i++)
		(*_mat)[i] = (int*)malloc(sizeof(int) * (_col));		
}
 
void initMat(int** _mat, int _row, int _col, int _val)
{
	int i, j;
	for(i = 0; i < _row; i++)
	{
		for(j = 0; j < _col;j++)		
			(_mat)[i][j] = _val;			
	}
}
```

You are passing ‘**int** ***\*mat**’ to the function ‘**create_mat***( )’  without allocating size and memory of the 2-D array. Thus, you need to pass the address of ‘**int** ***\*mat**’ as ‘**&****mat’ and the function receives it as ‘**int\***** **_mat****’ (3 pointer notation)

```c++
createMat(&matB,_row, _col);
void createMat(int*** _mat, int _row, int _col)
```

Notice how different ‘**malloc**’ syntax is used in ‘Main()’ function and in ‘create_mat()’ function.  Once, the memory of 2-D is allocated then you can pass the array to a function as

```c++
initMat(matB,_row, _col,3);
void initMat(int** _mat, int _row, int _col, int _val)
```



### Example 4

**Structure Dynamic Allocation- 2D Matrix**

[C_matrix_example.c](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/structure)

```c++
typedef struct {
	double** at;
	int rows;
	int cols;
}Matrix;


void main() {
	Matrix A, B, C;
	//You can make a matrix of the size you want
	A = createMat(3, 5);

}


//////////////////////////////////
Matrix createMat(int _rows, int _cols) {
	Matrix Out;
	// 1. allocate row first
	Out.at = (double**)malloc(sizeof(double*) * _rows);
	// 2. allocate column 
	for (int i = 0; i < _rows; i++)
		Out.at[i] = (double*)malloc(sizeof(double) * _cols);
	// 3. Initialize matrix with values
	Out.rows = _rows;
	Out.cols = _cols;
	for (int i = 0; i < _rows; i++)
		for (int j = 0; j < _cols; j++)
			Out.at[i][j] = 0;
	return Out;
}
```



### Example 5

**Structure Dynamic Allocation- 2D Matrix**

[myMatrix_ tutorial.h](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/structure)

[myMatrix_ tutorial.c](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/structure)

[C_matrix_example2.c](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/structure)

```c++
#include “myMatrix_tutorial.h"

int main()
{
	Matrix A, B, C;
	A = createMat(5, 3);
	initMat(A, 10);
	B = createMat(5, 3);
	initMat(B, 5);
	C = addMat(A, B);
	printMat(C);

	free(A);
	free(B);
	free(C);

	system("PAUSE");
	return 0;
}

//////////////////////////////
void initMat(Matrix _mat, double _val) {
	for (int i = 0; i < _mat.rows; i++)
		for (int j = 0; j < _mat.cols; j++)
			_mat.at[i][j] = _val;
}
```





---



## Exercise

* [Online C Compiler](https://www.onlinegdb.com/online_c_compiler)

* [Exercise Code](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/structure)

* [Exercise-Solution Code]()



### Exercise 1

Download the following files

* [myMatrix_ tutorial.h](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/structure)

* [myMatrix_ tutorial.c](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/structure)

* [myMatrix_exercise.c](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/structure)

Include  “myMatrix_tutorial.h” and add following functions 

`Matrix subMat (Matrix _A, Matrix _b);`

Add two matrices of 3x3 size. 

* You can create any value 2D matrix of integer type
* Print the input matrix and output matrix

Create a function that subtracts two matrices

