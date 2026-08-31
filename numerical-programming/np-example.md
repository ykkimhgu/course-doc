# Example: NP C-Programming

## NP Library Header

> version 2026

<details>

<summary>myNP.h</summary>

{% code expandable="true" %}
```cpp

/*==========================================================================*/
/*					Nonlinear Equation
/*==========================================================================*/
// Solve a single nonlinear equation
double newtonRaphson(double func(double _x), double dfunc(double _x), double _x0, double _tol);

/*==========================================================================*/
/*					Differentiation
/*==========================================================================*/
// Return the dy/dx results for the input data. (truncation error: O(h^2))
void gradient1D(double dydx[], double x[], double y[], int m);
extern	Matrix	gradient(Matrix _x, Matrix _y);

// Return the dy/dx results for the target equation. (truncation error: O(h^2))
void gradientFunc1D(double dydx[], double func(const double x), double x[], int m);
extern	Matrix	gradientFunc(double func(const double x), Matrix xin);

/*==========================================================================*/
/*					Integration
/*==========================================================================*/
extern double integral(double _x[], double _y[], int _m);
extern double integralFunc(double func(const double _x), double _x0, double _xf, int _m);

/*==========================================================================*/
/*					Linear System
/*==========================================================================*/
// Create inverse matrix
extern	Matrix	invMat(Matrix _A);

// Solving linear system problem
extern	Matrix	solveLinear(Matrix _A, Matrix _b);

/*==========================================================================*/
/*					Eigenvector/value
/*==========================================================================*/

// Create eigenvalue matrix
extern	Matrix	eigVal(Matrix _A);

// Create nomalized eigenvector matrix
extern	Matrix	eigVec(Matrix _A);

// Return nomalized eigenvecto, Diagnoal Eigenvalue matrix
extern	void eig(Matrix V, Matrix D, Matrix _A);

/*==========================================================================*/
/*					Linear Regression
/*==========================================================================*/
// Calculates coefficients of least squares regression - Line
Matrix	linearFit_mat(Matrix _X, Matrix _Y);
void linearRegression(double z_opt[], double xdata[], double ydata[], int dataN);


// Calculates coefficients of least squares regression - Nth order polynomial
Matrix	polyFit_mat(Matrix _vecX, Matrix _vecY, int orderN);
void polyFit(double vecZ[], double vecX[], double vecY[], int dataL, int orderN);

/*==========================================================================*/
/*					ODE
/*==========================================================================*/
// 1st order ODE
extern void odeRK2(double y[], double odeFunc(const double t, const double y), const double t0, const double tf, const double h, const double y_init);
extern void odeRK3(double y[], double odeFunc(const double t, const double y), const double t0, const double tf, const double h, const double y0);

// 2nd order ODE
void sys2RK2(double y1[], double y2[], void odeFuncSys(double dYdt[], const double t, const double Y[]), const double t0, const double tf, const double h, const double y1_init, const double y2_init);


```
{% endcode %}

</details>

### Non-linear solver

First, define a non-linear function in the form of f(v)=0.

