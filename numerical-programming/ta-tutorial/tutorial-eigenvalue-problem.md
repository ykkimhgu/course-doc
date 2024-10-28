# Tutorial: Eigenvalue problem

## Tutorial: MATLAB&#x20;

Estimate the eigenvalue and eigenvectors

```matlab
clear; close all;

A = [45 30 -25; 30 -24 68; -25 68 80];

disp('Eigvalue and vector of A (MATLAB):');
[eigVec,eigVal]=eig(A)
eigvalues=diag(eigVal)
```





## Exercise 1:  MATLAB

**Download the tutorial source file**

* &#x20;[TU\_Eigenvalue\_Student\_2024.mlx](https://github.com/ykkimhgu/NumericalProg-student/tree/main/tutorial/TU\_Eigenvalue)



**Fill-In the blanks.**&#x20;

```matlab
function [Q, R] = QRdecomp_student(A)  
% Factorization of given matrix [A] into Upper triagular [R] and orthogonormal [Q]
% Using HouseHold Matrix
% Input: A (nxn)
% Output: Q (nxn), R (nxn)
    
    % Initialization
    n = size(A,1);
    I=eye(n);
    R=A;
    Q = I;    
    
    for j = 1:n-1                

        % Create vector [c]
        % [YOUR CODE GOES HERE]
        % c = _______________;   
        

        % Create vector [e]
        e=zeros(n,1);
        % [YOUR CODE GOES HERE]
        % e = _______________;
        

        % Create vector [v]
        % [YOUR CODE GOES HERE], HINT: use norm(c,2)
        % v = _______________;
    

        % Create matrix [H]
        % [YOUR CODE GOES HERE]
        % H = _______________;

        
        % Update [Q], [R]
        Q = Q*H;
        R = H*R;
    end

end % end of function
```



**Run the code and check the answer with MATLAB's  eig(A)**

```matlab
% initialize  
N=100;
U=A; 

for i = 1:N 
    % Step 1: A=QR decomposition
    [Q, R] = QRdecomp_student(U); 
    
    % Step 2: Create Similar Matrix A = Q'AQ
    U = R*Q;   
    if(~mod(i,10))                
        disp(sprintf('iteration %d \n',i));
        U
    end
end

% Step 3: eigenvalues from U
lamdas = diag(U);
```



## Exercise 2: Eigenvalue in C-Programming



**Download the tutorial source file**

* [Assignment\_Eigenvalue\_ID.cpp](https://github.com/ykkimhgu/NumericalProg-student/blob/main/src/Assignment\_Eigenvalue\_ID.cpp)



**Create the function that returns the estimated eigenvalues**&#x20;

```c
Matrix eig(Matrix A);
void QRdecomp(Matrix A, Matrix Q, Matrix R); 

// Usage example
Matrix eigVals = eig(matA);
```



## Exercise 3: Eigenvector in C-Programming

See the _Assignment\_Eigenvalue_ for more detail



