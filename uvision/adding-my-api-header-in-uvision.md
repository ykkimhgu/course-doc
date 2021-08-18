# Adding my API header in uVision

For example, I want to include  "EC\_GPIO.h" and "EC\_GPIO.c" in main.c

## Method 1

Copy the  "EC\_GPIO.h" and "EC\_GPIO.c"  in the same project folder where **main.c** is located.

Then,  Click the Source group &gt; Add Existing Files to Group &gt; Select "EC\_GPIO.c"

Then, it should compile without errors.

![](../.gitbook/assets/image%20%2846%29.png)





## Method 2 \(recommend\)

Include header file, located in a specific location

* Example:  all header files are located in Github directory under **'lib'**
* eg:   C:\xxx\source\repos\GithubDesktop\EC-stm32f4\EC-STM32f411\Lib

### **Specify an 'Include Path' for your project's header files**

In the **Options for Target1, C/C++** tab of your project, the **Include Paths** box allows you to specify the one or more additional folders to search for header files. 

![](../.gitbook/assets/image%20%2845%29.png)

### **Create New Group** 

1. Right-click the **Project&gt;Target1.**   Select **Add Group**
2. Right-Click **Project&gt; New Group** and ****Select **Options for Group 'New Group'**

> You can rename the group such as "HAL"  in **Manage Project Items.**

![](../.gitbook/assets/image%20%2848%29.png)

### **Add a header file to the New Group** 

1. On **Options for Group 'New Group'** &gt; **C/C++ Tab&gt;  Include Paths&gt;**   choose where the header files are located

![](../.gitbook/assets/image%20%2847%29.png)



Right-click the **Project&gt; New Group**, and **Add Existing Files to Group** 

![](../.gitbook/assets/image%20%2844%29.png)



Add "EC\_GPIO.h" and "EC\_GPIO.c" , and other necessary " \*.h" and ".c" from the lib location.





\*\*\*\*

