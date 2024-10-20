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
<figure><img src="../../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>


# Step 5: Create a Project in PlatformIO 

<figure><img src="../../.gitbook/assets/image (124).png" alt=""><figcaption></figcaption></figure>





<figure><img src="../../.gitbook/assets/image (125).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../.gitbook/assets/image (126).png" alt=""><figcaption></figcaption></figure>

PlatformIO Toolbar
PlatformIO IDE Toolbar is located in VSCode Status Bar (left corner) and contains quick access buttons for the popular commands. Each button contains hint (delay mouse on it).

../../_images/platformio-ide-vscode-toolbar.png
PlatformIO Home

PlatformIO: Build

PlatformIO: Upload

PlatformIO: Clean

Serial Port Monitor

PlatformIO Core (CLI)

Project environment switcher (if more than one environment is available). See Section [env] of “platformio.ini” (Project Configuration File) .

You can override default buttons and create your own toolbar. See platformio-ide.toolbar in Settings.

<figure><img src="../../.gitbook/assets/image (122).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../.gitbook/assets/image (123).png" alt=""><figcaption></figcaption></figure>
