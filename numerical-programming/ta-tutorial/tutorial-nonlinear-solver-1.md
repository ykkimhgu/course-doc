---
description: For n equations with  n unknowns
---

# Tutorial: Systems of Nonlinear Equation

## Problem 1

Solve for **x, y**  from



$$
𝑓_1 (𝑥,𝑦)=𝑦−1/2 (𝑒^{(𝑥/2)}+𝑒^{(−𝑥/2)} )=0 \\
𝑓_2 (𝑥,𝑦)=9𝑥^2+25𝑦^2−225=0
$$

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

### Tutorial: MATLAB

#### Create MATLAB program for solving a system of non-linear equations

Download the tutorial source file and fill in the blanks.&#x20;

* MATLAB tutorial source file : [TU\_SystemNonlinear.zip](https://github.com/ykkimhgu/NumericalProg-student/blob/main/tutorial/TU_Nonlinear/TU_nonlinear_student.mlx)

***

#### 1)  Define functions that returns vector F(x) and matrix J(x).

&#x20;`Matrix myFuncEx1(Matrix X); // X: vector in.   Returns Vector F  (nx1)`

&#x20;`Matrix myJacobEx1(Matrix X); // X: vector in.  Returns Matrix J  (nxn)`

#### 2) Define a system non-linear solver

`Matrix nonlinearSys(Matrix Func(Matrix _Z), Matrix Jacob(Matrix _Z),       Matrix _Z0, double tol);`

returns  solution vector XN   (nx1)

#### 3) Then, solve the example problem





### Tutorial: C-Programming

1. Create a new project “ **TU\_SystemNonlinear**” with Visual Studio, under the directory `\NP\tutorial\`
2. Download the tutorial source file from

* C-program tutorial source file : [TU\_SystemNonlinear\_student.cpp](https://github.com/ykkimhgu/NumericalProg-student/blob/main/tutorial/TU_Nonlinear/TU_nonlinear_student.cpp)

3. Add the downloaded source file under the project folder.
4. Fill-in the blanks&#x20;





## Problem 2

Solve for the 3-DOF transformation, angle (θ) and translation T=\[Δx, Δy], to move the vehicle to the new pose?

> Solution= \[θ=30 deg , Δx =100 , Δy= 100 ]

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>
