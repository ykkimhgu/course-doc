---
description: Least Squares Regression
---

# Tutorial: Curve Fitting

## Part 1-1: Line Curve Fitting&#x20;

### Problem

#### Predict the pressure if the temperature is increased to 150C based on Charles's law for ideal gasP=kT, where k is a constant.

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt="" width="237"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1).png" alt="" width="375"><figcaption></figcaption></figure>

### Tutorial: Matlab

<pre class="language-matlab"><code class="lang-matlab">T=[30	40	50	60	70	80];
P=[1.05	1.07	1.09	1.14	1.17	1.21];

<strong>% MATLAB function of curvefitting
</strong>% z=[a1, a0]; 
Z=polyfit(T,P,1)

% yopt=Z(1).*x+Z(2);
x=30:10:150;
yopt=polyval(Z,x);  
</code></pre>

### Exercise: MATLAB

Download the tutorial source file

* [TU\_Curvefitting\_student\_2024.mlx](https://github.com/ykkimhgu/NumericalProg-student/blob/main/tutorial/TU_Curvefitting/TU_Curvefitting_student_2024.mlx)

Fill-in the blanks to create `function [a0,a1] = linearFit(X, Y)`

### Exercise: C

Download the tutorial source code

* [Assignment\_Curvefit\_student.cpp](https://github.com/ykkimhgu/NumericalProg-student/blob/main/src/Assignment_Curvefit_student.cpp)

Fill-in the blanks to create functions that  calculates coefficients of least squares regression (line)

`void linearFit(double vecZ[], double vecX[], double vecY[]);`&#x20;

If you choose to use  Matrix structure

`linearFit_mat(Matrix _X, Matrix _Y);`&#x20;



<pre class="language-c"><code class="lang-c"><strong>	double T[] = { 30, 40, 50, 60, 70, 80 };
</strong>	double P[] = { 1.05, 1.07, 1.09, 1.14, 1.17, 1.21 };
	double Z_Q1[2] = { 0 };
	int n = 1;	// nth order
	int m_Q1 = 6;	// length of dataset

	// Option 1: using 1D array
	n = 1;
	polyFit(Z_Q1, T, P, n);
	
	// Option 2	
	// Delete the below if you selected Option 1
	Matrix matT = arr2Mat(T, m_Q1, 1);
	Matrix matP = arr2Mat(P, m_Q1, 1);
	Matrix vecZ_Q1 = polyFit_mat(matT, matP, n);
	printMat(vecZ_Q1, "Z_Q1");

</code></pre>







## Part 1-2: Higher order polynomial curve fitting&#x20;

### Problem

Find the optimal higher-order polynomial to fit the given dataset. assume the model has n=4 order polynomial form

<figure><img src="../../.gitbook/assets/image (2).png" alt="" width="375"><figcaption></figcaption></figure>

### Exercise: MATLAB

```matlab

Xdata = 0:0.4:6;
Ydata = [0, 3, 4.5, 5.8, 5.9, 5.8, 6.2, 7.4, 9.6, 15.6, 20.7, 26.7, 31.1, 35.6, 39.3, 41.5];

% MATLAB function of curvefitting
% z=[an,..., a1, a0]; 

% polynomial order n=4
n=4;
Z=polyfit(Xdata,Ydata,n)

% yhat on the fitted curve:
% yopt=Z(5).*(x.^4) +...+ +Z(0);
Yopt=polyval(Z,Xdata);

figure
plot(Xdata,Ydata, '*r')
hold on
plot(Xdata,Yopt, '-b')
xlabel('strain','fontsize',15)
ylabel('stress','fontsize',15)
title('Polyfit')
```

### Exercise: C

Fill-in the blanks to create functions that  calculates coefficients of least squares regression of  Nth order polynomial&#x20;

`void polyFit(double vecZ[], double vecX[], double vecY[], int n);`

If you choose to use  Matrix structure

`Matrix polyFit_mat(Matrix _vecX, Matrix _vecY, int n);`



