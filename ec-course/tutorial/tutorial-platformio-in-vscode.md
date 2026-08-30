# Tutorial: STM32 in VS Code (PlatformIO)

In this class, we are going to use VS Code to program firmware on STM32f4.

## Part 0: Preparation

Prepare EC Workspace

{% embed url="https://ykkim.gitbook.io/ec/ec-course/preparation-for-ec#id-3.-prepare-ec-workspace" %}

##

## Part 1: Installation

### Step 1: Install Python

You can install Python in Window by following the instruction here:

[Install Python 3.x in Win OS](https://www.python.org/downloads/windows/)



How to check if you already have Python

* Open Window Command Prompt > Type `python --version`
*   If you see the version, then you already have python installed.

    ![image](https://github.com/user-attachments/assets/42cdb9a9-7097-4f1a-816d-d96ba8d86869)

### Step 2: Install VS Code

Refer to [Installation-guide: VS CDOE](https://ykkim.gitbook.io/dlip/installation-guide/ide/vscode)

###

### Step 3: Install PlatformIO Core

1. Download python file of PlatformIO Core: [get-platformio.py](https://raw.githubusercontent.com/platformio/platformio-core-installer/master/get-platformio.py)

* Save it as :  `get-platformio.py`

2. Run the downloaded python file

* **WIN OS**: In command prompt

```cmd
# change directory to the folder where you downloaded "get-platformio.py"
cd C:/path-to-dir/where/get-platformio.py/is-located

# run it
python.exe get-platformio.py
```

* **MAC OS**

```cmd
# change directory to the folder where you located downloaded "get-platformio.py"
cd /path-to-dir/where/get-platformio.py/is-located

# run it
python get-platformio.py
```

### Step 4: Install PlatformIO IDE extension in VS Code

1. Open VS Code

<figure><img src="../../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

2. Open `VSCode Package Manager`
3. Search for the official `PlatformIO IDE` in extension.
4. Install `PlatformIO IDE`

<figure><img src="../../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>

## Part 2: Configure Project (STM32f4) / One-time only

### Step 1: Create a Project in PlatformIO

1. Go to PlatformIO Home by clicking on the `PlatformIO Icon`
2. Create New Project

<figure><img src="../../.gitbook/assets/image (124).png" alt=""><figcaption></figcaption></figure>

3. Name the project as **`EC`**
4. Select setting as follows

* Board: ST Nucleo F411RE
* Framework: CMSIS
* Location: **Your EC workspace (e.g. `\repos\EC\`)**

<figure><img src="../../.gitbook/assets/image (126).png" alt=""><figcaption></figcaption></figure>

###

### Step 2: Project Configuration (platformio.ini)

For every new Tutorial or LAB, you do not need to re-configure the MCU setup.&#x20;

You only need one configuration file `platformio.ini` for all tutorial and assigments.

Copy this **`platformio.ini`** in the workspace folder

```ini
; PlatformIO Project Configuration File
;
;   Build options: build flags, source filter
;   Upload options: custom upload port, speed and extra flags
;   Library options: dependencies, extra library storages
;   Advanced options: extra scripting
;
; Please visit documentation for the other options and examples
; https://docs.platformio.org/page/projectconf.html

##################################################################
# Default Setup : DO-NOT CHANGE HERE  
##################################################################
[platformio]
src_dir = .
include_dir = include

# Default environment setting
[env]
platform = ststm32
board = nucleo_f411re
framework = cmsis
debug_tool = stlink
build_flags = -Wl,-u,_print_float,-u,_scanf_float, -std=c11, -O3


##################################################################
# User-Defined Environment   
##################################################################
#
## For example: LAB GPIO
#[env:LAB_GPIO]
#build_src_filter = +<lab/LAB_GPIO_DIO_LED/LAB_GPIO_DIO_LED.c> +<include/*.c>



```

![image](https://github.com/user-attachments/assets/ae464e5e-5559-4ecb-b6a7-ffdddfe4f2f7)

### Step 3: Adding Library

You can add your EC library header files under the directory of `\include`

> You can add your library files in the directory of `\include`&#x20;
>
> Make sure you have mentioned  `include_dir = include`  in `platformio.ini`



## Part 3: Create and Add New Environment

You can start your Tutorial or LAB  from  Part 3.

{% hint style="info" %}
**Each Lab and Tutorial session need to modify the environment.**
{% endhint %}

### Step 1: Create a new environment and program file

1. Open VSCode and select the EC workspace folder

* i.e. : **`\repos\EC\`**

2. Go to PlatformIO Home by clicking on the `PlatformIO Icon`
3. For this tutorial, create the project folder and program source file as

* Folder: **`EC\tutorial\TU_CreateProject_VSC\`**
* Main src: **`TU_CreateProject_Example1_main.c`**

4. Copy the following source code

{% code expandable="true" %}
```c
// TU_CreateProject_Example1_main.c

#include "stm32f4xx.h"
#define LED_PIN    5
#define BUTTON_PIN 13

// LED ON or OFF 

int main(void) {
/* Part 1. RCC Register Setting */
	// RCC Control Register (HSI)
	RCC->CR |= ((uint32_t)RCC_CR_HSION);
	// wait until HSI is ready
	while ( (RCC->CR & (uint32_t) RCC_CR_HSIRDY) == 0 ) {;}
	// Select HSI as system clock source
	// RCC Configuration Register
	RCC->CFGR &= (uint32_t)((uint32_t) ~(RCC_CFGR_SW));
	RCC->CFGR |= (uint32_t)RCC_CFGR_SW_HSI;
	// Wait till HSI is used as system clock source
	while ((RCC->CFGR & (uint32_t)RCC_CFGR_SWS) != 0 ) {;}
	// HSI is used as system clock
	// RCC Peripheral Clock Enable Register
	RCC->AHB1ENR |= RCC_AHB1ENR_GPIOAEN;

/* Part 2. GPIO Register Setting */
	// GPIO Mode Register
	GPIOA->MODER &= ~(3UL<<(2*LED_PIN));
	GPIOA->MODER |=   1UL<<(2*LED_PIN);


	// Dead loop & program hangs here
	while(1){
		// Turn ON LED2
		GPIOA->ODR |= (1UL << LED_PIN);

		// Turn OFF LED2
		//GPIOA->ODR &= ~(1UL << LED_PIN);
	}
}
```
{% endcode %}

### Step 2. Modify Configuration **`platformio.ini`**

Simply, add the new environment that links your new project main file.

Modify **`platformio.ini` ,** to add new environment

```
[env:TU_CreateProject_Example1]
build_src_filter = +<tutorial/TU_CreateProject_VSC/TU_CreateProject_Example1_main.c> +<include/*.c>
```

Example:

{% code expandable="true" %}
```ini
; PlatformIO Project Configuration File
;
;   Build options: build flags, source filter
;   Upload options: custom upload port, speed and extra flags
;   Library options: dependencies, extra library storages
;   Advanced options: extra scripting
;
; Please visit documentation for the other options and examples
; https://docs.platformio.org/page/projectconf.html

##################################################################
# Default Setup : DO-NOT CHANGE HERE  
##################################################################
[platformio]
src_dir = .
include_dir = include

# Default environment setting
[env]
platform = ststm32
board = nucleo_f411re
framework = cmsis
debug_tool = stlink
build_flags = -Wl,-u,_printf_float,-u,_scanf_float -std=c11 -O3



##################################################################
# User-Defined Environment   
##################################################################
#
# For example: LAB GPIO
#[env:LAB_GPIO]
#build_src_filter = +<lab/LAB_GPIO_DIO_LED/LAB_GPIO_DIO_LED.c> +<include/*.c>

# You can add new environments
[env:TU_CreateProject_Example1]
build_src_filter = +<tutorial/TU_CreateProject_VSC/TU_CreateProject_Example1_main.c> +<include/*.c>

# #You can add more new environments
#[env:TU_CreateProject_Example2]
#build_src_filter = +<tutorial/TU_CreateProject_VSC/TU_CreateProject_Example2_main.c> +<include/*.c>


```
{% endcode %}

### Step 3: Select Environment, Build and Run

1. Click on `Switch the environment` on VSCode
2. Select the Environment you want to build.
   * For this tutorial, select **\[env:TU\_CreateProject\_Example1]**
3. BUILD ( cliking on the check icon)
4. If you have MCU connected, you can also UPLOAD
5. Observe your MCU board. It should turn on LED2

![image](https://github.com/user-attachments/assets/50cdf9f0-0fd4-4b1b-9217-78495e87e981)



***

## Exercise:  Add another environment

For a new LAB or Tutorial, you do NOT need to create additional new projects.

Just add a new environment in **`platformio.ini`** and select this to build.



### Preparation

First, download the library header files

* Download link : [https://github.com/ykkimhgu/EC-student/tree/main/include/lib-student](https://github.com/ykkimhgu/EC-student/tree/main/include/lib-student)
* File: **`ecSTM32_simple.c`, `ecSTM32_simple.h`**

Save them in your  `include` folder

> See here for more information on header files: [https://ykkim.gitbook.io/ec/ec-course/tutorial/tutorial-library-header-files#ec-header-files](https://ykkim.gitbook.io/ec/ec-course/tutorial/tutorial-library-header-files#ec-header-files)

### Procedure

Repeat [Part 3 (Step 1 to Step 3).](tutorial-platformio-in-vscode.md#part-3-create-and-add-new-environment)

* Folder: **`EC\tutorial\TU_CreateProject_VSC\`**
* Main src : **`TU_CreateProject_Example2_main.c`**
* Environment: **`[env:TU_CreateProject_Example2]`**

**`TU_CreateProject_Example2_main.c`**

{% code expandable="true" %}
```c

//#include "ecSTM32F4v2.h"
#include "ecSTM32_simple.h"
#define LED_PIN PA_5
#define BUTTON_PIN PC_13

void setup(void);

int main(void) {
	setup();

	while (1) {
		delay_ms(1000);
		GPIO_write(LED_PIN, LOW);
		delay_ms(1000);
		GPIO_write(LED_PIN, HIGH);
	}
}


// Initialiization
void setup(void) {
	RCC_PLL_init();
	SysTick_init();

	// initialize the pushbutton pin as an input:
	GPIO_init(BUTTON_PIN, INPUT);
	// initialize the LED pin as an output:
	GPIO_init(LED_PIN, OUTPUT);
}

```
{% endcode %}

##

## Appendix

### PlatformIO Toolbar

[Read more about it here](https://docs.platformio.org/en/latest/integration/ide/vscode.html#platformio-toolbar)

![image](https://github.com/user-attachments/assets/cfb89c99-d11d-4af5-8353-79230c4d033f)

PlatformIO IDE Toolbar is located in VSCode Status Bar (left corner) and contains quick access buttons for the popular commands. Each button contains hint (delay mouse on it).

1. [PlatformIO Home](https://docs.platformio.org/en/latest/home/index.html#piohome)
2. PlatformIO: Build
3. PlatformIO: Upload
4. PlatformIO: Clean
5. [Serial Port Monitor](https://docs.platformio.org/en/latest/core/userguide/device/cmd_monitor.html#cmd-device-monitor)
6. [PlatformIO Core (CLI)](https://docs.platformio.org/en/latest/core/index.html#piocore)
7. Project environment switcher (if more than one environment is available). See \[Section [env\]](https://docs.platformio.org/en/latest/projectconf/sections/env/index.html#projectconf-section-env) of [“platformio.ini” (Project Configuration File)](https://docs.platformio.org/en/latest/projectconf/index.html#projectconf) .
