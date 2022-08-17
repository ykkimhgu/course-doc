# Array

## Lesson

**코딩도장 핵심요약**

[1D Array 핵심요약](https://dojang.io/mod/page/view.php?id=673)

## 

![](../../../.gitbook/assets/image%20%2873%29.png)



## Example Code

### Example 1

[C_array1d_example.c](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/pointer-array)

```c++
#include <stdio.h>
#include <stdlib.h>

void printVec(double *_vec, int _row);

int main()
{
	// Static Matrix Allocation  1-D array 
	// fixed array size and initial constant values
	double a[4] = { 1, 2, 3, 4 };
	double b[] = { 2, 3, 4, 5 };
	double c[4] = { 0 };

	// Print 1-D array element
	printVec(a, 4);

	system("pause");
	return 0;
}

void printVec(double *_vec, int _row)
{
	for (int i = 0; i<_row; i++)
	  printf("Vector[%d] = %f \n", i, _vec[i]);
	printf("\n");
}
```







## Exercise

* [Online C Compiler](https://www.onlinegdb.com/online_c_compiler)

* [Exercise Code](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/pointer-array)

* [Exercise-Solution Code]()



### Exercise 1

[C_array1D_exercise.c](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/pointer-array)

Declare and define the following functions 

`void addVec(float _src1[], float _src2[], float _dst[], int _vecLength);`&#x20;

```cpp

void main() 
{
// Exercise 1 ***********************************************
printf(“Exercise 1\n");

float x[4] = {1,2,3,4};
float y[4] = {5,6,7,8};
float out[4] = { 0 };
float out_dotProduct = 0;

int vecLength = 4;

addVec(x, y, out, vecLength);
printf("addVector result (x + y): ");
printVec(out, vecLength);

system("pause");
}

```

![Exercise\_1 result](<C:\Users\ykkim\source\repos\GithubDesktop\course-doc\.gitbook\assets\image (67).png>)



### Exercise 2

[C_array1d_exercise2.c](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/pointer-array)

Assign array address to pointer ptr. 

Print each element of 1D array by using pointer

```c++
#include <stdio.h>
int main(){
    int st[5] = { 1,2,3,4,5 };
    int* ptr;
    
    ptr = _______________;     
    for (int i = 0; i < 5; i++) {
        //  print each element by using pointer  e.g.  (ptr)
        
    }
}

```

