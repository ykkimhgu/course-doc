# Tutorial: Matrix Template


# Tutorial: Using Matrix Structure



This tutorial explains how to use a Matrix structure for solving linear equations.



For the assignment,  you must use the given Matrix structure and follow instructions for saving and modifying the data. 



# Preparation 



## Download

1. Download data files:  [NP_Matrix_TemplateCode_Data.zip](https://github.com/ykkimhgu/NumericalProg-student/blob/main/tutorial/NP_Matrix_TemplateCode_Data.zip) 

2. Download tutorial source files: 

* [TU_matrixTemplate_student.cpp](https://github.com/ykkimhgu/NumericalProg-student/tree/main/tutorial)

* [myMatrix_student.h,  myMatrix_student.cpp ](https://github.com/ykkimhgu/NumericalProg-student/tree/main/include)

  







## Create Data Folder

Create a folder in **C:\\** Drive and name the folder as `NP_Matrix_Data`

* **C:\NP_Matrix_Data**

  

For each assignment, create the assignment folder and save the dataset.



Example:    Assignment999

* **C:\NP_Matrix_Data\Assignment999 **



For this tutorial, unzip the downloaded data files and copy them under the data folder `Assignment999`



![NP_Matrix_Data_Example_img](https://github.com/ykkimhgu/NumericalProg-student/blob/main/docs/NP_Matrix_Data_example.png?raw=true)



## Create Project

Create a new empty project in Visual Studio Community. Name the project as **TU_MatrixTemplate**

* **e.g ) C:\Users\yourID\source\repos\NP\tutorial\TU_MatrixTemplate**

  

Create a new C/C++ source file for main()

* Name the source file as `TU_matrixTemplate.cpp`

* Use the following source code :   TU_matrixTemplate_student.cpp



Save the downloaded library header files in your `\include\` folder  :[myMatrix_student.h,  myMatrix_student.cpp ](https://github.com/ykkimhgu/NumericalProg-student/tree/main/include)

* Write your name in the comment section
* Re-name the header files as:   
  * myMatrix.h,  myMatrix.cpp 






# Create and Modify Dataset 

For each assignment, create the assignment folder and save the dataset.

* Example:    Assignment000
  * **C:\NP_Matrix_Data\Assignment000 **



Use the  **text file** and **file name**  as instructed in  each assignment. 

>  You must use the same name of  **text files**  as instructed. Otherwise, it will not be graded



Example: 

```
- Announcement for Assignment3 -

[File Path]
    C:/NP_Matrix_Data/Assignment3

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

#### Make Text File

![file_explorer_img](https://github.com/ykkimhgu/NumericalProg-student/blob/main/docs/file_explorer_img.png?raw=true)



#### Modifying Text File

* Change Row : **Enter**
* Change Column: **Tap**

> Notice: Regardless of number length



### Example :  

For Matrix,  type: 

* 1 tab 3 tab -2 tab 4 enter  ....    3  tab -1 tab 6 tab 2   ctrl+s (저장)

![matrix_text_file_example](https://github.com/ykkimhgu/NumericalProg-student/blob/main/docs/matrix_text_file.png?raw=true)

For Vector,  type: 

* -11 enter 6 enter  -9  enter  15   ctrl+s (저장)

![vector_text_file_example](https://github.com/ykkimhgu/NumericalProg-student/blob/main/docs/vector_text_file.png?raw=true)



# Using Matrix Structure Library


## Matrix structure library 

Provided library: `myMatrix.h `

```c++
// myMatrix.h
typedef struct { 
	double** at;
	int rows, cols;
}Matrix;


// Create Matrix with specified size
extern	Matrix	createMat(int _rows, int _cols);

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



### Example Code

```c++
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
	Matrix matD = zerosMat(matA.rows, matB.cols);


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





## What to change for Assignment


1. Initially, change the assignment number  for `#define ASGN `

    * DO NOT modify other code lines

    

```C
    #include "myMatrix.h"
    
    #define ASGN		999		// enter your assignment number
    #define EVAL		0		// [※ DO NOT EDIT !!!]
    
    
    int main(int argc, char* argv[])
    {
    	/*	 [※ DO NOT EDIT !!!]   Resources file path setting for evaluation	*/
    	std::string path = "C:/NP_data/Assignment" + std::to_string(ASGN) + "/";
    
    #if EVAL
    	path += "eval/";
    #endif
    
        ...
    
```

2. Read data text files.  You must use the given file names.

3. Then, apply your numerical programming algorithm.



```c++
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


4. Prints  vector or matrix  results. You have to give a brief description for each print. 

```c++
    
        /*==========================================================================*/
        /*                          Print your results                              */
        /*==========================================================================*/
        printMat(matA, "Problem 1, Matrix A");
        printMat(vecb, "Problem 1, Vector b");
    
    
```

5. Free memory allocated to a matrix.

   * Even if you omit the free process, the code will work.  However, it can cause memory leaks.

     

```C++
        /*==========================================================================*/
        /*                          Deallocate memory                               */
        /*==========================================================================*/
        freeMat(matA);
        freeMat(vecb);
   
   
```

---








# Tutorial & Exercise


Declare and define the following functions in  `myMatrix.h` and  `myMatrix.cpp`



```c++
// initialization of Matrix elements
extern	void	initMat(Matrix _A, double _val);

// Create matrix of all zeros
extern	Matrix	zeros(int _rows, int _cols);

// Create matrix of all ones
extern	Matrix	ones(int _rows, int _cols);

// Multiply matrix A to B
extern	Matrix	multMat(Matrix _A, Matrix _B);

// Create identity 
extern	Matrix	eye(int _rows, int _cols);

// Create Transpose matrix
extern	Matrix	transpose(Matrix _A);

// Copy matrix
extern	Matrix	copyMat(Matrix _A);

// Copy matrix Elements from A to B
extern	void	copyVal(Matrix _A, Matrix _B);
```




---



