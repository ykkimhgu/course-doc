---
description: For Numerical Programming
---

# Tutorial: Version Control in Github

## Version Control 

In this tutorial, we will learn how to use Github for version control of my library/SW  for our numerical method functions.

For each assignment, you probably have created ‘myNM.h, myNM.cpp’ files in every project folder.

If there are multiple source files of the same file name, you may have trouble in controlling the version of your library.

> Create and modify myNM.h, myNM.cpp in ‘\include’ folder only

One suggestion is to create and modify your header in a directory and include those files/directory for each project, without copying the header files in the project folder.

After modifying your library source in every assignment, you may have a problem  updating to the latest version.

For the version control, you should use git or github. For this tutorial, we will use Github Desktop.

## Exercise

### TA Session video

{% embed url="https://www.youtube.com/watch?v=yFgIvPB\_Q-c" %}



1. Create a **private** repository for numerical method

   * Example: NumericalProg

2. Create folders within the repository as instructed in the 

* [Tutorial: Create Github Repos of NP lib](tutorial-markdown.md#preparation)

3. Maintain your library source files`myNM.h, myNM.cpp` and `myMatrix.h, myMatrix.cpp` in the ‘\include’ folder only

{% hint style="info" %}
There should be only 1 copy of your header/lib files in local drive. Include this folder path to include these files in other program projects. 
{% endhint %}



4. Copy your final assignment main\(\) source file in  the folder `\sources` 

* Also, you can copy necessary other files such as `data txt`

5. Under the `\tutorial` folder, copy and push **tutorial, exercise**  files of this lecture. 

### When creating a new project for assignments

1.DO NOT copy the NM header files in each project folder.

2. Configure V.Studio project configuration to include the path for your header files. 

* located in that \`\include folder of the Git repository \(local drive\)
* For this example,  ' ../GithubDesktop/NumericalProg/include'

3. or you can directly include the header files in main\(\). 