Then, use a non-linear equation solver fzero (). ![image-20230818144953414](https://github.com/ykkimhgu/course-doc/assets/38373000/d6167efa-b2b9-4098-86be-68b2cc108cb9)

​

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

{% tabs %}
{% tab title="C Example" %}
```cpp
/*==========================================================================*/
/*					Example: Nonlinear Equation
/*==========================================================================*/
	// Initialization
	double tol = 0.00001;
	double x0 = 3;
	double x_sol;

	// NP Algorithm
	x_sol = newtonRaphson(func_nonlinear,dfunc_nonlinear,x0,tol);

	// Display Output	
	printf("\n Nonlinear solution x= %0.5f \t\n", x_sol);
	
	

/************     Define Nonlinear Functions     ************/
double func_nonlinear(double _x) {
	double F = 0;
	F = 8 - 4.5 * (_x - sin(_x));	
	return F;
}

double dfunc_nonlinear(double _x){
	double dF = 0;
	dF =  -4.5 * (1 - cos(_x));	
	return dF;
}
```
{% endtab %}

{% tab title="MATLAB" %}
```matlab
v0=0.5;
v=fzero(@fnSolarCell,v0)

function y=fnSolarCell(x)
    q=1.6022E-19; k=1.3806E-23; Voc=0.5; T=297;
    qkT=q/(k*T);
    y=exp(qkT*x)*(1+qkT*x)-exp(qkT*Voc);
end

```


{% endtab %}
{% endtabs %}

### Integration

#### Integrating discrete dataset: trapz()

Trapezoidal Method

{% tabs %}
{% tab title="C Example" %}
```cpp
/*==========================================================================*/
/*						Example: Integration
/*==========================================================================*/
	// Initialization
	double x_int[] = { 0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60 };
	double y_int[] = { 0, 3, 8, 20, 33, 42, 40, 48, 60, 12, 8, 4, 3 };
	int data_m = sizeof(x_int) / sizeof(x_int[0]);

	// NP Algorithm	
	double integralOut = integral(x_int, y_int, data_m);
		
	// Display Output	
	printf("\n\nIntegration from Data points = %0.4f\n\n",integralOut);
```
{% endtab %}

{% tab title="MATLAB" %}
```matlab
% Discrete dataset
x=[0 5 10 15 20 25 30 35 40 45 50 55 60]
y=[0 3 8 20 33 42 40 48 60 12 8 4 3]
plot(x,y)

% Matlab function
I_matlab = trapz(x,y);  
```


{% endtab %}
{% endtabs %}

#### Integrating a Function: integral(fun, a,b)

The area of the shaded region shown in the figure can be calculated by:

<figure><img src="../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% tabs %}
{% tab title="C Example" %}
```cpp
// NP Algorithm		
	double x_st = -3;
	double x_end = 3;
	int intevalN = 10;
	double integralFuncOut = integralFunc(func_integral,x_st, x_end, intevalN);

	// Display Output	
	printf("\n\nIntegration from Function = %0.4f\n\n", integralFuncOut);

```
{% endtab %}

{% tab title="MATLAB" %}
```matlab
a=-3; b=3; N=8;
h=(b-a)/N;
x=a:h:b;
fun=@(x) (1-x.^2).^0.5;
% Matlab function
I_matlab = integral(fun,a,b); 


```


{% endtab %}
{% endtabs %}

### Differentiation

#### Differentiating discrete dataset

{% tabs %}
{% tab title="C Example" %}
```cpp
/*==========================================================================*/
/*					Example: Differentiation from discrete dataset points       
/*==========================================================================*/
	
	// Initialization
	int m = 21;
	double t[21] = { 0 };
	for (int i = 0; i < m; i++) t[i] = 0.2 * i;
	double y_out[] = { -5.87, -4.23, -2.55, -0.89, 0.67, 2.09, 3.31, 4.31, 5.06, 5.55, 5.78, 5.77, 5.52, 5.08, 4.46, 3.72, 2.88, 2.00, 1.10, 0.23, -0.59 };
	double  dydt[21] = { 0 };

	// NP Algorithm	
	gradient1D(dydt, t, y_out, m);

	// Display Output	
	printf("\n Data Differentiation solution \t\n");
	for (int k = 0; k < m; k++)
		printf("dydt[%d]=%0.6f \t\n", k, dydt[k]);
```
{% endtab %}

{% tab title="MATLAB" %}
```matlab
% Differentiation from discrete data
X = [1 1 2 3 5 8 13 21];
Y = diff(X)

% Differentiation from discrete data
h = 0.001;       % step size
X = -pi:h:pi;    % domain
f = sin(X);      % range
Y = diff(f)/h;   % first derivative
Z = diff(Y)/h;   % second derivative
plot(X(:,1:length(Y)),Y,'r',X,f,'b', X(:,1:length(Z)),Z,'k')

```


{% endtab %}
{% endtabs %}

#### Differentiate a Function

{% tabs %}
{% tab title="C Example" %}
```cpp

/*==========================================================================*/
/*					Example: Differentiation from a function  
/*==========================================================================*/
	
	// Initialization
	double x[21] = { 0 };
	double dydx[21] = { 0 };  // m=21 points
	for (int i = 0; i < m; i++) x[i] = 0.2 * i;	
	
	// NP Algorithm	
	gradientFunc1D(dydx, func_diff, x, m);
	
	// Display Output	
	printf("\n Function Differentiation solution \t\n");
	for (int k = 0; k < m; k++)
		printf("dydx[%d]=%0.6f \t\n", k, dydx[k]);
```
{% endtab %}

{% tab title="MATLAB" %}
```matlab
% Ordinary Differentiation of a function
syms x
g = exp(x)*cos(x);
diff(g)

% 2nd order Differentiation of a function
diff(g,2)

% Partial Differentiation of a function
syms s t
f = sin(s*t);
diff(f,t)

```


{% endtab %}
{% endtabs %}

### Linear Equations and Eigenvalues

Solve for Ax=b

<figure><img src="../.gitbook/assets/image (3) (1).png" alt="" width="375"><figcaption></figcaption></figure>

{% tabs %}
{% tab title="C example" %}
```cpp
/*==========================================================================*/
/*					Example: Linear System
/*==========================================================================*/
	// Initialization
	double arrA[] = { 9.5, -2.5, 0, -2, 0, -2.5, 11, -3.5, 0, -5,
					0, -3.5, 15.5, 0, -4,     -2, 0, 0, 7, -3,
					0, -5, -4, -3, 12 };
	double arrB[] = { 12, -16, 14, 10, -30 };
	Matrix matA = arr2Mat(arrA, 5, 5);
	Matrix vecb = arr2Mat(arrB, 5, 1);
	Matrix vecx = zeros(5, 1);
	Matrix matA_eig = zeros(5, 5);	
	Matrix matA_eigVec = zeros(5,5);
		
	// NP Algorithm
	vecx = solveLinear(matA, vecb);
	eig(matA_eigVec, matA_eig, matA);

	// Display Output
	printMat(matA, "A");
	printMat(vecb, "b");
	printMat(vecx, "solution x");
	printMat(matA_eig, "eig values (A)");
	printMat(matA_eigVec, "eig vectors (A)");

	//  Deallocate Memory
	freeMat(matA);	freeMat(vecb); 	freeMat(vecx);
	freeMat(matA_eig);	freeMat(matA_eigVec);

```
{% endtab %}

{% tab title="MATLAB" %}
```matlab
% A, b
A=[9.5, -2.5, 0, -2, 0;     -2.5, 11, -3.5, 0, -5;     0,-3.5, 15.5, 0, -4;     -2,  0,  0, 7, -3;     0, -5,  -4, -3, 12];
b=[12; -16; 14; 10; -30];

% solve for Ax=b
x=A\b   
x=inv(A)*b
% condition number
c=cond(A)
% norm 
n=norm(A)
% eigenvalue/vector
[eigVec,eigVa]=eig(A)
% QR factorization
[Q,R]=qr(A)

% LU factorization
[L,U]=lu(A)   
```


{% endtab %}
{% endtabs %}

### Eigenvalues

What are the eigenvalues for a given m-c-k system response? Use m=10kg ; k=800 N/m; c=200 N/(m/s)

<figure><img src="../.gitbook/assets/image (8).png" alt="" width="174"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (9).png" alt="" width="375"><figcaption></figcaption></figure>

{% tabs %}
{% tab title="C Example" %}
```cpp
/*==========================================================================*/
/*					Example: Eigenvalues MCK
/*==========================================================================*/
	// Initialization
	double k = 800;
	double c = 200;
	double m = 10;
	double arrMCK[] = { 0, 1, -k / m, -c / m };
	
	Matrix matB = arr2Mat(arrMCK, 2, 2);
	Matrix matB_eig = zeros(2, 2);
	Matrix matB_eigVec = zeros(2, 2);

	// NP Algorithm	
	eig(matB_eigVec, matB_eig, matB);

	// Display Output
	printMat(matB_eig, "eig values of MCK");
	printMat(matB_eigVec, "eig vectors of MCK");


	//  Deallocate Memory
	freeMat(matB);	freeMat(matB_eigVec);	freeMat(matB_eig);
```
{% endtab %}

{% tab title="MATLAB" %}
```matlab
k=800; c=200; m=10;
A = [0 1; -k/m, -c/m];
disp('Eigvalue and vector of A (MATLAB):');
[eigVec,eigVa]=eig(A)
```


{% endtab %}
{% endtabs %}

### Linear Regression

{% tabs %}
{% tab title="C Example_1" %}
```cpp
/*==========================================================================*/
/*						Example: Linear Regression
/*==========================================================================*/
	
	// Initialization
	int dataN = 15;
	double xdata[] = { 1, 2,	3,	4,	5,	6,	7,	8,	9,	10,	11,	12,	13,	14,	15 };
	double ydata[] = { 2.272,	2.092,	1.887,	1.629,	1.482,	1.308,	1.030,	0.875,	0.693,	0.470,	0.336,	0.095, -0.163, -0.371, -0.511 };
	double z_opt[15] = {0};

	// NP Algorithm - Option 1
	linearRegression(z_opt, xdata, ydata, dataN);

	// Display Output
	printf("Linear Regression:\n\t a0=%0.3f \t a1=%0.3f \n\n", z_opt[0], z_opt[1]);
```
{% endtab %}

{% tab title="C Example_2" %}
{% code expandable="true" %}
```cpp
// Initialization
	int dataN = 15;
	double xdata[] = { 1, 2,	3,	4,	5,	6,	7,	8,	9,	10,	11,	12,	13,	14,	15 };
	double ydata[] = { 2.272,	2.092,	1.887,	1.629,	1.482,	1.308,	1.030,	0.875,	0.693,	0.470,	0.336,	0.095, -0.163, -0.371, -0.511 };
	double z_opt[15] = { 0 };
	Matrix vect = zeros(dataN, 1);
	vect = arr2Mat(xdata, dataN, 1);
	Matrix vecv = zeros(dataN, 1);
	vecv = arr2Mat(ydata, dataN, 1);

	// NP Algorithm - Option 2
	Matrix Zopt = polyFit_mat(vect, vecv, 1);
	// Display Output
	printMat(Zopt, "Linear Regression (a0, a1)");
	

	//  Deallocate Memory
	freeMat(Zopt);	freeMat(vect); freeMat(vecv);
```
{% endcode %}
{% endtab %}

{% tab title="MATLAB" %}
```matlab
t = 1:1:15;
V=[2.272	2.092	1.887	1.629	1.482	1.308	1.030	0.875	0.693	0.470	0.336	0.095	-0.163	-0.371	-0.511];

% Matlab function for polynomial fit
Zopt=polyfit(t,V,1);
Yopt=polyval(Zopt,t);  % Matlab function

figure
plot(t,V, '*r')
hold on
plot(t,Yopt, '-b')
xlabel('time','fontsize',15)
ylabel('V','fontsize',15)

```


{% endtab %}
{% endtabs %}

### Nonlinear Regression

{% tabs %}
{% tab title="C Example" %}
```cpp

/*==========================================================================*/
/*						Example: Non-Linear Regression
/*==========================================================================*/
	// Initialization
	double Z_opt[3] = { 0 };

	// NP Algorithm
	int orderN= 2;	
	polyFit(Z_opt, xdata, ydata, dataN, orderN);

	// Display Output
	printf("\n\n2nd Polynomial Regression:\n\t a0=%0.5f \t a1=%0.5f \t a2=%0.5f \n\n", Z_opt[0], Z_opt[1], Z_opt[2]);


```
{% endtab %}

{% tab title="MATLAB" %}
```matlab
t = 1:1:15;
V=[2.272	2.092	1.887	1.629	1.482	1.308	1.030	0.875	0.693	0.470	0.336	0.095	-0.163	-0.371	-0.511];

% Matlab function for polynomial fit
Zopt=polyfit(t,V,2);
Yopt=polyval(Zopt,t);  % Matlab function

figure
plot(t,V, '*r')
hold on
plot(t,Yopt, '-b')
xlabel('time','fontsize',15)
ylabel('V','fontsize',15)

```


{% endtab %}
{% endtabs %}





### Exponential Fitting

RC circuit with unknown capacitor C and resistor of 5M

a) Find the capacitance C from curve fitting b) Estimate the voltage when time=32sec

