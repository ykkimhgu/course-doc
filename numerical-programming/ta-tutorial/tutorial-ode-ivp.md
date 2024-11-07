# Tutorial: ODE-IVP

## Part 1: 1st order ODE-IVP  problem <a href="#tutorial-matlab" id="tutorial-matlab"></a>

Download the tutorial source file

* [TU\_ODE\_Part1\_Student\_2024.zip](https://github.com/ykkimhgu/NumericalProg-student/tree/main/tutorial/TU\_ODE)



### **Problem**

Solve for the response (Vout) of an RC circuit with a sinusoidal input (Vin), from t=0 to 0.1 sec.

where RC=tau=0.01; T=1/tau; f=100; Vm=1; w=2_pi_f;

<figure><img src="../../.gitbook/assets/image (127).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (128).png" alt=""><figcaption></figcaption></figure>

### Tutorial: MATLAB <a href="#tutorial-matlab" id="tutorial-matlab"></a>

MATLAB : `ode45()`&#x20;

Lets define the RC circuit ODE function f(t,v) as `gradFunc_RC(t,v)`.

```matlab
% Initial Condition
% time
a=0; b=0.1; 
h=0.001; 
t=a:h:b;
N = (b-a)/h;

% IVP-Initial Condition
v0 = 0;
y0= v0;

%% MATLAB's function ODE45
[tmat,vmat] = ode45(@gradFunc_RC, [a b], v0);  


% gradFunc_RC.m

% function dvdt = gradFunc_RC(t,v)
%    tau=0.01; f=100; Vm=1;
%    dvdt =-v/tau + (1/tau)*Vm*cos(2*pi*f*t);
% end

```

### Analytical Solution (Ground-truth)

&#x20;The true analytical solution can be expressed as

<figure><img src="../../.gitbook/assets/image (129).png" alt=""><figcaption></figcaption></figure>



```matlab
% Analytical Solution 
tau=0.01; f=100; Vm=1; w=2pif;
A=Vm/(sqrt(1+(wtau)^2)); 
alpha=-atan(wtau);
v_true=A*(-cos(alpha)exp(-t/tau)+cos(wt+alpha));
```



***



### Exercise  <a href="#exercise-1-matlab" id="exercise-1-matlab"></a>





#### **Exercise 1: Euler's Explicit Method**

```matlab
[t, yE] = odeEU_student(@gradFunc_RC,a,b,h,y0);

figure()
plot(t,yE,'--b',t,v_true,'k')
xlabel('t'); ylabel('v(t)')
```



```matlab
% function [x, yE] = odeEU_student(ODE,a,b,h,y0)
% Variable Initialization
N = (b-a)/h;
yE=zeros(1,N+1);
t=zeros(1,N+1);

% Initial Condition
yE(1) = y0;
t(1)=a;

% Euler Explicit ODE Method
for i = 1:N
    % Calculate: t(i+1)=_________
    % [TO-DO] your code goes here
    
    % Estimate: yE(i+1)=________
    % [TO-DO] your code goes here    
end

% end % End of Function
```



#### Exercise 2: Euler's Modified Method

<figure><img src="../../.gitbook/assets/image (130).png" alt="" width="375"><figcaption></figcaption></figure>

Create `odeEM_student.m`&#x20;

Modify the given template code&#x20;

```matlab
[t, yEM] = odeEM_student(@gradFunc_RC,a,b,h,y0);
```





#### Exercise 3: 2nd Order Runge-Kutta

<figure><img src="../../.gitbook/assets/image (131).png" alt="" width="314"><figcaption></figcaption></figure>



Let alpha=1, C1=0.5, C2=0.5.

Modify the given template code:\


```matlab
[t, yRK2] = odeRK2_student(@gradFunc_RC,a,b,h,y0);
```



## Part 2: Higher-order ODE-IVP

Download the tutorial source file

* [TU\_ODE\_Part2\_Student\_2024.zip](https://github.com/ykkimhgu/NumericalProg-student/tree/main/tutorial/TU\_ODE)



### **Problem**

Solve for the response of the given system:

<figure><img src="../../.gitbook/assets/image (132).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (133).png" alt="" width="309"><figcaption></figcaption></figure>

Let, m=1; k=6.9; c=7; f=5;\
u(t)=Fin(t)=2_cos(2_pi_f_t)



### Tutorial: MATLAB



Lets define the mck ODE functions as gradFunc\_mck(t, vecY).

<figure><img src="../../.gitbook/assets/image (134).png" alt=""><figcaption></figcaption></figure>

**gradFunc\_mck.m**

```matlab
function [dYdt] = gradFunc_mck(t,vecY)
    % Input:
    %   vecY=[y ; z] 
    % Output:
    %   dYdt=[dydt ; dzdt] 

    % Variable Initialization
    dYdt=zeros(2,1);    
    
    % System Modeling parameters 
    m=1; k=6.9; c=10*0.7;
    FinDC=2;    f=5;
    Fin=FinDC*cos(2*pi*f*t);
    
    % Output    
    dYdt(1)=vecY(2);
    dYdt(2)=1/m*(Fin-c*vecY(2)-k*vecY(1));

end 
```

**MATLAB : ode45()**

```matlab
% Initial Condition
% time
a=0;
b=1;
h=0.01;
N = (b-a)/h;


% IVP-Initial Condition
yINI = 0;
vINI = 0.2;

% ODE Solver
[tref, vecY] = ode45(@gradFunc_mck, [a:h:b], [yINI, vINI]);
```

### Exercise

#### Exercise 1: Euler's Explicit Method for 2nd-order ODE

**sys2EU\_student.m**&#x20;

First, fill-in this blank and complete the algorithm

```matlab
% function [t, yE, vE] = sys2EU_student(gradF,a,b,h,yINI, vINI)

    % Variable Initialization
    N = (b-a)/h;    
    t=zeros(1,N+1);    
    yE=zeros(1,N+1);
    vE=zeros(1,N+1);

    % Initial Condition
    yE(1) = yINI;
    vE(1) = vINI;
    t(1)=a;


    % Euler Explicit ODE Method
    for i = 1:N
        t(i+1) = t(i) + h;        
        dYdt=gradFunc_mck(t(i),[yE(i),vE(i)]);
        
        % Estimate: yE(i+1)=________
        % [TO-DO] your code goes here  

        % Estimate: vE(i+1)=________
        % [TO-DO] your code goes here  
    end

% end  % End of Function
```



Then, create the function file as `sys2EU_student.m`

`[t, yE, vE] = sys2EU_student(@gradFunc_mck,a,b,h,yINI, vINI);`



#### Exercise 2: 2nd order Runge-Kutta for 2nd-order ODE

Create `sys2RK2_student.m`&#x20;

Modify the given template code

```matlab
[t, yRK2, vRK2] = sys2RK2_student(@mckFunc,a,b,h,yINI,vINI);
```



## Assignment

### Q1. Create C/C++ function for 1st order ODE

#### Use 2nd, 3rd order Runge-Kutta method

```c
// 2nd order Runge-Kutta method 
void odeRK2(double y[], double odeFunc(const double t, const double y), 
            const double t0, const double tf, const double h, const double y_init) 

// 3rd order Runge-Kutta method 
void odeRK3 (double y[], double odeFunc(const double t, const double y), 
             const double t0, const double tf, const double h, const double y_init) 

```

> For RK2, use alpha=1, C1=0.5
>
> For RK3, use classical third-order Runge-Kutta



### Q2. Create C/C++ function for 2nd order ODE

#### Use 2nd order Runge-Kutta method

```cpp
void sys2RK2(double y1[], double y2[], 
                void odeFuncSys(double dYdt[], const double t, const double Y[]), 
                const double t0, const double tf, const double h, const double y1_init, 
                const double y2_init);

```
