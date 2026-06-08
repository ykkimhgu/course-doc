# Tutorial: Gradient Descent

Solve the optimal parameters (w) that minimizes the cost function L(w) using Gradient Descent.



## Part 1: Train a Linear Regression Model using Gradient Descent

### Download <a href="#tutorial-matlab" id="tutorial-matlab"></a>

#### MATLAB <a href="#tutorial-matlab" id="tutorial-matlab"></a>

Download the tutorial source file and fill in the blanks.&#x20;

* MATLAB tutorial source file : [TU\_GradientDescent\_student\_MATLAB.zip](https://github.com/ykkimhgu/NumericalProg-student/tree/main/tutorial/TU_GradientDescent)

#### C-Programming <a href="#exercise-2-eigenvalue-in-c-programming" id="exercise-2-eigenvalue-in-c-programming"></a>

Download the tutorial source file and fill in the blanks.&#x20;

* C tutorial source file : [TU\_GradientDescent\_Part1\_Student.cpp](https://github.com/ykkimhgu/NumericalProg-student/tree/main/tutorial/TU_GradientDescent)



### Problem 1 : 1D-Linear Regression

An experiment is conducted to calibrate the strain-gauge for Force-Strain experiment.

Train a prediction model using the linear regression model and predict the voltage if the force is 90N

<figure><img src="../../.gitbook/assets/image (148).png" alt=""><figcaption></figcaption></figure>

* Train Dataset

{% code expandable="true" %}
```
L=[30	40	50	60	70	80]   (N)
V=[1.05	1.07	1.09	1.14	1.17	1.21]  (V)
```
{% endcode %}





### Problem 2: 2D-Linear Regression

Predict Car's MPG(Mileage Per Gallon / km per liter) : 연비

Find linear relationship of MPG with car's weight & displacement (주행거리)&#x20;

Then, Predict MPG for a car with Weight=3000 , Displacement=300&#x20;

* Feature: 2-Dimensions, p=2&#x20;
* y = w0+w&#x31;_&#x78;1+ w&#x32;_&#x78;2

<figure><img src="../../.gitbook/assets/image (149).png" alt=""><figcaption></figcaption></figure>

## Part 2: Solve a minimization problem using Gradient Descent

### Problem 1

\
Find the point (x\*) that minimize the function L(x)

* L(x)=x^2-4x+6

\
Use gradient descent with initial point at x0=0

Use the learning rate as

* η=0.25
* η=1
* η=0.05

<figure><img src="../../.gitbook/assets/image (150).png" alt=""><figcaption></figcaption></figure>

### Problem 2: &#x20;

Find the points (x\*, y\*) that minimize the function L(x,y)

<figure><img src="../../.gitbook/assets/image (151).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (152).png" alt=""><figcaption></figcaption></figure>