<figure><img src="../.gitbook/assets/image (4) (2).png" alt=""><figcaption></figcaption></figure>

{% tabs %}
{% tab title="C Example" %}
```cpp
```
{% endtab %}

{% tab title="MATLAB" %}
```matlab
Xdata = 1:1:15;
Ydata = [9.7 8.1 6.6 5.1 4.4 3.7 2.8 2.4 2.0 1.6 1.4 1.1 0.85 0.69 0.6];


% Matlab function
Zopt=polyfit(Xdata,log(Ydata),1)
R=5e6;
a0=Zopt(2);
a1=Zopt(1);
V0=exp(a0);
tau=-1/a1;
C=tau/R;

% Exponential model
Yopt=V0*exp(-1/(R*C).*Xdata);

figure
plot(Xdata,Ydata, '*r')
hold on
plot(Xdata,Yopt)
hold on
xlabel('time (s)','fontsize',15)
ylabel('V_R(volt)','fontsize',15)

```


{% endtab %}
{% endtabs %}

### 1st order ODE-IVP

Solve for the response of an RC circuit with a DC input. Let tau= 4.9919; Vm=11.91;

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

{% tabs %}
{% tab title="First Tab" %}
```c++
/*==========================================================================*/
/*					Example: ODE 1st order
/*==========================================================================*/
	// Initialization
	double a = 0;
	double b = 15;
	double h_ode1 = 1;
	double v_RK[200] = { 0 };
	double vinit = 11.91;
	int mtest = 10;

	// NP Algorithm
	ode(v_RK, func_odeRC, a, b, h_ode1, vinit, 2);

	// Display Output
	printf("ODE RC:\n\t at t=%0.1f \t V=%0.6f \t\n", h_ode1 * mtest, v_RK[mtest]);
	for (int k = 0; k < 20; k++)
		printf("V=%0.6f \t\n", v_RK[k]);


/************     Define ODE Functions     ************/
double func_odeRC(const double t, const double v) {
	double tau = 4.9919;
	double T = 1 / tau;
	double Vm = 0;
	double out = -T * v + T * Vm;	
	return  out;
}
```
{% endtab %}

