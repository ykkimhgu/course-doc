# Structure

## Lesson

**코딩도장 핵심요약**: [구조체 사용하기 핵심요약](https://dojang.io/mod/page/view.php?id=799)

We can define our own data type of a set of related field members.

Each field member can be defined with a different data type.

Structure declaration and definition

* Structure variables, Tagged structures, Type-defined structures

![](<../../.gitbook/assets/image (33).png>)

## Example Code

### Example 1

[C\_structure\_example.c](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/structure)

```cpp
typedef struct {
	uint16_t sec;
	uint16_t min;
	uint16_t hour;
} TIME_TypeDef;
TIME_TypeDef time;

//variable with position_t type. 4 Bytes are allocated in RAM
time.hour=18; 
time.min=20; 
time.sec=01; 
// Also, we can define pointers to structures
TIME_TypeDef *pTime;
pTime=&time;
pTime->hour=17;
```

### Example 2

```cpp
struct Data {
    char c1;
    int *numPtr;    // 포인터
};

int main()
{
    int num1 = 10;
    struct Data d1;    // 구조체 변수
    struct Data *d2 = malloc(sizeof(struct Data));    
    // 구조체 포인터에 메모리 할당

    d1.numPtr = &num1;
    d2->numPtr = &num1;
```

구조체 변수 d1의 멤버 numPtr을 역참조 하는 방법과 구조체 포인터 d2의 멤버 numPtr을 역참조 하는 방법을 그림으로 표현하면 다음과 같은 모양이 됩니다.

구조체 멤버가 포인터일 때 역참조하기

![fig. 49-1](https://dojang.io/pluginfile.php/482/mod\_page/content/32/4901.png)

만약 역참조한 것을 괄호로 묶으면 어떻게 될까요? 이렇게 하면 구조체 변수를 역참조한 뒤 멤버에 접근한다는 뜻이 됩니다. \*(\*d2).numPtr처럼 구조체 포인터를 역참조하여 numPtr에 접근한 뒤 다시 역참조할 수도 있습니다.

> **(\*구조체포인터).멤버**
>
> **\*(\*구조체포인터).멤버**

```cpp
d2->c1 = 'a';
printf("%c\n", (*d2).c1);      //  a: 구조체 포인터를 역참조하여 c1에 접근
                               // d2->c1과 같음
printf("%d\n", *(*d2).numPtr); // 10: 구조체 포인터를 역참조하여 numPtr에 접근한 뒤 다시 역참조
                               // *d2->numPtr과 같음
```

여기서 (\*d2).c1은 d2->c1과 같고, \*(\*d2).numPtr은 \*d2->numPtr과 같습니다. 즉, 구조체 포인터를 역참조한 뒤 괄호로 묶으면 -> 연산자에서 . 연산자를 사용하게 되므로 포인터가 일반 변수로 바뀐다는 뜻입니다. 역참조의 원리와 같죠.

![fig. 49-2](https://dojang.io/pluginfile.php/482/mod\_page/content/32/4902.png)

### Example 3

{% hint style="info" %}
구조체 별칭으로 선언한 포인터도 구조체 멤버에 접근할 때는 -> (화살표 연산자)를 사용합니다. `p1->age = 30;`
{% endhint %}

struct 키워드로 포인터를 선언하고 메모리를 할당했으니 typedef로 정의한 구조체 별칭으로도 포인터를 선언하고 메모리를 할당할 수 있겠죠?

* **구조체별칭 \*포인터이름 = malloc(sizeof(구조체별칭));**

```cpp
#define _CRT_SECURE_NO_WARNINGS    // strcpy 보안 경고로 인한 컴파일 에러 방지
#include <stdio.h>
#include <string.h>    // strcpy 함수가 선언된 헤더 파일
#include <stdlib.h>    // malloc, free 함수가 선언된 헤더 파일

typedef struct {    // 구조체 이름이 없는 익명 구조체
    char name[20];        // 구조체 멤버 1
    int age;              // 구조체 멤버 2
    char address[100];    // 구조체 멤버 3
} Person;           // typedef를 사용하여 구조체 별칭을 Person으로 정의

int main()
{
    Person *p1 = malloc(sizeof(Person));    // 구조체 별칭으로 포인터 선언, 메모리 할당

    // 화살표 연산자로 구조체 멤버에 접근하여 값 할당
    strcpy(p1->name, "홍길동");
    p1->age = 30;
    strcpy(p1->address, "서울시 용산구 한남동");

    // 화살표 연산자로 구조체 멤버에 접근하여 값 출력
    printf("이름: %s\n", p1->name);       // 홍길동
    printf("나이: %d\n", p1->age);        // 30
    printf("주소: %s\n", p1->address);    // 서울시 용산구 한남동

    free(p1);    // 동적 메모리 해제

    return 0;
}
```

### Example 4

{% hint style="info" %}
지금까지 malloc 함수로 구조체 포인터에 동적 메모리를 할당했습니다. 그럼 동적 메모리를 할당하지 않고 구조체 포인터를 사용하는 방법은 없을까요? 이때는 구조체 변수에 & (주소 연산자)를 사용하면 됩니다.

**구조체포인터 = &구조체변수;**
{% endhint %}

```
#include <stdio.h>

struct Person {    // 구조체 정의
    char name[20];        // 구조체 멤버 1
    int age;              // 구조체 멤버 2
    char address[100];    // 구조체 멤버 3
};

int main()
{
    struct Person p1;      // 구조체 변수 선언
    struct Person *ptr;    // 구조체 포인터 선언

    ptr = &p1;    // p1의 메모리 주소를 구하여 ptr에 할당

    // 화살표 연산자로 구조체 멤버에 접근하여 값 할당
    ptr->age = 30;

    printf("나이: %d\n", p1.age);      
    // 나이: 30: 구조체 변수의 멤버 값 출력
    printf("나이: %d\n", ptr->age);    
    // 나이: 30: 구조체 포인터의 멤버 값 출력

    return 0;
}
```

ptr에 p1의 메모리 주소를 할당했으므로 ptr의 멤버를 수정하면 결국 p1의 멤버도 바뀝니다. 접근하는 방식만 차이가 있을 뿐 결국 같은 곳의 내용을 수정하게 됩니다(메모리 주소는 컴퓨터마다, 실행할 때마다 달라집니다).

구조체 변수의 주소와 구조체 포인터

![](https://dojang.io/pluginfile.php/484/mod\_page/content/23/unit49-3.png)

### Example 5: Structure in a Structure

Structure within Structure is a Useful technique for embedded programming (especially using FSM)

```cpp
struct State{
	uint8_t Out
	uint8_t Time;
	const struct State *Next[2];
};
typedef cost struct State State_t ;
State_t  FSM[4]={
{0x21, 3000, {&FSM[0], &FSM[1] }}
{0x22, 500, {&FSM[1], &FSM[1] }}
};
```

***

## Exercise

* [Online C Compiler](https://www.onlinegdb.com/online\_c\_compiler)
* [Exercise Code](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/structure)
* [Exercise-Solution Code](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/structure/solution)

### Exercise 1

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

![Exercise\_1 result](<../../.gitbook/assets/image (69).png>)

### Exercise 2

[C\_structure\_exercise.c](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/structure)

Define a structure member as

* Typedef Struct Handong
* Members: char building\_name\[100], int room\_number, char room\_name\[100];

Create structure variables room1, room2, room3. Assign the member values as

|       | **Building name** | **Room number** | **Room name** |
| ----- | ----------------- | --------------- | ------------- |
| room1 | Newton            | 109             | SSS-LAB       |
| room2 | Newton            | 118             | Control-Lab   |
| room3 | Newton            | 119             | SW-Lab        |

Create roomPt as Pointer variable of Handong type

Print each room names as follows

![image](https://user-images.githubusercontent.com/38373000/185043527-b1cacb2c-1645-4c1d-91ee-e59a3f1d62ea.png)

### Exercise 3

[C\_structure\_exercise3.c](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/structure)

* Define a structure member for 3D position
* Create the following functions

```cpp
typedef struct {
	int x;
	int y;
	int z;
} POSITION_TypeDef;

//
void addPos(POSITION_TypeDef pos1, POSITION_TypeDef pos1, POSITION_TypeDef *posOut);
void getDist(POSITION_TypeDef pos1, POSITION_TypeDef pos1, POSITION_TypeDef *posOut);
void printPos (POSITION_TypeDef Pos);
```

Use your functions in Main() as

```cpp
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
getDist(Pos[0], Pos[1], &PosOut);
printPos(PosOut);

system("pause");
}
```

![Exercise\_2 result](<../../.gitbook/assets/image (70).png>)
