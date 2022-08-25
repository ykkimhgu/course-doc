# Visual Studio 2022

## How to Install

Microsoft 계정을 통해, 무료로 다운로드 할 수 있습니다.

\
\


[Click here to Download](https://visualstudio.microsoft.com/ko/vs/community/)

![image](https://user-images.githubusercontent.com/84503980/185387298-f280c598-c184-49a5-bc39-f87c82c94355.png)

**"Visual Studio 다운로드"** 버튼을 누른 뒤, **Visual Studio Community 2022** 를 눌러 설치를 시작합니다.

\
\


![image](https://user-images.githubusercontent.com/84503980/185388422-de85399e-b0a7-45b5-a8ff-fdc859927f8c.png)

**C++를 사용한 데스크톱 개발** 항목을 선택한 후, 설치 버튼을 누릅니다.

\
\


![image](https://user-images.githubusercontent.com/84503980/185389166-eead55f5-e955-4fd7-95f3-87e73f94a263.png)

설치가 끝날 때 까지 기다립니다. 네트워크 환경에 따라 설치 시간이 오래 걸릴 수 있습니다. [**DNS 변경**](https://ivyit.tistory.com/190)을 통해, 보다 빠르게 설치할 수 있습니다.

\
\


![image](https://user-images.githubusercontent.com/84503980/185390095-d0510671-f8d7-4a10-b00c-73cc276c9d6b.png)

**Visual Studio**는 **Microsoft** 계정과 연동되어 있기 때문에, 로그인 화면이 나옵니다. 본인의 계정으로 로그인합니다. 당장 진행하지 않고 건너뛰어도 무방합니다.

\
\


![image](https://user-images.githubusercontent.com/84503980/185390456-00fc4d8f-fdc5-4b35-a08a-856d6d529f74.png)

자신이 원하는 테마를 설정합니다. 테마 변경 또한 언제든 다시 할 수 있습니다.

\
\


## Visual Studio 사용하기

![image](https://user-images.githubusercontent.com/84503980/185390762-7065b68c-fab3-468b-b244-d773bdeaf696.png)

**새 프로젝트 만들기** 를 클릭합니다.

\
\


![image](https://user-images.githubusercontent.com/84503980/185390827-548640f3-9048-478c-8e0d-bf8469045809.png)

기본값으로 설정된 **빈 프로젝트** 를 선택한 후 다음 버튼을 누릅니다.

\
\


![image](https://user-images.githubusercontent.com/84503980/185392645-d08260f5-a092-46ca-b12e-ca718c3e0cf2.png)

**프로젝트란?** 작업하고 있는 main 코드를 실행시키기 위해 필요한 여러 개의 소스 파일과 헤더 파일을 하나로 묶어 놓은 집합체를 의미합니다.

**솔루션이란?** 여러 개의 프로젝트가 모인 것으로, 프로젝트의 상위 폴더 개념으로 이해할 수 있습니다.\
프로젝트 생성 과정에서 프로젝트와 동일한 이름으로 자동 생성되는데, 임의로 이름을 변경할 수도 있습니다. 이미 생성된 솔루션 안에 새로운 프로젝트를 추가하는 것 또한 가능합니다.

\
\


![image](https://user-images.githubusercontent.com/84503980/185393298-cd0bbffd-7aec-45b0-a7a7-c01b682fd112.png)

프로젝트 생성 시, 우측에 **솔루션 탐색기** 가 보입니다. 위 사진과 같이, 소스 파일에서 오른쪽 마우스 클릭을 통해 **새 항목** 을 추가합니다.

\
\


아래 소스코드를 입력한 후, 실행합니다\
(디버그 -> 디버그하지 않고 시작) 또는 (CTRL + F5)

```c
#include <stdio.h>

int main()
{
    printf("Hello, Handong!\n");

    return 0;

}
```

\
\


**기존 항목**을 클릭하여, 외부의 소스 파일을 프로젝트에 추가하여 실행시키는 것도 가능합니다.

* [**Source Code**](https://github.com/ykkimhgu/Tutorial-C-Program/tree/main/installVisualStudio)

![image](https://user-images.githubusercontent.com/84503980/185394697-c321c21d-0bf2-4fee-a4dd-ff7678d7fbfe.png)

정상적으로 따라왔다면, 위와 같은 결과가 출력됨을 확인할 수 있습니다.

## Visual Studio 사용하기 2

위에서, 단순 문장을 출력하는 코드를 작성했습니다. 다음으로, 두 숫자를 입력받아 합을 출력하는 코드를 작성해 봅시다.

위에서 생성한 프로젝트 안에, **C\_visualStudio\_exercise2.c** 코드를 생성하고, 실행합니다.

\
\


```c
#include <stdio.h>

int main() {
	int a=0, b=0;
	int sum=0;

	printf("Enter two numbers: ");
	scanf("%d %d", &a, &b);
	sum = a + b;
	printf("%d + %d = %d\n", a, b, sum);
	printf("Thank you!");
	
	return 0;

}
```

\
\


![image](https://user-images.githubusercontent.com/84503980/185396376-4474bf9d-3614-4549-9ef4-495fb1b70193.png)

그런데, build를 하면 **scanf** 에서 오류가 나는 것을 확인할 수 있습니다. 노란색 경고의 경우 단순히 버려지는 값이 있음을 알리기 위해 인텔리센스가 안내하는 내용입니다. 원하는 작업대로 정상 동작 한다면 무시해도 됩니다.

**scanf** 오류 해결을 위해, 해당 프로젝트에 우클릭, **속성** 버튼을 클릭합니다.

![image](https://user-images.githubusercontent.com/84503980/185396912-3846f393-e587-4338-a929-3acc9ea40d53.png)

속성의 **C/C++ -> 전처리기** 에 들어가서, **전처리기 정의** 부분에\
\_CRT\_SECURE\_NO\_WARNINGS; 를 추가합니다.

이후 적용 -> 확인을 통해 창을 닫습니다.

\
\


![image](https://user-images.githubusercontent.com/84503980/185397537-142644d8-45c3-4577-9f3b-ea2caf11b9cd.png)

이후 다시 빌드를 하면, **scanf** 오류가 해결되었음을 확인할 수 있습니다. 그런데, 또 다른 **main**이 정의되었다는 에러가 발생했습니다.

같은 프로젝트 내에서 **C\_visualStudio\_exercise.c**, **C\_visualStudio\_exercise2.c** 두 개의 소스파일을 빌드하는 과정에서 main이 중복되기 때문입니다.

\
\
![image](https://user-images.githubusercontent.com/84503980/185398246-cfa1ab71-b2f1-442f-a636-82a77ab489e7.png)

위와 같이 기존 main 함수의 이름을 임의로 바꾸어 주거나, 기존 소스 파일을 프로젝트에서 제거할 경우 오류 없이 정상적으로 **C\_visualStudio\_exercise2.c**가 빌드됩니다.

![image](https://user-images.githubusercontent.com/84503980/185398682-dbffe166-e721-48ab-bc5a-7e264d9b323d.png)

위와 같이 **속성->빌드에서 제외** 항목을 바꿔주는 것도 하나의 방법입니다.

\
\


## Visual Studio 활용하기: 디버깅

디버깅은 코드의 에러를 찾는 데 매우 효과적인 방법입니다.

### 한 줄씩 디버깅하기

![image](https://user-images.githubusercontent.com/84503980/185399262-71a4c79e-737f-42f6-b8dd-e2df606bbcb2.png)

\
\


위와 같이, 단축키 **F11** 을 누르면 위에서부터 한 줄씩 코드를 실행시킵니다. **F11**을 누를 때 마다 좌측 화살표가 한 줄씩 내려가며 코드가 실행됩니다. 이를 통해 프로그램의 진행 상황을 파악할 수 있고, 에러가 발생한 경우 어느 위치에서 에러가 발생하는지를 알 수 있습니다.

\
\


### 중단점(Break Point) 활용하여 디버깅하기

단축키 **F9** 단축키를 통해 중단점을 설정할 수 있습니다.

![image](https://user-images.githubusercontent.com/84503980/185399797-0f4a8a89-b628-4c59-bc1c-a340ac29a309.png)

위 사진은 9번째 줄에 중단점을 설정한 상태입니다. 이 상태에서 **F5**를 눌러 디버깅을 시작하면, 설정한 중단점부터 디버깅을 시작합니다.

디버깅이 시작된 이후에는, **F11**을 통해 한 줄씩 코드를 실행시킬 수 있습니다.

Visual Studio 디버깅에 관해 보다 상세한 설명이 필요하다면, [Link](https://dojang.io/mod/page/view.php?id=806) 를 참조하시기 바랍니다.
