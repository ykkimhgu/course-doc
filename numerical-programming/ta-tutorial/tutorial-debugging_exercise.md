# Tutorial: Debugging\_Exercise

## Tutorial: Common Debugging Problems



Here is a list of common debugging mistakes that students make in C programming.

### List of Common Mistakes

**Basic Sytanx Error**

* Missing variable type declaration
* Missing semicolon
* Missing closing bracket in a conditional or loop statement
* Format specifier error
* More arguments than format specifiers
* Type error in `printf`

**Using Header File**

* Not Including header file
* Include path error
* Duplicate declaration of functions

**Variable Type Usage**

* Dividing an interger
* Multiplying an interger with a double(float) number

**Array**

* Array size declaration
* Array size overflow
* Missing library declaration for dynamic memory allocation

###

### Preparation

&#x20;0\.  Read about  [Debugging in Visual Studio](https://dojang.io/mod/page/view.php?id=806)

{% embed url="https://youtu.be/whoJbCQKiLs?list=PLa9dKeCAyr7jsbboqbsSnsTIVds0Dl3Ec" %}

1. Download the tutorial file and unzip.
   * [Download file](https://github.com/ykkimhgu/NumericalProg-student/tree/main/tutorial/TU_Debugging_Example)
   * `TU_Debugging_Exercise.zip`
2. Copy Library Header File to **include** directory: `.\NP\include\`
   * `NP_debugging_header.cpp`, `NP_debugging_header.h`
   * Read here for detail:
3. Create a new project under **tutorial** directory: `.\NP\tutorial\`
   * Project name: `TU_Debugging`
4. Copy the source file under **project** directory: `\NP\tutorial\TU_Debugging\`
   * File: `TU_Debugging_Examples.cpp`
   * See Source File in Appendix
5. Add the source file in Visual Studio '소스파일'

```cpp
#include <stdio.h>
#include "../include/NP_debugging_header.h"

/////////////////////////////////////////////////////////
//	Function Declaration
/////////////////////////////////////////////////////////

/////////////////////////////////////////////////////////
//	MAIN Function
/////////////////////////////////////////////////////////

int main() {
	int a = 1;
	int b = 2;
	int c = 3
		d = 4;

	int count = 0;

	for (int j = 0; j < 6; j++) {
		if (j == 5)
			printf("j is %d\n", j);

	printf("a= %d,  b= %f \n", a, b);
	printf("sum is %d\n", sum_int(a, b));
	printf("difference is %d\n", subtract_int(a, b));

	return 0;
}

/////////////////////////////////////////////////////////
//					Function Definition
/////////////////////////////////////////////////////////

// Sum of two inte

gers
int sum_int(int a, int b) {
	int sum = a + b;
	return sum;
}
```

### Procedure

Run the program and fix all the debugging errors

You MUST read the error message first!!

<figure><img src="../../.gitbook/assets/image (138).png" alt=""><figcaption></figcaption></figure>

#### **Error: Header file include path**

**Error Message:**

<figure><img src="../../.gitbook/assets/image (139).png" alt="" width="375"><figcaption></figcaption></figure>

**Error Reason:**

1. Wrong directory path
2. Not included in V.Studio Project's include folder

**Solution:**

* Fix include directory path

```cpp
#include <stdio.h>
#include "../../include/NP_debugging_header.h"
```

* Add in V.Studio Project's include folder

<figure><img src="../../.gitbook/assets/image (140).png" alt="" width="375"><figcaption></figcaption></figure>

#### **Error: Missing Semicolon & Variable Type**

**Error Message:**

<figure><img src="../../.gitbook/assets/image (141).png" alt="" width="375"><figcaption></figcaption></figure>

**Error Reason:**

* Missing semicolon
* Missing variable type

**Solution:**

* add semicolon
*   include variable type

    ```cpp
    	int a = 1;
    	int b = 2;
    	int c = 3;
    	int	d = 4;
    ```



#### **Error: Missing Function Declaration**

**Error Message:**

<figure><img src="../../.gitbook/assets/image (142).png" alt="" width="375"><figcaption></figcaption></figure>

**Error Reason:**

* Declaration of function missing: `sum_int()`

**Solution:**

*   Add function declaration before main(): if the function _definition_ is located after `main()`

    ```cpp
    /////////////////////////////////////////////////////////
    // Function Declaration
    /////////////////////////////////////////////////////////

    int sum_int(int a, int b);  // <--- declare before main(). 

    int main() {
    	int a = 1;
    ```





Now, find the rest of errors and fix them.

***

## Assignment: Debugging

Due in 1 week.

### Preparation

1. Download the assignment file and unzip.
   * [Download file here](https://github.com/ykkimhgu/NumericalProg-student/blob/main/src/Assignment_Debugging.cpp)
   * `Assignment_Debugging.cpp`
2. Copy Library Header File to **include** directory: `.\NP\include\`
   * `NP_debugging_header.cpp`, `NP_debugging_header.h`
   * The same files used in the Tutorial.
3. Create a new project under the **assignment** directory: `.\NP\assignment\`
   * Project name: `Assignment_Debugging`
4. Copy the source file under the **project** directory: `\NP\Assignment\Assignment_Debugging\`
   * File: `Assignment_Debugging.cpp`
5. Add the source file in the Visual Studio Project

### Procedure

[Download the report template](https://github.com/ykkimhgu/NumericalProg-student/blob/main/docs/NP_Assignment_Debugging_Report_Template_2025.docx)

Run the code and read the error message first!!

Find all the errors and fix them.



You must write the report by including the following components

#### **Error:&#x20;**_**Error Name**_

**Error Message:**

* _Show the error message_

**Error Reason:**

* _Write the error reasons_

**Solution:**

* _Show how you have fixed it_
