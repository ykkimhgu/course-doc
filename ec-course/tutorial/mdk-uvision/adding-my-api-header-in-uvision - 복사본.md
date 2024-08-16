# Tutorial: Adding library header in uVision

For example, include "EC\_GPIO.h" and "EC\_GPIO.c" in `main.c`

## Method 1 (recommended)

Include header files, located in a specific location

Example: all header files are located in Github directory under **'lib'**
* eg: C:\..\..\source\repos\GithubDesktop\EC-stm32f4\EC-STM32f411\Lib\

### **Specify 'Include Path' for your project's header files**

In the **Options for Target1, C/C++** tab of your project, the **Include Paths** box allows you to specify the one or more additional folders to search for header files.

![](<../../../.gitbook/assets/image (45).png>)

### **Create New Group**

1. Right-click the **Project>Target1.** Select **Add Group**
2. Right-Click **Project> New Group** and Select **Options for Group 'New Group'**

> You can rename the group such as "HAL" in **Manage Project Items.**

![](<../../../.gitbook/assets/image (48).png>)

### **Add a header file to the New Group**

1. On **Options for Group 'New Group'** > **C/C++ Tab> Include Paths>** choose where the header files are located

![](<../../../.gitbook/assets/image (47).png>)

Right-click the **Project> New Group**, and **Add Existing Files to Group**

![](<../../../.gitbook/assets/image (44).png>)

Add "EC\_GPIO.h" and "EC\_GPIO.c" , and other necessary " \*.h" and ".c" from the lib location.



---


## Method 2 (easy but not recommended)

Copy the "EC\_GPIO.h" and "EC\_GPIO.c" in the same project folder where **main.c** is located.

Then, Click the Source group > Add Existing Files to Group > Select "EC\_GPIO.c"

Then, it should compile without errors.

![](<../../../.gitbook/assets/image (46).png>)



***
