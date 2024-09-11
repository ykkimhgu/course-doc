# Tutorial: Integration



# Exercise - MATLAB (25 min)

Estimate the velocity from the dataset of acceleration

```matlab
clear all
x=[0 5 10 15 20 25 30 35 40 47 50 54 60];
y=[0 3 8 20 33 42 40 48 60 12 8 4 3];
N=length(x);
plot(x,y,'.-');

% Matlab function
I_matlab = trapz(x,y);    
```

**Download the tutorial source file and fill in the blanks.**

Run the code and validate your answer

* MATLAB tutorial source file : [TU\_Integration\_Matlab 2024.zip](https://github.com/ykkimhgu/NumericalProg-student/blob/main/tutorial/TU_Integration/Tutorial_Integration_matlab_2024.zip)

***

# Exercise - C Programming

Create a new empty project in Visual Studio Community. 

Name the project as **TU\_Integration**

* **e.g ) C:\Users\yourID\source\repos\NP\tutorial\TU\_Integration**

Create a new C/C++ source file for main()

* Name the source file as `TU_integration_main.cpp`
* Paste from the following code: [TU\_integration\_student](https://github.com/ykkimhgu/NumericalProg-student/blob/main/tutorial/TU\_integration\_student.cpp)

Download the answer report file for this tutorial. : [Answer Report](https://github.com/ykkimhgu/NumericalProg-student/blob/main/tutorial/TU\_Integration\_Answer\_\_yourName\_ID.docx)
> This is NOT for the assignment

***

## Part 1 : Integration of Discrete Points

Create a function for numerical differentiation from a set of data. Read the instructions in the source code.

### **Exercise 1: Trapezoid (25 minutes)**

Create Trapezoidal method for discrete data inputs. Upload the result in LMS.

$$
I(f) = \frac{1}{2}\sum\limits_{i = 0}^{N - 1} {\left[ {f\left( {{x_i}} \right) + f\left( {{x_{i + 1}}} \right)} \right]({x_{i + 1}} - {x_i})}
$$

```cpp
double trapz (double x[ ], double y[ ], int m);
```

* In the report, screen capture the output window and paste your code
* Use 1D array type with dataset length m.
* intervals= N, # dataset=N+1=m, The ranges are x\[0] to x\[m-1]

Declare and define your functions in your header files, located in `\include` folder

* function definitions: `myNP.h`
* function declaration: `myNP.cpp`

####

### **Exercise 2: Simpson13 (25 minutes)**

Create Simpson13 method for discrete data inputs. Upload the result in LMS.

* In the report, screen capture the output window and paste your code
* Use Simpson 13 method :
* N even numbers, same intervals, from a(=x0) to b(=xN), h=(b-a)/N.
* The interval should be h=(b-a)/N
* Defined in **myNM.cpp** source file

```cpp
double simpson13(double x[ ], double y[ ], int m);
```

Simpson 13 method :

$$
I = \frac{h}{3}[f({x_0}) + 4\sum\limits_{i = 1,3,5}^{N - 1} {f\left( {{x_i}} \right) + 2\sum\limits_{k = 2,4,6}^{N - 2} {f\left( {{x_k}} \right)} + } f({x_N})]
$$

***

## **Part 2: Integration from a Function**

### Exercise 3 : (25 min)

Create Simpson13() when a function is given as the input. Upload the result in LMS

```cpp
double myFunc (const double x);  // in main.cpp
double integral(double func(const double x), double a, double b, int n);  // in myNM.h
```

* In the report, screen capture the output window and paste your code
* Use Simpson 13 method :
* N even numbers, same intervals, from a(=x0) to b(=xN), h=(b-a)/N.
* The interval should be h=(b-a)/N
* Defined in **myNM.cpp** source file

Create the following function that calls `myFunc()` . The true answer is PI/2

$$
I(f) = \int_{ - 1}^1 {\sqrt {1 - {x^2}} dx}
$$
