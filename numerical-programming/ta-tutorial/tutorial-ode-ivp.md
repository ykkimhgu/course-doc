# Tutorial: ODE-IVP

## Part 1: 1st order ODE-IVP  problem <a href="#tutorial-matlab" id="tutorial-matlab"></a>

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



### Exercise : MATLAB <a href="#exercise-1-matlab" id="exercise-1-matlab"></a>

Download the tutorial source file

* [TU\_ODE\_Part1\_Student\_2024.zip](https://github.com/ykkimhgu/NumericalProg-student/blob/main/tutorial/TU\_ODE/TU\_ODE\_Part1\_Student\_2024.zip)

Then, fill-In the blanks.



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

### **Problem**

Solve for the response of the given system:

