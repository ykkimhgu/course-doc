# Tutorial: Cortex Debugging in VS Code

## Introduction

#### [방법1:  PlatformIO 디버깅 및 레지스터 확인 방법](tutorial-cortex-debugging-in-vs-code.md#platformio) (실시간 X)

#### [방법2: Live Monitoring : Cortex-Debug (실시간 ](tutorial-cortex-debugging-in-vs-code.md#live-monitoring-cortex-debug) 가능)

***



## 방법1:   PlatformIO 디버깅 및 레지스터 확인

이 가이드는 **추가 확장 설치 없이** PlatformIO IDE에 기본 내장된 디버거만으로 중단점(Breakpoint) 디버깅을 하고, CPU 레지스터와 주변장치(Peripheral) 레지스터 값을 확인하는 방법을 설명합니다.

> 실행 중인 상태에서 변수를 자동으로 계속 갱신해서 보고 싶다면(Live Watch) CORTEX\_DEBUG\_GUIDE 참고하세요. 이 문서에서 다루는 방법은 프로그램이 **멈췄을 때만** 값을 확인할 수 있습니다.

***

### 1. 사전 준비

* 디버깅할 환경(`platformio.ini`의 `[env:...]`)이 VS Code 하단 상태바에서 선택되어 있어야 합니다.
* ST-Link로 보드가 연결되어 있어야 합니다.
* 이 프로젝트는 이미 `.vscode/launch.json`에 `PIO Debug` 설정이 자동으로 들어있고, 보드(STM32F411xE)에 맞는 SVD 파일(`STM32F411xx.svd`) 경로도 PlatformIO가 자동으로 채워주기 때문에 **추가 설정이 필요 없습니다.**

### 2. 디버깅 시작하기

1. 디버깅하려는 `.c` 파일에 해당하는 환경을 VS Code 하단 상태바에서 선택 (예: `LAB_GPIO_DIO_LED_Photosensor`, `TU_FND_display` 등)
2. 코드에서 멈추고 싶은 줄의 왼쪽 여백을 클릭해 빨간 점(Breakpoint)을 찍는다
3. 왼쪽 "Run and Debug" 패널(`Ctrl+Shift+D`)을 열고 상단 드롭다운에서 `PIO Debug` 선택
4. `F5`를 누르면 자동으로 **빌드 → 업로드 → 디버그 시작**까지 한 번에 진행됨 (코드를 수정하고 업로드 없이 바로 디버깅만 하고 싶다면 `PIO Debug (without uploading)` 사용)
5.  설정한 Breakpoint에서 프로그램이 멈추면 상단에 재생/스텝 실행 툴바가 나타납니다

    | 버튼                     | 기능                       |
    | ---------------------- | ------------------------ |
    | ▷ Continue (F5)        | 다음 Breakpoint까지 계속 실행    |
    | ⤵ Step Over (F10)      | 한 줄 실행 (함수 안으로는 안 들어감)   |
    | ⤷ Step Into (F11)      | 한 줄 실행 (함수 호출이면 안으로 들어감) |
    | ⤴ Step Out (Shift+F11) | 현재 함수를 끝까지 실행하고 빠져나옴     |
    | ⏹ Stop (Shift+F5)      | 디버깅 종료                   |

### 3. 주변장치 레지스터 (Peripheral Registers, SVD) 보기

GPIO, RCC, TIM, ADC 같은 **주변장치의 실제 레지스터 값**(예: `GPIOA->MODER`, `GPIOA->ODR`)은 SVD 파일을 기반으로 만들어지는 **PERIPHERALS** 패널에서 확인합니다.

1. 디버깅 중 Breakpoint에 멈춘 상태에서, 왼쪽 "Run and Debug" 사이드바 하단의 `PERIPHERALS` 섹션을 찾아 펼침
2. `GPIOA`, `GPIOB`, `RCC`, `ADC1`, `TIM2` 등 주변장치 이름이 트리로 나열됨
3. 원하는 주변장치(예: `GPIOA`)를 펼치면 그 안의 레지스터(`MODER`, `ODR`, `IDR`, `BSRR` 등)가 나오고, 각 레지스터를 한 번 더 펼치면 **비트필드 단위**로 현재 값을 볼 수 있음
4. 코드를 Step 실행하면서 레지스터 값이 어떻게 바뀌는지 직접 확인 가능

**실습 팁**: `LAB_GPIO_DIO_LED_Photosensor`처럼 레지스터를 직접 조작하는 실습에서는, `GPIOx->MODER`나 `GPIOx->ODR`에 값을 쓰는 코드 바로 다음 줄에 Breakpoint를 걸고 `PERIPHERALS` 패널에서 실제로 레지스터 비트가 의도한 대로 바뀌었는지 확인해보세요.

### 4. Watch 창에 직접 입력하기

`WATCH` 패널에 표현식을 직접 입력해도 됩니다.

변수처럼 `GPIOA->ODR` 또는 `GPIOA->MODER` 같은 C 표현식도 `WATCH`에 그대로 입력해서 값을 볼 수 있습니다 (단, 이 방법 역시 프로그램이 멈춰있을 때만 값이 갱신됩니다).

### 6. 자주 발생하는 문제

| 증상                                                      | 원인 / 해결                                                                                                                                 |
| ------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `PERIPHERALS`에 레지스터 이름만 있고 값이 안 보이거나 `<unreadable>`로 나옴 | 알려진 PlatformIO 이슈. 한 번 Step 실행을 해보거나, 디버깅을 재시작해보세요. 계속 안 되면 ST-Link 펌웨어 업데이트나 다른 디버그 도구(OpenOCD 등)로 바꿔보는 것도 방법입니다.                      |
| `REGISTERS` / `PERIPHERALS` 값이 실행 중(Continue)에는 갱신 안 됨  | 정상입니다 — PlatformIO 기본 디버거는 프로그램이 **멈춰야** 값을 읽어올 수 있습니다. 실행 중 실시간 갱신이 필요하면 Cortex-Debug의 Live Watch를 사용하세요 (CORTEX\_DEBUG\_GUIDE.md 참고). |
| Breakpoint에서 안 멈추고 그냥 지나감                               | Build가 최신 코드로 안 됐을 수 있음 — 다시 F5로 재시작(재빌드+재업로드)해보세요.                                                                                     |
| `Unable to start debugging`                             | 보드 USB 재연결, 다른 디버그/시리얼 모니터 세션이 포트를 점유하고 있지 않은지 확인                                                                                       |

## 방법2: Live Monitoring : Cortex-Debug&#x20;



PlatformIO의 기본 디버거(`PIO Debug`)는 프로그램을 멈춘 상태에서만 변수 값을 볼 수 있습니다. **Cortex-Debug** 확장을 쓰면 보드가 멈추지 않고 계속 동작하는 상태에서 전역 변수 값을 실시간으로 모니터링(**Live Watch**)할 수 있습니다. 예를 들어 포토센서 ADC 값이나 카운터 변수가 실시간으로 바뀌는 것을 그래프 없이 숫자로 바로 확인할 수 있습니다.

***

### 1. 사전 준비

* PlatformIO IDE 확장이 이미 설치되어 있고, 최소 한 번은 프로젝트를 빌드한 상태여야 합니다. (`.pio/build/<환경이름>/firmware.elf` 파일이 존재해야 함)
* ST-Link 드라이버 및 보드 연결은 평소 Upload/Debug 할 때와 동일하게 되어 있으면 됩니다.

### 2. Cortex-Debug 확장 설치

VS Code에서 다음 중 한 가지 방법으로 설치합니다.

**방법 A — 확장 마켓플레이스에서 설치 (권장)**

1. `Ctrl+Shift+X`로 Extensions 패널 열기
2. 검색창에 `Cortex-Debug` 입력
3. 게시자(Publisher)가 **marus25**인 확장을 Install

**방법 B — 터미널에서 설치**

```
code --install-extension marus25.cortex-debug
```

설치하면 아래 보조 확장들도 함께 설치됩니다 (자동, 별도 설치 불필요).

* `mcu-debug.debug-tracker-vscode`
* `mcu-debug.peripheral-viewer`
* `mcu-debug.memory-view`
* `mcu-debug.rtos-views`

### 3. 내 PC의 PlatformIO 도구 경로 확인하기

launch.json 설정에는 OpenOCD, 툴체인(gdb), SVD 파일의 **절대 경로**가 필요합니다. 이 경로는 Windows 계정명(`사용자이름`)마다 다르므로 본인 PC에서 직접 확인해야 합니다. 기본적으로는 아래 형태입니다 (`사용자이름`만 본인 것으로 바꾸면 됨).

| 항목                 | 경로                                                                      |
| ------------------ | ----------------------------------------------------------------------- |
| OpenOCD 실행파일       | `C:/Users/사용자이름/.platformio/packages/tool-openocd/bin/openocd.exe`      |
| OpenOCD scripts 폴더 | `C:/Users/사용자이름/.platformio/packages/tool-openocd/openocd/scripts`      |
| ARM 툴체인 bin 폴더     | `C:/Users/사용자이름/.platformio/packages/toolchain-gccarmnoneeabi/bin`      |
| STM32F411 SVD 파일   | `C:/Users/사용자이름/.platformio/platforms/ststm32/misc/svd/STM32F411xx.svd` |

> 확인이 안 되면 VS Code 터미널에서 `echo $USERPROFILE` (PowerShell: `echo $env:USERPROFILE`)로 본인 홈 디렉토리 경로를 확인한 뒤 위 표의 `C:/Users/사용자이름` 부분에 대입하세요.

### 4. launch.json에 Cortex-Debug 설정 추가

> ⚠️ **주의**: `.vscode/launch.json` 파일 맨 위에 "AUTOMATICALLY GENERATED FILE. PLEASE DO NOT MODIFY IT MANUALLY"라고 적혀 있습니다. PlatformIO가 환경(env)을 바꾸거나 IntelliSense를 재생성할 때 이 파일을 통째로 덮어쓸 수 있습니다. 기존 `"PIO Debug"` 설정들은 **지우지 말고**, `"configurations"` 배열 끝에 아래 블록을 콤마(`,`)로 구분해서 추가하세요. 만약 PlatformIO가 파일을 재생성해서 이 설정이 사라졌다면, 이 가이드를 보고 다시 추가하면 됩니다.

```json
{
    "type": "cortex-debug",
    "request": "launch",
    "name": "Cortex-Debug (Live Watch)",
    "cwd": "${workspaceFolder}",
    "executable": "C:/Users/사용자이름/source/repos/EC2026/.pio/build/환경이름/firmware.elf",
    "servertype": "openocd",
    "serverpath": "C:/Users/사용자이름/.platformio/packages/tool-openocd/bin/openocd.exe",
    "armToolchainPath": "C:/Users/사용자이름/.platformio/packages/toolchain-gccarmnoneeabi/bin",
    "configFiles": [
        "interface/stlink.cfg",
        "target/stm32f4x.cfg"
    ],
    "searchDir": [
        "C:/Users/사용자이름/.platformio/packages/tool-openocd/openocd/scripts"
    ],
    "svdFile": "C:/Users/사용자이름/.platformio/platforms/ststm32/misc/svd/STM32F411xx.svd",
    "liveWatch": {
        "samplesPerSecond": 4,
        "enabled": true
    }
}
```

바꿔야 할 부분은 두 가지뿐입니다.

* `사용자이름` → 본인 Windows 계정명
* `환경이름` → 지금 실습 중인 `platformio.ini`의 `[env:...]` 이름 (예: `LAB_GPIO_DIO_LED_Photosensor`, `TU_FND_display` 등 — 상태바에서 선택된 환경 이름과 동일)

### 5. 실행 순서

1. **먼저 빌드**: VS Code 하단 PlatformIO 상태바의 체크(✓, Build) 버튼을 눌러 최신 `firmware.elf`를 생성 (Cortex-Debug는 PlatformIO의 Upload 기능을 쓰지 않고, 이미 빌드된 elf 파일을 그대로 플래시합니다)
2. 왼쪽 "Run and Debug" 패널(`Ctrl+Shift+D`)에서 상단 드롭다운을 `Cortex-Debug (Live Watch)`로 선택
3. `F5`로 디버깅 시작 → 보드에 플래시되고 프로그램이 실행됨
4. 코드에 Breakpoint를 걸지 않았다면 프로그램은 멈추지 않고 계속 동작합니다

### 6. Live Watch로 실시간 변수 보기

1. 왼쪽 CORTEX DEBUG 패널에서 **LIVE WATCH** 탭 클릭
2. `+` 버튼을 눌러 보고 싶은 변수 이름 입력 (예: 포토센서 ADC 값을 저장하는 전역 변수명)
3. `liveWatch.samplesPerSecond` 값(위 설정에서는 초당 4회)만큼 자동으로 값을 읽어와 갱신
4. 프로그램을 멈추지 않고도 센서 값이 바뀌는 것을 실시간으로 확인 가능

**주의할 점**

* Live Watch는 **전역(global)/static 변수만** 볼 수 있습니다. 함수 안의 지역 변수는 보이지 않습니다.
* 일반 "VARIABLES"/"WATCH" 패널은 프로그램이 멈춰야(Breakpoint) 값이 갱신되지만, **LIVE WATCH 탭만** 실행 중에도 자동 갱신됩니다. 두 패널을 헷갈리지 마세요.

### 7. 자주 발생하는 문제

| 증상                                          | 원인 / 해결                                                                                             |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| `Unable to start debugging` / ST-Link 연결 오류 | 보드 USB 재연결, 다른 디버그 세션(PlatformIO PIO Debug 등)이 동시에 켜져 있지 않은지 확인                                     |
| Live Watch 값이 전혀 안 바뀜                       | 변수 이름 오타 확인, 해당 변수가 `static` 또는 전역으로 선언되어 있는지, 컴파일러 최적화(`-O3`)로 인해 변수가 레지스터에만 존재해 최적화로 사라진 건 아닌지 확인 |
| `executable` 경로를 못 찾음 오류                    | 4단계 실행 전 반드시 PlatformIO Build를 먼저 실행했는지, `환경이름` 폴더명이 실제 `.pio/build/` 아래 폴더명과 일치하는지 확인              |
| launch.json에 추가한 설정이 사라짐                    | PlatformIO가 launch.json을 재생성한 것 — 이 가이드의 4단계 블록을 다시 추가                                              |
