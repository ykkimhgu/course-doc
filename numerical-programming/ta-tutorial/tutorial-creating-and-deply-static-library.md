# Tutorial: Creating and Deply Static Library

### Introduction <a href="#introduction" id="introduction"></a>

You will learn how to create and deply your NP functions as static library (\*.lib) using Visual Studio.

This will hide your source code of function definitions.&#x20;

The user can only see the function declaration in the header file (\*.h), NOT the definitions.



#### Preparation of library files <a href="#download" id="download"></a>

Download tutorial header files and Copy into the project folder

* [myMatrix\_student.h](https://github.com/ykkimhgu/NumericalProg-student/tree/main/include)
* [myMatrix\_student.cpp](https://github.com/ykkimhgu/NumericalProg-student/tree/main/include)

You can use your own header files for this tutorial.

> These files do not have main()



## Part 1: Building Library (\*.lib)

#### Create Project <a href="#create-project" id="create-project"></a>

Create a new empty project in Visual Studio Community.&#x20;

* Name the project as **NP\_Matrix\_Library**

Add  [myMatrix\_student.cpp](https://github.com/ykkimhgu/NumericalProg-student/tree/main/include)  to Souce File in VS

Add  [myMatrix\_student.h](https://github.com/ykkimhgu/NumericalProg-student/tree/main/include) to Header File in VS



#### Configure for a Static Library:

* In the Solution Explorer (usually on the right), right-click your project and select Properties.
* At the top of the Properties window, ensure Configuration is set to&#x20;
  * **Configuration(구성):** All Configurations
  * **Platform (플랫폼) :** All Platforms.
* Go to Configuration Properties > General.
* Change the Configuration Type&#x20;
  * from _Application (.exe)_ to Static library (.lib) 정적 라이브러리
* Click Apply and OK.

<figure><img src="../../.gitbook/assets/image (153).png" alt=""><figcaption></figcaption></figure>

#### Build the Library:

At the very top of Visual Studio, note your build architecture (e.g., **x64** and **Debug**).

* Go to Build -> Build Solution.  (Ctrl+B)
* Look at the Output window at the bottom. It will show you the exact folder path where \*`.lib` was saved (usually inside an `x64\Debug` folder in your project directory).
* Example:  `.\NP_Library\x64\Debug\`**`NP_Matrix_Library.lib`**



{% code expandable="true" %}
```
1>------ 빌드 시작: 프로젝트: NP_Matrix_Library, 구성: Debug x64 ------
1>myMatrix_student.cpp
1>NP_Library.vcxproj -> .\NP_Library\x64\Debug\NP_Matrix_Library.lib
========== 빌드: 1개 성공, 0개 실패, 0개 최신 상태, 0개 건너뜀 ==========
```
{% endcode %}



#### Deploy the Library:

Deploy the set of two files

* &#x20;`myMatrix_student.h`
* &#x20;`NP_Matrix_Library.lib`&#x20;

##

## Part 2: Using the Deployed Library (\*.lib)

#### Create Project <a href="#create-project" id="create-project"></a>

Create a new empty project in Visual Studio Community.&#x20;

* Name the project as **TU\_Library\_Example**

Create a New Source File:  `TU_Library_Example_Main.cpp`

{% code expandable="true" %}
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

#include "myMatrix_student.h"

int main(int argc, char* argv[]) {

    int M = 3;
    int dim = 1;
    double z[3] = { 1, 2, 3 };

    Matrix Zinit = arr2Mat(z, M, dim);
    Matrix Z = createMat(M, 1);
    
    copyMatrix(Z, Zinit);
    printMat(Z, "Z is ");

    freeMat(Z); 
    
    system("pause");
    return 0;
}

```
{% endcode %}



#### Add Files in Header

These files need to be in the same directory as the project.(where their `.vcxproj` file is located)

* &#x20;`myMatrix_student.h`
* &#x20;`NP_Matrix_Library.lib`&#x20;



#### Build:

Compile and see if the program works properly



## Part 3: Exercise

Create and deply your NP header files `myNP.h` and  `myNP.lib`&#x20;



