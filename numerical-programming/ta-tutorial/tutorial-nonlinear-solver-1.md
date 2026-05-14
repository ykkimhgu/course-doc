---
description: For n equations with  n unknowns
---

# Tutorial: Systems of Nonlinear Equation

## Preparation

Create a new project “ **TU\_SystemNonlinear**” with Visual Studio, under the directory `\NP\tutorial\`

Download the tutorial source files and save them under the project folder

* C-program tutorial source file : [TU\_SystemNonlinear\_student.cpp](https://github.com/ykkimhgu/NumericalProg-student/blob/main/tutorial/TU_Nonlinear/TU_nonlinear_student.cpp)
* Matlab tutorial files:  [TU\_SystemNonlinear.zip](https://github.com/ykkimhgu/NumericalProg-student/blob/main/tutorial/TU_SystemNonlinear/TU_NonLinearSystem_student_MATLAB.zip)





## Problem 1

Solve for **x, y**  from



$$
𝑓_1 (𝑥,𝑦)=𝑦−1/2 (𝑒^{(𝑥/2)}+𝑒^{(−𝑥/2)} )=0 \\
𝑓_2 (𝑥,𝑦)=9𝑥^2+25𝑦^2−225=0
$$

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

### Tutorial: MATLAB

#### Create MATLAB program for solving a system of non-linear equations

Download the tutorial source file and fill in the blanks.&#x20;

* MATLAB tutorial source file : [TU\_SystemNonlinear.zip](https://github.com/ykkimhgu/NumericalProg-student/blob/main/tutorial/TU_SystemNonlinear/TU_NonLinearSystem_student_MATLAB.zip)

***



### Tutorial: C-Programming

#### &#x20;Define functions of vector F(x) and matrix J(x)

&#x20;`Matrix myFuncEx1(Matrix X); // X: vector in.   Returns Vector F  (nx1)`

&#x20;`Matrix myJacobEx1(Matrix X); // X: vector in.  Returns Matrix J  (nxn)`

#### Define a  non-linear system solver

`Matrix nonlinearSys(Matrix Func(Matrix _Z), Matrix Jacob(Matrix _Z),       Matrix _Z0, double tol);`

returns  solution vector XN   (nx1)

#### Solve the example problem



## Problem 2

Solve for the 3-DOF transformation, angle (θ) and translation T=\[Δx, Δy], to move the vehicle to the new pose

See lecture note for detail.

> Solution= \[θ=30 deg , Δx =100 , Δy= 100 ]

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

### Hint

<figure><img src="../../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>
