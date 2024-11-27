# Problem Bank

# Additional Problems for Numerical Programming
---


# Non-Linear Equations



## Problem 1

**Find the root for f(x) between x=[4, 4.5]** :  *f* (*x*) = sin(10*x*) + cos(3*x*)

![image-20241127103937635](.\image\image-20241127103937635.png)







# Linear Equations



## Problem 1

**Find the acceleration for each mass of the three mass-four spring system., when**

*  *x*1 = 0.05 m, *x*2 = 0.04 m, *x*3 = 0.03 m

  

![image-20241127104000466](.\image\image-20241127104000466.png)


The dynamic of the system can be expressed as

![image-20241127104033155](.\image\image-20241127104033155.png)

The parameters: 

* *k*1 = *k*4 = 10 N/m, *k*2 = *k*3 = 30 N/m, and *m*1 = *m*2 =*m*3 = 1 kg

### Hint

Solve as **Ay=b**,  where **y**=[acc1;  acc2;  acc3]

### 

---



## Problem 2

An electrical network has two voltage sources and six resistors. By applying
both Ohm’s law and Kirchhoff’s Current law: 

![image-20241127105437524](.\image\image-20241127105437524.png)

**Solve the linear system for the current i1, i2, and i3 if** 

* R1 = 1, R2 = 2, R3 = 1, R4 = 2, R5 = 1, R6 = 6
* V1 = 20, V2 =30






---



## Problem 3

**Consider the loading of a statically determinate pin-jointed truss shown below. The truss has seven members and five nodes, and is under the action of the forces R1, R2, and R3 parallel to the y-axis**

**Find the member forces (Fi) obtained from the following system of equations **

![image-20241127105800651](.\image\image-20241127105800651.png)

![image-20241127105853843](.\image\image-20241127105853843.png)


---

# Eigenvalue-vector

## Problem 1

**Building Earthquake Analysis**

A three-story building modeled as a mass-spring system.  Each floor mass is represented by *m_i*, and each floor stiffness is represented by *k_i* for *i* = 1 to 3.  For this case, the analysis is limited to horizontal motion of the structure as it is subjected to horizontal base motion due to earthquakes.

* *X_i* represent horizontal floor translations (m)  **[Eigenvector]**
* *ω_n* is the *natural*, or *resonant*, *frequency* (radians/s). **[Eigenvalue]**



**Determine the eigenvalues and eigenvectors for this system.** 



![image-20241127111634179](.\image\image-20241127111634179.png)

![image-20241127112046935](.\image\image-20241127112046935.png)



## Hint

Solve by expressing it as 
$$
{\bf{[A - }}{\rm{\lambda }}{\bf{I]v = 0}}
$$


---



# Least Squares Regression


## Problem 1

A parachutist jumps from a plane and the distance of his drop is measured. Suppose that the distance of descent d as a function of time t can be modeled by
$$
d = αt + βt^2e^{−0.1t}
$$


Find values of α and β that best fit the data  as 

* t=[ 5 10 15 20 25 30]
* d=[ 30 83 126 157 169 190]





### 



# ODE-IVP



## Problem 1

**Newton's Law of Cooling**

The rate at which the temperature of a body changes is proportional to the difference between the temperature of the body **T(t)** and the temperature of the surrounding medium (**Tm**) :

![image-20241127110358127](.\image\image-20241127110358127.png)



**Approximate the temperature T at t = 4 min of a ceramic insulator**

* Baked at 400 C (T_init) and cooled in a room in which the temperature(Tm) is 25 C

* Use k = −0.213

  

### Hint

### Solution

T(4) = 184.96.



## Problem 2

**Headlamp Mirror Design**

An automobile heading light mirror is designed to reflect the light given off by the headlamp in rays parallel to the real surface. By using the principle of optics that the angle of light incidence equals the angle of light reflection. Assume the first-order differential equation that models the desired shape of the mirror is

![image-20241127110912733](.\image\image-20241127110912733.png)

The mirror is designed so that the distance of the mirror directly above the lamp is 1 cm, i.e.,  **y(0) = 1**

**Estimate y at  (a)  x = 1  (b) x = 2**

* Use the Runge-Kutta method with h = 0.1

