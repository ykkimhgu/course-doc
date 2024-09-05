# Tutorial: Nonlinear solver


# Problem 

Solve for x, in a non-linear function of  

`f(x)= 8-4.5\*(x-sin(x))`

![image](https://user-images.githubusercontent.com/38373000/188638678-2bc85921-cc0c-49d9-a3d8-2d7be91df1fc.png)


# Tutorial: MATLAB

### Sample Code
```matlab
clear all
F = inline('8-4.5*(x-sin(x))');
dF=inline('-4.5*(1-cos(x))');​
x=0:0.001:10;
plot(x,F(x)); grid on
```


## Exercise 

Download the tutorial source file and fill in the blanks. Run the code and validate your answer.

* MATLAB tutorial source file : [TU\_nonlinear\_student.mlx](https://github.com/ykkimhgu/NumericalProg-student/blob/main/tutorial/TU_Nonlinear/TU_nonlinear_student.mlx)


---




# Tutorial: C-Programming 

1. Create a new project “ **TU\_Nonlinear**” with Visual Studio, under the directory   `\NP\tutorial\`
2. Download the tutorial source file from

* C-program tutorial source file : [TU\_nonlinear\_student.cpp](https://github.com/ykkimhgu/NumericalProg-student/blob/main/tutorial/TU_Nonlinear/TU_nonlinear_student.cpp)

3. Add the downloaded source file under the project folder. 


4. Prepare your library header files in your project:  `myNP_tutorial.h` and `myNP_tutorial.c`
    * This is the same header files used in the previous tutorial: [Tutorial-Sine Taylor: Part 2](https://ykkim.gitbook.io/ec/numerical-programming/ta-tutorial/tutorial-sine-taylor#part-2)
    * They must be located in  `\NP\include\` folder
    * If you do not have them, you can  [downloaded from the link](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/sineTaylor)
     

5. **Rename your library header files** as   `myNP_yourID.h` and `myNP_yourID.c`
    * Example:  `myNP_20030011.c`, `myNP_20030011.h`
    * From now on, you will update your functions in the library header files.



## Exercise 1
On the given source code template `TU_nonlinear_student.cpp`, fill-in the missing codes.

**Bisection Method**

Assuming (func(a) \* func(b) <0 )

1.  First, Write down a pseudocode for the bisection

    ``` c
    // YOUR pseudocode goes here// YOUR pseudocode goes here
    ```
2. Based on the pseudocode, fill in the blanks in the source code.

    ``` c
    double bisection(float _a, float _b, float _tol);
    ```

3. Move your functions from main source file to your header files
   * function definitions: `myNP_yourID.h`
   * function declaration: `myNP_yourID.c`

##

## Exercise 2

Modify your Bisection function with considering the following conditions

* if(func(a) \* func(b) > 0), No solution exists
* if(func(a) \* func(b) ==0), Either (1) a=Xtrue or (2) b=Xtrue
* if(k==Nmax) Solution did not converge within given iteration

##

## Exercise 3

**Newton Raphson Method**

1.  First, Write down a pseudocode for the method

    ```cpp
    // YOUR pseudocode goes here// YOUR pseudocode goes here
    ```
2. Based on the pseudocode, fill in the blanks in the source code.

   ```cpp
   double newtonRaphson(float _a, float _b, float _tol);
   ```

3. You need to define the function `f(x)` and the derivative function `dfdx(x)` as

   ```cpp
   double func(float _x);
   double dfunc(float _x);
   ```

   > For dfdx(x), get the derivative formula analytically.

4. After you have compile them successfully, move your functions to your header files


###

## Exercise 4

**Modify Newton Raphson Method with function callback**

[Read how to pass a function as an input argument](https://ykkim.gitbook.io/ec/numerical-programming/ta-tutorial/tutorial-function-callback)

Modify the newton-raphson function that calls the problem functions f(x) and dfdx(x) as input arguments. 

```cpp
double newtonRaphson(double func(double _x), double dfunc(double _x), double _x0, double _tol);
```

Now,  the problem function `f(x)` and the derivative function `dfdx(x)` should be located in the main source code `TU_nonlinear_student.cpp`
> NOT in `myNP_yourID.h`

   ```cpp
   // Move these declaration and definitions of problem functions to your lib header 
   double func(float _x);
   double dfunc(float _x);
   ```
