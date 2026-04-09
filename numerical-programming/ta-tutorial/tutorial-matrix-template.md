# Tutorial: Matrix Structure

This tutorial explains how to use a Matrix structure for solving linear equations.

For the assignment, you must use the given Matrix structure and follow instructions for saving and modifying the data.

## Tutorial 1: Preparation of Matrix Structure

### Download

1. Download data files:&#x20;

* [NP\_Matrix\_TemplateCode\_Data.zip](https://github.com/ykkimhgu/NumericalProg-student/tree/main/tutorial/TU\_Matrix\_C\_Program)

2. Download tutorial source and header files:

* [TU\_matrixTemplate\_student.cpp](https://github.com/ykkimhgu/NumericalProg-student/tree/main/tutorial/TU\_Matrix\_C\_Program)
* [myMatrix\_student.h](https://github.com/ykkimhgu/NumericalProg-student/tree/main/include)
* [myMatrix\_student.cpp](https://github.com/ykkimhgu/NumericalProg-student/tree/main/include)

### Create Data Folder (상대경로 방법, recommended)

Create a data folder in the workspace name the folder as `NP_Data`

* **..\repos\NP\NP\_Data**

For each assignment, create the assignment folder and save the dataset.

Example: Assignment999

* **\NP\NP\_Data\Assignment999**

For this tutorial, unzip the downloaded data files and copy them under the data folder `Assignment999`

![image](https://github.com/user-attachments/assets/9a848a9d-80b2-4615-a5bc-c71a8154f991)


### Create Data Folder (절대경로 방법, not recommended <-- for TA only)

Create a folder in **C:\\** Drive and name the folder as `NP_Data`

* **C:\NP\_Data**

For each assignment, create the assignment folder and save the dataset.

Example: Assignment999

* **C:\NP\_Data\Assignment999**

For this tutorial, unzip the downloaded data files and copy them under the data folder `Assignment999`

![NP\_Matrix\_Data\_Example\_img](https://github.com/ykkimhgu/NumericalProg-student/blob/main/docs/NP\_Matrix\_Data\_example.png?raw=true)


### Create Project

Create a new empty project in Visual Studio Community. Name the project as **TU\_MatrixTemplate**

* **e.g ) C:\Users\yourID\source\repos\NP\tutorial\TU\_MatrixTemplate**

Create a new C/C++ source file for main()

* Name the source file as `TU_matrixTemplate.cpp`
* Use the downloaded source code : TU\_matrixTemplate\_student.cpp

Save the downloaded library header files in your `\include\` folder&#x20;

* [myMatrix\_student.h, myMatrix\_student.cpp](https://github.com/ykkimhgu/NumericalProg-student/tree/main/include)
* Write your name in the comment section
* Re-name the header files as:
  * myMatrix.h, myMatrix.cpp
> Then, you need to change in `myMatrix.cpp` as  `#include "myMatrix.h"`
 
## Tutorial 2: Create and Modify Dataset

For each assignment, create the assignment folder and save the dataset.

* Example: Assignment000
  * `\NP\NP_Data\Assignment000`&#x20;


Use the **text file** and **file name** as instructed in each assignment.

> You must use the same name for the  **text files** as instructed. Otherwise, it will not be graded

Example:

```
- Announcement for Assignment3 -

[File Path]
    ../../NP_Data/Assignment3
    // C:/NP_Data/Assignment3

[File Name]
Q1.
    matrix A : prob1_matA
    vector b : prob1_vecb
Q2.
    matrix A : prob2_matA
    vector b : prob2_vecb
Q3.
    matrix A : prob3_matA
    vector b : prob3_vecb
```


#### **How to modify in the data file**

* Change Row : **Enter**
* Change Column: **Tap**

#### Example :

Type:

* 1 tab 3 tab -2 tab 4 enter .... 3 tab -1 tab 6 tab 2 ctrl+s (저장)

![matrix\_text\_file\_example](https://github.com/ykkimhgu/NumericalProg-student/blob/main/docs/matrix\_text\_file.png?raw=true)

For Vector, type:

* \-11 enter 6 enter -9 enter 15 ctrl+s (저장)

![vector\_text\_file\_example](https://github.com/ykkimhgu/NumericalProg-student/blob/main/docs/vector\_text\_file.png?raw=true)


## Tutorial 3: Using Matrix Structure Library

### Matrix structure library

Provided library: `myMatrix.h`

```cpp
// myMatrix.h
typedef struct { 
	double** at;
	int rows, cols;
}Matrix;


// Create Matrix with specified size
extern	Matrix	createMat(int _rows, int _cols);

// Copy matrix
extern	Matrix	copyMat(Matrix _A);

// Free a memory allocated matrix
extern	void	freeMat(Matrix _A);

// Create a matrix from a text file
extern	Matrix	txt2Mat(std::string _filePath, std::string _fileName);

//// Print matrix
extern	void	printMat(Matrix _A, const char* _name);

// Matrix addition
extern	Matrix	addMat(Matrix _A, Matrix _B);

// ....

```


#### Example Code

```cpp
/*==========================================================================*/
/*					Variables declaration & initialization					*/
/*--------------------------------------------------------------------------*/
	// Option 1:  Read from datafile
	Matrix matA = txt2Mat(path, "prob1_matA");
	Matrix vecb = txt2Mat(path, "prob1_vecb");

	// Option 2:  Create an empty Matrix or Vector
	int rows = 4;
	int cols = 4;
	Matrix matC = createMat(rows, cols);
		
	// Option 3:  Create a zero matrix with specific size
	Matrix matD = zeros(matA.rows, matA.cols);


/*==========================================================================*/
/*					Accessing, modifying Matrix 							*/
/*--------------------------------------------------------------------------*/
	// Example:   Accessing each element in Matrix
	for (int i = 0; i < matA.rows; i++)
		for (int j = 0; j < matA.cols; j++)
			matC.at[i][j] = matA.at[i][j];
	printMat(matA, "matA");

	
	// Exmaple: Applying your NP algorithm
	Matrix matAdd = addMat(matA, matC);
	printMat(matAdd, "matU + matA");


/*==========================================================================*/
/*							  Deallocate memory 							*/
/*==========================================================================*/
	freeMat(matA);		freeMat(vecb);
```

### What to change for Assignment

1. Initially, change the assignment number for `#define ASGN`
   * DO NOT modify other code lines

```cpp
    #include "myMatrix.h"
    
    #define ASGN		999		// enter your assignment number
    #define EVAL		0		// [※ DO NOT EDIT !!!]
    
    
    int main(int argc, char* argv[])
    {
    	/*	 [※ DO NOT EDIT !!!]   Resources file path setting for evaluation	*/
    	//std::string path = "C:/NP_Data/Assignment" + std::to_string(ASGN) + "/";
    	std::string path = "../../NP_Data/Assignment" + std::to_string(ASGN) + "/";
    
    #if EVAL
    	path += "eval/";
    #endif
    
        ...
    
```

2. Read data text files. You must use the given file names.
3. Then, apply your numerical programming algorithm.

```cpp
/*==========================================================================*/
/*					Variables declaration & initialization					*/
/*--------------------------------------------------------------------------*/
/*   - You can change the variable names									*/
/*   - However, you must use the specified txt file name					*/	
/*==========================================================================*/
Matrix matA = txt2Mat(path, "prob1_matA");
Matrix vecb = txt2Mat(path, "prob1_vecb");
Matrix matU = txt2Mat(path, "prob1_matU");
Matrix vecd = txt2Mat(path, "prob1_vecd");
Matrix vecx_true = txt2Mat(path, "prob1_vecx_true");
// Your Code goes Here
// Your Code goes Here



/*==========================================================================*/
/*					Apply your numerical method algorithm					*/
/*==========================================================================*/
Matrix matAdd = addMat(matA, matU);		// example code
// Your Code goes Here
// Your Code goes Here  

//   ...
```

4. Prints vector or matrix results. You have to give a brief description for each print.

```cpp
    
        /*==========================================================================*/
        /*                          Print your results                              */
        /*==========================================================================*/
        printMat(matA, "Problem 1, Matrix A");
        printMat(vecb, "Problem 1, Vector b");
    
    
```

5. Free memory allocated to a matrix.
   * Even if you omit the free process, the code will work. However, it can cause memory leaks.

```cpp
        /*==========================================================================*/
        /*                          Deallocate memory                               */
        /*==========================================================================*/
        freeMat(matA);
        freeMat(vecb);
   
   
```

***

## Exercise and Assignment

Declare and define the following functions in `myMatrix.h` and `myMatrix.cpp`

```cpp

// Create matrix of all zeros
extern	Matrix	zeros(int _rows, int _cols);

// Create matrix of all ones
extern	Matrix	ones(int _rows, int _cols);

// Create identity matrix
extern	Matrix	eye(int _rows, int _cols);


// Matrix subtraction
extern	Matrix	subMat(Matrix _A, Matrix _B);

// Multiply  matrix A and matrix B
extern	Matrix	multMat(Matrix _A, Matrix _B);

// Multiply  matrix A with a scalar k
extern	Matrix	smultMat(Matrix _A, double _k);

// Create Transpose matrix
extern	Matrix	transpose(Matrix _A);


```

***
## Tutorial 4:  Modifying and Returning Matrix by a Function

## Case: Return Type is `Matrix`  

An error occurs when `freeMat` is used inside a function.
 
### Option 1. Using copyVal

```c++
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include "../../include/myMatrix.h"

Matrix myFunc_Q1(Matrix Z);
void printAddress(Matrix Z, const char* name);

int main(int argc, char* argv[]){
    int M = 3;
    int dim = 1;

    double z[3] = { 1, 2, 3 };
    Matrix Z = arr2Mat(z, M, dim);
    Matrix Z_out = zeros(Z.rows, Z.cols);
    
    printf("\n**************************************************");
    printf("\n|          Question 1. (Matrix, copyVal)         |");
    printf("\n**************************************************\n");

    printMat(Z_out, "Z_out is (before)");
    Z_out = myFunc_Q1(Z);
    printMat(Z_out, "Z_out is (after)");
    
    freeMat(Z);
    freeMat(Z_out);

    system("pause");
    return 0;
}

Matrix myFunc_Q1(Matrix Z)
{
	int n = Z.rows;
	Matrix F = zeros(n, 1);

	printf("\n[myFunc_Q1]\n");
	printAddress(Z, "Z in myFunc_Q1");

	printAddress(F, "F in myFunc_Q1 ");
	copyVal(Z, F);
	printAddress(F, "F in myFunc_Q1 (after)");

	return F;	
}

void printAddress(Matrix Z, const char* name)
{
	printf("%s\n", name);
	printf("  &Matrix struct   : %p\n", &Z);
	printf("  Matrix.at        : %p\n", Z.at);
	printf("  &Matrix.at[0][0] : %p\n\n", &Z.at[0][0]);
}


```



### Option 2. Using copyMat

```c++
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include "../../include/myMatrix.h"

Matrix myFunc_Q2(Matrix Z);
void printAddress(Matrix Z, const char* name);

int main(int argc, char* argv[]){
    int M = 3;
    int dim = 1;

    double z[3] = { 1, 2, 3 };
    Matrix Z = arr2Mat(z, M, dim);
    Matrix Z_out1 = zeros(Z.rows, Z.cols);
    
    printf("\n**************************************************");
    printf("\n|          Question 2. (Matrix, copyMat)         |");
    printf("\n**************************************************\n");


    printMat(Z_out1, "Z_out1 is (before)");
    Z_out1 = myFunc_Q2(Z);
    printMat(Z_out1, "Z_out1 is (after)");
    
    freeMat(Z);
    freeMat(Z_out1);

    system("pause");
    return 0;
}

Matrix myFunc_Q2(Matrix Z)
{
	int n = Z.rows;
	Matrix F = zeros(n, 1);

	printf("\n[myFunc_Q2]\n");
	printAddress(Z, "Z in myFunc_Q2");

	printAddress(F, "F in myFunc_Q2 ");
	F= copyMat(Z);
	printAddress(F, "F in myFunc_Q2 (after)");
	return F;	
}

void printAddress(Matrix Z, const char* name)
{
	printf("%s\n", name);
	printf("  &Matrix struct   : %p\n", &Z);
	printf("  Matrix.at        : %p\n", Z.at);
	printf("  &Matrix.at[0][0] : %p\n\n", &Z.at[0][0]);
}

```


* There is no issue when using either `copyVal` or `copyMat`.



## Case: Return type is `void`

### Option 1. Using copyVal 
* The structure is copied, but the internal pointer remains the same. Can apply modification.
  
```c++
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include "../../include/myMatrix.h"

void myFunc_Q3(Matrix Z, Matrix U);
void printAddress(Matrix Z, const char* name);

int main(int argc, char* argv[]){
    int M = 3;
    int dim = 1;

    double z[3] = { 1, 2, 3 };
    Matrix Z = arr2Mat(z, M, dim);
    Matrix Z_out2 = zeros(Z.rows, Z.cols);
    
    printf("\n**************************************************");
    printf("\n|         Question 3. (void, copyVal)            |");
    printf("\n**************************************************\n");

    printMat(Z_out2, "Z_out2 is (before)");
    myFunc_Q3(Z, Z_out2);
    printMat(Z_out2, "Z_out2 is (after)");
    
    freeMat(Z);
    freeMat(Z_out2);

    system("pause");
    return 0;
}

void myFunc_Q3(Matrix Z, Matrix U) {

	printf("\n[myFunc_Q3]\n");
	printAddress(U, "U in myFunc_Q3");

	// Copy U
	copyVal(Z, U);
	printAddress(U, "U in myFunc_Q3 (after)");	
	return;
}


void printAddress(Matrix Z, const char* name)
{
	printf("%s\n", name);
	printf("  &Matrix struct   : %p\n", &Z);
	printf("  Matrix.at        : %p\n", Z.at);
	printf("  &Matrix.at[0][0] : %p\n\n", &Z.at[0][0]);
}


```
### Option 2. Using copyMat (Need to pass by Pointer)
* The address of the Matrix is sent to the function.
* Need to use as a pointer in the passed Function.
  
```c++
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include "../../include/myMatrix.h"

void myFunc_Q5(Matrix *Z, Matrix *U);
void printAddress(Matrix Z, const char* name);

int main(int argc, char* argv[]){
    int M = 3;
    int dim = 1;

    double z[3] = { 1, 2, 3 };
    Matrix Z = arr2Mat(z, M, dim);
    Matrix Z_out4 = zeros(Z.rows, Z.cols);
    
    printf("\n**************************************************");
    printf("\n|         Question 5. (void, copyMat)            |");
    printf("\n**************************************************\n");

    printMat(Z_out4, "Z_out4 is (before)");
    myFunc_Q5(&Z, &Z_out4);
    printMat(Z_out4, "Z_out4 is (after)");
    
    freeMat(Z);
    freeMat(Z_out4);

    system("pause");
    return 0;
}

void printAddress(Matrix Z, const char* name)
{
	printf("%s\n", name);
	printf("  &Matrix struct   : %p\n", &Z);
	printf("  Matrix.at        : %p\n", Z.at);
	printf("  &Matrix.at[0][0] : %p\n\n", &Z.at[0][0]);
}

void myFunc_Q5(Matrix *Z, Matrix *U) {

	printf("\n[myFunc_Q5]\n");

	printf("&U (address of pointer variable) : %p\n", (void*)&U);
	printf("U (points to Matrix struct)      : %p\n", (void*)U);
	printf("U->at (data pointer)             : %p\n\n", (void*)U->at);

	*U = copyMat(*Z);

	printf("&U (address of pointer variable) : %p\n", (void*)&U);
	printf("U (points to Matrix struct)      : %p\n", (void*)U);
	printf("U->at (data pointer)             : %p\n\n", (void*)U->at);

	return;
}
```



### Option 3. Using copyMat (Does not return modified Matrix)

* The Matrix is copied and sent to the function.
* Does not return the modified Matrix to 'main'

```c++
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include "../../include/myMatrix.h"

void myFunc_Q4(Matrix Z, Matrix U);
void printAddress(Matrix Z, const char* name);

int main(int argc, char* argv[]){
    int M = 3;
    int dim = 1;

    double z[3] = { 1, 2, 3 };
    Matrix Z = arr2Mat(z, M, dim);
    Matrix Z_out3 = zeros(Z.rows, Z.cols);
    
    printf("\n**************************************************");
    printf("\n|         Question 4. (void, copyMat)            |");
    printf("\n**************************************************\n");

    printMat(Z_out3, "Z_out3 is (before)");
    myFunc_Q4(Z, Z_out3);
    printMat(Z_out3, "Z_out3 is (after)");
    
    freeMat(Z);
    freeMat(Z_out3);

    system("pause");
    return 0;
}

void printAddress(Matrix Z, const char* name)
{
	printf("%s\n", name);
	printf("  &Matrix struct   : %p\n", &Z);
	printf("  Matrix.at        : %p\n", Z.at);
	printf("  &Matrix.at[0][0] : %p\n\n", &Z.at[0][0]);
}

void myFunc_Q4(Matrix Z, Matrix U) {
	printf("\n[myFunc_Q4]\n");
	printAddress(U, "U in myFunc_Q4");

	U = copyMat(Z);

	printAddress(U, "U in myFunc_Q4 (after)");
	return;
}
```

## Other Options
```c++
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include "../../include/myMatrix_student.h"

Matrix myFunc_Q1(Matrix Z);
Matrix myFunc_Q2(Matrix Z);
void myFunc_Q3(Matrix Z, Matrix U);
void myFunc_Q4(Matrix Z, Matrix U);
void myFunc_Q5(Matrix Z, Matrix* U);
void printAddress(Matrix Z, const char* name);

int main(int argc, char* argv[]) {
    int M = 3;
    int dim = 1;

    double z[3] = { 1, 2, 3 };
    Matrix Z = arr2Mat(z, M, dim);
    printMat(Z, "Z is ");

    printf("\n**************************************************");
    printf("\n|          Question 1. (Matrix, copyVal)         |");
    printf("\n**************************************************\n");

    printAddress(Z, "Z is  ");
    Matrix Z_out = zeros(Z.rows, Z.cols);
    
    printMat(Z_out, "Z_out is (before)");
    printAddress(Z_out, "Z_out  ");

    Z_out = myFunc_Q1(Z);
    printMat(Z_out, "Z_out is (after)");
    printAddress(Z_out, "Z_out (after)  ");
    


    printf("\n**************************************************");
    printf("\n|          Question 2. (Matrix, copyMat)         |");
    printf("\n**************************************************\n");
    printAddress(Z, "Z is  ");

    Matrix Z_out1 = zeros(Z.rows, Z.cols);
    printMat(Z_out1, "Z_out1 is (before)");
    printAddress(Z_out1, "Z_out1  ");

    Z_out1 = myFunc_Q2(Z);
    printMat(Z_out1, "Z_out1 is (after)");
    printAddress(Z_out1, "Z_out1 (after)  ");


    

    printf("\n**************************************************");
    printf("\n|         Question 3. (void, copyVal)            |");
    printf("\n**************************************************\n");
    
    Matrix Z_out2 = zeros(Z.rows, Z.cols);
    printMat(Z_out2, "Z_out2 is (before)");
    printAddress(Z_out2, "Z_out2  ");

    myFunc_Q3(Z, Z_out2);
    printMat(Z_out2, "Z_out2 is (after)");
    printAddress(Z_out2, "Z_out2 (after)  ");

    
    printf("\n**************************************************");
    printf("\n|         Question 4. (void, copyMat)            |");
    printf("\n**************************************************\n");

    printAddress(Z, "Z is  ");

    Matrix Z_out3 = zeros(Z.rows, Z.cols);
    printMat(Z_out3, "Z_out3 is (before)");
    printAddress(Z_out3, "Z_out3  ");

    myFunc_Q4(Z, Z_out3);
    printMat(Z_out3, "Z_out3 is (after)");
    printAddress(Z_out3, "Z_out3 (after)  ");



    

    printf("\n**************************************************");
    printf("\n|         Question 5. (void, copyMat)            |");
    printf("\n**************************************************\n");
    
    printAddress(Z, "Z is  ");
    
    Matrix Z_out4 = zeros(Z.rows, Z.cols);
    printMat(Z_out4, "Z_out4 is (before)");
    printAddress(Z_out4, "Z_out4  ");

    myFunc_Q5(Z, &Z_out4);
    printMat(Z_out4, "Z_out4 is (after)");
    printAddress(Z_out4, "Z_out4 (after)  ");



    freeMat(Z);    
    freeMat(Z_out);
    freeMat(Z_out1);
    freeMat(Z_out2);
    freeMat(Z_out3);
    freeMat(Z_out4);

    system("pause");
    return 0;
}

Matrix myFunc_Q1(Matrix Z)
{
    int n = Z.rows;
    Matrix F = zeros(n, 1);

    printf("\n[myFunc_Q1]\n");
    printAddress(Z, "Z in myFunc_Q1");

    printAddress(F, "F in myFunc_Q1 ");
    copyVal(Z, F);
    printAddress(F, "F in myFunc_Q1 (after)");

    return F;
}

Matrix myFunc_Q2(Matrix Z)
{
    int n = Z.rows;
    Matrix F = zeros(n, 1);

    printf("\n[myFunc_Q2]\n");
    printAddress(Z, "Z in myFunc_Q2");

    printAddress(F, "F in myFunc_Q2 ");
    F = copyMat(Z);
    printAddress(F, "F in myFunc_Q2 (after)");
    return F;
}

void myFunc_Q3(Matrix Z, Matrix U) {

    printf("\n[myFunc_Q3]\n");
    printAddress(U, "U in myFunc_Q3");

    // Copy U
    copyVal(Z, U);
    printAddress(U, "U in myFunc_Q3 (after)");
    return;
}

void myFunc_Q4(Matrix Z, Matrix U) {
    printf("\n[myFunc_Q4]\n");
    printAddress(U, "U in myFunc_Q4");

    U = copyMat(Z);

    printAddress(U, "U in myFunc_Q4 (after)");
    return;
}

void myFunc_Q5(Matrix Z, Matrix* U) {

    printf("\n[myFunc_Q5]\n");
    printf("Matrix->at             : %p\n\n", (void*)U->at);
        
    *U = copyMat(Z);
    printf("Matrix->at)             : %p\n\n", (void*)U->at);

    return;
}


void printAddress(Matrix Z, const char* name)
{
    printf("%s\n\r", name);
    //printf("  &Matrix.at[0][0] : %p\n\n", &Z.at[0][0]);
    //printf("  &Matrix struct   : %p\n\n\r", &Z);
    printf("  Matrix.at        : %p\n", Z.at);
}



```
***
