# Tutorial: for loop in C vs Matlab

## Introduction

There can be some confusion in translating a code from Matlab to C.

The most common mistake is in the for-loop syntax and index number.



### Array Index

C: starts from 0

Matlab: starts from 1

```matlab
// Array
// A = [10, 20, 30, 40, 50];
// int A[5] = {10, 20, 30, 40, 50};


// Matlab
for i = 1:5   

// C
 for (int i = 0; i < 5; i++) 
```

### Example

#### **MATLAB :**&#x20;

```matlab
% MATLAB Array
A = [10, 20, 30, 40, 50];

for i = 1:5
    fprintf("A(%d) = %d\n", i, A(i));
end

```

**MATLAB Output**

```matlab
A(1) = 10
A(2) = 20
A(3) = 30
A(4) = 40
A(5) = 50
```

#### **C-Programming**&#x20;

```c
# C-Programming Array
#include <stdio.h>

int main() {
    int A[5] = {10, 20, 30, 40, 50};

    for (int i = 0; i < 5; i++) {
        printf("A[%d] = %d\n", i, A[i]);
    }

    return 0;
}
```

**C-Programming Output**

```c
A[0] = 10
A[1] = 20
A[2] = 30
A[3] = 40
A[4] = 50
```

