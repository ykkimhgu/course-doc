# Installing VS Code(Mac/Linux)

C/C++ Programming environment for Mac users

### 개요

- [Visual Studio Code](#visual-studio-code)


- [부록1: CMake 설치](#부록1-cmake-설치)

- [부록2: Visual Sutdio 빌드 도구 설치](#부록2-visual-studio-빌드-도구-설치)

---



# Visual Studio Code

## I. 특징

 Visual Studio의 텍스트 편집기 기반으로 Microsoft 사에서 만든 **코드 편집기**이다. 엄밀하게 말하면 IDE가 아니다. 하지만 Visual Studio Code(이하 VScode)에서 제공하는 다양한 확장 옵션으로 IDE와 비슷하게 사용할 수 있다.



### 장점

1. **가볍게 실행되는 프로그램**
   텍스트 편집기 기반이기 때문에 엄청 가볍다. 사실상 기능 많은 메모장에 가깝다.

2. **다양한 언어 지원**
   C, C++, Python, JAVA 등 다양한 언어를 하나의 툴에서 개발할 수 있다.

3. **다양한 확장 옵션**
   오픈 소스이기 때문에 다양한 확장을 통해서 IDE와 비슷한 기능을 확보할 수 있다.

4. **크로스 컴파일 지원**
   Windows, Linux, Mac을 모두 지원한다. 다양한 OS에서 개발할 경우 아래의 CLion과 함께 좋은 선택지다.

5. **유저 친화적 GUI**
   GUI가 단순하기 때문에 복잡하지 않다.

### 단점

1. **프로젝트 단위 빌드 힘듬**
   프로젝트 단위로 개발하기 위해서는 CMake와 같은 빌드 도구의 힘을 빌려야 한다. 때문에 CMake 작성법을 추가로 학습해야 하며, 빌드 에러가 발생할 수 있다.

2. **느린 속도(확장을 많이 깔았을 때)**
   확장을 통해서 IDE와 비슷한 기능을 확보한 경우 IDE만큼 속도가 느려지는 경우가 많다. 특히 언어 확장을 다수 설치한 경우 두드러진다. 때문에 본인이 필요한 기능만 설치하는 것이 좋다.

3. **힘든 프로젝트 초기 설정**
   CMake 초기 설정시에 컴파일러, 빌드 툴 등을 수동으로 지정해야 한다.
   
   

## II. 설치

1. [설치 사이트](https://visualstudio.microsoft.com/ko/downloads/)에 접속하여 설치 파일을 다운로드 받는다.
   
   <img title="" src="./img/vscode_site.png" alt="" data-align="center" style="zoom:25%;">

2. 다운로드 받은 설치 파일을 실행한다.
   
   <img title="" src="./img/vscode_installer.png" alt="" data-align="center" style="zoom:150%;">
   
   <img title="" src="./img/vscode_install1.png" alt="" data-align="center" style="zoom:50%;">
   
   <img title="" src="./img/vscode_install2.png" alt="" data-align="center" style="zoom:50%;">
   
   <img title="" src="./img/vscode_install3.png" alt="" data-align="center" style="zoom:50%;">
   
   <img title="" src="./img/vscode_install4.png" alt="" data-align="center" style="zoom:50%;">
   
   <img title="" src="./img/vscode_install5.png" alt="" data-align="center" style="zoom:50%;">

3. C/C++ 개발에 필요한 확장을 설치한다.

<img title="" src="img/vscode_window.png" alt="" data-align="center" style="zoom:25%;">

<img title="" src="img/ex.png" alt="" data-align="center" style="zoom:150%;">

4. CMake를 설치한다.([부록1: CMake 설치](#부록1-cmake-설치) 참고)

5. 빌드 도구를 설치한다. (Windows만 해당, [부록: Visual Sutdio 빌드 도구 설치](#부록2-visual-studio-빌드-도구-설치) 참고)
   
   

## III. 프로젝트 설정

1. 임의의 위치에 폴더를 하나 만든다. (본 문서에서는 `Tutorial`)

2. VScode에서 해당 폴더를 열고, `F1`키를 누른다. 이후 `CMake: Quick Start`를 입력한다.
   
   <img title="" src="img/vscode2.png" alt="" data-align="center" style="zoom:33%;">

3. 프로젝트 이름을 입력한다. 프로젝트 이름은 생성한 폴더 이름과 동일하게 설정한다.
   
   <img title="" src="img/vscode3.png" alt="" data-align="center" style="zoom:33%;">

4. 언어는 `C`를 선택한다.
   
   <img title="" src="img/vscode4.png" alt="" data-align="center" style="zoom:33%;">

5. 실행파일(Excutable)을 선택한다.
   
   <img title="" src="img/vscode5.png" alt="" data-align="center" style="zoom:33%;">

6. 빌드 설정을 추가한다.
   
   <img title="" src="img/vscode7.png" alt="" data-align="center" style="zoom:33%;">

7. `컴파일러에서 만들기`를 선택한다.
   
   <img title="" src="img/vscode8.png" alt="" data-align="center" style="zoom:33%;">

8. 컴파일러를 선택한다. **윈도우는 Visual Studio**, **Mac은 Clang**을 선택한다.
   
   <img title="" src="img/vscode9.png" alt="" data-align="center" style="zoom:33%;">

9. `CMakeLists.txt`에 아래 내용을 복사한다. `<PROJECT>`부분은 <u>**본인의 프로젝트 이름**으로 **수정**</u>한다.
   
   ```cmake
   cmake_minimum_required(VERSION 3.25)
   project(<PROJECT> LANGUAGES C)
   
   set(CMAKE_C_STANDARD 17)
   
   file(GLOB_RECURSE SOURCE
        ${CMAKE_CURRENT_SOURCE_DIR}/src/*.c
        )
   
   add_executable(${PROJECT_NAME} main.c ${SOURCE})
   target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/include)
   ```
   
   

10. `main.c`를 아래와 같이 수정후 실행해 본다.
    
    ```c
    #include <stdio.h>
    
    int main()
    {
        printf("Hello World!");
    
        return 0;
    }
    ```
    
    

---





# 부록1: CMake 설치

## Windows

1. [CMake 설치 사이트](https://cmake.org/download/)에 접속하여 최신버전 CMake를 설치한다.<img title="" src="img/cmake_download.png" alt="" data-align="center" style="zoom:50%;">

2. 설치 프로그램에서 아래 순서와 같이 설치한다.
   
   <img title="" src="img/cmake_install1.png" alt="" data-align="center" style="zoom:33%;">
   
   <img title="" src="img/cmake_install2.png" alt="" data-align="center" style="zoom:100%;">
   
   <img title="" src="img/cmake_install3.png" alt="" style="zoom:33%;" data-align="center">

## Linux

1. apt를 업데이트 한다
   
   ```bash
   sudo apt update
   ```

2. CMake를 다운로드 받는다
   
   ```bash
   sudo apt install cmake
   ```
   
   

## Mac

1. Homebrew를 설치한다
   
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

2. Homebrew가 설치되었는지 확인한다.
   
   ```bash
   brew -v
   ```

3. `command not found: brew` 에러가 발생하는 경우 아래 명령을 터미널에 입력한다.
   
   ```bash
   echo 'export PATH=/opt/homebrew/bin:$PATH'
   ```

---

# 부록2: Visual Studio 빌드 도구 설치

1. [설치 사이트](https://visualstudio.microsoft.com/ko/downloads/)에 접속하여 인스톨러를 다운로드 받는다.
   
   <img title="" src="img/vsbuildtool_install.png" alt="" data-align="center" style="zoom:50%;">

2. 인스톨러에서 아래와 같이 설정 후 설치한다.
   
   <img title="" src="img/vsbuildtool_installer.png" alt="" data-align="center" style="zoom:50%;">
