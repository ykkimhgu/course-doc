# Tutorial: Installing MDK uVision

## Installation

{% hint style="info" %}
윈도10 User(계정) 이름이 한글이면 설치 후 동작이 안 될 수 있음!!!

무조건 영문 계정에서 설치해야 함!!!!
{% endhint %}

![](<../../../.gitbook/assets/image (109).png>)

### Download

Go to keil.com and Download **MDK-Lite (MDK-Core)** : [click here](https://www2.keil.com/mdk5)

> v5.35 @ 2021.8

![](<../../../.gitbook/assets/image (14).png>)

### Install uVision

Start installation.

> This lecture uses mdk535.exe

![](<../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png>)

Pack directory는 default로 설정 (경로에 한글이름 없게, 띄어쓰기 지양)

![](<../../../.gitbook/assets/image (6) (1).png>)

It will ask for Ulink driver installation. Proceed with installation

> Keil Tools 범용 직렬버스 컨트롤러 설치(USB 연결 드라이브)

![](<../../../.gitbook/assets/image (40).png>)

### Install Packs

After the uVision IDE is installed, we need to install necessary software Packs.

We are going to use STM32F411RE board for the lab, and we will install additional software packs specific for this board.

> If you use STM32F401RE, then search for this model

On Left Window Pane: **Devices** tab: **STMicroelectronics > STM32F4 Series> STM32F411RE> STM32F411RETx** 선택

{% hint style="info" %}
uVision이 업데이트 설치 완료 될때까지 Packs>Action:install이 비활성화 됨
{% endhint %}

On Right Window Pane: Packs tab: **Keil::STM32F4xxFxx\_DFP** Install

![](<../../../.gitbook/assets/image (25).png>)

###

### Install ST-Link Driver

[Download driver (STSW-LINK009) here](https://www.st.com/en/development-tools/stsw-link009.html)

> ST-LINK, ST-LINK/V2, ST-LINK/V2-1, STLINK-V3 USB driver signed for Windows10. This USB driver (STSW-LINK009) is for ST-LINK/V2, ST-LINK/V2-1 and STLINK-V3 boards(STM8/STM32 discovery boards, STM8/STM32 evaluation boards and STM32 Nucleo boards).
>
> It declares to the system the USB interfaces possibly provided by the ST-LINK: ST Debug, Virtual COM port and ST Bridge interfaces. **The driver must be installed prior to connecting the device, in order to have a successful enumeration.**

After you download "**en.stsw-link009\_v2.0.2.zip**", unzip the file.

Update the driver by

1. Connect MCU to PC
2. 윈도우 **장치 관리자 > 범용직렬버스장치> ST-Link Debug >드라이버 업데이트>** Select en-stsw-link009 unzipped folder

{% hint style="info" %}
MCU board (STM32F411) must be connected to your PC to install the USB driver
{% endhint %}

![](<../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1).png>)

> What is ST Link utiliy? [https://m.blog.naver.com/ansdbtls4067/221510252896](https://m.blog.naver.com/ansdbtls4067/221510252896)

### Update Install ST-Link Driver

[When uVision crashes when compiling](https://developer.arm.com/documentation/ka005381/latest/) Download the [ST-Link driver STLinkUSBDriver.dll](STLinkUSBDriver6.1.2.0Signed.zip) file and copy it to the subfolder of your MDK installation directory, such as C:\Keil\_v5\ARM\STLink\\
