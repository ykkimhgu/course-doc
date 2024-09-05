# Tutorial: Adding library header in uVision

## Tutorial: Adding library header in uVision

This tutorial explains how to include header files in uVision project.

## Preparation

#### 1. Download header files

Here, we assume that necessary header files are stored in your workspace `..\..\repos\EC\include\`

Download tutorial source files

* [Library header files](https://github.com/ykkimhgu/EC-student/tree/main/include/lib-student):
  * [ecSTM32\_simple.h](https://github.com/ykkimhgu/EC-student/blob/main/include/lib-student/ecSTM32_simple.h), [ecSTM32\_simple.c](https://github.com/ykkimhgu/EC-student/blob/main/include/lib-student/ecSTM32_simple.c)
  * [ecPinNames.h](https://github.com/ykkimhgu/EC-student/blob/main/include/lib-student/ecPinNames.h), [ecPinNames.c](https://github.com/ykkimhgu/EC-student/blob/main/include/lib-student/ecPinNames.h)



Include header files, located in a specific folder

* save the downloaded header files in your workspace: `..\..\repos\EC\include\`

![image](https://github.com/user-attachments/assets/8be8b761-4253-4706-b396-8a94f808bf0e)

#### 2. Create a New Project in uVision

You can refer to [Tutorial: Create a Project with uVision](https://ykkim.gitbook.io/ec/ec-course/tutorial/mdk-uvision/create-a-project-with-uvision)

You can skip this if you already have the project opened.

#### 3. Create a new source main file.

Create a new program file as `TU_CreateProject_main.c`. This is the same file used in [Tutorial: Create a Project with uVision](https://ykkim.gitbook.io/ec/ec-course/tutorial/mdk-uvision/create-a-project-with-uvision)

We will modify the main program code as

```c
#include "ecSTM32_simple.h"


#define LED_PIN PA_5
#define BUTTON_PIN PC_13

void setup(void);

int main(void) { 
 	setup();
	int buttonState=0;
	
	while(1){
		// check if the pushbutton is pressed. Turn LED on/off accordingly:
		buttonState = 	GPIO_read(BUTTON_PIN);
		if(buttonState)	GPIO_write(LED_PIN, LOW);
		else 		GPIO_write(LED_PIN, HIGH);
	}
}


// Initialiization 
void setup(void) {
	RCC_HSI_init();
	// initialize the pushbutton pin as an input:
	GPIO_init(BUTTON_PIN, INPUT);  
	// initialize the LED pin as an output:
	GPIO_init(LED_PIN, OUTPUT);    
}
```

## Include Library Path

We will learn how to include Library Path in your project

#### 1. **Specify 'Include Path' for your Project Target**

Open **Options for Target** (press ALT+F7) > **C/C++** tab > **Include Paths**

Add the path location for the include files.

![image](https://github.com/user-attachments/assets/952c39a7-a752-4fe6-a5e1-abaf5fa71e2b)

#### 2. **Create a New Group folder** and rename

1. Right-click on the **Project>Target1.** Then, select **Add Group**
2. Rename the New Group by going to **Manage Project Items.**
3. Change the name such as "**Include**"

![image](https://github.com/user-attachments/assets/54bd619a-60f8-4e89-aec1-a93bb3fac555)

### 3. Specify 'Include Path' for your Include

1. Right-Click on **Project> Include**> **Options for Group 'New Group'**
2. **Options for Group 'Include'** > **C/C++ Tab> Include Paths>** choose where the header files are located

> Options for Group, NOT Target1

![image](https://github.com/user-attachments/assets/51f0de8d-cf94-434a-aadb-aa6a38d4ffca)

#### 4. \*\*Add Existing Header files \*\*

**Project> Include > Add Existing Files to Group**

Add your libraries, such [`ecSTM32_simple.h, ecSTM32_simple.c`](https://github.com/ykkimhgu/EC-student-2023/tree/main/tutorial/tutorial-student/Tutorial-AddLibrary-uVision)

Also, you can add more files

![image](https://github.com/user-attachments/assets/27b8c700-c4bf-46af-bb79-9cd99014b7f5)

#### 5. Run the project and see results
