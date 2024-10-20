# Tutorial: PlatformIO in VSCode

# Step 1: Install Python

You can install Python in Window by following the instruction here:

[Install Python 3.x in Win OS](https://www.python.org/downloads/windows/)

How to check if you already have Python
* Open Window Command Prompt >  Type `python --version`
* If you see the version, then you already have python installed.

  ![image](https://github.com/user-attachments/assets/42cdb9a9-7097-4f1a-816d-d96ba8d86869)


# Step 2: Install VS Code

Refer to [Installation-guide: VS CDOE](https://ykkim.gitbook.io/dlip/installation-guide/ide/vscode)



# Step 3: Install PlatformIO Core
> You may not need this step if you are going to use VS Code. But, install anyway.

1. Download python file of PlatformIO Core:   [Download get-platformio.py](https://raw.githubusercontent.com/platformio/platformio-core-installer/master/get-platformio.py)
* Download and save it as in py file

**WIN OS**: 
In command prompt

```cmd
# change directory to the folder where is located downloaded "get-platformio.py"
cd C:/path-to-dir/where/get-platformio.py/is-located

# run it
python.exe get-platformio.py
```

**MAC OS**
```cmd
# change directory to the folder where is located downloaded "get-platformio.py"
cd /path-to-dir/where/get-platformio.py/is-located

# run it
python get-platformio.py
```


# Step 4: Install PlatformIO IDE extension in VS Code
1. Open VS Code

<figure><img src="../../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

2. Open VSCode Package Manager
3. Search for the official PlatformIO IDE in extension.
4. Install PlatformIO IDE

<figure><img src="../../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>


# Step 5: Setting Up the Project in PlatformIO 

1. Go to PlatformIO Home by clicking on the `PlatformIO Icon`
2. Create New Project

<figure><img src="../../.gitbook/assets/image (124).png" alt=""><figcaption></figcaption></figure>



3. Name the project as `EC` or `EC2024`
4. Select Setting as follows
* Board: ST Nucleo F411RE
* Framework: CMSIS
* Location: Your EC workspace




<figure><img src="../../.gitbook/assets/image (126).png" alt=""><figcaption></figcaption></figure>


# Appendix
## PlatformIO Toolbar
[Read more about it here](https://docs.platformio.org/en/latest/integration/ide/vscode.html#platformio-toolbar)


![image](https://github.com/user-attachments/assets/cfb89c99-d11d-4af5-8353-79230c4d033f)


PlatformIO IDE Toolbar is located in VSCode Status Bar (left corner) and contains quick access buttons for the popular commands. Each button contains hint (delay mouse on it).


1. [PlatformIO Home](https://docs.platformio.org/en/latest/home/index.html#piohome)
2. PlatformIO: Build
3. PlatformIO: Upload
4. PlatformIO: Clean
5. [Serial Port Monitor](https://docs.platformio.org/en/latest/core/userguide/device/cmd_monitor.html#cmd-device-monitor)
6. [PlatformIO Core (CLI)](https://docs.platformio.org/en/latest/core/index.html#piocore)
7. Project environment switcher (if more than one environment is available). See [Section [env\]](https://docs.platformio.org/en/latest/projectconf/sections/env/index.html#projectconf-section-env) of [“platformio.ini” (Project Configuration File)](https://docs.platformio.org/en/latest/projectconf/index.html#projectconf) .

