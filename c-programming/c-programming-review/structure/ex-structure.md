# Exercise: Structure

### Exercise\_1: Creating structures

Fill in the code to get the output of

* 이름: 고길동
* 학년: 1 반: 3
* 평균점수: 65.389999

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Student {
    char name[20];
    int grade;
    int class;
    float average;
};

int main()
{
    struct Student *s1 = ___________________________;

    _______________________________
    ________________________________
    ________________________________
    ________________________________

    printf("이름: %s\n", s1->name);
    printf("학년: %d\n", s1->grade);
    printf("반: %d\n", s1->class);
    printf("평균점수: %f\n", s1->average);

    free(s1);

    return 0;
}
```

[Check answer here](https://dojang.io/mod/page/view.php?id=422)

![Exercise\_1 result](<../../../.gitbook/assets/image (68).png>)

### Exercise\_2: Passing structure to a function

Declare and define the following structure in header “EC\_Tutorial.h”. Also, define the following functions in "EC\_Tutorial.c"

```cpp
typedef struct {
	int x;
	int y;
	int z;
} POSITION_TypeDef;

//
void addPos(POSITION_TypeDef pos1, POSITION_TypeDef pos1, POSITION_TypeDef *posOut);
void printPos (POSITION_TypeDef Pos);
```

Use your functions in Main() as

```cpp
#include “EC_Tutorial.h"

void main() 
{
...
// Exercise 2 ***********************************************
printf(“\nExercise 2\n");
POSITION_TypeDef Pos[2];
POSITION_TypeDef PosOut = {0, };
 
Pos[0].x = 2; Pos[0].y = 2; Pos[0].z = 2;
Pos[1].x = 5; Pos[1].y = 5; Pos[1].z = 5;

addPos(Pos[0], Pos[1], &PosOut);
printPos(PosOut);

system("pause");
}
```

![Exercise\_2 result](<../../../.gitbook/assets/image (70).png>)