{% tab title="Second Tab" %}
```matlab
% Initial Condition
a=0; b=15; h=1; 
y0 = 11.91;
t=a:h:b;

%% MATLAB's function ODE45
[tmat,vmat] = ode45(@myRC, [a b], y0);  % Fourth/Fifth RK
figure()
plot(tmat,vmat,'.b')
xlabel('time (s)','fontsize',15)
ylabel('V_R(volt)','fontsize',15)

function dvdx = myRC(v)
    tau=4.9919; T=1/tau;  Vm=11.91;
    dvdx =-T*v + 1*T*Vm;
end

```


{% endtab %}
{% endtabs %}

### 2nd order ODE - IVP

Solve an m-c-k system with a sinusoidal input.

Use m=10kg ; k=800 N/m; c=200 N/(m/s), f=10Hz , h=0.01, Fdc=100N.

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

{% tabs %}
{% tab title="C Example" %}
```cpp
/*==========================================================================*/
/*					Example:ODE 2nd order
/*==========================================================================*/

	// Initialization
	double t0 = 0;
	double tf = 1;
	double h = 0.01;
	double y[200] = { 0 };
	double v[200] = { 0 };
	double y0 = 0;
	double v0 = 0;
	int ntest = 100;

	// NP Algorithm
	sys2RK2(y, v, func_odeMCK, t0, tf, h, y0, v0);
	
	// print your result
	printf("\n\nODE 2nd order MCK:\n\r");
	printf("y[t=%0.1f]=%0.6f \t v[t=%0.1f]=%0.6f\n\n", h*ntest, y[ntest], h * ntest, v[ntest]);
	

	
/************     Define ODE Functions     ************/	
void func_odeMCK(double dYdt[], const double t, const double Y[])
{
	double m = 10;
	double c = 200;
	double k = 800;
	double f = 10;
	double Fin = 100 * cos(2 * PI * f * t);

	dYdt[0] = Y[1];
	dYdt[1] = (-k * Y[0] - c * Y[1] + Fin) / m;
}

```
{% endtab %}

{% tab title="MATLAB" %}
```matlab
% Initial Condition
y0 = 0; v0 = 0;
Yinit = [y0 v0];
a=0; b=1; h=0.01;
tspan = [a:h:b];

% MATLAB's function ODE45
[Time Y] = ode45(@mckFunc,tspan,Yinit);

figure
subplot(2,1,1)
    plot(Time,Y(:,1),'--b')
    xlabel('Time (s)')
    ylabel('Position (m)')
    title('ode45')
subplot(2,1,2)
    plot(Time,Y(:,2),'k')
    xlabel('Time (s)')
    ylabel('Velocity (m/s)')

function [dXdt] = mckFunc(t,x)
    dXdt=zeros(2,1); % column vector
    m=10; k=800; c=200; f=10;
    FinDC=100;
    Fin=FinDC*cos(2*pi*f*t);
    dXdt(1)=x(2);
    dXdt(2)=1/m*(Fin-c*x(2)-k*x(1));
end

```


{% endtab %}
{% endtabs %}

###

## More tutorial codes

### [NP lecture tutorial codes](https://github.com/ykkimhgu/NumericalProg-student/tree/main/tutorial)
