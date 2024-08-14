# Tutorial: Repository Management

## Overall Repository

> C:\Users\\(user\_name)\source\repos

![overall repository](https://user-images.githubusercontent.com/91526930/191556057-65dca8d4-1ed8-465f-be78-dad817e5d10f.png)

## 1. EC workspace (Lab, Tutorial, Lib)

This is the local folder where you do your tutorial, lab.

### Create the EC workspace: &#x20;

* Create a folder named as "EC" in "C:\users(user\_name)\source\\**repos**".
* Create 3 folders named as "Tutorial", "LAB" and "lib" in "EC" folder.
  * Tutorial / LAB : includes the project files.
  * lib : includes header files.

![project repository](https://user-images.githubusercontent.com/91526930/191545719-22cd8352-764b-4ec5-ba44-1f001e28e89b.png)

### Create a new project & Include library paths

* Create a new folder in "LAB" or "Tutorial".
  * named as "LAB\_(title)" or "TU\_(title)"
* Create a new project and set for embedded board. [Tutorial: Create a project with uVision](https://ykkim.gitbook.io/ec/course/tutorial/mdk-uvision/create-a-project-with-uvision)
* Then, set "Include Paths" as "C:\users(user\_name)\source\repos\\**EC\lib**".

![include path](https://user-images.githubusercontent.com/91526930/191547513-cd560068-4d3b-4294-97a8-729898d1c6d6.png)



## 2. EC-student repository (provided source codes from EC github)

This is where you download or fetch  the provided source codes used in EC lecture.&#x20;



### Clone the repository "EC-student"

> [github.com/ykkimhgu/EC-student](https://github.com/ykkimhgu/EC-student)

* Open with Github Desktop.

![open with github desktop](https://user-images.githubusercontent.com/91526930/191557234-4d78a0b4-6caf-4a75-9de2-14453dc80d04.png)

* Check "Local path" and Click "Clone".

![clone a repository](https://user-images.githubusercontent.com/91526930/191557650-3c358004-3040-433f-b393-62e7af8cf8ce.png)

* Then, you can see the repository "EC-student" is copied in your local PC.
* You can copy & paste some files for tutorial or lab.

![local repository](https://user-images.githubusercontent.com/91526930/191560757-d2575d84-97f1-49e0-b7d7-49909913fc0f.png)

## 3. Your private github repository

This is for managing the version of your library and  your source file. DO NOT include the project related files.

### Create a new git repository

* Create a new repository in [Github](https://www.github.com).
  * Repository name : EC-(your initials)-(3 ends of student number)
* You must choose **Private**.
* Add a README file.

![create a repository](https://user-images.githubusercontent.com/91526930/191548704-cf0feaa9-09ea-4e3a-a4d8-e4f9586699c4.png)

### Clone github repository in local folder

* Open with Github Desktop from github repository

![open with github desktop](https://user-images.githubusercontent.com/91526930/191561482-6196cbea-0aae-4462-a982-f583f169e24a.png)

* Check "Local path", and click "clone".

![clone a repository](https://user-images.githubusercontent.com/91526930/191552158-3a893f73-ba20-48d4-a237-aca4465706b1.png)

### Create folders

* docs : documents for functions
* include : includes header files
* lab : includes report and source files
* tutorial : includes source files

![local repository](https://user-images.githubusercontent.com/91526930/191554413-5ac21137-b68f-4792-8b42-e2ef27aec442.png)

### Commit & Push

* Copy & paste only source files from your project directory.

![copy src files](https://user-images.githubusercontent.com/91526930/191562662-0cf216f2-dc2e-4d55-8506-f8800d613b1f.png)

* Github Desktop shows changed things.

![github desktop](https://user-images.githubusercontent.com/91526930/191564347-80c028a6-c4f4-4b77-8ef0-bc4f9acd5904.png)

* **Commit** : records the modified things.

![commit](https://user-images.githubusercontent.com/91526930/191563571-29cd4122-d2f8-4559-8dca-e4080b7521d6.png)

* **Push** to github

![push](https://user-images.githubusercontent.com/91526930/191565111-0ab1454e-63bf-430e-ac82-85e5e5cbeda6.png)

![github](https://user-images.githubusercontent.com/91526930/191565626-489af687-d429-4bd8-9849-2b439fe60f80.png)

## Collaborator

* Click \[Settings] - \[Collaborators] - \[Add people]
* Search professor and TA id and add them as collaborator.
  * ykkimhgu
  * ckdals9150

![add collaborators](https://user-images.githubusercontent.com/91526930/191632075-90f7d8d2-422b-4bdb-bf9e-19c624bb88b9.png)

![add a collaborator](https://user-images.githubusercontent.com/91526930/191632610-e2cb1c52-1602-4b7d-a67d-b20f98d291df.png)
